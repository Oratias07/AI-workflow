# Prompt: Code Security Review

**Use case:** Systematic security analysis of C, C++, or TypeScript code. Useful when reviewing your own projects, auditing a codebase before submission, or preparing for a security-focused role where you need to demonstrate code review skills.

---

## Prompt

```
You are a security-focused code reviewer. Analyze the following code for security vulnerabilities and unsafe patterns.

**Code to review:**
[PASTE CODE HERE]

**Language:** [C / C++ / TypeScript / other]
**Context:** [Brief description of what this code does and where it runs]

---

For each issue found, provide:
1. **Vulnerability type** — e.g., buffer overflow, use-after-free, injection, improper input validation
2. **Location** — function name or line number
3. **Severity** — Critical / High / Medium / Low
4. **Explanation** — why this is a problem and what an attacker could do with it
5. **Fix** — the corrected code snippet, not just a description

After the per-issue breakdown, provide:
- A summary of the overall security posture
- The top 1–2 changes that would have the highest security impact
- Any patterns (not just individual bugs) that suggest systemic issues

Be specific. Don't flag things that aren't actual risks. If the code is clean, say so and explain why.
```

---

**When to use this prompt:**
- Before pushing code to a public repo
- When reviewing C/C++ systems code for memory safety issues
- When auditing TypeScript/Node.js code for injection or auth flaws
- As a learning exercise — compare the AI's findings against what you catch manually
