# Copilot Instructions for Defra AI Copilot Setup Guide

## Project overview

This is a **Jekyll-based documentation site** (GitHub Pages) providing GitHub Copilot setup guidance for Defra teams. It showcases example configuration files for agents, instructions, and prompts, and provides cross-tool mapping guidance for other AI coding assistants.

**Key facts:**
- Jekyll theme: `jekyll-theme-minimal`
- Base URL: `/defra-ai-config-examples`
- Target audience: UK Government digital professionals setting up AI coding assistants
- All content is public — do not include OFFICIAL-SENSITIVE or classified material

## Architecture & structure

```
pages/
  getting-started.md           # Main setup guide
  cross-tool-config.md         # Using examples with other AI tools
  agents/                      # Agent mode examples (app developer, code reviewer, tester)
  instructions/                # Instruction file examples (root, Node.js, C#, frontend, Python)
  prompts/                     # Prompt examples (ADR, tests, security review, scaffold)
_layouts/
  default.html                 # Single custom layout with sidebar nav and GOV.UK styling
_sass/
  defra-styles.scss            # GOV.UK colour tokens and component overrides
assets/
  css/                         # Compiled styles (imports theme + defra-styles)
  images/                      # Site images
  js/                          # Client-side scripts
scripts/
  local-dev/                   # setup.sh, serve.sh, build.sh helper scripts
```

## Naming conventions

- **Content files**: `kebab-case.md` (e.g. `node-backend.md`, `getting-started.md`)
- **Jekyll includes/layouts**: `kebab-case.html`
- **SCSS partials**: `_kebab-case.scss`
- **Image files**: `kebab-case.png` or `kebab-case.svg`
- **Front matter keys**: `snake_case` (e.g. `page_title`, `nav_order`)

## Branching and version control

- Main branch is always shippable — it must build and render correctly at all times
- All work is done on branches, never directly on main
- Branch naming: `<type>/<brief-description>`
  - Types: `feature/`, `fix/`, `docs/`, `chore/`
- Commit messages use conventional format: `type: short description`
  - Types: `feat`, `fix`, `docs`, `style`, `chore`
- Squash and merge when closing PRs

## Quality gates

All changes must meet these checks before merging:

- Jekyll site builds without errors (`bundle exec jekyll build`)
- All internal links resolve correctly — no broken relative URLs
- All content follows the writing style guide (plain English, British spelling, active voice)
- New examples include back-navigation links at the bottom of the page
- At least one approving code review from another team member

## Writing style & voice

- **Plain English**: common words, sentences under 25 words, British English spelling throughout
- **Active voice**: "Configure the file" not "The file should be configured"
- **Address readers as "you"**: direct and personal, consistent with GOV.UK style
- **Start instructions with verbs**: "Run the command", "Copy the file"
- **Avoid**: "please", "simply", "just", "easy", "straightforward"
- **Use real government scenarios** as examples where possible
- **Terminology**: use "AI coding assistant" (not "AI IDE"), "instructions file" (not "system prompt" for Copilot)

## Content conventions

### Links

- External links: append `{:target="_blank"}` and add "(opens in new tab)" in the link text
  ```markdown
  [GitHub Copilot docs](https://docs.github.com/en/copilot){:target="_blank"} (opens in new tab)
  ```
- Internal links within rendered pages: use Liquid `relative_url` filter
  ```markdown
  [Getting started]({{ "/pages/getting-started" | relative_url }})
  ```
- Internal links in `README.md` or non-rendered files: use relative file paths
  ```markdown
  [Getting started](pages/getting-started.md)
  ```

### Example config files

Show example configuration content inside a fenced code block tagged `markdown`:

````markdown
```markdown
---
applyTo: "**/*.js"
---

# Rules here
```
````

### Page structure

Every content page must include:
1. A H1 heading matching the navigation label
2. A brief intro paragraph explaining the purpose of the page
3. The main content
4. A back-navigation line at the bottom:
   ```markdown
   [Back to instructions index]({{ "/pages/instructions" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
   ```

### Adding new example pages

When adding a new example (instructions file, agent, prompt):
1. Create the `.md` file in the appropriate `pages/` subdirectory
2. Add a row to the index table on the parent `index.md` page
3. Add a `<li>` entry in `_layouts/default.html` under the relevant nav section
4. Include a back-navigation link at the bottom of the new page

## Jekyll and Liquid templating

- Use `{{ "/path" | relative_url }}` for all internal asset and page references — never hard-code the base URL
- Front matter is YAML; use two-space indentation
- Do not change `baseurl` in `_config.yml` without updating all nav links in `_layouts/default.html`
- Keep layouts minimal — one layout (`default.html`) handles all pages
- Excluded directories (`scripts/`, `vendor/`) must remain in `_config.yml`'s `exclude` list

## Security

- Do not include credentials, API keys, tokens, or secrets anywhere in this site — it is public
- Do not reference internal Defra systems, internal URLs, or OFFICIAL-SENSITIVE content
- All external URLs must be publicly accessible
- Do not add tracking scripts beyond the approved Google Analytics include (`_includes/google-analytics.html`)

## Local development

```bash
./scripts/local-dev/setup.sh   # First-time Ruby/Bundler/gem install
./scripts/local-dev/serve.sh   # Dev server at http://localhost:4000/defra-ai-config-examples
./scripts/local-dev/build.sh   # Production build to _site/
```

## How Copilot should respond

When generating or editing content and code for this project:

- Follow the writing style guide — plain English, active voice, British spelling, sentences under 25 words
- Follow patterns already established in existing pages — check `pages/instructions/` before creating new examples
- Prefer modifying existing files over creating new ones when the change fits naturally
- Keep changes minimal and focused on the request — do not restructure unrelated sections
- When adding new example config files, use the fenced markdown code block pattern already used throughout the site
- Always include back-navigation links at the bottom of new pages
- Always use `relative_url` for internal links — never hard-code `/defra-ai-config-examples/` as a prefix
- Flag any content that could be OFFICIAL-SENSITIVE or contain internal Defra system details before including it
- If a request conflicts with GOV.UK style or Defra guidance, flag the conflict rather than silently ignoring it
- Do not add emojis, informal language, or marketing copy — this is a government guidance site

## Related Defra sites

- [Defra AI SDLC Playbook](https://defra.github.io/defra-ai-sdlc/) — foundational AI guidance
- [Defra AI Tool Guidance](https://defra.github.io/ai-sdlc-tool-guidance/) — tool assessments and privacy reviews
- [Defra Software Development Standards](https://github.com/DEFRA/software-development-standards) — coding standards

## Licence

All content is published under the [Open Government Licence v3](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
