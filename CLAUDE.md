# CLAUDE.md

このリポジトリでの作業時にClaude Codeへ提供するガイダンス。

## プロジェクト概要

Astroで構築した個人の**研究・学習ノート**サイト。GitHub Pagesにデプロイ: https://napnel.github.io

**目的**: 技術学習、研究ノート、ナレッジ管理のためのDigital Garden

**対象読者**: 主に自分自身（セルフドキュメント、ナレッジベース）

**更新頻度**: 週1回以上

## 開発コマンド

```bash
npm install      # 依存関係インストール
npm run dev      # 開発サーバー起動 (localhost:4321)
npm run build    # 本番ビルド (./dist/)
npm run preview  # 本番ビルドのプレビュー
```

## アーキテクチャ

### Content Collections

AstroのContent Collections API (`src/content.config.ts`) で型安全にコンテンツ管理:

- 記事は `src/content/blog/` にMarkdown/MDXで配置
- Zodでスキーマ定義。必須: `title`, `description`, `pubDate`
- オプション: `updatedDate`, `heroImage`, `tags`, `category`
- `getCollection('blog')` でアクセス

### ルーティング

- `src/pages/` でファイルベースルーティング
- 記事ページ: `src/pages/blog/[...slug].astro`
- `getStaticPaths()` でビルド時にルート生成

### サイト設定

- サイトURL: `astro.config.mjs` で `https://napnel.github.io`
- 定数: `src/consts.ts`
- 統合: MDX, Sitemap

### デプロイ

GitHub Actions (`.github/workflows/deploy.yml`):
1. `develop` ブランチへのpushまたは手動トリガーでビルド
2. Node.js 20使用
3. `npm ci && npm run build`
4. `./dist/` をGitHub Pagesにデプロイ

## ブランチ戦略

- **develop**: デフォルトブランチ。開発作業はここ、pushで自動デプロイ
- **feature/***: 機能ブランチ。完了したらdevelopにマージ
- **main**: 未使用（レガシー）

## プロジェクト構成

```
src/
├── assets/          # 静的アセット
├── components/      # Astroコンポーネント
├── content/
│   └── blog/        # 記事 (Markdown/MDX)
├── layouts/         # レイアウト
├── pages/           # ルート
│   ├── blog/
│   │   ├── [...slug].astro  # 記事ページ
│   │   └── index.astro      # 記事一覧
│   ├── about.astro
│   ├── index.astro
│   └── rss.xml.js
└── styles/          # スタイル
```

## 記事の追加

`src/content/blog/` に `.md` または `.mdx` を作成:

```md
---
title: "タイトル"
description: "説明"
pubDate: 2026-01-01
updatedDate: 2026-01-02    # オプション
heroImage: ./image.png      # オプション
tags: ["Astro", "WebDev"]   # オプション
category: "Tutorial"        # オプション
---

本文...
```

## デザイン方針

**Digital Garden** アプローチ:
- コンテンツ重視、装飾は最小限
- 読みやすさと整理を重視
- 完成度より学習と知識の蓄積を優先

## 文体

温度感低めの独り言スタイル:

**使う表現**
- 「〜らしい」「〜っぽい」「〜かもしれない」（断定しすぎない）
- 「〜だった」「〜した」（淡々と事実を述べる）
- 「よさそう」「使えそう」（控えめな評価）
- 短文、体言止め

**避ける表現**
- 「〜ですよね！」「〜しましょう！」（読者への語りかけ）
- 「めちゃくちゃ便利」「最高」（テンション高い表現）
- 感嘆符の多用
