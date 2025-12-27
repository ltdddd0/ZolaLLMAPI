This project provides tools and documentation for deploying and interacting with the **PanSou** search engine using Docker. PanSou is a unified search interface for various cloud storage resources (Baidu, Aliyun, Quark, etc.).

## Project Structure

- `api-guideline.md`: Detailed documentation for interacting with the PanSou API.
- `init.org`: Emacs Org-mode file containing Docker deployment commands and API usage examples.
- `search.sh`: A shell script to perform searches from the command line.
- `temp/`: Directory containing additional manual and guidelines.

## Quick Start

### 1. Deployment

You can deploy PanSou using Docker. Example commands are available in `init.org`.

```bash
# Run the PanSou backend with authentication
docker run -d --name pansou -p 8888:80 \
  -e AUTH_ENABLED=true \
  -e AUTH_USERS=admin:admin123 \
  -e AUTH_TOKEN_EXPIRY=24 \
  ghcr.io/fish2018/pansou:latest
```

### 2. Authentication

Authenticate to obtain a JWT token:

```bash
curl -X POST http://localhost:8888/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}'
```

### 3. Search

Use the `search.sh` script or call the API directly:

```bash
# Using the script
./search.sh "your_search_keyword"

# Using cURL
curl -X POST http://localhost:8888/api/search \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"kw":"your_search_keyword"}'
```

## API Features

- **Multi-channel Search:** Search across multiple Telegram channels and plugins.
- **Resource Merging:** Automatically merges links by cloud drive type.
- **Plugin Support:** Extendable search capabilities via plugins.

## Output Format

The API returns results in JSON format. Key fields include:

- `total`: Number of results found.
- `merged_by_type`: Resources grouped by storage type (e.g., `quark`, `aliyun`, `baidu`).
- `results`: Detailed list of individual search hits.

Example output:
```json
{
  "code": 0,
  "message": "success",
  "data": {
    "total": 20,
    "merged_by_type": {
      "quark": [
        {
          "url": "https://pan.quark.cn/s/...",
          "note": "Resource title...",
          "datetime": "2024-10-27T14:46:26Z"
        }
      ]
    }
  }
}
```

For more detailed information, refer to `api-guideline.md`.
