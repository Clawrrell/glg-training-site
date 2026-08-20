# ⚠️ THIS REPOSITORY IS RETIRED — REDIRECT-ONLY

**As of the Azure cutover (August 2026), this repo serves redirect stubs. It is NOT production.**

Pushing here deploys **nothing**. Editing a `module-*.html` here changes **nothing** that any learner sees.

---

## Where production actually lives

| | |
|---|---|
| **Production URL** | `https://red-dune-0e206040f.7.azurestaticapps.net` |
| **Served from** | `~/repos/glg-engine-wt` — sibling worktree, branch **`engine`** |
| **Pushes to** | remotes **`engine-github`** AND **`engine-private`** |
| **Never pushes to** | **`origin`** (this repo) — enforced by a pre-push hook |

The pre-push hook exists because `api/data/bank-*.json` is **the answer key**. Publishing the engine
branch to GitHub Pages would re-create DEFECT-MOD-028 (answer keys served to learners). If that hook
fires, you are pushing to the wrong place — **do not force past it.**

## What this repo still contains

- **Redirect stubs** (~1.3 KB each) pointing at the Azure host. These are live and functional.
- **Stale pre-cutover module copies.** `module-cm3.html` here is 185,907 bytes; the real one on
  `engine` is a different file entirely. **They are different versions — never copy one over the
  other.** Port changes deliberately, verified against the target file's own text.

---

## 🔴 STANDING DEPLOY RULE — ALWAYS LIVE-VERIFY BY HTTP GREP

**Never trust a push success or an HTTP 200 as proof of deployment. Grep production content.**

```bash
curl -s "https://red-dune-0e206040f.7.azurestaticapps.net/module-XX.html?cb=$(date +%s)" -o /tmp/live.html
grep -c "<string the fix ADDS>"    /tmp/live.html   # expect >= 1
grep -c "<string the fix REMOVES>" /tmp/live.html   # expect 0
```

A cache-busting query param is required. Check **both** directions — a string that should now be
present *and* a string that should now be gone. Byte-count is a useful cross-check: the live response
should match the file you deployed.

### Why this rule exists — two confirmed wrong-target deploy events

1. **2026-08-19** — `git push origin engine` was refused by the pre-push hook. Correct refusal; the
   engine branch belongs to `engine-github`/`engine-private`. The guard caught it.
2. **2026-08-20** — the CM-3 statutory-conflation fix was authored, committed and pushed to
   `origin/main` (`108cb78`). Push succeeded. HTTP returned 200. **Production still served the old
   content**, because this repo is redirect-only. Only a content grep caught it. The fix was then
   re-applied to the engine copy and verified live (`1bfc4a0`).

Same root cause both times: **two remotes with opposite rules and nothing on disk saying so.**
This README is that missing note.

Commit `108cb78` on `origin/main` is left in place deliberately — the content is correct, it is
simply on a dead copy. It is not reverted, and it is not deployed.

---

*Last updated 2026-08-20. If you are an agent session reading this: load it before any deploy.*
