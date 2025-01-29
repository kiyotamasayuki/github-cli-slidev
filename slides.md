---
theme: seriph
background: https://cover.sli.dev
title: Github CLIとは
info: |
  Presentation slides for developers.
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Github CLI

#### GitHub CLIを使用することでGitHubの機能を簡単に少ないコマンド入力で実現できます。

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---

# GitHub CLIで出来ることは?

すべての作業をターミナル 1 か所で行うことができるように、pull request、issues、GitHub Actions、およびその他の GitHub 機能を使用することができます

- 📝 **リポジトリの操作** - リポジトリの表示、作成、複製、フォーク
- 🎨 **Issueの操作** - Issue の作成、クローズ、編集、一覧表示
- 🧑‍💻 **プルリクエストの操作** - プルリクエストのレビュー、diff、マージ、一覧表示
- 🤹 **ワークフローの操作** - ワークフロー（github actions）の実行、表示
- 🛠 **GitHub APIの操作** - GitHub API から情報を取得します
  <br>
  <br>

その他にもたくさん。。。。

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# インストールとセットアップ方法

GitHub CLiのインストールとセットアップ方法です。詳細は[公式リポジトリ](https://github.com/cli/cli#installation)を参照してください。

<v-click>
1. GitHub CLIをインストールします

```
brew install gh
```

</v-click>
<br>
<v-click>
2. Githubのアカウントを認証します

```
gh auth login
```

</v-click>
<br>
<v-click>
3. 下記文言が出たら認証完了です。

```sh
✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as <username>
```

</v-click>
<br>
<v-click>
これでインストールとセットアップは完了です。
</v-click>

---

# 基本的な使用例

基本的には対話形式ですすめていくことになります。

- プルリクエストの作成

```
# Create a pull request interactively
~/Projects/my-project$ gh pr create

Creating pull request for feature-branch into main in owner/repo
? Title My new pull request
? Body [(e) to launch nano, enter to skip]
http://github.com/owner/repo/pull/1

~/Projects/my-project$
```

- プルリクエストの一覧

```
# Viewing a list of open pull requests
~/Projects/my-project$ gh pr list

Pull requests for owner/repo

#14  Upgrade to Prettier 1.19                           prettier
#14  Extend arrow navigation in lists for MacOS         arrow-nav
#13  Add Support for Windows Automatic Dark Mode        dark-mode
#8   Create and use keyboard shortcut react component   shortcut

~/Projects/my-project$
```

---

# よく使うCLIコマンド例

基本的に使用するコマンドは`alias`を設定してショートカットコマンドで使用しています。

## Command Shortcuts

|           |                                                                                                                                                                                                 |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gh prc`  | pr create --assignee @me --web <div class='comment'><v-click>PRの作成者を自分にセットしてブラウザでPRを開く</v-click></div>                                                                     |
| `gh prcl` | pr list --search "is:open is:pr author:@me" --web <div class='comment'><v-click>自分が作成したオープンのPR一覧をブラウザで開く</v-click></div>                                                  |
| `gh prrl` | pr list --search "is:pr is:open user-review-requested:@me" --web <div class='comment'><v-click>自分がレビュワーでレビューリクエストがされているオープンのPR一覧をブラウザで開く</v-click></div> |

<style>
  .comment {
  color: gray;
}
</style>

---

# GitHub CLIのextensions

GitHub CLIには拡張機能が存在しており、誰でも作成・使用することができます。

```html
gh extension install <ユーザー名 / リポジトリ名(拡張機能の名前)>
```

<v-click>

・[gh-poi](https://github.com/seachicken/gh-poi?tab=readme-ov-file)
<br>
<br>
　どのローカル ブランチがマージされたかを判断し、それらを安全に削除します。

</v-click>

<v-click>

・[gh-branch](https://github.com/mislav/gh-branch)
<br>
<br>
　ブランチのあいまい検索、ブランチ間のすばやい切り替えを行えます。

</v-click>

<v-click>

・[gh-copilot](https://github.com/github/gh-copilot)
<br>
<br>
　コマンドラインの提案と説明をしてくれます。

</v-click>

<v-click>
<div class='warning'>
  <a href='https://github.com/customer-terms/github-copilot-product-specific-terms'>GitHub Copilot Product Specific Terms</a>(6.B.1)に「Copilot for the Command Line Interfaceを使用した場合はPromptsが保持される」という記載があるので業務での使用はできないです。
</div>
</v-click>

<style>
  .warning {
  color: red;
  font-weight: bold;
  padding: 5px;
  border: 2px solid red;
  border-radius: 5px;
}
</style>

---

# その他の使用例

github actions内でghコマンドを使用したりすることもできます。

```js {all|21}
name: 初回コミットからマージまでの時間を計測してコメント
on:
  pull_request:
    branches:
      - develop # baseブランチの指定
    types: [closed]

jobs:
  lead-time:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    steps:
      - name: checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: calc time
        run: |
          gh api graphql -f id="${{ github.event.pull_request.node_id }}" -f query='
            query ($id: ID!) {
              node(id: $id) {
                ... on PullRequest {
                  mergedAt
                  commits(first: 1) {
                    nodes {
                      commit {
                        authoredDate
                      }
                    }
                  }
                }
              }
            }
          ' |
          jq -r '.data.node | .mergedAt, .commits.nodes[0].commit.authoredDate | fromdate | strftime("%s")' |
          xargs -n 2 |
          awk '{printf "%d", ($1 - $2) / 60 / 60 }'|
          xargs -I{} echo LEAD_TIME_HOUR={} >> $GITHUB_ENV
      - name: comment
        run: |
          gh pr comment ${{ github.event.pull_request.html_url }} \
          -b "@${{ github.event.pull_request.user.login }} お疲れ様でした!!🍺 リードタイムは$(expr ${LEAD_TIME_HOUR} / 24)日 (${LEAD_TIME_HOUR}時間)でした!!🎉"
```

---

EOF

---
