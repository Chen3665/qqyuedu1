# File: .github/workflows/repo-sync.yml

name: sync-ziye12-JavaScript

on:

  schedule:

    - cron: '1 */3 * * *'

  workflow_dispatch:

  watch:

    types: started

  repository_dispatch:

    types: sync-ziye12-JavaScript

jobs:

  repo-sync:

    env:

      PAT: ${{ secrets.PAT }} #此处PAT需要申请，教程详见：https://www.jianshu.com/p/bb82b3ad1d11

      dst_key: ${{ secrets.GITEE_PRIVATE_KEY }} # 我自己同步到gitee使用，其他人可忽略

    runs-on: ubuntu-latest

    if: github.event.repository.owner.id == github.event.sender.id

    steps:

      - uses: actions/checkout@v2

        with:

          persist-credentials: false

      - name: sync ziye12-JavaScript

        uses: repo-sync/github-sync@v2

        if: env.PAT

        with:

          source_repo: "https://github.com/ziye12/JavaScript.git"

          source_branch: "master"

          destination_branch: "master"

          github_token: ${{ secrets.PAT }}

      # 我自己同步到gitee使用，其他人可忽略

      - name: sync github -> gitee

        uses: Yikun/hub-mirror-action@master

        if: env.dst_key

        with:

          # 必选，需要同步的Github用户（源）

          src: github/Sunert

          # 必选，需要同步到的Gitee的用户（目的）

          dst: gitee/Sunert

          # 必选，更新指定库名字

          static_list: "Scripts"

          # 必选，Gitee公钥对应的私钥，https://gitee.com/profile/sshkeys

          dst_key: ${{ secrets.GITEE_PRIVATE_KEY }}

          # 必选，Gitee对应的用于创建仓库的token，https://gitee.com/profile/personal_access_tokens

          dst_token: ${{ secrets.GITEE_TOKEN }}
