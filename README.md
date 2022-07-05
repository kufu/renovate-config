# renovate-config

SmartHRのプロダクトが共通で継承するrenovate設定です。

ライブラリの更新に追従し、古いコードを残さないようにするために導入しましょう！

## 各プロダクトでの使い方

このリポジトリに記述されているのは全プロダクト共通の設定のため、実際に利用するリポジトリではこの設定を継承・必要に応じて上書きしつつ利用します。

### リポジトリの設定

リポジトリに対してrenovateを有効化しましょう。

社内の運用に従う必要があるため、詳細は身近なフロントエンドメンバーに質問してみるとよいと思います。

### 設定ファイルの作成

プロジェクト直下に `renovate.json` を作成します。

最低限、以下の内容を記述してください。

```json
{
  "extends": [
    "github>kufu/renovate-config"
  ],
  "enabledManagers": ["npm"]
}
```

その他、共通設定を上書きしたい項目については [Configuration Options - Renovate Docs | Renovate Docs](https://docs.renovatebot.com/configuration-options/) を参照してください。
