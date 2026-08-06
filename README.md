# nullsilver index

The registry of lab projects. nullsilver.com reads `projects.json` from this repo's
raw URL at request time (cached ~60s) and then pulls each listed project's
`state.json` and `events.jsonl` straight from that project's repo. Nothing else on
the site depends on this repo — an empty list renders the site exactly as it is.

## projects.json

```json
{
	"format": "1.0",
	"projects": [
		{
			"slug": "remora",
			"repo": "nullsilver-labs/remora",
			"branch": "main",
			"kind": "research",
			"title": "Remora: modular LLM orchestration",
			"summary": "One or two sentences, plain text, standing on their own.",
			"mark": "remora",
			"added": "2026-08-06"
		}
	]
}
```

| field | meaning |
| --- | --- |
| `slug` | URL segment on the site (`/research/<slug>`); unique across the index |
| `repo` | `owner/name` on GitHub; must be public — the site fetches raw, unauthenticated |
| `branch` | branch the site reads from; defaults to `main` if omitted |
| `kind` | `research` \| `artifact` \| `writing` — mirrors the protocol's `kind` |
| `title` | as it should appear on the site (usually the protocol's `title`) |
| `summary` | human-written; the lab does not author this |
| `mark` | name of the project's mark; the site uses its committed copy (`src/lib/marks/`) if it has one, else `marks/<mark>.svg` from this repo, else the shared `plate` mark |
| `added` | ISO date the project entered the index |

The project's live facts — status, phase, run, revision, timeline — are **not**
duplicated here. The site reads them from the project repo itself, per the
project's own `FORMAT.json` contract. This file only says *which repos exist and
what to call them*.

## Adding a project

By hand for now: add the object, commit, push. The site picks it up within a
minute or two. The intended future flow is that `lab init` registers the project
itself (a commit to this repo from the lab machine), which is why the schema is
kept small enough for a machine to fill in everything but `summary` and `mark`.

Removing a project from the index unlists it from the site; it does not touch the
project repo. Evidence lives in project repos and is immutable there.
