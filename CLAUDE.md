# CLAUDE.md

## プロジェクト概要
ブラウザでオセロとチェスをCPU相手に遊べる静的Webアプリ。
サーバー・DB・APIキーは使わない。数日で完成させる学習用の小規模プロジェクト。
詳細は `docs/plan.md`（企画）と `docs/design.md`（設計）を参照。

## 技術スタック
- Vite + React + TypeScript
- react-router-dom（画面遷移）
- js-chess-engine（チェスのルール・合法手・AI）
- thaw-reversi-engine.ts（オセロのAI）／必要なら reversi-js（盤面管理）
- デプロイ：Vercel（GitHubのmainにpushで自動デプロイ）

## コマンド
- `npm install`：依存関係のインストール
- `npm run dev`：開発サーバー（http://localhost:5173）
- `npm run build`：本番ビルド（`dist/`）
- `npm run preview`：ビルド結果の確認
- `npm run lint`：oxlint（create-vite 同梱のリンター。ESLint は使わない）

## コーディング規約
- コメントは日本語、コミットメッセージは英語（例：`feat: add othello adapter`）
- コンポーネントは関数コンポーネント＋hooks。クラスコンポーネントは使わない
- ゲームロジックは `src/games/` に閉じ込め、`src/components/` は `GameAdapter` 経由でのみ触る
- `any` は使わない。ライブラリの戻り値には型を付ける

## 実装時の注意点
- ライブラリのAPIは推測せず、必ず `node_modules/<pkg>/README.md` を読んでから使う
- thaw-reversi-engine.ts が盤面管理・合法手判定を持たない場合は reversi-js を併用し、
  それでも噛み合わなければ相談する（オセロの合法手判定だけ自前実装も許容）
- 実装は `docs/design.md` の「実装の順番」どおりに1ステップずつ進め、各ステップで動作確認する
- スマホ優先：盤面は画面幅に合わせる、マスは最低44px四方のタップ領域
- 永続化（localStorage等）はMVPでは入れない