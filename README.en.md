[日本語](README.md) · **English** · [中文](README.zh.md)

# ja-human-writing

A Claude Code skill for drafting and editing Japanese while preserving the writer's judgment and voice.

v0.1.1 · 2026-09

## Keep what already works

Light editing preserves facts and the strength of claims. For example, this illustrative draft repeats itself:

```text
今回の変更で、検索時間が短くなる可能性があります。
つまり、検索にかかる時間が短くなる可能性があるということです。
```

The second sentence can go:

```text
今回の変更で、検索時間が短くなる可能性があります。
```

The qualification “may reduce search time” stays. The skill should neither turn it into a guarantee nor invent measured timings. It should not change sentence lengths just to create variation.

## Drafting and editing have different boundaries

- **Drafting:** establish the sources and the writer's judgment. For longer pieces, five concrete materials are a planning reminder, not a quota. One substantial experience or case may be enough.
- **Editing:** preserve structure, facts, quotes, negation, conditions, and certainty. Keep sound sentences and the author's habits. Restructure only when requested.
- **Reviewing findings:** optional [natural-japanese](https://github.com/coji/natural-japanese) lint supplies candidates. Make a change only when there is a concrete reading problem.

This repository contains instructions, not its own AI-detection engine. Its vocabulary and structure references draw on [stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) and [slop-nuki](https://github.com/chezou/slop-nuki), among others. Matches are not automatic deletion rules.

## Changes in v0.1.1

- No mandatory extreme sentence lengths, first-person pronouns, or noun-ending ratios.
- No deletion quotas for three-part lists, uncertainty markers, or genuine “A, not B” corrections.
- No invented agents or personal experiences when clarifying a subject, and no automatic hearsay added to verified facts.
- Punctuation, invitations, navigation, quotes, and valid Markdown are judged in context.
- Lint output goes into a dedicated temporary directory. Existing drafts and JSON files are preserved.

## What the evidence supports

Onishi Yume's undergraduate thesis compares 20 human and 20 AI texts of roughly 500 Japanese characters in its main study. Its observations do not establish editing rules for every social post or current model. The previous skill also confused sentence-length **standard deviation** with the length of individual sentences.

Zaitsu & Jin (2023) studied authorship classification in Japanese. It did not test whether deleting phrases or changing sentence lengths improves writing.

See [evidence and limits](ja-human-writing/references/evidence-ja.md) for primary sources, the inspected lint implementation, and withdrawn inferences. Synonyms, metaphors, and connectives are judged by meaning and readability, not adjusted to imitate a statistical distribution.

## Install

**Prerequisites**: a working [Claude Code](https://claude.com/claude-code) install. You use the terminal exactly once. No extra cost — it runs inside your existing Claude usage.

Open a terminal and paste these three lines in order.

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/vincent-wen789/ja-human-writing.git
cp -R ja-human-writing/ja-human-writing ~/.claude/skills/
```

For the machine checker too (optional, recommended), two more:

```bash
git clone https://github.com/coji/natural-japanese.git
cp -R natural-japanese/skills/natural-japanese ~/.claude/skills/
```

No terminal after this.

<details>
<summary><b>What these commands actually do</b></summary>

- `mkdir -p ~/.claude/skills` — makes the folder skills live in (does nothing if it already exists)
- `git clone` — downloads a folder from GitHub into wherever you currently are
- `cp -R ... ~/.claude/skills/` — copies that folder to where Claude Code looks for skills
- **It only copies.** Nothing is deleted, no system settings change. Don't like it? Delete the folder and you're back where you started
- **No admin rights needed** — it all happens inside your home folder, so it usually works on a locked-down work machine
- **"Claude Code" is not the browser or desktop Claude app.** It's the terminal one, and you need it installed first. It's covered by your Claude plan; **this skill itself is free**
</details>

- **The machine checker is optional.** Skip natural-japanese and that step degrades to a manual checklist; the skill still works
- If you do use it, you need [uv](https://docs.astral.sh/uv/). It installs the Python dependencies itself — no manual pip step
- **Windows**: read `cp -R` as PowerShell's `Copy-Item -Recurse`
- **The skill instructions are written in Japanese**, because the rules describe Japanese-specific patterns. You can talk to Claude in any language

## Using it

Ask Claude Code:

```text
Lightly edit this Japanese draft. Preserve the facts and tone; change only empty padding, repetition, or wording that actually gets in the reader's way.
```

Ask for change explanations if you want them. For a new piece, provide material and ask for a draft. Missing facts can be researched within the task; personal experience must come from the writer.

Business and technical documents keep their normal register and information structure. They do not need forced personal narration or sentence-length variation.

## Your voice

Provide one to three pieces you like as references for vocabulary, judgment, and register. Repeated terms and useful metaphors can be part of your voice. The skill should preserve them rather than normalize your writing to a supposed human distribution.

## Relationship to other skills

The material check and speaker positioning adapt [human-writing](https://github.com/KKKKhazix/human-writing). Vocabulary overlaps with stop-ai-slop-jp and slop-nuki. When combining rules, this skill prioritizes information preservation and author voice over individual pattern matches.

natural-japanese is an optional linter; manual checks are available without it. Reference files load only when relevant. Context cost depends on the model and files loaded; the old token estimates have been removed.

## Limits and validation

- Intended mainly for opinion pieces, personal writing, and social posts. Register matters.
- Short texts often fall below statistical thresholds. A finding, or its absence, does not establish naturalness or authorship.
- [examples/](examples/) preserves a historical worked example and lint counts. It is neither proof of effectiveness nor the current recommended editing procedure.
- This revision addresses rule consistency and preservation boundaries. No independent reader blind test has established that it improves overall writing quality.
- Upstream changes are tracked manually. Reports of mistakes and false positives are welcome.

Written by a marketer living in Japan ([@vinentW789](https://x.com/vinentW789)), for the practical work of writing and publishing Japanese.

## Layout

| File | Contents |
|---|---|
| [SKILL.md](ja-human-writing/SKILL.md) | Drafting/editing boundaries, register, material, speaker position, inspection workflow |
| [forbidden-ja.md](ja-human-writing/references/forbidden-ja.md) | Contextual checks for wording, structure, and honorifics |
| [evidence-ja.md](ja-human-writing/references/evidence-ja.md) | Sources, implementation observations, limits, withdrawn inferences |
| [examples/](examples/) | Historical before/after example and rerun instructions |

## Credits

- Skeleton: [KKKKhazix/human-writing](https://github.com/KKKKhazix/human-writing) (MIT) — the Chinese "living-person writing" skill. Material gate and speaker positioning are ported from it
- Machine checking: [coji/natural-japanese](https://github.com/coji/natural-japanese) (MIT) — morphological-analysis lint (sudachipy) with corpus calibration. Not vendored; install alongside
- Ban-list material: [iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) (MIT), [chezou/slop-nuki](https://github.com/chezou/slop-nuki) (MIT)
- Evidence: Zaitsu &amp; Jin 2023, *PLoS One* 18(8), [PMID 37556434](https://pubmed.ncbi.nlm.nih.gov/37556434/) / Onishi Yume, *A Study of "Natural Japanese" as Seen Through AI-Generated Text* (Hiroshima University, Faculty of Letters, 2026 undergraduate thesis)

## License

MIT
