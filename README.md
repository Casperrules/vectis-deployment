# Vectis - High-Performance Standalone Semantic Search

Vectis is a blazing fast, single-binary semantic search engine built in Rust. It combines traditional keyword search (BM25) with state-of-the-art vector search (HNSW) to provide accurate, context-aware results without the complexity of enterprise search clusters.

## 🚀 Why Vectis? (Unique Features)

Unlike other search solutions that require complex setups (Python, Docker, JVM, Postgres), Vectis is designed for **simplicity and performance**:

- **📦 Zero Dependencies**: A single executable file. No Python environment, no Java VM, no external database, and no Docker containers required.
- **🧠 Embedded AI**: Runs the `all-MiniLM-L6-v2` transformer model locally using the ONNX Runtime. Generates embeddings on the fly without calling external APIs (like OpenAI).
- **🔍 Hybrid Search**: Combines **Tantivy** (BM25 keyword search) and **HNSW** (Hierarchical Navigable Small World vector search) using Reciprocal Rank Fusion (RRF) for superior relevance.
- **⚡ Dual Mode**: Functions as both a high-performance **REST API Server** and a **CLI Client** in the same binary.
- **📄 Native File Support**: Automatically parses and indexes **CSV** (product data, logs) and **TXT** files. CSV rows are indexed as individual documents with metadata.
- **⚡ Async Architecture**: Heavy operations like **Upload**, **Deletion**, and **Reindexing** are decoupled from the API, running in background workers to ensure responsiveness.
- **💾 Resource Efficient**: Uses ~200MB of RAM compared to 1GB+ for Elasticsearch/OpenSearch. Starts in < 1 second.

## 📊 Benchmarks

Vectis is optimized for low-latency search on consumer hardware.

**Test Environment**: macOS (Apple Silicon), `all-MiniLM-L6-v2` model.

### Search Latency

| Operation         | Latency    | Notes                                                                  |
| ----------------- | ---------- | ---------------------------------------------------------------------- |
| **Hybrid Search** | **~17 ms** | Includes embedding generation + HNSW search + BM25 search + RRF merge. |
| **RRF Merge**     | **~12 µs** | Negligible overhead for combining results.                             |

### Comparison vs. Elasticsearch

| Metric               | Vectis 🦀    | Elasticsearch ☕️  | Advantage           |
| -------------------- | ------------ | ------------------ | ------------------- |
| **Startup Time**     | **< 1s**     | > 10s              | **Instant**         |
| **Memory Footprint** | **~200 MB**  | > 1 GB (JVM)       | **5x Efficiency**   |
| **Vector Search**    | **~17 ms**   | ~50-200 ms         | **Local Execution** |
| **Deployment**       | **1 Binary** | Docker/JVM Cluster | **Simplicity**      |

> _Note: Indexing throughput is ~56 docs/sec on CPU, which is optimized for personal/SMB use cases rather than massive-scale ingestion._

## 🛠️ Installation & Usage

### 1. Start the Server

```bash
./vectis serve
# Listening on http://0.0.0.0:3000
```

### 2. CLI Interaction (New Terminal)

**Register & Login**

```bash
./vectis register --username admin --password secret
./vectis login --username admin --password secret
# Returns a JWT Token
```

**Upload Data**
Supports `.txt` and `.csv` (automatically parses rows as documents).

```bash
./vectis upload --file products.csv --token "YOUR_TOKEN"
# Returns a Task ID. The CLI/UI polls for completion.
```

**Reindex Data**
Re-run the embedding and indexing process for all your files.

```bash
./vectis reindex --token "YOUR_TOKEN"
# Returns a Task ID.
```

**Search**

```bash
./vectis search --query "fast gaming laptop" --token "YOUR_TOKEN"
```

## 🏗️ Architecture

- **Language**: Rust 🦀
- **Web Framework**: Axum
- **Keyword Search**: Tantivy (Lucene alternative in Rust)
- **Vector Search**: HNSW (hnsw_rs)
- **ML Runtime**: ONNX Runtime (ort)
- **Database**: SQLite (Embedded, for user management)
- **Storage**: Local filesystem (no external services)

## 📜 License

MIT
