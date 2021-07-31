# sync2all
同步各大佬脚本库
actions sync食用方法


name: XXXX-sync # yml名称（区分同步脚本名称）
on:
  schedule:
    - cron: '1 */2 * * *' # cron定时更新频率
      workflow_dispatch:
  watch:
    types: started
  repository_dispatch:
    types: sync-XXXXXX-XXXXXX #sync-作者-库名
jobs:
  repo-sync:
    env:
      PAT: ${{ secrets.PAT }} 
    runs-on: ubuntu-latest
    if: github.event.repository.owner.id == github.event.sender.id
    steps:
      - uses: actions/checkout@v2
        with:
          persist-credentials: false

      - name: sync cdle-jd_study #sync-作者-库名
        uses: repo-sync/github-sync@v2
        if: env.PAT
        with:
          source_repo: "https://github.com/XXXX/XXXXX.git" #同步库的git地址
          source_branch: "main" #同步库的分支名称
          destination_branch: "cdle2sync" #同步本地的分支名称
          github_token: ${{ secrets.PAT }} #读取github key变量
