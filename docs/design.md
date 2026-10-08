# 設計メモ：BoardGames（仮）

## 画面一覧と遷移
1. **トップ画面** `/`：ゲーム選択（オセロ／チェス）＋先手・後手の選択
2. **対局画面** `/othello`, `/chess`：盤面・手番表示・ステータス（チェック／パス）・「はじめから」「トップへ」
3. **結果表示**：対局画面上にモーダルで勝敗を出す（別画面にしない）

遷移：トップ → 対局 → （終局）結果モーダル → 「もう一度」で同じ対局 or 「トップへ」

## データ構造（保存するもの・形式）
永続化はしない（リロードで消えてよい）。Reactのstateのみ。

```ts
type GameId = 'othello' | 'chess';
type Player = 'human' | 'cpu';

// 両ゲーム共通のインターフェース（UIはこれだけを見る）
interface GameAdapter {
  board: Cell[][];                 // 8x8。UI描画用に正規化した盤面
  turn: 'black' | 'white';         // チェスは black=黒駒, white=白駒
  legalMoves: Move[];              // 選択中マスから可能な手（ハイライト用）
  status: 'playing' | 'check' | 'pass' | 'finished';
  result?: 'black' | 'white' | 'draw';
  move(m: Move): void;
  cpuMove(): void;
  reset(): void;
}
```
ポイント：**ライブラリごとの差をAdapterで吸収**し、盤面コンポーネントは1つで済ませる。

## 外部API・ライブラリ
- `react`, `react-dom`, `react-router-dom`（画面遷移）
- `js-chess-engine`：チェスのルール判定・合法手・AI（レベル指定可）
- `thaw-reversi-engine.ts`：オセロのAI（盤面文字列＋探索深さ→最善手）
  - 盤面状態の管理・合法手判定が足りない場合は `reversi-js` を併用
- 外部APIなし、APIキーなし、`.env`不要

## ディレクトリ構成
board-games/
├── src/
│ ├── main.tsx
│ ├── App.tsx # ルーティング
│ ├── pages/
│ │ ├── Home.tsx
│ │ └── Play.tsx # GameAdapterを受け取る共通対局画面
│ ├── components/
│ │ ├── Board.tsx # 8x8盤面（タップ・ハイライト）
│ │ ├── StatusBar.tsx
│ │ └── ResultModal.tsx
│ └── games/
│ ├── types.ts # GameAdapter / Cell / Move
│ ├── useChess.ts # js-chess-engine のAdapter（hook）
│ └── useOthello.ts # reversiエンジンのAdapter（hook）
├── docs/ (plan.md, design.md)
├── CLAUDE.md / README.md
└── package.json, vite.config.ts


## 実装の順番（Claude Codeに渡す手順）
1. `npm create vite@latest`（React + TypeScript）で雛形作成、Vercelにデプロイして「Hello」が公開URLで見える状態にする
2. `types.ts` と `Board.tsx`：ダミー盤面を描画し、マスのタップで座標がconsoleに出る
3. `useOthello.ts`：人の着手→合法手ハイライト→CPU応手→パス→終局判定まで
4. `Play.tsx` + `StatusBar` + `ResultModal` でオセロを1局通せる
5. `useChess.ts`：同じAdapterを満たす形で実装（駒の表示はUnicode記号 ♔♕… で十分）
6. `Home.tsx` とルーティング、先手・後手選択
7. スマホ表示の調整（盤面を画面幅いっぱい、タップ領域の確保）
8. README整備、最終デプロイ