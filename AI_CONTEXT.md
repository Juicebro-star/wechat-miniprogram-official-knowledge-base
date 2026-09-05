# AI Context: WeChat Mini Program Official Knowledge Base

## Purpose

Use this repository to answer general WeChat Mini Program development questions and to ground AI coding work in official documentation.

The corpus was captured from the official documentation tree:

`https://developers.weixin.qq.com/miniprogram/dev/framework/`

Snapshot date: `2026-09-05`.

## Corpus map

| Question area | Start here |
|---|---|
| Getting started, project structure, configuration | `quickstart/`, `structure.md`, `config.md`, `MINA.md` |
| App Service, page lifecycle, routing, modules | `app-service/` |
| WXML, WXSS, events, rendering, animation | `view/` |
| Custom components, properties, lifetimes, behaviors, relations | `component-framework/`, `custom-component/` |
| Runtime, update mechanism, Skyline, Worklet | `runtime/` |
| Plugins | `plugin/` |
| Network, storage, files, canvas, lazy loading | `ability/` |
| Subpackages and workers | `subpackages.md`, `subpackages/`, `workers.md` |
| Device and hardware capabilities | `device/` |
| Login, sharing, messaging, ads, open capabilities | `open-ability/` |
| Debugging, performance, audits, compatibility | `usability/`, `performance/`, `audits/`, `compatibility.md` |
| Security, privacy, internationalization, release | `security.md`, `user-privacy/`, `Internationalization-Friendly.md`, `operation.md`, `release/` |

## Retrieval workflow

For each question:

1. Classify the question into one of the areas above.
2. Search the relevant directory and open the smallest set of pages needed.
3. Check the page's `official_url` and the current official page when the answer depends on a changing platform rule.
4. Answer with three layers when useful:
   - what the official documentation states;
   - what follows from the example or architecture;
   - what still needs to be tested in the target project.

## AI coding workflow

When a separate application repository is provided, use this sequence:

1. Inspect its file tree, package configuration, target platform, and current tests.
2. Restate the requested behavior and identify the affected pages, components, APIs, and data flow.
3. Retrieve the relevant official pages from this repository.
4. Propose the smallest consistent change.
5. Implement only after checking existing conventions and configuration.
6. Run lint, type checks, unit tests, build checks, or simulator/device verification available in the application repository.
7. Report changed files, official references, verification results, and unresolved uncertainty.

## Scope limitations

This snapshot covers the official `/miniprogram/dev/framework/` documentation tree. It does not claim to be the complete Component, API, Server API, Cloud Development, or Developer Tools documentation set. If a question depends on one of those areas, follow the official links in the relevant page and verify the current documentation.

