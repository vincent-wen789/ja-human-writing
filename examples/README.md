# 検証用サンプル

このスキルの効き目を自分の手で再現するためのファイル。

| ファイル | 中身 | lint 結果 |
|---|---|---|
| `before.md` | わざと AI 臭く書いた1000字の記事(取引所のセキュリティ事件を解説する体) | **8件**(禁止語6・`low_burstiness` 1・`low_specificity` 1) |
| `after.md` | 同じ題材を、このスキルの規律に沿って書き直したもの | **0件** |

## 再現手順

```bash
uv run ~/.claude/skills/natural-japanese/scripts/lint.py examples/before.md --genre essay
uv run ~/.claude/skills/natural-japanese/scripts/lint.py examples/after.md  --genre essay
```

## 途中で何が起きたか

書き直し1周目は **1件** 残った。`low_burstiness` ——文の長短のメリハリ不足。

禁止語は全部消して、一人称で書いて、判断も入れて、それでもリズムだけ機械のままだった。極端に短い文(「気づいたのは26時間後。」)と、長い一文を混ぜて2周目でゼロになった。

**表面の語彙より、リズムの方が後まで残る。** これが `references/evidence-ja.md` に書いた「二層」の、手元での再現。

## 注意

サンプルは1件。効果の証明ではなく、**手順が再現できることの確認用**。統計的な主張をするには全く足りない。
