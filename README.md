# throughline-style-sources

A front door to a family of **[throughline](https://pypi.org/project/throughline/) sources**
— published engineering style guides reverse-engineered into standalone, grounded
requirements graphs that a consuming project composes with
[throughline-compose](https://github.com/rhodium-org/throughline-compose).

Each source is its own repository: a directory of small YAML items with permanent UIDs,
validated by `tl check`. A consumer imports a source under a namespace it chooses and
references its rules as `<namespace>:SR-0001`. This repository holds **no graph of its
own** — it is an index and a short guide to composing the family.

## Two orthogonal axes

Pick the **language** source for the language you write, and compose it alongside a
**concern** source (front-end or back-end), so your code is grounded in both at once:

```toml
# a Go micro-service that must also honour Twelve-Factor
[[sources]]
namespace = "go"
url = "https://github.com/rhodium-org/throughline-go"
ref = "v2026-08"

[[sources]]
namespace = "backend"
url = "https://github.com/rhodium-org/throughline-backend"
ref = "v2026-08"
```

```yaml
# then, in your own items:
links:
- target: go:SR-0034          # a Go naming rule
  type: satisfies
- target: backend:SR-0007     # a Twelve-Factor config rule
  type: satisfies
```

`tl-compose check` resolves the references; bare `tl check` fails fast and points you at
`tl-compose`.

## The sources

### Programming-language style — compose the one for your language

| Guide | Repo | Namespace | Rules / sections | Licence |
|---|---|---|---|---|
| PEP 8 + PEP 257 (Python) | [throughline-python](https://github.com/rhodium-org/throughline-python) | `py` | 52 / 7 | PSF, public |
| Google TypeScript Style Guide | [throughline-typescript](https://github.com/rhodium-org/throughline-typescript) | `ts` | 68 / 11 | CC BY 3.0 |
| Google Java Style Guide | [throughline-java](https://github.com/rhodium-org/throughline-java) | `java` | 54 / 8 | CC BY 3.0 |
| Google Go Style Guide | [throughline-go](https://github.com/rhodium-org/throughline-go) | `go` | 64 / 9 | CC BY 3.0 |
| Google C++ Style Guide | [throughline-cpp](https://github.com/rhodium-org/throughline-cpp) | `cpp` | 67 / 9 | CC BY 3.0 |
| Microsoft C# Coding Conventions | [throughline-csharp](https://github.com/rhodium-org/throughline-csharp) | `csharp` | 68 / 12 | CC BY 4.0 |
| GNU Coding Standards (C) | [throughline-c-gnu](https://github.com/rhodium-org/throughline-c-gnu) | `cgnu` | 55 / 9 | GFDL v1.3 |
| Linux kernel coding style | [throughline-c-linux](https://github.com/rhodium-org/throughline-c-linux) | `clinux` | 57 / 21 | GPL-2.0 |

The two C sources are **deliberately distinct, not a hierarchy** — `c-gnu` (own-line braces,
space indentation) and `c-linux` (K&R braces, tab-8) are genuinely opposite conventions with
no shared core. Each warns against composing both.

### Front-end concern — three alternatives, pick one

Offered as choices by priority, not a hierarchy; composable together but each stands alone.

| Guide | Repo | Namespace | Rules / sections | Priority it serves |
|---|---|---|---|---|
| Google Web Vitals (web.dev) | [throughline-frontend-web-vitals](https://github.com/rhodium-org/throughline-frontend-web-vitals) | `webvitals` | 54 / 8 | runtime performance (CC BY 4.0) |
| MDN Web Docs best practices | [throughline-frontend-mdn](https://github.com/rhodium-org/throughline-frontend-mdn) | `mdn` | 55 / 8 | open-web semantics & accessibility (CC BY-SA 2.5) |
| Google HTML/CSS Style Guide | [throughline-frontend](https://github.com/rhodium-org/throughline-frontend) | `frontend` | 41 / 8 | markup / style conventions (CC BY 3.0) |

### Back-end concern — compose alongside a language source

| Guide | Repo | Namespace | Rules / sections | Licence |
|---|---|---|---|---|
| The Twelve-Factor App | [throughline-backend](https://github.com/rhodium-org/throughline-backend) | `backend` | 46 / 12 | CC BY-SA 3.0 |

## How each source is modelled

- A single root `intent` (`INT-0001`, `normative: false`) states why the guide exists.
- Each major section of the guide is a `user_requirement` that `derives_from` the intent.
- Each individual rule is a `system_requirement` that `implements` its section, carrying the
  guide's own reference in `attrs.source_ref` (e.g. `"Google Go Style Guide: Naming —
  Receiver Names"`).
- Every item is `status: approved`, **never `ratified`**: these graphs re-express borrowed
  guidance, which is not independently verified against its source. Treat a rule as a
  faithful transcription, not a warranty.

## Editions — dated tags

Style guides are living documents. A material revision is cut as a dated tag on each source
repo (e.g. `v2026-08`); a consumer pins the ref it wants. Older editions live on
`release/<date>` branches so shared rules keep their UIDs.

## Provenance & licensing

Every source reproduces its guide's rule text under **that guide's own open licence** (CC BY,
CC BY-SA, GFDL, GPL, or public domain — none proprietary), noted per row above and in each
repo's `NOTICE`. The **structure and tooling** of these repositories are Apache-2.0 (see
[LICENSE](LICENSE)), authored by Dr Henry J Grech-Cini. The reproduced rule text remains the
work of its respective authors.
