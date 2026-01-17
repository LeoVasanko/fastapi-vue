# fastapi-vue

Implements static files routing with caching, compression and SPA support. Can be mounted at site root, unlike the built-in module.

- Automatic zstd compression
- ETag-based caching with immutable headers for hashed assets
- SPA (Single Page Application) support
- Favicon handling from hashed assets
- Dev mode integration with Vite

## Installation

```bash
uv add fastapi-vue
```

## Usage

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
from fastapi_vue import Frontend

# Point to your built frontend assets
frontend = Frontend(
    "path/to/frontend-build",
    spa=True,
    favicon="/assets/favicon.ico",
    cached=["/assets/"],
)

@asynccontextmanager
async def lifespan(app: FastAPI):
    await frontend.load()
    yield

app = FastAPI(lifespan=lifespan)

# All your other routes...

# Final catch-all route for frontend files (keep at end of file)
frontend.route(app, "/")
```

## Configuration

- `directory`: Path to static files directory
- `spa`: Enable SPA mode (serve index.html for unknown routes)
- `cached`: Path prefixes for immutable cache headers (e.g., `["/assets/"]`)
- `favicon`: Path prefix to serve at `/favicon.ico` (e.g., `"/assets/favicon.webp"`)
- `zstdlevel`: Compression level (default: 18)

## See also

Script [fastapi-vue-setup](https://github.com/LeoVasanko/fastapi-vue-setup) to create a project with statically built Vue using this module, and Vite devmode support.
