# Prompt 模板片段：文件下载模式

> **版本**: v1.0  
> **创建日期**: 2026-02-05  
> **适用场景**: 任何涉及浏览器文件下载的 Prompt  
> **来源**: s-1-2 模型文件下载（7 轮方案迭代）+ s-2-2 申请文件下载（路由/token 重复踩坑）

---

## 为什么需要这个模板

Agent 在遇到文件下载场景时，默认会使用 Blob + `URL.createObjectURL` 方案。这在实际浏览器中不可靠（Chrome 安全策略导致 `Content-Disposition` 文件名不被尊重）。每次都会重走弯路：

| 常见弯路 | 问题 |
|----------|------|
| Axios Blob + `URL.createObjectURL` | Chrome 下载文件名随机 |
| `window.open(url)` | 打开空白标签页 |
| Hidden iframe | Chrome 不尊重 Content-Disposition |
| `<a>` + `.click()` via JS | 同上 |
| `window.location.href` | 同上 |

**已验证的方案**是最简单的：原生 `<a href>` 链接 + 后端 `?token=` query param。

---

## 使用方式

将下面的代码块**直接复制到你的 Prompt 中**对应的后端和前端实现部分。替换 `{{...}}` 占位符为实际值。

---

## 后端实现（FastAPI）

```python
# ⚠️ 直接复制到 Prompt 的后端实现部分

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
    文件下载端点。
    认证优先级：Authorization header > ?token= query param
    """
    # 1. 认证：优先 header，fallback query param
    jwt_token = None
    if authorization and authorization.startswith("Bearer "):
        jwt_token = authorization[7:]
    elif token:
        jwt_token = token
    
    if not jwt_token:
        raise HTTPException(status_code=401, detail="认证信息缺失")
    
    # 2. 验证 token 并获取用户（使用项目的 JWT 验证逻辑）
    user = verify_token(jwt_token)  # 替换为实际的验证函数
    
    # 3. 查找文件并检查权限
    file_record = get_file_record({{path_params}})  # 替换为实际的查询逻辑
    if not file_record:
        raise HTTPException(status_code=404, detail="文件不存在")
    
    # 4. 构建 Content-Disposition（ASCII fallback + UTF-8）
    original_filename = file_record.filename  # 如 "模型_v2.zip"
    ascii_filename = re.sub(r'[^\x20-\x7E]', '_', original_filename)  # 非 ASCII 用 _ 替换
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

### 关键点

- **认证双通道**：`Authorization` header（API 调用）+ `?token=` query param（浏览器 `<a href>` 链接）
- **ASCII fallback**：用 `_` 替换非 ASCII 字符，**不要用 `?`**（Chrome 会拒绝含 `?` 的文件名）
- **双 filename**：`filename`（ASCII fallback）+ `filename*`（UTF-8 原始名）同时存在
- **路由注册**：此端点必须在路由表中注册（`app.include_router(...)` 或等效方式），不是"加个方法就能用"

---

## 前端实现（Vue 3 + Element Plus）

```vue
<!-- ⚠️ 直接复制到 Prompt 的前端实现部分 -->

<template>
  <!-- 使用原生 <a> 链接，不通过 JS 触发 -->
  <a
    :href="downloadUrl"
    class="download-link"
  >
    <el-button type="primary" :icon="Download">
      下载文件
    </el-button>
  </a>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { Download } from '@element-plus/icons-vue'
import { useAuthStore } from '@/stores/auth'

const props = defineProps<{
  {{prop_type_definition}}  // 如 fileId: string
}>()

const authStore = useAuthStore()

const downloadUrl = computed(() => {
  const token = authStore.accessToken
  return `/api/v1/{{download_endpoint_path}}?token=${token}`
})
</script>
```

### 反模式（不要使用）

```typescript
// ❌ 不要使用 Blob 下载
const response = await axios.get(url, { responseType: 'blob' })
const blob = new Blob([response.data])
const link = document.createElement('a')
link.href = URL.createObjectURL(blob)  // Chrome 文件名不可靠

// ❌ 不要使用 window.open
window.open(downloadUrl)  // 打开空白标签页

// ❌ 不要使用 JS 触发的 <a> click
const a = document.createElement('a')
a.href = downloadUrl
a.click()  // Chrome 不尊重 Content-Disposition

// ❌ 不要使用 iframe
const iframe = document.createElement('iframe')
iframe.src = downloadUrl  // 不可靠
```

---

## Prompt 引用示例

在你的 Prompt 中这样引用：

```markdown
### 文件下载实现

采用已验证的标准下载模式（参见 `templates/file-download-pattern.md`）：
- 后端：`?token=` query param + Content-Disposition 双 filename
- 前端：原生 `<a :href>` 链接

具体实现如下：

[从模板复制后端代码，替换 {{download_endpoint_path}} 为实际路径]
[从模板复制前端代码，替换 {{prop_type_definition}} 为实际属性]
```

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2026-02-05 | 初始版本。从 s-1-2（7 轮下载方案迭代）和 s-2-2（路由/token 重复踩坑）中提炼 |
