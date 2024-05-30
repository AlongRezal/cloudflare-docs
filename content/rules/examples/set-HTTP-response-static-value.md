---
pcx_content_type: example
summary: A Transform Rule where a header named `X-Bot-Score` is set to a static value (`Cloudflare`) in the response.
tags:
  - Transform
title: Set an HTTP response header to a static value
---

# Set an HTTP response header to a static value

The following HTTP response header modification rule sets a header named `X-Source` to a static value (`Cloudflare`) in the response:

{{<example>}}

Text in **Expression Editor**:

```txt
starts_with(http.request.uri.path, "/en/")
```

Selected operation under **Modify response header**: _Set static_

**Header name**: `X-Source`

**Value**: `Cloudflare`

{{</example>}}

This rule would overwrite any existing `X-Source` headers already present in the response.