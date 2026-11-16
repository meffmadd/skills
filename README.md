# skills

My personal skills repository.

| Skill | Purpose |
| --- | --- |
| [simplification-review](skills/simplification-review/SKILL.md) | Rank evidence-backed opportunities to remove superfluous abstractions, mechanisms, and optional features. |
| [visual-plan](skills/visual-plan/SKILL.md) | Turn a spec or issue into a collapsible file-tree implementation plan with concise pseudocode diffs. |

## Install

With the [skills CLI](https://skills.sh) (works for pi and 100+ other agents):

```bash
npx skills add meffmadd/skills
```

Or pi-native:

```bash
pi install git:github.com/meffmadd/skills
```

## Usage

### simplification-review

Ask your agent to use the skill, or invoke it directly in pi:

```text
/skill:simplification-review Review this repo for overengineering.
```

The skill traces workflows, applies a deletion test, and ranks supported simplification options by superfluousness. It distinguishes refactors from interface changes, feature cuts, and guarantee reductions. It reviews only unless you request implementation.

### visual-plan

Ask your agent to use the skill, or invoke it directly in pi:

```text
/skill:visual-plan https://github.com/owner/repo/issues/2
/skill:visual-plan <inline implementation request>
```

The skill turns a spec, issue, or prompt into one collapsible Markdown file tree: directories nest by indentation, each file has a five-to-seven-word headline, and a short pseudocode diff sits under an unlabeled disclosure arrow. It writes the plan to the temp directory and offers to open it; viewers that fold `<details>` hide the diffs, others show the whole tree.

## Development

```bash
git clone https://github.com/meffmadd/skills
cd skills
npm test                       # run tests
npx skills add . -a pi -g       # install your working copy globally
```

Edits in the repo do not propagate to installed copies; refresh a local-path install by re-running `npx skills add . -a pi -g` (`npx skills update` cannot refresh local sources).
