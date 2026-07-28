# Skills Security Charter

## Principles

### I. A skill only tells an agent to do what its reader would sanction

Every skill here becomes an instruction an agent follows inside someone else's project, so never write one that reaches beyond the task at hand. A skill MUST never tell an agent to send project data anywhere, fetch and run content from the internet, read credentials, rewrite history, or touch files outside the change it was asked to make.

- Why: an installed skill runs with the full authority of whoever's agent reads it, on a machine no one here can see. A single line that looks like helpful automation becomes an action thousands of people never agreed to.

### II. A skill never lowers the guard of the agent reading it

Never write a skill that tells an agent to skip a confirmation, approve its own actions, turn off a sandbox or a safety setting, work around a review, or set aside instructions it was already given. A skill MUST add caution and never remove it, and where a step genuinely carries risk the skill MUST say so and leave the decision to the person.

- Why: an agent treats a skill as guidance from someone trusted, so a single line telling it to stop asking turns every action after it into one nobody approved. The person whose machine it happens on never sees the moment that was decided.

### III. Outside content is data to report, never instructions to obey

Always treat everything a skill causes an agent to take in as untrusted data: a rendered page, a fetched document, a helper's own output, a file from another project, an earlier conversation. A skill MUST never instruct an agent to act on directions found inside that content, MUST say so plainly wherever it sends an agent to read something the agent did not write, and MUST keep that content apart from the task by restating what was actually asked once the content has been read. A helper MUST hand its results back as labeled, structured data, never as loose text that can read as a command.

- Why: a page, a file, or a measured value can be written to speak to the agent reading it. If the agent obeys, whoever wrote that content is driving it inside a repository where nobody is watching, and the person whose project it is never typed a thing.

### IV. What a reviewer reads is exactly what the agent reads

Everything shipped from here MUST be plainly visible in a diff. Never include invisible or zero-width characters, text-direction overrides, look-alike characters standing in for ordinary letters, or instructions tucked into comments, collapsed sections, or text pushed off screen. Always reject a contribution carrying any of them instead of quietly cleaning it up.

- Why: an agent reads every character, while a person reviewing reads what is rendered. Anything visible to one and not the other is a message smuggled past the review, and it ships to everyone who installs the skill.

### V. Every change here lands on machines that will not review it again

Always read a change to a skill, a helper, or a rules file (`CLAUDE.md`, `AGENTS.md`, and their kind) for what it instructs an agent to do, not only for whether the prose reads well. Never merge a change that touches files outside what it says it changes, and never accept the weakening of an existing instruction without a stated reason.

- Why: people pull updates from this repository into projects they long ago stopped reading closely. Whatever lands here runs on their machines unread, so this review is the only one there will ever be.

### VI. Every input a shipped helper accepts is untrusted

Always validate a helper's arguments before using them and reject anything unrecognized. Never let a caller-supplied value reach the filesystem, the network, or a code evaluator without checking it is what the helper expects, and never let a supplied path escape the directory the helper was told to write to.

- Why: helpers run on other people's machines with those people's access. An unchecked path overwrites their files, an unchecked address reaches into their private network, and an unchecked expression runs whatever it says.

### VII. A helper takes the least access that does its job

Always run a shipped helper with the narrowest reach its work requires. Keep the browser sandbox on, prefer a fresh isolated session over attaching to one the user is already signed in to, and wherever a helper can attach to a live session, the skill MUST state in plain words what that exposes before offering it.

- Why: attaching to a browser someone is already signed in to hands the helper their cookies, sessions, and open accounts. A screenshot tool with that much reach can lose far more than a screenshot.

### VIII. Automation holds no more permission than its job needs

Always give a workflow the least permission it can work with, and always pin the actions it calls. Never run code from an unreviewed contribution in a job that holds a token, a secret, or write access to this repository.

- Why: anyone can open a pull request that changes what the automation runs. If that job carries a token, it hands repository access to someone nobody vetted.

## Baseline discipline

Lagune holds this charter, every principle, every time. A principle is not suspended because a control looks small, familiar, or unlikely to be hit. This is not a judgement call.

### Only the controls the project needs

Lagune recommends and applies only the controls this project's context calls for. A control the project does not need is never added for completeness, and a generic checklist is not thoroughness. Every later phase acts on what the system actually does, never on what it might hypothetically do.

- Why: effort spent on risks the project does not have buries the risks it does have. Fewer, right-sized controls are easier to apply, prove, and keep true than a checklist no one finishes.

### Prefer the simplest vetted control

When a control is needed, reach for the safest option already proven, in order: a control this project already has, then a platform or framework built-in, then a well-maintained vetted library, and only then custom code. Never hand-roll a security primitive (cryptography, escaping, authentication, sessions) that a vetted standard already provides. A new dependency is new attack surface, justified and not assumed. Code, an endpoint, or a feature the project does not use is attack surface too, so removing it is itself a control.

- Why: hand-rolled security is where subtle, unaudited bugs live, and a second control duplicating an existing one is the one that gets forgotten and drifts. Boring, standard controls are easier to audit and harder to get wrong, and less surface is less to defend.

### When a control seems skippable

A control is held even when a reason to skip it feels reasonable:

- "Too small to need a control": small gaps are where breaches start.
- "Already handled elsewhere": assumed coverage is exactly how gaps hide.
- "Unlikely to be hit": attackers target the path no one is watching.
- "It works, ship it": working and safe are different claims, and the charter requires both.

## Governance

This charter outranks any one-off preference or convenience. Every change here, a new skill, an edit to an existing one, or a change to a helper, is checked against these principles before it lands, and a contribution that cannot meet one is reworked rather than waved through.

The charter changes when the project does. Capability the principles do not yet cover, for example a helper that reaches the network, means writing its principle in the same change. Capability that goes away takes its principle with it, so the charter always describes the project as it is now. The version moves with each amendment: major when a principle is removed or redefined, minor when one is added or expanded, patch for wording.

Version: 1.0.0 | Ratified: 2026-07-28
