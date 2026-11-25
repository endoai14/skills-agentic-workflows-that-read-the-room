---
name: update-github-info
description: Refresh Mona's GitHub Info page with concise, practical updates from official GitHub sources.
intent: Keep Mona's GitHub Info page current with useful, source-backed guidance from GitHub Blog and Changelog updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
strict: true
engine:
  id: copilot
  copilot-sdk: true
network:
  allowed:
    - github.blog
    - github.com
tools:
  bash: false
  cli-proxy: false
  edit: true
  web-fetch: {}
safe-outputs:
  create-pull-request:
    title-prefix: "[github-info] "
    base-branch: main
    draft: false
    max: 1
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md`. Fetch both
https://github.blog/latest/ and https://github.blog/changelog/ with the web-fetch
tool before proposing any changes.

Select only recent, useful developments that fit the page's practical guidance
for developers. Keep summaries short, preserve the existing editorial direction
and useful content, and link each update to its original GitHub Blog or Changelog
source. Do not infer details that the sources do not support.

Edit only `site/content/github-info.md`. If either source cannot be fetched, or
there is not enough reliable evidence to make a useful update, use
`report_incomplete` and do not open a pull request. If the sources contain no
material new information or the page needs no change, use `noop` with a brief
reason.

When you make a substantive change, use the configured `create-pull-request`
safe output to open one non-draft PR against `main`. Summarize the changes and
include their source links in the PR description, and make clear that the PR is
for Mona to review. Do not write directly to `main`, use other write methods,
or merge the PR.
