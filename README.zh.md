[日本語](README.md) · [English](README.en.md) · **中文**

# ja-human-writing

帮助撰写和修改日文、保留作者判断与语气的 Claude Code skill。

v0.1.1 · 2026-09

## 已经自然的句子，就留下

轻改成稿时保留事实和判断力度，只处理具体问题。比如下面这个示意段落，后一句重复了前一句：

```text
今回の変更で、検索時間が短くなる可能性があります。
つまり、検索にかかる時間が短くなる可能性があるということです。
```

可以改成：

```text
今回の変更で、検索時間が短くなる可能性があります。
```

“可能缩短搜索时间”的限定保留。不改成保证有效，不编造实测秒数，也不为了句长参差去拆句。

## 新写和改稿，边界不同

- **新写文章**：整理材料出处和作者判断。长文可以用五件具体材料作提醒，但不是配额；一件足够完整的经历或案例也能支撑文章。
- **修改成稿**：保留结构、事实、引用、否定、条件和确信程度。自然的句子与作者语癖不动，明确要求重组才调整结构。
- **判断机检提示**：可选的 [natural-japanese](https://github.com/coji/natural-japanese) lint 负责找候选问题，只有能说明具体阅读障碍才改。

仓库提供的是指令与流程，没有自制 AI 检测引擎。措辞和结构参考了 [stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp)、[slop-nuki](https://github.com/chezou/slop-nuki) 等清单，但命中不等于需要删除。

## v0.1.1 调整了什么

- 不再强制极长句、极短句、一人称和体言止め比例。
- 不按数量删除三点结构、不确定性表达或真实的“A不是B”纠偏。
- 不为补主语而添加行动者、个人经历，也不把核验过的事实机械改成传闻。
- 标点、邀请、导航、引用和合法 Markdown 按语境判断。
- lint 输出放独立临时目录，保留用户原有的稿件和 JSON。

## 研究能支持到哪里

大西夢的本科论文在本调查中比较了约500字的人类、AI文本各20篇。观察结果不能直接变成所有社交短帖、所有当前模型的改稿规则。旧版还把句长的**标准差**误读成每句话的长度，这次已修正。

Zaitsu & Jin（2023）研究的是日文文本来源分类，没有验证“删除某些词、调整句长能让文章更好”。

原论文链接、核对过的 lint 实现和撤回的推断见[证据与边界](ja-human-writing/references/evidence-ja.md)。同义词、比喻和接续词都按含义与阅读效果判断，不为模仿某种统计分布而增删。

## 安装

**前提**:装好 [Claude Code](https://claude.com/claude-code)。终端只用这一次。不额外花钱(跑在你现有的 Claude 额度里)。

打开终端,把这三行按顺序粘进去。

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/vincent-wen789/ja-human-writing.git
cp -R ja-human-writing/ja-human-writing ~/.claude/skills/
```

要装机检的话(可选、推荐),再来两行:

```bash
git clone https://github.com/coji/natural-japanese.git
cp -R natural-japanese/skills/natural-japanese ~/.claude/skills/
```

之后不用再碰终端。

<details>
<summary><b>这两行到底在干什么</b></summary>

- `mkdir -p ~/.claude/skills` —— 建好放 skill 的地方(已经有就什么都不做)
- `git clone` —— 把 GitHub 上的一个文件夹下载到你当前所在的位置
- `cp -R ... ~/.claude/skills/` —— 把那个文件夹复制到 Claude Code 找 skill 的地方
- **它只是复制。** 不删东西,不改系统设置。不喜欢就把复制过去的文件夹删掉,一切照旧
- **不需要管理员权限**(全在你自己的用户目录里完成,公司发的电脑通常也能跑)
- **「Claude Code」不是浏览器或桌面 app 里的 Claude**,是跑在终端里的那个,要先装好。费用包含在你的 Claude 订阅里,**这个 skill 本身免费**
</details>

- **机检是可选的。** 不装 natural-japanese,那一步降级成人工清单,skill 照样跑
- 要用机检的话需要 [uv](https://docs.astral.sh/uv/),Python 依赖它自己会装,不用手动 pip
- **skill 指令本身是日文写的**(规则讲的就是日语特有现象),但你用什么语言跟 Claude 说话都不影响
- **Windows**:`cp -R` 换成 PowerShell 的 `Copy-Item -Recurse`

## 怎么用

直接向 Claude Code 提出要求：

```text
轻改这篇日文草稿。事实和语气不变，只处理空转、重复或妨碍理解的表达。
```

需要修改理由时一并说明。新写文章可以提供材料后请求成稿；缺事实则在任务范围内核实，缺个人体验则向作者确认，不靠编造填补。

商务与技术文档优先信息组织，保留正常语域，不强加个人叙事或句长变化。敬语可按需检查。

## 参考你的语气

提供1〜3篇自己认可的旧稿，让它参考用词、下判断的方式和敬体松紧。术语重复、有效比喻可能就是作者声音，不必改成统计意义上的“人味”。

## 与其他 skill 的关系

材料盘点和说话位置来自对 [human-writing](https://github.com/KKKKhazix/human-writing) 的日文适配。词表与 stop-ai-slop-jp、slop-nuki 有重叠；合并使用时，本 skill 以信息保真和作者声音优先，不机械叠加禁令。

natural-japanese 是可选的机检依赖，未安装时仍可人工检查。参考文件按需读取。上下文成本取决于模型和实际加载文件，旧版的 token 估算已移除。

## 边界与验证

- 主要面向观点稿、个人文章和社交短帖，不跨语域套同一套改法。
- 短文常达不到统计门槛；报错或沉默都不能证明自然度或文本来源。
- [examples/](examples/) 保留旧版示例与当时的 lint 记录，不是效果证明，也不是当前推荐改法。
- 本次修正规则一致性与改稿边界，尚无独立读者盲测证明整体文风改善。
- 上游变化由人工跟进；发现错误或误报欢迎提 Issue。

作者是在日 marketer（[@vinentW789](https://x.com/vinentW789)），因自己需要写日文、发日文而制作，持续在使用中修正。

## 结构

| 文件 | 内容 |
|---|---|
| [SKILL.md](ja-human-writing/SKILL.md) | 创作与改稿边界、语域、材料、说话位置、检查流程 |
| [forbidden-ja.md](ja-human-writing/references/forbidden-ja.md) | 按语境判断的措辞、结构和敬语检查表 |
| [evidence-ja.md](ja-human-writing/references/evidence-ja.md) | 来源、实现观察、适用限制和撤回的推断 |
| [examples/](examples/) | 历史 before/after 与重跑方式 |

## 出处与致谢

- 骨架:[KKKKhazix/human-writing](https://github.com/KKKKhazix/human-writing) (MIT) — 中文的「活人感」写作 skill,材料闸和说话位置从它移植
- 机检:[coji/natural-japanese](https://github.com/coji/natural-japanese) (MIT) — 形态素解析(把句子拆成词的分析)lint + 语料校准。没有 vendoring,建议并装
- 词表素材:[iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) (MIT)、[chezou/slop-nuki](https://github.com/chezou/slop-nuki) (MIT)
- 实证:Zaitsu & Jin 2023, *PLoS One* 18(8), [PMID 37556434](https://pubmed.ncbi.nlm.nih.gov/37556434/) / 大西夢『AI生成文からみた「自然な日本語」についての研究』(広島大学文学部 2026 卒業論文)

## License

MIT
