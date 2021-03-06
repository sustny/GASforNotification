# GAS for Notification

## 概要
情弱なので最近存在に気付きました。

JavaScript互換かつGoogle系のサービスと相性がいいみたいなので積極的に覚えていきます。

また、このリポジトリにあるスクリプトはGoogleスプレッドシートと対応しています。

-----

## Main.gs

各スクリプトに記載された関数を呼び出し情報を生成するスクリプト。流れは以下。

1. スプレッドシートに記載されたイベント情報から直近で開催予定のイベントを検索
2. 条件(イベント開催日や記載の有無)を判定の上で各スクリプトを呼び出す

また、Main()でプログラムの処理時間を計測。

### EventInfo.gs
イベントの開催日が明日or1週間後だった場合、イベントの情報を生成し戻り値としてMain()へ引き渡す。

### TideInfo.gs
イベントが釣り関連だった場合、開催地から最も近い潮位観測地点の当日+翌日の潮位情報を生成し戻り値としてMain()へ引き渡す。

→TideInfoのみ実装したものを別リポジトリで公開しています

https://github.com/sustny/TideInfo

### LineNotify.gs
EventInfo.gsとTideInfo.gsで生成されたイベント情報をLINEグループに通知する。

### Hidden.gs
ある(行|列)のセルが空白なら、その(行|列)ごと隠す。

-----

## プログラムの流れ

0. (スクリプトは毎日19時頃自動で動くように設定済)
1. Main.gs: シートから直近のイベントが記入されている行を探す
2. Main.gs: イベントが明日or1週間後の場合、行番号と明日or1週間後の日付を引数としてEventInfo()に渡す
3. EventInfo.gs: 渡された引数から情報を生成しMain()へ返す
4. Main.gs: もしイベントが釣り関連だった場合、釣り場の位置情報と日付を引数としてTideInfo()に渡す
5. TideInfo.gs: 渡された引数から情報を生成しMain()へ返す
6. Main.gs: 2->5をcount回繰り返す(ただしイベントの記入がされていない行に突入したらbreak)
7. Main.gs: ついでにHidden()も実行しておく
8. Main.gs: Logger.log()で実行時間を表示し終了

-----

## ライセンス

各スクリプトは、MITライセンスのもとに公開いたします。LICENSE.txt を参照してください。

## License

This software is released under the MIT License, see LICENSE.txt.
