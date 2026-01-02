# Vectis User Guide

Welcome to the comprehensive user guide for **Vectis**, the high-performance, standalone semantic search engine.

## Table of Contents

1.  [Installation](#1-installation)
2.  [Configuration (CORS & Security)](#2-configuration)
3.  [Running the Server](#3-running-the-server)
4.  [Using the CLI](#4-using-the-cli)
5.  [API Reference](#5-api-reference)
6.  [Troubleshooting](#6-troubleshooting)

---

## 1. Installation

Vectis is distributed as a single executable binary.

1.  **Download** the latest release zip file (`vectis_deployment.zip`).
2.  **Unzip** the archive to your desired location.
3.  **Verify** the contents:
    - `vectis` (The executable)
    - `.env.example` (Configuration template)
    - `README.md` & `QUICKSTART.md`

## 2. Configuration

Vectis uses a `.env` file for configuration.

1.  **Create the config file**:

    ```bash
    cp .env.example .env
    ```

2.  **Edit `.env`**:
    Open the file in any text editor.

    ```ini
    # The port the server will listen on (Default: 3000)
    PORT=3000

    # Secret key for signing JWT tokens. CHANGE THIS for production!
    JWT_SECRET=your-super-secret-key-change-me

    # CORS Configuration (Crucial for Frontend Access)
    # Set this to the URL where your frontend/website is hosted.
    # Examples:
    # - Local development: http://localhost:8080
    # - Production: https://my-search-app.com
    FRONTEND_ORIGIN=http://localhost:8080
    ```

### ⚠️ Important: CORS Configuration

**Cross-Origin Resource Sharing (CORS)** is a security feature that restricts which websites can talk to your Vectis server.

- If your frontend is hosted at `https://example.com`, you **MUST** set `FRONTEND_ORIGIN=https://example.com`.
- If this is not set correctly, your browser will block requests with a "CORS Policy" error.

## 3. Running the Server

To start the search engine server:

```bash
./vectis serve
```

You should see output like:

```
🚀 Vectis search engine running on http://0.0.0.0:3000
```

The server is now ready to accept API requests and CLI commands.

## 4. Using the CLI

The `vectis` binary also acts as a command-line client. Open a **new terminal window** to run these commands.

### A. Registration & Login

First, create an admin account and get an access token.

```bash
# Register a new user
./vectis register --username admin --password secret

# Login to get a token
./vectis login --username admin --password secret
```

_Copy the long string returned by the login command. This is your `TOKEN`._

### B. Uploading Data

You can upload `.txt` or `.csv` files.

- **TXT**: Indexed as a single document.
- **CSV**: Each row is indexed as a separate document. Columns are stored as metadata.

```bash
./vectis upload --file products.csv --token "YOUR_TOKEN"
```

_Returns a `Task ID`. The server processes the file in the background._

### C. Searching

Perform a hybrid search (Keyword + Vector).

```bash
./vectis search --query "gaming laptop" --token "YOUR_TOKEN"
```

### D. Reindexing

If you want to regenerate embeddings for all your files (e.g., after an update):

```bash
./vectis reindex --token "YOUR_TOKEN"
```

### E. Deleting Data

To delete all your files and clear the index:

```bash
./vectis delete --token "YOUR_TOKEN"
```

## 5. API Reference

You can interact with Vectis using any HTTP client (curl, Postman, JS fetch).

| Method   | Endpoint             | Description                                                         |
| :------- | :------------------- | :------------------------------------------------------------------ |
| `POST`   | `/api/auth/register` | Register a new user. Body: `{"username": "...", "password": "..."}` |
| `POST`   | `/api/auth/login`    | Login. Returns JWT. Body: `{"username": "...", "password": "..."}`  |
| `POST`   | `/api/upload`        | Upload files (Multipart Form). Returns `task_id`.                   |
| `GET`    | `/api/search?q=...`  | Search. Query param `q`.                                            |
| `GET`    | `/api/tasks/:id`     | Check status of a background task (Upload/Delete/Reindex).          |
| `POST`   | `/api/reindex`       | Trigger reindexing. Returns `task_id`.                              |
| `DELETE` | `/api/delete_all`    | Delete all data. Returns `task_id`.                                 |

## 6. Troubleshooting

### "CORS Policy" Error in Browser

- **Cause**: The `FRONTEND_ORIGIN` in your `.env` file does not match the URL in your browser address bar.
- **Fix**: Update `.env` to match your frontend URL exactly (no trailing slash) and restart the server.

### "Connection Refused"

- **Cause**: The server is not running.
- **Fix**: Ensure you ran `./vectis serve` and it is still running in a terminal.

### "Unauthorized"

- **Cause**: Invalid or expired token.
- **Fix**: Run `./vectis login` again to get a fresh token.
