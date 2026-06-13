# tech-notes — 運用ルール（AIエージェント向け）

このリポジトリは「tech-notes」技術ノートシステムです。AIエージェント（Hermes, Codex 等）が以下のルールに従って運用します。

## ディレクトリ構造

```
tech-notes/
├── index.html                # トップ（記事一覧）
├── topics/                   # 各記事ディレクトリ
│   └── [slug]/
│       └── index.html
├── styles/
│   └── main.css              # ダークテーマCSS
├── images/                   # 画像ファイル
├── drafts/                   # 未完了対話セッション（コミット対象外）
├── .github/workflows/
│   └── static.yml           # GitHub Pages deploy
├── .nojekyll                 # Jekyll 無効化
├── .gitignore
├── AGENTS.md                 # 本ファイル
└── README.md
```

## 記事ルール

- **1記事 = 1ディレクトリ**: `topics/<slug>/index.html`
- **slug** は英数字とハイフンのみ使用（例: `python-asyncio`, `git-rebase-guide`）
- **slug に日本語は使用しない**

## 動作モード

### ワンショットモード
1. ユーザーが「調べてノート: <トピック>」と指示
2. 調査（web_search）→ HTML生成 → git add / commit / push まで一貫実行
3. コミットメッセージ: `notes: <slug>`

### 対話モード
1. ユーザーが通常の会話で技術トピックについて質問
2. Hermesが応答しながら対話を継続
3. ユーザーが「保存して」と言ったら、会話全体をHTML化して保存
4. 「保存して」と言われるまではコミットしない
5. コミットメッセージ: `notes: <slug>`

## 記事追加手順

1. `topics/_template.html` の内容を `topics/<slug>/index.html` にコピー
2. `{{ title }}` `{{ date }}` `{{ tags }}` `{{ body_html }}` を実際の内容に置換
3. 目次は h2 見出しから手動生成
4. `🔗 参照URL` セクションに調査に使用したURLをリスト
5. `index.html`（トップページ）の `<!-- 記事追加ここから -->` 〜 `<!-- 記事追加ここまで -->` 内に新規エントリを追記
6. `git add topics/<slug>/` → `git commit -m "notes: <slug>"` → `git push`

## コミットルール

- コミットメッセージは `notes: <slug>` 形式に統一
- 1記事 = 1コミット
- GitHub Pages は main ブランチのルートを参照

## 禁止事項

- 機密情報（API キー、パスワード、トークン等）をファイルに含めない
- 著作物の無断転載をしない
- 参照元のURLを必ず記事内に記載する

## 参照URL

各記事の末尾に `🔗 参照URL` セクションを設け、調査に使用したURLをリストすること。
