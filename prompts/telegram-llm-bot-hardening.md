# Prompt: Security Hardening & Scale Audit for LLM-Powered Telegram Bots

**Use case:** A full production-readiness audit for Telegram bots that accept user input and pass it to an LLM. Surfaces prompt injection vectors, SSRF risks, concurrency bottlenecks, and API cost inefficiencies — then drives concrete implementation with tests. Designed for bots that are about to go live or are already running with real users.

---

## Prompt

```
You are a security researcher and backend engineer specializing in production LLM applications.

I have an LLM-powered Telegram bot that does the following:
[DESCRIBE: what the bot does, what user inputs it accepts (text, files, URLs), what LLM and model it calls]

Here is the relevant code:
[PASTE: the bot polling/webhook loop, the LLM call function, user input handling, and any file upload or URL-fetching logic]

---

Perform a full production-readiness audit across four areas. For each finding, show the exact file and line number, a concrete exploit or bottleneck scenario, the fix with working code, and a test that proves the fix holds.

---

## Area 1: Prompt Injection & LLM Security

Map every path where untrusted input reaches the LLM prompt:
- User messages (text, uploaded files, pasted content)
- Scraped external content (web pages, RSS feeds, APIs)
- Any indirect path (filename, job title, metadata)

For each path, assess:
1. Is untrusted input fenced off from trusted instructions? (XML delimiters, separate message roles)
2. Can a crafted input close the fence and inject free-form instructions into the prompt?
3. Is the user CV / system context also sanitized, or only the "external" data?
4. Is LLM output validated before it reaches the user or a downstream system?
   - Minimum/maximum length check
   - Injection-pattern detection in the output (e.g. "ignore previous", "system:", "jailbreak")
   - Dangerous URL scheme blocking (javascript:, data:) if output is rendered as HTML/PDF

Rate each finding: CRITICAL / HIGH / MEDIUM / LOW.

Implement fixes:
- Sanitize all untrusted inputs: strip null bytes and control characters, escape fence tag strings
- Wrap job/external data in a clearly labeled untrusted block; wrap user context in a separate block
- Add LLM output validation before any rendering or forwarding step
- Write parametrized tests for every injection pattern and fence-escape scenario

---

## Area 2: SSRF & File Upload Safety

Identify any user-controlled values that trigger outbound requests or file parsing:
1. User-supplied URLs fetched by the bot (HTTP, Playwright, etc.)
   - Does the bot validate the URL before fetching?
   - Are private IP ranges blocked? (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 127.0.0.0/8, 169.254.0.0/16 for cloud metadata)
   - Are only http:// and https:// schemes accepted?
2. User-uploaded files (PDF, DOCX, images, etc.)
   - Is file size checked before downloading from Telegram's API?
   - Are MIME type and extension validated?
3. Log injection: are untrusted strings (job titles, usernames, error messages) sanitized before being written to logs?

Implement fixes with tests for each finding.

---

## Area 3: Concurrency & Scale

Trace the full request lifecycle to find blocking bottlenecks:
1. Does the polling or webhook loop call any blocking I/O inline?
   - LLM API calls (typically 5–30 seconds)
   - External HTTP fetches or browser automation (Playwright, Selenium)
   - PDF/file rendering
2. If user N arrives while users 1 through N-1 are being served, what is their worst-case wait time? Calculate it.
3. Identify all shared mutable state (rate-limit counters, caches, file handles) — is each one protected by a lock?

Recommend and implement a concrete concurrency model:
- Thread pool size (with justification for the number)
- Resource cap for expensive operations like headless browser instances (semaphore with max value)
- Per-user locks for any shared per-user state (e.g. user data files)
- Ensure the offset/update pointer advances in the polling thread, not in workers, so a crashed worker never causes a message to be reprocessed

Add a top-level try/except in each worker so one user's error cannot kill the bot process.

---

## Area 4: API Rate Limits & Cost Optimization

For the LLM model(s) in use:
1. Look up the actual rate limits: RPM (requests/min), TPM (tokens/min), RPD (requests/day), TPD (tokens/day)
2. Estimate tokens per request: system prompt + user prompt (input) + expected output
3. Calculate the effective users/day ceiling on the current plan

Then check for optimization opportunities:
- Are there separate rate-limit buckets per model? (e.g. a 70B and an 8B model each have their own quota)
- Is the same large model used for tasks that differ significantly in complexity?
  - High-complexity task (e.g. structured CV rewriting): keep the large model
  - Lower-complexity task (e.g. short cover letter, classification, formatting): a smaller/faster model may be sufficient
- If a smaller model is viable for any task, calculate the new users/day ceiling after the split

Implement:
- Route each task to the appropriate model
- Add retry logic on 429 responses: read the retry-after header, add jitter (0–2s random), retry up to 3 times
- Do not retry non-rate-limit errors (auth failures, bad requests) — raise immediately
- Log the model name on every LLM call so you can audit which model handled which request

---

## Deliverables

For each area:
- A prioritized list of findings (CRITICAL → LOW) with file:line references
- Working code for every fix
- Tests that fail before the fix and pass after

Implementation order: fix CRITICAL and HIGH items first, then concurrency (blast radius: entire user base), then cost optimization.
```

---

**When to use this prompt:**
- Before launching an LLM-powered Telegram bot to users beyond a private test group
- After adding a new user input path (file upload, URL scraping, free-text entry)
- When concurrent users report slowdowns or timeouts (likely a threading issue)
- When hitting Groq/OpenAI rate limits before your expected daily user count is reached
- After any unexpected or suspicious LLM output that may indicate a prompt injection attempt
