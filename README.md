# speedbay-it/.github

Organisation-wide defaults for issues and pull requests, built for the move of fleet work
tracking from markdown ROADMAP/BACKLOG files to GitHub Issues (standards ROADMAP §4.295).

## Status: dormant while private

GitHub only applies default community health files from an organisation's `.github`
repository when that repository is **public**:

> "The `.github` repository must be **public**."
> - https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file

This repository was created private on purpose. Nothing here affects any other repository
until it is made public, so it can be reviewed and edited ahead of the cut-over.

**Making it public is the switch.** At cut-over:

```sh
gh repo edit speedbay-it/.github --visibility public --accept-visibility-change-consequences
```

Nothing in this repository is secret: it holds only forms, a template and this README. Read
the files once more before flipping it, because once public they are world-readable.

To switch back off, make it private again.

## What is here

| Path | Purpose |
|---|---|
| `.github/ISSUE_TEMPLATE/bug.yml` | Issue form, sets Issue Type **Bug**. Fields: Symptoms (required), Surfacing, Fix path, Priority, Effort. |
| `.github/ISSUE_TEMPLATE/feature.yml` | Issue form, sets Issue Type **Feature**. Fields: Design summary (required), Why now / what unblocks it, Exit criterion, Priority, Effort. |
| `.github/ISSUE_TEMPLATE/task.yml` | Issue form, sets Issue Type **Task**. Fields: What (required), Exit criterion, Priority, Effort. |
| `.github/ISSUE_TEMPLATE/config.yml` | Turns off blank issues and links to the org Project "Speed Bay tracker" (https://github.com/orgs/speedbay-it/projects/2). |
| `.github/pull_request_template.md` | One-line summary, a `Closes owner/repo#N` line, the STANDARDS/ROADMAP §-reference line, and a reminder that release PR titles keep `vX.Y.Z: subject`. |

Each form also carries `projects: ["speedbay-it/2"]`, so a new issue lands on the tracker
Project when the person filing it has write access to that Project. Otherwise the Project's
auto-add workflow, or triage, adds it.

## Things to know before cut-over

- **A repository's own forms override all of these.** From the same GitHub page: "if a
  repository defines valid issue templates or issue template configuration in its own
  `.github/ISSUE_TEMPLATE` folder, none of the contents of the default `.github/ISSUE_TEMPLATE`
  folder will be used." A repository with its own forms keeps them and gets none of these.
  The same applies per file to `pull_request_template.md`.
- **The Priority and Effort dropdowns do not set the org issue fields.** GitHub: "Issue fields
  cannot currently be pre-filled via URL query parameters or set through issue templates."
  (https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/adding-and-managing-issue-fields).
  The dropdown answer is written into the issue body. Triage, or automation through the
  issue-field-values REST API, copies it into the **Priority** and **Effort** fields.
- **`required: true` is only enforced on public repositories** (GitHub form schema docs:
  https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-githubs-form-schema).
  On a private repository the form still shows, but the required fields can be left empty.
- **The form syntax** follows
  https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms
  (top-level `name`, `description`, `body` required; `type`, `projects`, `labels`, `title`
  optional).

## CodeRabbit: not configured here

Checked against CodeRabbit's own documentation on 2026-10-03:

- **CodeRabbit does not read organisation-wide config from `.github`.** It reads it from a
  separate repository that must be named `coderabbit`: "Create a repository named `coderabbit`
  in your organization." CodeRabbit must be installed on that repository, which may be private.
  A repository's own `.coderabbit.yaml` overrides the central file.
  Source: https://docs.coderabbit.ai/configuration/central-configuration
- **The key that skips PRs by author is `reviews.auto_review.ignore_usernames`**: "Skip reviews
  for PRs authored by these usernames (exact match; not email addresses)". Type: array of
  strings, default `[]`. Matching is exact and case-sensitive, with no wildcards, and bot
  accounts are listed with their `[bot]` suffix.
  Sources: https://docs.coderabbit.ai/reference/configuration and
  https://docs.coderabbit.ai/configuration/auto-review
- A related key, `reviews.auto_review.ignore_title_keywords`, skips PRs whose title contains
  any of the listed keywords (case-insensitive).

A `.coderabbit.yaml` in this repository would only configure reviews of this repository, so
none is added. To add the setting later, put this in the repository where CodeRabbit is
installed as `.coderabbit.yaml` at its root, or in a new org `coderabbit` repository to cover
every repository CodeRabbit is installed on:

```yaml
# yaml-language-server: $schema=https://coderabbit.ai/integrations/schema.v2.json
reviews:
  auto_review:
    ignore_usernames:
      - "speedbay-fleet-sync[bot]"
      - "dependabot[bot]"
```

## Editing

Commits go straight to `main`; this repository is not PR-gated. Keep forms within GitHub's
offline-checkable rules: top-level keys from the allowed set, a `type` and a unique `id`
(letters, digits, `-`, `_`) on every non-markdown body item, and non-empty, distinct dropdown
options.
