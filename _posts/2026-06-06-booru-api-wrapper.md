---
title: "Python Booru API Wrapper — Async Client Library"
description: "Build a Python async client for booru APIs with type hints, retry logic, rate limiting, and multi-format support. Includes code examples and setup guide."
---

# Python Booru API Wrapper Guide

Building a robust API client for booru image boards requires handling multiple API formats, rate limits, and authentication methods. Here's how to create a maintainable async wrapper.

## Core Architecture

```python
import httpx
from typing import Any, Optional

class BooruClient:
    BASE_URLS = {
        "rule34": "https://rule34.ink/api/v1",
        "danbooru": "https://danbooru.donmai.us",
    }

    def __init__(self, base_url: str | None = None):
        self.base_url = base_url or self.BASE_URLS["rule34"]
        self.client = httpx.AsyncClient(timeout=30)

    async def search(self, tags: list[str], limit=100):
        params = {"tags": " ".join(tags), "limit": limit}
        resp = await self.client.get(f"{self.base_url}/posts.json", params=params)
        resp.raise_for_status()
        return resp.json()
```

For a complete implementation, reference the [rule34.ink API](https://rule34.ink/api) documentation.

## Features

- Async/await for concurrent requests
- Automatic retry with exponential backoff
- Rate limit awareness and header parsing
- Multi-format support (JSON, XML)
- Type-annotated responses
