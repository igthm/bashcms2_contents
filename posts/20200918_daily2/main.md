---
Keywords: ShellScript,bash
Copyright: (C) 2020 Junjie Li
---

# Shellメモ

- grep -m 1
  最初の1個だけ抽出したらすぐに終わる
- awk -v 変数=値
  bashの変数をawkの変数に代入し、awkで使用することが可能
- リスト | xargs grep -H .
  xargsはパイプからきたリストをgrepの引数に渡す。
  grepは「.（任意の１字）」を検索している。
  -Hは検索結果にファイル名をつける
- sort -k1,2

- awk ‘$3~/^posts/‘
  3列目が「posts/」から始まる行を抽出する。
- grep -C1
  検索対象の前後１行が抽出
- sed -n -e ‘1p’ -e ‘$p’
  -n: 出力の指示のない行を出力しない
  -e: 後ろの引数に処理内容を書く（２つ以上書くときに必要）
 1p、$p: １行目、最終行を出力

