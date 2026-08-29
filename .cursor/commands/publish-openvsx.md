---
name: publish-openvsx
description: Walk Open VSX publish after tests. Do not invent PATs.
---

# Publish Open VSX

Required for Cursor ([docs](https://cursor.com/help/customization/extensions)).

1. Spec approved, tests green, `vsce package` works
2. User supplies Open VSX PAT via env — never commit it
3. `ovsx publish` with the **same** `publisher.extension` as dual-publish if TRACK publish is dual
4. Optional: Cursor verification (public domain homepage + forum)

Do not publish Microsoft Marketplace *instead of* Open VSX.
