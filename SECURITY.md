# Security Policy

Devkit is a knowledge warehouse of documentation and reusable ingredients rather
than a running service. The main security concerns here are:

- Secrets, credentials, or private data accidentally committed to an ingredient.
- References or MCP/service guidance that points at a compromised or malicious
  source.
- Instructions in a skill or template that would lead a generated project into
  an insecure default.

## Supported versions

Only the current `main` branch is maintained. Fixes are applied there.

| Version | Supported |
| ------- | --------- |
| `main`  | ✅        |
| older tags / forks | ❌ |

## Reporting a vulnerability

Please report suspected security issues **privately**:

- Preferred: open a private advisory via **GitHub → Security → Report a
  vulnerability** on this repository, or
- Email **muhmmadrehan37@gmail.com** with the details.

Do not open a public issue or pull request for a security problem until it has
been resolved.

Please include:

- What the problem is and where it is (file path, link, or ingredient).
- Why it is a risk and, if known, how it could be exploited or misused.
- Any suggested fix.

## What to expect

- Acknowledgement of your report within about 7 days.
- An assessment and, where valid, a fix on `main` as soon as is practical.
- Credit in the fix commit or release notes if you would like it.

## Committed secrets

If you find a secret committed to this repository, report it privately as above.
It will be removed and the affected credential should be treated as compromised
and rotated by its owner.
