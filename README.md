# Drone selection decision-support system for small EU ports

A course project (DV2573 Decision Support Systems, Blekinge Institute of Technology). It ranks 20 commercial drones for five port missions and explains the ranking using EU drone rules.

**Demo:** [frontend](https://intelligent-dss-for-drone-selection-sandy.vercel.app) · [API docs](https://intelligent-dss-for-drone-selection-in.onrender.com/docs)

## What we did
- Collected 20 commercial drones (DJI, Parrot, Autel, Wingtra, Skydio, Nordic Drones and others) with 20 criteria each from manufacturer specs.
- Defined five port missions: surveillance, security, inspection, environmental monitoring and emergency response, with budgets from EUR 8,000 to 35,000.
- Wrote 22 rules from EU 2019/947 that remove drones that cannot fly a mission.
- Weighted the criteria with AHP, ranked the drones with TOPSIS, and checked how stable the ranking is with a Monte Carlo test.
- Added an assistant that explains the result and answers questions from the 617-page EASA rules PDF, with page numbers.

## Results
- The AHP matrix has a consistency ratio of 0.0159.
- The Monte Carlo test (300 runs, weights changed by ±20%) labels each ranking as HIGH, MEDIUM or LOW stability.
- The assistant's answers cite pages of the EASA rules.

## How it was built

```
scenario + drone specs -> rule filter (22 rules) -> AHP weights -> TOPSIS ranking
                       -> Monte Carlo sensitivity -> LLM explanation (RAG on EASA rules)
```

- **RAG:** the EASA PDF is split into about 3,600 chunks, embedded with Cohere and searched with FAISS (top 4).
- **LLM:** Ollama `llama3.1:8b` locally and Groq `openai/gpt-oss-120b` in production.
- **Frontend and hosting:** static page on Vercel, API on Render.

## Tech stack
Python, FastAPI, NumPy, FAISS, Cohere embeddings, Ollama, Groq, HTML/JavaScript, Vercel, Render.

## Team
Sriya Chittaneni, Sri Sumedh Pisapati, Meghashyam Sai Dontha, Daiki Saito, Venkata Naga Sai Yaswanth Deevi.
