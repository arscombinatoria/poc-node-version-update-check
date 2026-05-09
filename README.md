# poc-node-version-update-check

`.node-version` に記載された Node.js バージョンの更新を定期監視し、
更新候補が見つかったときに GitHub Issue を自動作成するための PoC リポジトリです。

## 要件

毎日 **06:00（JST）** に GitHub Actions を実行し、`.node-version` のバージョンに対して以下を判定します。

- **マイナーバージョン更新**（例: `20.11.x` → `20.12.x`）
- **メジャーバージョン更新（LTS）**（例: `20.x` → `22.x`）

判定結果に応じて、Issue を次のルールで扱います。

- 更新候補がある場合:
  - **マイナー更新用 Issue** と **メジャー更新用 Issue** を分けて作成
- 更新候補がない場合:
  - 何もしない
- すでに同種の Issue が存在する場合:
  - 新規作成しない（重複防止）

## 想定ワークフロー概要

1. スケジュールトリガー（cron）で毎日 06:00 JST 実行
2. `.node-version` を読み取り、現在利用中の Node.js バージョンを取得
3. Node.js の公開バージョン情報を参照して更新有無を判定
   - マイナー更新があるか
   - 次の LTS メジャー更新先があるか
4. 条件に一致した場合のみ Issue を作成
   - 種別（major / minor）ごとにタイトル・ラベルを分離
   - 既存 Issue を検索し、重複作成を回避

## 期待する Issue の分離方針

- `type:node-minor-update` ラベル（または同等）
- `type:node-major-update` ラベル（または同等）

例:

- `chore: Node.js minor update available (20.11 -> 20.12)`
- `chore: Node.js major LTS update available (20 -> 22)`

## 備考

このリポジトリは要件整理・検証用の PoC です。
実運用では、利用する Node.js バージョン取得方法、参照 API、Issue テンプレート、
重複判定ロジック（タイトル一致 / ラベル一致 / open のみ対象など）をチーム運用に合わせて調整してください。
