# BoardGames（仮）

ブラウザでオセロとチェスをCPU相手に遊べるアプリ。インストール・登録不要、スマホ対応。

## デモ
- URL：（後で埋める）
- スクリーンショット：（後で埋める）

## 主な機能
- オセロ／チェスを選んでCPUと対局
- 置ける場所・動かせるマスをハイライト表示
- パス・チェック・終局を自動判定して結果を表示
- 先手・後手の選択、「はじめから」でリセット

## 技術スタックと選定理由
- **Vite + React + TypeScript**：Web UIの学習に直結し、小規模アプリを素早く作れる
- **js-chess-engine**：チェスのルール判定とAIを1つのライブラリで賄える
- **thaw-reversi-engine.ts**：αβ探索付きのオセロAI。自前でAIを書かずに済む
- **Vercel**：GitHubにpushするだけで無料で公開できる

2つのゲームを共通の `GameAdapter` インターフェースで抽象化し、盤面UIを1つで共有している。

## ローカルでの動かし方
```bash
git clone https://github.com/Y-Kenshiro/board-games.git
cd board-games
npm install
npm run dev
```
ブラウザで http://localhost:5173 を開く。

## 将来の拡張案
- CPUの強さ選択
- 待った・棋譜の保存
- 人 vs 人モード、オンライン対戦
- 将棋などゲームの追加

## 作って学んだこと
（後で埋める）