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

### Ruby のバージョンをまとめて上げる

`.ruby-version`・Dockerfile・`Gemfile.lock` の Ruby と `ruby/setup-ruby` を、1 つの PR で上げられます。
`ruby` preset を継承し、`enabledManagers` に `ruby-version` と `custom.regex` を含めてください。
Dockerfile の `FROM` と `ruby/setup-ruby` も上げるには、`dockerfile` と `github-actions` も含めてください。

```json
{
  "extends": [
    "github>kufu/renovate-config",
    "github>kufu/renovate-config:ruby"
  ],
  "enabledManagers": ["dockerfile", "github-actions", "ruby-version", "custom.regex"]
}
```

Dockerfile で `ARG` に Ruby のバージョンを書いている場合は、`ARG` の直前に次のコメントを足してください。
`FROM ruby:4.0.10-slim` のように `FROM` に直接書いている場合は不要です。

```dockerfile
# renovate: datasource=ruby-version depName=ruby
ARG RUBY_VERSION=4.0.10
```

`Gemfile.lock` の `RUBY VERSION` を更新するのは、Bundler 4.0.0 以降で書かれた patchlevel の無い形（例: `ruby 4.0.10`）だけです。
Bundler 2.7.x 以前で書かれた `ruby 3.3.6p108` の形は更新しないので、Bundler を 4.0.0 以降に上げてください。

### Node.js のバージョンをまとめて上げる

`.node-version`・`.nvmrc`・Dockerfile・CircleCI・`package.json` の `engines.node` の Node.js を、1 つの PR で上げられます。
`node` preset を継承し、`enabledManagers` に、Node.js のバージョンを書いている場所に合わせて次の manager を含めてください。

| 書いている場所 | manager |
| --- | --- |
| `.node-version` | `nodenv` |
| `.nvmrc` | `nvm` |
| `package.json` の `engines.node` | `npm` |
| Dockerfile の `FROM` | `dockerfile` |
| Dockerfile の `ARG` | `custom.regex` |
| `.circleci/config.yml` の image | `circleci` |

```json
{
  "extends": [
    "github>kufu/renovate-config",
    "github>kufu/renovate-config:node"
  ],
  "enabledManagers": ["npm", "nodenv", "nvm", "dockerfile", "circleci", "custom.regex"]
}
```

Dockerfile で `ARG` に Node.js のバージョンを書いている場合は、`ARG` の直前に次のコメントを足してください。
`FROM node:24.11.1-alpine` のように `FROM` に直接書いている場合は不要です。

```dockerfile
# renovate: datasource=node-version depName=node
ARG NODE_VERSION=24.11.1
```

docker image は `24.11.1-alpine` のように `x.y.z` で始まる tag だけを更新し、`-alpine` などの suffix は残します。`lts` や `24` のような tag は、この preset では扱いません。

その他、共通設定を上書きしたい項目については [Configuration Options - Renovate Docs | Renovate Docs](https://docs.renovatebot.com/configuration-options/) を参照してください。
