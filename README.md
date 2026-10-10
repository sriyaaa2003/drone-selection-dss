# Drone selection decision-support system for small EU ports

Course project (DV2573 Decision Support Systems, Blekinge Institute of Technology). It ranks 20 commercial drones for five port missions and explains the result, with answers grounded in EU drone regulation text.

**Demo:** [frontend](https://intelligent-dss-for-drone-selection-sandy.vercel.app) · [API docs](https://intelligent-dss-for-drone-selection-in.onrender.com/docs)

## Pipeline

```
scenario + drone specs -> rule filter (22 rules) -> AHP weights -> TOPSIS ranking
                       -> Monte Carlo weight sensitivity -> LLM explanation (RAG on EASA rules)
```

| Step | What it does |
|---|---|
| Rule base | 22 rules from EU 2019/947 and operational limits remove drones that cannot fly the mission |
| AHP | 20x20 pairwise matrix (Saaty scale), consistency ratio 0.0159 |
| TOPSIS | Closeness coefficient CC = D- / (D+ + D-) on the weighted, normalised matrix |
| Sensitivity | 300 Monte Carlo runs with ±20% weight perturbation, labelled HIGH / MEDIUM / LOW stability |
| Explanation | Local Ollama (`llama3.1:8b`) in development, Groq (`openai/gpt-oss-120b`) in production, set by `LLM_PROVIDER` |
| RAG | 617-page EASA Easy Access Rules PDF, about 3,600 chunks, Cohere embeddings, FAISS top-4, answers cite page numbers |

## Data

20 commercial drones (DJI, Parrot, Autel, Wingtra, Skydio, Nordic Drones and others) described by 20 criteria taken from manufacturer specifications, and five port scenarios (surveillance, security, inspection, environmental monitoring, emergency response, budgets EUR 8,000 to 35,000).

## Run

```bash
cd api && pip install -r requirements.txt
export LLM_PROVIDER=ollama          # or groq, with GROQ_API_KEY
export COHERE_API_KEY=...           # only to rebuild the index: python retrieval/ingest.py
uvicorn index:app --reload --port 8003
cd ../public && python -m http.server 8081
```

Endpoints: `GET /api/drones`, `/api/scenarios`, `/api/ahp`; `POST /api/evaluate`, `/api/evaluate/custom-scenario`, `/api/ai/overview`.

## Limits

The scores come from published specifications and expert pairwise judgements, not field trials. There are no automated tests.

## Team

Sriya Chittaneni, Sri Sumedh Pisapati, Meghashyam Sai Dontha, Daiki Saito, Venkata Naga Sai Yaswanth Deevi. References: Saaty and Vargas (2012), Hwang and Yoon (TOPSIS), EU 2019/947.
