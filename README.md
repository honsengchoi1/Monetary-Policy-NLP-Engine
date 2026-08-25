# Monetary Policy NLP Engine: Unsupervised Macro Regime Detection & Signal Architecture

**Architect:** Hon Seng Choi | Director of Market & Risk Analytics
**Target Scope:** Global Macro Strategy, Rates Volatility, Systematic Signal Generation
**Core Infrastructure:** Python, Scikit-Learn (NLP), JSON (Stateless Ledger), Streamlit (Decoupled UI)

👉 **[Launch Interactive Risk HUD (Live Production Build)](https://honsengchoi1-monetary-policy-nlp-engine-srcdashboard-11ohxq.streamlit.app/)** 

---

## 1. Executive Business Objective & Strategic Edge

Traditional macroeconomic analysis relies heavily on subjective, human interpretation of central bank communications, introducing behavioral bias and processing latency. This repository contains a production-grade ELT pipeline and Unsupervised Language Analytics Engine designed to systematically quantify Federal Reserve policy shifts and extract actionable alpha from unstructured text data.

**The Real-Time Triangulation Edge:** 
While FOMC minutes are released on a 3-week lag following a rate decision, this engine's commercial power lies in establishing a quantifiable, structural policy baseline. By coupling this established baseline with the real-time NLP ingestion of Fed Chair press conferences, regional speeches, and the Beige Book, quantitative desks can instantly triangulate the Fed’s forward-looking intent. This provides an enormous predictive advantage, allowing the firm to actively manage affected portfolio exposures and scale risk dynamically before broader market pricing occurs.

---

## 2. Commercial Signal Generation & The Bottom Line

To solve the quantitative "Black Box" problem, this engine does not merely flag statistical percentage anomalies. It attributes a fundamental macro narrative to the mathematical signal, generating direct commercial value across two pillars:

*   **Capital Preservation (Risk Sizing):** The engine establishes a mathematical baseline representing routine administrative noise (52.10% text modification). By instantly flagging systemic deviations from this mean without human latency, the firm can dynamically scale down risk exposure to safeguard corporate capital prior to a tail-risk event.
*   **Systematic Alpha Generation:** Utilizing absolute vector subtraction, the engine extracts the exact **Text Drivers** (the top high-variance vocabulary tokens) driving the regime shift. Because these anomaly words are rare, categorical events, they are passed through specialized monetary policy LLMs (e.g., an FOMC-fine-tuned RoBERTa model) to extract their fundamental semantic meaning. This transforms qualitative anomalies into continuous sentiment scores, creating robust, independent feature inputs (X) for downstream predictive trading models.

---

## 3. The Core NLP Analytics Engine (`src/analytics_engine.py`)

The pipeline explicitly separates raw data ingestion from pure mathematical execution:

*   **TF-IDF Vectorization (The Importance Filter):** Automatically strips out repetitive, administrative central bank syntax (e.g., "january", "committee") and assigns heavy mathematical weight to unique, unexpected vocabulary vectors (e.g., "invasion", "banking").
*   **Cosine Similarity (The Magnitude Vector):** Measures the geometric distance between sequential meetings, quantifying the overall magnitude of the thematic policy shift regardless of total document length.
*   **Absolute Vector Difference (Narrative Attribution):** Subtracts the previous period's TF-IDF array from the current array, mathematically sorting the absolute differences in descending order to definitively isolate and rank the underlying Text Drivers causing the anomaly.

### Unsupervised Historical Signal Detection
The engine successfully isolated the following macro turning points purely through vocabulary variance, with zero manual input:

| Anomaly Date | Macro Shift | High-Variance Text Drivers | Systemic Regime Identified |
| :--- | :--- | :--- | :--- |
| **March 2022** | **80.34%** | `invasion`, `ukraine`, `russian` | Geopolitical conflict derailing global macro inflation projections. |
| **March 2023** | **73.02%** | `banking`, `signature`, `closures` | Regional banking liquidity crisis; immediate Fed pivot to systemic stabilization. |
| **March 2026** | **67.18%** | `east`, `conflict`, `middle` | Escalation of energy market supply-chain risk vectors. |

---

## 4. System Architecture & Zero-Latency Data Engineering

The system is built on a decoupled architecture ensuring high-fidelity data governance and rapid UI rendering.

> [ FEDERAL RESERVE ARCHIVE ] => [ ELT PIPELINE ] => [ JSON STATELESS LEDGER ] => [ NLP ENGINE ] => [ WEB HUD ]
>    (HTML Scraping)          (Incremental Parser)    (In-Memory Persistence)     (Vector Math)     (Streamlit)

**Why a JSON Ledger vs. Relational SQL?**
Relational databases (SQL) are designed for heavy transactional mapping. However, FOMC minutes are unstructured, periodically released text blobs. By structuring the parsed payload into a lightweight JSON ledger (`fomc_cleaned_data.json`), the pipeline achieves **zero-latency, in-memory loading** directly into Streamlit and Pandas DataFrames, completely eliminating database server overhead and read/write bottlenecks.

*   `date`: The 8-digit unique primary key (`YYYYMMDD`). Ensures idempotency (no duplicate scrapes).
*   `text_length`: Quality control checksum metric. Detects web-scraping failures or unexpected HTML DOM structure changes on the central bank's servers.
*   `cleaned_content`: The fully sanitized payload passed directly into the Scikit-Learn vectorization engine.

---

## 5. Execution Protocol & Local Deployment

This pipeline utilizes an incremental scrape-and-append methodology. 

**1. Install Dependencies**
```bash
pip install -r requirements.txt
```

**2. Run the Automated Ingestion Pipeline**
Executes the scraping protocol. Automatically audits the JSON ledger to prevent duplicate entries and logs operational metadata to `pipeline_audit_log.json`.
```bash
python src/data_pipeline.py
```

**3. Launch the Interactive Decoupled UI**
The dashboard automatically imports the `analytics_engine.py` module to execute the vector math in the background. It utilizes high-precision session-state key binding (`on_click=trigger_param_reset`) to ensure fluid parameter adjustments without multi-viewport rendering crashes.
```bash
streamlit run src/dashboard.py
```

## 6. Future Evolution: Multi-Channel Ingestion & Directional Sentiment

To transition this pipeline from a pure magnitude detector into a fully autonomous trading system, the next iteration will focus on multi-source triangulation and semantic classification:

*   **Directional Sentiment Classifier (Hawk / Dove / Neutral):** While Cosine distance successfully measures the *magnitude* of a policy shock, it does not interpret *direction*. The next phase integrates a specialized central bank NLP model (e.g., FOMC-RoBERTa) to semantically classify the extracted Text Drivers. This will definitively map whether the structural shift represents Hawkish tightening (triggering a short-duration bias) or Dovish easing (triggering a long-duration bias).
*   **Multi-Channel Policy Triangulation:** FOMC minutes inherently carry a 3-week publication lag. This core NLP architecture will be expanded to ingest real-time, unstructured text streams—specifically live Fed Chair Press Conferences, Regional Fed speeches, and the Beige Book. By running these real-time transcripts against the established 52.10% FOMC baseline, the engine will identify intraday policy deviations before the broader market prices them in.