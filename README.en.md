[日本語](README.md) · **English** · [中文](README.zh.md)

# ja-human-writing

A Claude Code skill for writing Japanese that reads like a person wrote it.

v0.1.0 · 2026-08

## The kind of text it fixes

```
近年、デジタル資産の世界は急速な発展を遂げています。しかし、その一方で、
セキュリティに関する課題も浮き彫りになってきていると言えるでしょう。
```

*"In recent years, the world of digital assets has seen rapid development. At the same time, it could be said that security challenges have come into relief."*

## The kind it aims for

```
7月7日、Gate のあるユーザーの口座から170万ドルが41分で抜かれた。
気づいたのは26時間後。
```

*"On July 7, $1.7M was drained from a Gate user's account in 41 minutes. They noticed 26 hours later."*

The difference isn't vocabulary. The first one has no date, no amount, and nobody doing anything — the grammatical subject of "challenges came into relief" is *challenges*. You can tell the writer never actually looked at the thing.

**Deleting banned phrases won't turn the first into the second.** What's missing is material, and a person standing somewhere. That's the gap this skill fills.

## What this is, and what it isn't

**This is not an NLP tool. It's a prompt and workflow layer.** It ships no detection engine of its own.

- The measurable parts (sentence-length variance, banned phrases, translationese) go to [coji/natural-japanese](https://github.com/coji/natural-japanese)'s lint
- The banned-vocabulary lists were imported and reorganized from [stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) and [slop-nuki](https://github.com/chezou/slop-nuki). **They overlap** — see "Relationship to existing skills"
- **The core of this skill is three things**: a material gate before drafting, explicit speaker positioning, and a loop that judges each machine finding one at a time instead of auto-applying them. The first two are ported from the Chinese-language [human-writing](https://github.com/KKKKhazix/human-writing); what's new here is the Japanese adaptation and the third part

## Three parts

**1. A gate before you write**

Non-fiction over ~1200 Japanese characters (roughly 500–700 English words) needs five concrete, sourced pieces of material before drafting starts. Dates, amounts, proper nouns, things people actually said, things that failed. If five don't exist, go research or write something honestly shorter — a long piece with no material stays padding no matter how much you revise it.

**2. Three "wrong direction" warnings**

The standard fixes for AI-sounding Japanese work backwards. See below.

**3. Detection and judgment are separate jobs**

The machine raises suspicions; **Claude then decides "fix or keep" on each one, using the skill's rules.** You are not asked to judge Japanese naturalness yourself — you approve the result, but finding the problems is the machine's and Claude's job, not yours. That matters if you're working in a language you're still learning.

## The three fixes that run backwards

| Common advice | What the data shows | Right direction |
|---|---|---|
| Replace repeated words with synonyms | Lexical diversity (TTR — distinct words ÷ total words) is **higher in AI**. Humans are the ones who repeat | Don't substitute. Let repetition stand |
| Add metaphors, it reads too flat | AI uses **several times more metaphor** than humans. 「まるで〜のよう」("just like…") is an AI tell | Don't add. Delete template metaphors |
| Too many connectives, cut them | In formal registers **usage is nearly identical**. The real difference is *variety* — AI leans on the same three | Keep the count, vary the type |

### How strong is the evidence (stated plainly)

These three rest on **two sources of unequal strength**.

- **Peer-reviewed**: Zaitsu &amp; Jin 2023, *PLoS One* 18(8), [PMID 37556434](https://pubmed.ncbi.nlm.nih.gov/37556434/) — discriminates Japanese AI-generated text using function-word rates and related features. Anyone can pull it up right now
- **An undergraduate thesis**: Onishi Yume, *A Study of "Natural Japanese" as Seen Through AI-Generated Text* (Hiroshima University, Faculty of Letters, 2026) — 20 AI vs 20 human texts. **A 20-sample undergraduate thesis, not easily verifiable from outside**

The *direction* in the table above ("higher in AI", "more metaphor") comes from the thesis. **The specific numbers are not used as thresholds, so they aren't quoted here** — the skill itself only acts on direction. The figures are in `references/evidence-ja.md` with attribution, but read them with the caveat attached: directional, not universal thresholds.

The thesis's most useful contribution wasn't a number, it was a frame. Split "AI-ness" into two layers: features readers can point at (specific vocabulary, comma placement), and features they can't name but still sense (sentence-length variance, density of hedging). **Almost all the measured difference lived in the second layer.**

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

Just ask Claude Code:

```
You:     Strip the AI smell out of this draft

Claude:  Machine check found 7 issues. I'll fix 3.

         L3  「〜と言えるでしょう」 → changed to a direct statement
         L9  every paragraph is the same length → split one into two
         L21 this paragraph has no proper nouns and no numbers.
             Is there an actual shop name or date that goes here?

         Leaving the other 4. The repeated 「〜と思う」 is doing
         real work as your speaking voice, so it stays.
```

Other ways people use it:

- **"Write a note post about X"** (note.com, the Japanese blogging platform) — the material gate runs first, and if material is thin you get questions back before any draft. Questions come in one batch, three maximum
- **"Check the keigo in this email"** — double honorifics, 「させていただく」 overuse, stacked cushion phrases

## Teaching it your voice (optional)

"If it enforces rules, won't it sand off my own habits?" That's handled by design.

Give it one to three pieces of your own writing you're happy with, and it uses your sentence rhythm, how you land judgments, and how tight you keep the polite register as a baseline. **Habits that recur across multiple pieces are protected even if they appear on a ban list.** Human style varies within one piece and stays consistent across many; AI does the exact opposite (uniform inside one piece, no identity across several). So those recurring habits are the thing worth defending.

## Relationship to existing skills

**The banned-vocabulary lists were imported and reorganized from stop-ai-slop-jp and slop-nuki. That content overlaps.** Stating it plainly rather than burying it in credits.

The difference is two things: the layer *before* writing (material gate, speaker positioning), and *which way to steer* when fixing — get the three reversals above wrong and you'll "clean up" your text in the AI direction.

If you install all three: natural-japanese is only called as a linter, so no conflict. stop-ai-slop-jp and this skill will **load overlapping vocabulary lists** — not contradictory, but it costs context. **One of the two is enough.** If you already run stop-ai-slop-jp and don't care about the pre-writing layer, there isn't much reason to switch.

**Measured context cost**: only the description (a few dozen tokens) is always resident. When the skill fires, `SKILL.md` is roughly **10k tokens**; `references/` load only when needed (ban list ~7.7k, evidence layer ~3.9k). If you're running ten-plus skills, judge from those numbers.

## Limits

- **The empirical base compares essays and compositions. Short-form social posts are not in it.** At 140–500 characters the length statistics go quiet from insufficient sample
  - **What still works on short text**: set phrases (「いかがでしたか」 etc.), translationese, subject substitution, symbol residue, presence or absence of material — none of these depend on length
  - **What doesn't**: length variance, paragraph structure, noun-ending ratios. The skill explicitly marks these "not applicable" and silences them
  - **Don't read that silence as a clean bill of health.** That part you check yourself
- **Built around essays, opinion pieces, and reported writing. Technical articles aren't the center of the target.** The material gate asks for dates, amounts, quotes, and failures — that fits writing with experience or reporting in it. Technical explanation has a different shape of material (code, error messages, versions, repro steps). The ban lists and style rules still apply, so for technical writing run the lint with `--genre tech` and treat the material gate as advisory
- One source is an undergraduate thesis (20 samples). As above, only its direction is used, not its numbers
- **Validated on exactly one worked example** (`examples/`). That shows the procedure reproduces; it does not demonstrate effectiveness
- Version 0.1.0. Tracking upstream changes (natural-japanese, stop-ai-slop-jp) is manual for now. Issues welcome
- Written by a marketer living in Japan ([@vinentW789](https://x.com/vinentW789)), not an engineer. It came out of needing to ship Japanese writing, and it gets fixed as long as I keep using it. No company behind it

## Layout

| File | Contents |
|---|---|
| `SKILL.md` | Main. Document-type routing → material gate → speaker positioning → 13 writing rules → pre-delivery ban table → inspection loop |
| `references/forbidden-ja.md` | Ban patterns (set phrases / AI pet words / grandiose vocabulary / subject substitution / structure / keigo / kango register / symbols) |
| `references/evidence-ja.md` | Evidence layer. Two-layer frame, three components of human-ness, metrics, three reversals |
| `examples/` | Real before/after text with lint output. Reproduce it yourself |

## Credits

- Skeleton: [KKKKhazix/human-writing](https://github.com/KKKKhazix/human-writing) (MIT) — the Chinese "living-person writing" skill. Material gate and speaker positioning are ported from it
- Machine checking: [coji/natural-japanese](https://github.com/coji/natural-japanese) (MIT) — morphological-analysis lint (sudachipy) with corpus calibration. Not vendored; install alongside
- Ban-list material: [iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) (MIT), [chezou/slop-nuki](https://github.com/chezou/slop-nuki) (MIT)
- Evidence: Zaitsu &amp; Jin 2023, *PLoS One* 18(8), [PMID 37556434](https://pubmed.ncbi.nlm.nih.gov/37556434/) / Onishi Yume, *A Study of "Natural Japanese" as Seen Through AI-Generated Text* (Hiroshima University, Faculty of Letters, 2026 undergraduate thesis)

## License

MIT
