# ja-human-writing

**禁止語リストで消臭する前に、書く前の規律。** 素材ゲートと語りの位置、日本語ネイティブの禁止パターン、そして計量言語学の実測にもとづく三つの「逆方向」注意(通説どおりに直すと、かえって AI っぽくなる)を一つにまとめた、日本語執筆・改稿用の Claude Code スキル。機械検査は [coji/natural-japanese](https://github.com/coji/natural-japanese) に委託する。

## なにが違うのか

日本語の「AI臭」対策スキルはすでにいくつもある。禁止語を消す・記号を直す・構造テンプレを崩す——どれも書いた**後**の消臭で、このレイヤーは [stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) や [natural-japanese](https://github.com/coji/natural-japanese) がすでに強い。

本スキルはその一つ上流を担当する。

1. **書く前の規律** — 1200 字を超えるノンフィクションは、出どころ付きの具体的な素材 5 件を先に揃える(素材ゲート)。誰が・何をきっかけに・何を判断として語るのかを先に決める(語りの位置)。素材の無い長文は、どう推敲しても水増ししか残らない。
2. **三つの「逆方向」注意** — 計量言語学の実測では、(1) 語彙多様性はむしろ AI の方が高い(繰り返すのは人間)、(2) 比喩は AI の方が 5 倍近く多い、(3) 接続詞の使用量に人機差はほぼ無く、差は種類にある。つまり「類義語に置換」「比喩を足す」「接続詞を削る」という定番の直しは、全部逆に効く。
3. **編成(オーケストレーション)** — 機械で測れるもの(文長のばらつき・禁止語・翻訳調)は natural-japanese の形態素解析 lint に任せ、機械で測れないもの(書き手の声か slop か)は gut-check の二問で人が判定する。検出と判定を分ける。

## インストール

```bash
git clone https://github.com/YOUR_NAME/ja-human-writing.git
cp -R ja-human-writing/ja-human-writing ~/.claude/skills/
```

機械検査も使う場合(推奨):

```bash
git clone https://github.com/coji/natural-japanese.git
cp -R natural-japanese/skills/natural-japanese ~/.claude/skills/
```

natural-japanese が無くてもスキルは動く。lint 工程が手動チェックに降格するだけ。

## 使い方

Claude Code で:

- 「この記事の AI臭を消して」「もっと自然な日本語にして」
- 「○○について note 記事を書いて」(素材ゲートが先に走る)
- ビジネスメールの敬語チェック(二重敬語・させていただく濫用・クッション連打)

## 構成

| ファイル | 中身 |
|---|---|
| `SKILL.md` | 本体。レジスタ判定 → 素材ゲート → 語りの位置 → 執筆の規律 13 条 → 納品前の禁止表 → 検査工程 |
| `references/forbidden-ja.md` | 禁止パターン統合表(定型句/偏愛語/壮大化語彙/false agency/構造/敬語 slop/漢語レジスタ/記号) |
| `references/evidence-ja.md` | 実証層。二層の枠組み、人間らしさの三要素、操作化した指標、三つの逆方向 |

## 出典と謝辞

- 設計の骨格:[KKKKhazix/human-writing](https://github.com/KKKKhazix/human-writing) (MIT) — 中国語の「活人感」執筆スキル。素材ゲートと語りの位置の考え方はここから移植した
- 機械検査:[coji/natural-japanese](https://github.com/coji/natural-japanese) (MIT) — sudachipy 形態素解析 lint とコーパス校正。本スキルは vendoring せず併用を推奨
- 禁止表の素材:[iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) (MIT)、[chezou/slop-nuki](https://github.com/chezou/slop-nuki) (MIT)
- 実証層:大西夢『AI生成文からみた「自然な日本語」についての研究』(広島大学文学部 2026 卒業論文)/Zaitsu &amp; Jin 2023, *PLoS One* 18(8), PMID 37556434

## License

MIT

---

*English: A Claude Code skill for writing Japanese that reads human. Instead of another banned-word list, it adds the upstream layer: material gates and speaker positioning before writing, plus three empirically-grounded "reverse direction" warnings (fixing repetition, adding metaphors, and cutting conjunctions all make text MORE AI-like, per corpus studies). Machine checking is delegated to [coji/natural-japanese](https://github.com/coji/natural-japanese). Skill instructions are in Japanese.*
