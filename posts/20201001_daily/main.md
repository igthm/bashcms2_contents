---
Keywords: DailyArticle
Copyright: (C) 2020 Junjie Li
---

# 今日は10月1日

- MacのTimeMachineで過去に戻って見たけど、外部モニターに映らなかったので、MiniPort to HDMIケーブルが壊れたかな
- Let's note にDockerをインストールして、システムを移行した。
 - メモ：
  - github の webhook が上手く機能するように、ルーターのポート転送設定を忘れずに行うこと(PCを変えたため)
  - export LANG=ja_JP.UTF-8 の再設定が必要
- システム再構築進捗：
 - 「4.6時刻に関する情報の付加」完了
 - 前回気づかなかった点：
  - html ファイルの中で $hoge$ のような未定義の変数はyamlファイルを pandoc に読み込ませることで参照していた
