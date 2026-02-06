# Prompt Template Snippet: File Download Pattern

> **Version**: v1.0  
> **Created**: 2026-02-05  
> **Use Case**: Any prompt involving browser file downloads  
> **Origin**: s-1-2 model file download (7 rounds of approach iteration) + s-2-2 application file download (repeated route/token pitfalls)

---

## Why This Template Exists

When Agents encounter file download scenarios, they default to Blob + `URL.createObjectURL`. This is unreliable in actual browsers (Chrome security policies cause `Content-Disposition` filenames to be ignored). Each time the Agent goes through the same detours:

| Common Detour | Problem |
|---------------|---------|
| Axios Blob + `URL.createObjectURL` | Chrome downloads with random filename |
| `window.open(url)` | Opens blank tab |
| Hidden iframe | Chrome ignores Content-Disposition |
| `<a>` + `.click()` via JS | Same as above |
| `window.location.href` | Same as above |

**The validated approach** is the simplest: native `<a href>` link + backend `?token=` query param.

---

## How to Use

**Copy the code blocks below directly into your Prompt** in the corresponding backend and frontend implementation sections. Replace `{{...}}` placeholders with actual values.

---

## Backend Implementation (FastAPI)

```python
# ⚠️ Copy directly into the backend implementation section of your Prompt

from fastapi import APIRouter, Query, Header, HTTPException
from fastapi.responses import FileResponse
from typing import Optional
import urllib.parse
import re

router = APIRouter()

@router.get("/{{download_endpoint_path}}")
async def download_file(
    {{path_params}},
    token: Optional[str] = Query(None, description="JWT token (fallback for <a href> downloads)"),
    authorization: Optional[str] = Header(None),
):
    """
    File download endpoint.
    Auth priority: Authorization header > ?token= query param
    """
    # 1. Auth: prefer header, fallback to query param
    jwt_token = None
    if authorization and authorization.startswith("Bearer "):
        jwt_token = authorization[7:]
    elif token:
        jwt_token = token
    
    if not jwt_token:
        raise HTTPException(status_code=401, detail="Authentication required")
    
    # 2. Verify token and get user (use project's JWT verification logic)
    user = verify_token(jwt_token)  # Replace with actual verification function
    
    # 3. Find file and check permissions
    file_record = get_file_record({{path_params}})  # Replace with actual query logic
    if not file_record:
        raise HTTPException(status_code=404, detail="File not found")
    
    # 4. Build Content-Disposition (ASCII fallback + UTF-8)
    original_filename = file_record.filename  # e.g., "model_v2.zip"
    ascii_filename = re.sub(r'[^\x20-\x7E]', '_', original_filename)  # Replace non-ASCII with _
    utf8_filename = urllib.parse.quote(original_filename)
    
    headers = {
        "Content-Disposition": (
            f'attachment; filename="{ascii_filename}"; '
            f"filename*=UTF-8''{utf8_filename}"
        )
    }
    
    return FileResponse(
        path=file_record.file_path,
        headers=headers,
        media_type="application/octet-stream",
    )
```

### Key Points

- **Dual auth channel**: `Authorization` header (API calls) + `?token=` query param (browser `<a href>` links)
- **ASCII fallback**: Replace non-ASCII chars with `_`, **never use `?`** (Chrome rejects filenames containing `?`)
- **Dual filename**: Both `filename` (ASCII fallback) and `filename*` (UTF-8 original) must be present
- **Route registration**: This endpoint must be registered in the router (`app.include_router(...)` or equivalent) — just adding a method isn't enough

---

## Frontend Implementation (Vue 3 + Element Plus)

```vue
<!-- ⚠️ Copy directly into the frontend implementation section of your Prompt -->

<template>
  <!-- Use native <a> link, do NOT trigger via JS -->
  <a
    :href="downloadUrl"
    class="download-link"
  >
    <el-button type="primary" :icon="Download">
      Download File
    </el-button>
  </a>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { Download } from '@element-plus/icons-vue'
import { useAuthStore } from '@/stores/auth'

const props = defineProps<{
  {{prop_type_definition}}  // e.g., fileId: string
}>()

const authStore = useAuthStore()

const downloadUrl = computed(() => {
  const token = authStore.accessToken
  return `/api/v1/{{download_endpoint_path}}?token=${token}`
})
</script>
```

### Anti-Patterns (Do NOT Use)

```typescript
// ❌ Do NOT use Blob download
const response = await axios.get(url, { responseType: 'blob' })
const blob = new Blob([response.data])
const link = document.createElement('a')
link.href = URL.createObjectURL(blob)  // Chrome filename unreliable

// ❌ Do NOT use window.open
window.open(downloadUrl)  // Opens blank tab

// ❌ Do NOT use JS-triggered <a> click
const a = document.createElement('a')
a.href = downloadUrl
a.click()  // Chrome ignores Content-Disposition

// ❌ Do NOT use iframe
const iframe = document.createElement('iframe')
iframe.src = downloadUrl  // Unreliable
```

---

## Prompt Reference Example

Reference in your Prompt like this:

```markdown
### File Download Implementation

Use the validated standard download pattern (see `templates/file-download-pattern.md`):
- Backend: `?token=` query param + Content-Disposition dual filename
- Frontend: native `<a :href>` link

Specific implementation:

[Copy backend code from template, replace {{download_endpoint_path}} with actual path]
[Copy frontend code from template, replace {{prop_type_definition}} with actual props]
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2026-02-05 | Initial version. Extracted from s-1-2 (7 rounds of download approach iteration) and s-2-2 (repeated route/token pitfalls) |
