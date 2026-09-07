# Contributing to Devkit

Thanks for your interest in improving Devkit. This repository is a **warehouse**
of reusable ingredients (templates, skills, MCPs, services, references, and
standards) that an AI assistant — the **chef** — draws on to help people turn an
idea into a well-reasoned software project. Contributions should keep that
warehouse small, trustworthy, and broadly useful.

Please also read the [Code of Conduct](CODE_OF_CONDUCT.md). By participating you
agree to abide by it.

## Ways to contribute

- **Report a problem** — an ingredient that is inaccurate, out of date, or
  misleading; a pipeline step that is unclear; a broken link.
- **Propose a new ingredient** — a template, skill, MCP, service, reference, or
  standard that has proven reusable across real work.
- **Improve an existing ingredient** — tighten wording, fix an assumption,
  update a version, add a "when not to use it" note.
- **Improve the pipeline** — the skills in [`skills/`](skills/) that the chef
  follows in order.

Open an issue before a large change so we can agree on the approach.

## What belongs in the warehouse

The warehouse may contain reusable knowledge, references, templates, skills,
MCPs, service information, dependency guidance, standards, and carefully chosen
examples that are broadly useful across real projects.

It should **not** contain project-specific application code, prewritten recipes,
one-off solutions, secrets, or a large speculative encyclopedia. The warehouse
should grow from lessons discovered through real projects, not be filled ahead
of need.

## Adding an ingredient

Add an ingredient only after a real need has shown that it is reusable.

1. Check that an equivalent ingredient does not already exist.
2. Place it in the right category: `templates/`, `skills/`, `mcps/`,
   `services/`, `references/`, or `standards/`.
3. Explain what it is, when to use it, when **not** to use it, and where it came
   from.
4. Include only the minimum reusable material. Remove project-specific details
   and secrets.
5. Link authoritative sources and note important assumptions, compatibility
   limits, or maintenance needs.
6. Note how it was validated in practice.

Match the structure and tone of the existing files in that directory, and add a
row to any index table (for example the pipeline table in the `README.md` or a
category `README.md`) that should reference it.

## Style

- Write in plain language first; move into technical detail only as needed.
- Keep Markdown wrapped at roughly 80 columns to match the existing files.
- Prefer relative links between files in the repo.
- One logical change per pull request.

## Pull request process

1. Fork the repository and create a branch from `main`.
2. Make your change, keeping it focused and self-contained.
3. Check that links resolve and that Markdown renders cleanly.
4. Open a pull request using the template. Explain the real need behind the
   change, which ingredient(s) it touches, and how it was validated.
5. Be ready to iterate on review feedback.

## Reporting security issues

Do not open a public issue for a security problem. See
[SECURITY.md](SECURITY.md) for how to report it privately.

## License

By contributing, you agree that your contributions will be licensed under the
[MIT License](LICENSE) that covers this project.
