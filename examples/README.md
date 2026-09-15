# 旧版の作例と lint 記録

このフォルダは v0.1.0 時点の before/after を保管している。現在の推奨編集手順や、効果を証明する比較実験ではない。

| ファイル | 内容 | 当時記録した lint 結果 |
|---|---|---|
| `before.md` | 意図的に定型表現を入れた記事 | 8件 |
| `after.md` | 同じ題材を旧版の規律で書き直した記事 | 0件 |

当時は `low_burstiness` を消すため、極端に短い文と長い文を混ぜた。v0.1.1 ではこの方法を推奨しない。指摘が減っても、自然さ、事実の保全、作者の声が改善した証拠にはならない。

また、この組は素材を追加する書き直しを含み、原文の情報だけを保つ軽い改稿の例ではない。軽い改稿の境界は [SKILL.md](../ja-human-writing/SKILL.md) を参照。

## 現在の環境で実行する

natural-japanese と uv を導入済みの場合、リポジトリのルートで実行する。インストール先が異なる場合はパスを変更する。

```bash
uv run ~/.claude/skills/natural-japanese/scripts/lint.py examples/before.md --genre essay
uv run ~/.claude/skills/natural-japanese/scripts/lint.py examples/after.md --genre essay
```

件数はlintの版や設定によって変わる。再実行で確認できるのは、その実装の検出結果まで。独立した読者による盲検比較は行っていない。
