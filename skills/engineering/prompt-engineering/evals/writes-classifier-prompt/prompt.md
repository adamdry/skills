---
tags: [skill-creator]
max_turns: 40
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

Write me a prompt that sorts inbound emails for a UK lettings agency into one of five categories: urgent_repair, routine_repair, rent_query, viewing_request, other. Our Python service reads the answer and routes the email, so it needs exactly one label it can match. Model is claude-haiku-5-5 at temperature 0, one call per email, and it has to answer in about a second. We have 200 emails our property managers labelled by hand, and we can hold 50 of those back for testing. Missing an urgent repair (gas, flooding, no heating in winter) is far worse than a false alarm. The emails are whatever tenants and the public send us, so treat the text as untrusted. English only.
