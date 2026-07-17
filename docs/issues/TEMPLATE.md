<!-- docgraph: ignore -->
<!--
  ISSUE REPORT TEMPLATE — copy to docs/issues/YYYY-MM-DD-short-slug.md and fill in.
  This file is excluded from the docgraph index (the marker above). The COPY you
  create is a real governed node, so keep its frontmatter + Agent Index and link it
  from a governed doc (intent-map "Open issues" or current-state "Risks").

  Conventions (see ./README.md):
    - Filename: YYYY-MM-DD-short-slug.md
    - Kind: issue ; Level: I1 (repo-wide) or I2 (local)
    - Keep frontmatter Status: == Agent Index Status: (mismatch -> field_conflict)
    - Open -> Status: active ; Resolved/Won't fix/Duplicate -> Status: retired (via doc-retirer)
    - Lead with the smallest decisive evidence; separate observation from hypothesis.
-->
---
Status: active            # active while Open; retired when Resolved/Won't fix/Duplicate
Owner: <owner>
Created: <YYYY-MM-DD>
Last verified: <YYYY-MM-DD>
Kind: issue
Level: I1                 # I1 repo-wide | I2 narrow/local
---

# <One-line problem statement, not a category>

## Agent Index

- **Kind:** issue
- **Status:** active            # must match frontmatter Status
- **Level:** I1
- **Last verified:** <YYYY-MM-DD>
- **Resolution:** Open          # Open | Resolved | Won't fix | Duplicate
- **Severity:** <High|Medium|Low> — <one phrase, e.g. "fails silently">
- **Affected version:** <pkg + version / commit>
- **Read when:** <when a future agent should pull this up>
- **Search terms:** <comma-separated symptoms, error strings, component names>
- **Relates to:** [<governed doc>](<relative/path.md>)

## Summary

<2–4 sentences: what's wrong, why it matters, how it was found. State the core
contradiction up front if there is one.>

## Impact

- <Who/what is affected; is it silent? data loss? feature dead? perf?>

## Environment / versions

```
<pkg + versions, OS, launch command, relevant env vars, repo under test>
```

## Evidence — reproduction & diagnosis

<Lead with the smallest decisive test. Show commands AND output. Number steps.>

### 1. <Decisive observation>
```
<command / query>
<output>
```
<What it proves.>

### 2. <Supporting observation>
...

## Root-cause hypotheses (ranked)

1. **(Most likely) <hypothesis>** — <why it fits the evidence; what would confirm it>.
2. **<alternative>** — <fit / what distinguishes it>.

<Note what evidence is still needed to disambiguate (e.g. server stderr).>

## Defects to fix

1. **<defect>** — <one line>. (Primary)
2. ...

## Recommended next steps

1. <The single most disambiguating action first.>
2. ...

## What is NOT broken (to scope the fix)

- <Adjacent things you ruled out, so a reader doesn't re-investigate them.>

## Resolution

_Open._ When fixed: set frontmatter + Agent Index `Status: retired`, add the
resolving commit/spec link here, move the README pointer to "Resolved", and route
the retirement through `doc-retirer`.
