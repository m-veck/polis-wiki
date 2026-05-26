# Agent Guidelines for polis-wiki

## Folder Structure

This wiki uses the following organization:

- **`/source/`** — Raw uploads. Leave unchanged.
- **`/knowledge/`** — Processed content. Create summaries, extractions, and analysis here.
- **`/meta/`** — Configuration and guidelines. Update when needed.

## Custom Folders

Users may add domain-specific folders as needed (e.g. /incidents/, /logs/, /docs/).
Inspect the tree before writing to discover existing structure.

## Naming Conventions

- Use kebab-case for filenames: `my-document.md`, `api-reference.md`
- For dated content, use ISO format: `2026-05-24-incident-report.md`
- Avoid spaces and special characters

## File Operations

- **Read:** Read any file to understand context.
- **Create:** Add new docs to `/source/`, processed output to `/knowledge/`.
- **Update:** Always read first to get an `operation_id`, then write with `based_on_read`.
- **Delete:** Only when explicitly requested by the wiki owner.

## Stale Writes

If a write returns `file_was_updated` or `file_was_deleted`, another agent or user
changed the file. Re-read it before retrying.

## Commits

Changes are auto-committed after 3 minutes of idle, or when another agent starts
working on the wiki. Use `commit_now` to explicitly commit a logical unit of work.

## Discovery — INDEX.md

This wiki maintains an `INDEX.md` at the wiki root. It's a one-line-per-file
map of everything in the wiki — the primary discovery tool.

**When you enter the wiki:**
- Read `/INDEX.md` first. It's faster than walking the tree or running search.

**When you change content:**
- After *creating* a file: add a line under the right folder group with a
  one-sentence description (use `edit_file` with `based_on_read`).
- After *renaming or deleting*: update or remove the line.
- After *editing* content: only touch INDEX.md if the description is now
  inaccurate.

**Format:**

    # Index

    ## /source/
    - /source/api-reference.md — REST endpoints exposed by the service.
    - /source/setup.md — Bootstrap and initial deployment.

    ## /knowledge/
    - /knowledge/glossary.md — Terms used across this wiki.
    - /knowledge/incident-2026-05-21.md — Postmortem for the May 21 outage.

Group by top-level folder. Alphabetise within group. Keep descriptions to one
sentence.

## Searching

Order of preference for "does this wiki cover X?":

1. Read `/INDEX.md` — fastest, covers most cases.
2. `search_files(wiki, pattern)` — when INDEX.md is incomplete or you need
   string matches inside files.
3. `list_directory` + `directory_tree` — for structural exploration.

Avoid chained search_files calls when INDEX.md would have answered.
