---
name: chirpy-pageviews-goatcounter
description: How to enable article view counts using GoatCounter in Chirpy theme
metadata:
  type: project
---

This Jekyll blog uses the Chirpy theme, which supports article page view counts via GoatCounter.

**Configuration location:** `_config.yml` lines 77-78

**How to enable:**
1. Register at https://www.goatcounter.com/ to get a site ID
2. Update `_config.yml`:
   - Line 63-64: Add `analytics.goatcounter.id: your-site-id`
   - Line 78: Set `pageviews.provider: goatcounter`
