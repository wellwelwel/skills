# Skills

🎓 The conventions I would otherwise repeat in every project, written once and installed wherever I need them.

They are opinionated on purpose. Each one carries how I want code written, how prose is edited, and how an interface is verified, so an agent starts a new project already knowing my defaults instead of guessing them. None of them assumes a stack, a framework, or a repository layout, and where a project already has an established convention, the project wins.

> [!NOTE]
>
> They are personal, not a product. Read them, take the ones that match how you work, and change whatever does not 🙋🏻‍♂️

## Installation

```sh
npx skills@latest add wellwelwel/skills
```

Pick the skills you want and the agents to install them on. They land in your repository as ordinary files that you own, so nothing changes behind your back. Pull my later changes when you want them with `npx skills update`.

- Every skill can be called by name, and the agent also reaches for one on its own when the task fits.

---

## Skills

### Development

- **[engineering](./skills/development/engineering/SKILL.md)**: the conventions behind every source file, covering naming, control flow, comments, TypeScript type rules, testing, and commit messages.

### Frontend

- **[ui](./skills/frontend/ui/SKILL.md)**: the details that separate an interface that works from one that feels finished, from concentric radii, optical alignment, and layered shadows to interruptible animations, typography, and reserving space so nothing ever reflows.
- **[cdp](./skills/frontend/cdp/SKILL.md)**: verification of what the browser actually renders, driving a running page through Chrome's DevTools Protocol to measure the DOM and capture screenshots. A spacing claim becomes a number instead of an impression, which is where [ui](./skills/frontend/ui/SKILL.md) sends anything that has to be seen to be trusted.

### Texts

- **[writer](./skills/texts/writer/SKILL.md)**: how prose is written and edited, covering language and style, plain language, translation, and the discipline of reconciling a document against the present instead of letting it grow by accumulation.

---

## Credits

- [ui](./skills/frontend/ui/SKILL.md) is adapted from [make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better/tree/384562064fcdd99778fcbafd8729626fe6aab02f) by [Jakub Krehel](https://github.com/jakubkrehel).

---

## License

This project is under the [**MIT License**](https://github.com/wellwelwel/skills/blob/main/LICENSE).<br />
Copyright © 2026-present [**Weslley Araújo**](https://github.com/wellwelwel) and [**contributors**](https://github.com/wellwelwel/skills/graphs/contributors).
