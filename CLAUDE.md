Skills live under `skills/`, one folder per skill, grouped into buckets by the domain they serve:

- `development/`: how code is written
- `frontend/`: what runs in a browser
- `texts/`: how prose is written

Create a new bucket only when no existing one honestly fits. The folder name is the skill name, and the `name` in the frontmatter matches it.

The frontmatter also carries `metadata.version`, which starts at `1.0.0` on a new skill and is bumped in the same change as every edit to that skill, however small. Versions are per skill and independent, so bump only what the change touches: patch when the wording changed but the guidance stands, minor when the skill starts saying something it did not say before, major when a rule is reversed or removed and an agent following the previous version would now be wrong.

Every skill here must be agnostic of any single project. It carries a personal convention, never a repository's layout, stack, domain, or toolchain. Whatever depends on the project is written as a condition instead of an assumption, for example "when the project dedicates a place to type declarations", and where a project already has an established convention, the skill defers to the project. A skill that only works in the codebase it came from does not belong here.

Adding, renaming, or removing a skill means updating the top-level [README.md](./README.md) in the same change: one entry under the skill's bucket, the name linked to its `SKILL.md`, and one line saying what it covers. That list is the only map a reader gets, so a skill it never mentions, or one it still points to after a move, is a map that lies.

One `SKILL.md` holds the whole skill for as long as it stays readable. When it stops being readable, move the depth into sibling files next to it and link them from `SKILL.md`, which stays the entry point and the index. Executable helpers live in a `tools` folder beside the `SKILL.md` and are invoked through the installed skill's own path, never through a path in the consuming project.

Prose in this repo follows [writer](./skills/texts/writer/SKILL.md), including the READMEs and this file. TypeScript follows [engineering](./skills/development/engineering/SKILL.md), and tests also follow [testing](./skills/development/testing/SKILL.md). The skills govern the repo that ships them.

Adapted material keeps its attribution in the skill and in the README's Credits section, pinned to the commit it was adapted from, so the reference keeps describing what was actually taken.

Run `npm run lint:fix` before finishing, and `npm run typecheck` when anything under a `tools` folder changed.
