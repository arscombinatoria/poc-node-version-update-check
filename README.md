# poc-node-version-update-check

`.node-version` の更新漏れを検知するための、シンプルな検証用（PoC）リポジトリです。

## 目的

Node.js のバージョンを管理しているプロジェクトで、以下のような差分不整合を早期に検知することを目的にしています。

- `package.json`（`engines.node`）は更新されたが `.node-version` が古いまま
- `.node-version` は更新されたが、他の設定ファイルと整合していない

このリポジトリでは、そうした更新漏れを CI やローカルチェックで検出するための最小構成を扱います。

## 想定ユースケース

- Node.js バージョンをチームで統一したい
- Renovate / Dependabot などでバージョン更新が入る環境
- リリース前に「実行環境バージョンのズレ」を防ぎたい

## 使い方（例）

```bash
# リポジトリをクローン
 git clone <your-repo-url>
 cd poc-node-version-update-check

# 必要に応じてチェック用スクリプトを実行
 # 例: npm run check:node-version
```

> ※ 実際のコマンドは、この PoC に追加するスクリプト構成に合わせて調整してください。

## CI 連携のイメージ

Pull Request 作成時に、`.node-version` と `package.json` の Node.js バージョン整合性チェックを実行します。

- 一致していれば CI 成功
- 不一致なら CI 失敗（更新漏れを早期に発見）

## 今後の拡張候補

- `.nvmrc` / `.tool-versions` など複数ファイル間の整合性チェック
- GitHub Actions での自動検証テンプレート追加
- バージョン更新時の自動 PR コメント出力

## ライセンス

必要に応じて追記してください。
