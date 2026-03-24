# AI-Workflow

**A structured approach to working with AI as a developer**

---

## What This Repo Is

Most developers use AI tools reactively — paste a problem, accept the output, move on. This repo documents a different approach: treating AI as a technical collaborator that requires precise input, critical review, and clear behavioral expectations.

It contains the prompts I use, the principles I hold AI to, and the configuration that shapes how I work. It's not a showcase of AI-generated output — it's a record of methodology.

---

## How I Work With AI

**Precise input.** Vague prompts produce vague results. Before I ask anything, I define the context, the constraints, and what a good answer actually looks like. Most bad AI output is a prompt problem.

**Critical review.** AI-generated code goes through the same scrutiny as any other code: memory safety, edge cases, security implications. Trusting output blindly is how subtle bugs get shipped to production.

**Iterative refinement.** First outputs are drafts. I push back on the approach, ask for tradeoffs, and request alternatives I didn't think of. The most useful thing AI can do is tell me my framing is wrong before I implement it.

**Production standards only.** I don't keep code that "works for now." If it wouldn't survive a real deployment — with real users, real edge cases, real attackers — it doesn't go in.

---

## What's In This Repo

```
AI-workflow/
├── CLAUDE.md                          # Behavioral config — how I expect AI to work with me
└── prompts/
    ├── code-security-review.md        # Systematic security audit of C/C++ and TypeScript code
    ├── vulnerability-analysis.md      # Deep-dive analysis of a specific vulnerability or CVE
    ├── learning-roadmap.md            # Building a personalized cybersecurity learning path
    └── cv-optimization.md             # Tailoring a technical CV for security and software roles
```

Each prompt is built to be reusable and role-specific — not generic "explain this code" requests, but structured prompts with defined output formats, severity levels, and context parameters.

---

## My AI Principles

**Truth over comfort.** I don't want an AI that validates my assumptions. I want one that corrects them. An AI that agrees with everything I say is actively harmful — it gives me confidence in bad decisions.

**Best solution, not safest one.** When tradeoffs exist, I want them named. Not the cautious hedge, not the generic recommendation — the option that's actually right, with an honest account of what it costs.

**Concrete over abstract.** Theory is useful. When I'm solving a real problem, I need working examples, specific recommendations, and code I can run — not generalities about best practices.

**Challenge my reasoning.** If my stated problem isn't the real problem, I want to know before I build the wrong solution. The value of a good collaborator — human or AI — is pushback at the right moment.
