# Renovate Config

CCS で用いる [Renovate](https://docs.renovatebot.com/) の設定です。

## 使い方

プロジェクトルートに `renovate.json` ファイルを配置してください。

ファイルの内容を以下の通りにすると、このリポジトリの設定を用いることができます。

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>densanken/renovate-config"]
}
```

`renovate.json` を main ブランチに反映後、[Renovate App](https://github.com/apps/renovate) をリポジトリに設定する必要があります。

### 設定のカスタマイズ

最小限のカスタマイズで問題ない場合、上記設定後にカスタマイズしたい項目を追記することで設定の上書きが可能です。

大きくカスタマイズしたい場合については、[default.json](/default.json) のような記述を `renovate.json` に行うことで可能です。

なお、`renovate.json` は JSONC をサポートしているため、コメントを記載することができます。

[Configuration Options - Renovate Docs](https://docs.renovatebot.com/configuration-options/)

## 設定変更時の注意

基本的に CCS の各リポジトリの Renovate Config は [default.json](/default.json) の設定を用いています。

そのため、設定を変更する場合はその影響を十分考慮してください。

## 設定内容

- extends
  - 既存プリセットを順に適用します。配列の後ろに書いたプリセットほど後から上書きされます。
  - プリセットの考え方: https://docs.renovatebot.com/key-concepts/presets/
  - プリセット例:
    - config presets: https://docs.renovatebot.com/presets-config/
    - default presets: https://docs.renovatebot.com/presets-default/
    - security presets: https://docs.renovatebot.com/presets-security/

  - `config:recommended`
    - Renovate の推奨プリセットを適用します。
    - https://docs.renovatebot.com/presets-config/#configrecommended

  - `security:openssf-scorecard`
    - PR の本文に OpenSSF Scorecard のバッジを表示します。
    - https://docs.renovatebot.com/presets-security/#securityopenssf-scorecard

  - `security:minimumReleaseAgeNpm`
    - npm パッケージの更新について、リリースから3日経過するまで更新 PR を作成しないようにします。
    - https://docs.renovatebot.com/presets-security/#securityminimumreleaseagenpm

  - `:semanticCommits`
    - Renovate が作成するコミットメッセージやPR タイトルを semantic commits（Conventional Commits）形式にします。
    - https://docs.renovatebot.com/presets-default/#semanticcommits
    - https://docs.renovatebot.com/configuration-options/#semanticcommits

  - `:semanticCommitTypeAll(chore)`
    - semantic commits を使う前提で、Renovate が作るコミットの type を常に chore に寄せるためのプリセットです。
    - https://docs.renovatebot.com/presets-default/#semanticcommittypeallarg0

  - `:disableRateLimiting`
    - Renovate の PR 作成数制限（時間あたり、同時オープン数）を無効化するためのプリセットです。
    - https://docs.renovatebot.com/presets-default/#disableratelimiting
    - https://docs.renovatebot.com/configuration-options/#prconcurrentlimit
    - https://docs.renovatebot.com/configuration-options/#prhourlylimit

  - `:timezone(Asia/Tokyo)`
    - schedule を評価する基準タイムゾーンを Asia/Tokyo にします。
    - https://docs.renovatebot.com/presets-default/#timezonearg0
    - https://docs.renovatebot.com/configuration-options/#timezone

- `labels`: ["dependencies", "renovate"]
  - Renovate が作成する PR に付与するラベルです（トップレベルなので、原則すべての Renovate PR に付与されます）。
  - https://docs.renovatebot.com/configuration-options/#labels

- `osvVulnerabilityAlerts`: true
  - osv.dev の脆弱性情報を参照して、修正アップデートが存在する場合に脆弱性対応の PR を作成します（主に direct dependency が対象）。
  - https://docs.renovatebot.com/configuration-options/#osvvulnerabilityalerts

- `automergeStrategy`: "squash"
  - automerge 時のマージ方式を squash にします（automergeType=pr の場合に利用されます）。
  - https://docs.renovatebot.com/configuration-options/#automergestrategy

- `dependencyDashboard`: true
  - 依存関係更新の一覧 issue（Dependency Dashboard）を作成します。
  - https://docs.renovatebot.com/configuration-options/#dependencydashboard

- `branchConcurrentLimit`: 0
  - Renovate が同時に持つ更新ブランチ数の上限です。0 は上限なしを意味します。
  - https://docs.renovatebot.com/configuration-options/#branchconcurrentlimit

- `packageRules`
  - 依存関係の種類・更新種別・パッケージ名などにマッチさせて、挙動（ラベル、グルーピング、automerge など）をルールとして適用します。
  - 複数ルールがマッチした場合はマージされ、同じ項目は後ろのルールが優先されます。
  - https://docs.renovatebot.com/configuration-options/#packagerules

  - `matchUpdateTypes`: ["minor", "patch"]
    - minor/patch の更新にだけ、このルールを適用します（major は対象外）。
    - https://docs.renovatebot.com/configuration-options/#matchupdatetypes

  - `automerge`: true
    - 上記の matchUpdateTypes に該当する更新（minor/patch）について、条件を満たせば自動マージします。
    - https://docs.renovatebot.com/configuration-options/#automerge
