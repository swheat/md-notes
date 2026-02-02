# Repository Guidelines

## Project Structure & Module Organization
Content lives entirely in Markdown. Top-level directories such as `api`, `cloud-infrastructure`, `devops`, `iac`, `policy`, and `testing` map to practice areas; each folder holds topical `.md` briefs (for example, `cloud-infrastructure/aws-control-tower-setup.md`). Add new notes by picking the closest domain or creating a well-named subfolder (kebab-case, no spaces). Keep overview docs like the root `README.md` synchronized whenever you add or move material so contributors can discover new content quickly.

## Build, Test, and Development Commands
`rg "<phrase>" -n` – fastest way to locate existing guidance before writing new docs.  
`markdownlint "**/*.md"` – run from the repo root to enforce consistent headings, spacing, and list punctuation.  
`vale .` – optional prose lint if you have Vale installed; it catches passive voice and jargon before review.

## Coding Style & Naming Conventions
Write in Markdown with a single `#` heading per file and Title Case section headers. Favor short paragraphs, ordered lists for procedures, and fenced code blocks (```bash```, ```json```) for commands or payloads. File names should describe the scenario (`aws-cost-analysis-pipeline.md` pattern) and reside beside related docs. Use relative links (`../devops/run-github-actions-locally.md`) so navigation works offline. Include callouts or tables only when they improve scannability.

## Testing Guidelines
Every contribution must at least pass `markdownlint` and a manual preview (GitHub web preview or `grip`). When documenting command sequences, copy them into a scratch shell to confirm they execute as written. For architecture diagrams or tables, verify that widths render in both dark and light themes. If you cite external resources, spot-check the URLs with `curl -I <link>` to avoid dead references.

## Commit & Pull Request Guidelines
Follow the existing history: short, present-tense summaries such as `add sandbox guardrails note` or `update blockchain overview`. Each commit should group related edits per directory. Pull requests need a one-paragraph summary, directory checklist (e.g., `cloud-infrastructure`, `tagging`), confirmation that lint/tests ran, and any screenshots or diagrams for visual updates. Link back to the originating issue or request so downstream agents understand the context.
