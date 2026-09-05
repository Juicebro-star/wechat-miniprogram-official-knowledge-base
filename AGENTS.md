# Agent Instructions

This repository is a public, source-bounded knowledge base for WeChat Mini Program development. It is a documentation corpus, not an application project.

## Read first

1. `AI_CONTEXT.md` for scope, routing, and answer rules.
2. `README.md` for source, snapshot date, and public-use boundaries.
3. `00_目录.md` to locate the relevant official pages.

## Source rules

- Treat the Markdown pages and `raw-html/` files as an archived snapshot of official WeChat documentation.
- Preserve the `official_url` and the page URL when citing a claim.
- Distinguish clearly between: documented fact, inference from an example, and behavior that must be verified in the current project or tool version.
- When a rule may have changed, check the linked official page before making a release, security, privacy, or compatibility decision.
- Do not invent API signatures, configuration fields, platform limits, or audit requirements.

## AI coding rules

When this repository is used alongside an application repository:

1. Inspect the application repository's own files before proposing changes.
2. Identify the relevant documentation section before writing code.
3. Explain the design choice and cite the official page used.
4. Keep project-specific requirements, secrets, AppIDs, environment values, and user data in the application repository or its private configuration; never add them here.
5. After editing code, run the checks that the application repository provides and report what was actually verified.
6. Do not describe code as released, approved, or production-ready unless that state has been verified.

## Public-repository boundary

Do not add personal information, private project material, credentials, AppIDs, private endpoints, local absolute paths, chat exports, or unrelated application code to this repository.

