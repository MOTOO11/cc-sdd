# cc-sdd 利用手順（具体例付き）

このドキュメントは、cc-sdd を使って仕様駆動で開発を進めるための手順を、具体例を交えつつまとめたものです。

## 1. 前提

- Node.js が利用可能であること
- 対象プロジェクトのルートで作業すること

## 2. クイックスタート（最小手順）

```bash
# 対象プロジェクトのルートへ移動
cd your-project

# 例: Claude Code 向けテンプレートを導入
npx cc-sdd@latest --claude --lang ja
```

この時点で、プロジェクト内に仕様駆動用のテンプレートと設定が展開され、以降の `/kiro:*` コマンドが利用可能になります。

## 3. タイミング別に生成されるファイル

以下は「いつ」「どのコマンドで」「どのファイルが生成されるか」をまとめた一覧です。

| タイミング     | 代表コマンド                              | 生成されるもの（例）                                                                                                                                                                                                     |
| -------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 導入直後       | `npx cc-sdd@latest --claude --lang ja`    | 設定とテンプレート（例: [.kiro/settings/templates/](.kiro/settings/templates/)、[.kiro/settings/rules/](.kiro/settings/rules/)）                                                                                         |
| 仕様の器を作る | `/kiro:spec-init <作りたいもの>`          | 仕様用ディレクトリ（例: [.kiro/specs/](.kiro/specs/)）                                                                                                                                                                   |
| 要件作成       | `/kiro:spec-requirements photo-albums-en` | 要件定義（例: [.kiro/specs/photo-albums-en/requirements.md](.kiro/specs/photo-albums-en/requirements.md)）                                                                                                               |
| 設計作成       | `/kiro:spec-design photo-albums-en -y`    | 設計書（例: [.kiro/specs/photo-albums-en/design.md](.kiro/specs/photo-albums-en/design.md)）。調査が必要な場合は研究メモ（例: [.kiro/specs/photo-albums-en/research.md](.kiro/specs/photo-albums-en/research.md)）も生成 |
| タスク分解     | `/kiro:spec-tasks photo-albums-en -y`     | 実装タスク（例: [.kiro/specs/photo-albums-en/tasks.md](.kiro/specs/photo-albums-en/tasks.md)）                                                                                                                           |
| 実装           | `/kiro:spec-impl photo-albums-en`         | 仕様に沿ったコード変更（出力先は対象プロジェクトの各ソース）                                                                                                                                                             |

## 4. 仕様作成フロー（例：写真アルバム機能）

以下は「写真アルバム機能」を例にした基本フローです。

```bash
/kiro:spec-init 写真アルバム機能（アップロード、タグ付け、共有）
/kiro:spec-requirements photo-albums-ja
/kiro:spec-design photo-albums-ja -y
/kiro:spec-tasks photo-albums-ja -y
```

生成物の例：

- `.kiro/specs/photo-albums-ja/requirements.md`
- `.kiro/specs/photo-albums-ja/design.md`
- `.kiro/specs/photo-albums-ja/tasks.md`

## 5. 既存コード拡張フロー（例：検索機能の改善）

既存機能を拡張する場合は、現状の把握（steering）から始めます。

```bash
/kiro:steering 既存の検索機能の構造と制約を整理
/kiro:spec-init 検索機能の関連度改善
/kiro:spec-design search-relevance-ja -y
/kiro:spec-tasks search-relevance-ja -y
```

必要に応じて `validate-gap` / `validate-design` を挟み、既存構造とのギャップや設計の整合性を確認します。

## 6. 実装フェーズ

タスクが確定したら、実装を進めます。

```bash
/kiro:spec-impl photo-albums-ja
```

生成されたタスクに沿って実装が進み、必要に応じて依存関係を考慮した並行実行も可能です。

## 7. よく使うオプション

```bash
# 変更内容を適用せずにプレビュー
npx cc-sdd@latest --dry-run

# 仕様ディレクトリを変更
npx cc-sdd@latest --kiro-dir docs
```

## 8. 主要ドキュメント

- コマンド一覧: docs/guides/command-reference.md
- 仕様駆動ガイド: docs/guides/spec-driven.md
- カスタマイズ: docs/guides/customization-guide.md

## 9. 使い方まとめ（最短）

1. `npx cc-sdd@latest --claude --lang ja`
2. `/kiro:spec-init <作りたいもの>`
3. `/kiro:spec-requirements <spec名>`
4. `/kiro:spec-design <spec名> -y`
5. `/kiro:spec-tasks <spec名> -y`
6. `/kiro:spec-impl <spec名>`
