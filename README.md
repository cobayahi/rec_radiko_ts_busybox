# rec_radiko_ts_busybox
このスクリプトは uru2 氏の rec_radiko_ts (https://github.com/uru2/rec_radiko_ts) を BusyBox 上で動作させるために、最小限の修正を加えたフォークです。

## 動作確認環境
- Windows 11 24H2
    - busybox64u (https://frippery.org/files/busybox/) 1.38.0
    - xmllint (https://www.zlatkovic.com/pub/libxml/64bit/)
        - libxml 2.9.3 (64bit)
        - libiconv 1.14 (64bit)
        - zlib 1.2.8 (64bit)
    - ffmpeg 5.1.8


## オリジナルからの変更点
動作確認に用いた環境で生じた問題に関連して、以下の変更を行っています。

### 乱数生成方式を変更
`/dev/random` が利用できなかったため、`date +%s%N` による簡易的・疑似的な置換を行っています。
`%N` の精度は BusyBox のビルド等に依存するようですので、必要に応じて更に別の方式へと置換してください。

### xmllint 出力の改行問題に対応
xmllint の出力に不具合を生じたため、タイムフリープレイリスト群の抽出処理を調整しています（具体的にはメインプレイリストの抽出にのみ留めています）。

### FFmpeg のログレベル
オリジナルの `error` から `info` に変更しています。必要に応じて他のレベルへ変更してください。

## 使い方
BusyBox ほかのパスや一時ファイルの保存先（環境変数 `TMPDIR` にて設定）は予め適当に設定してください。
```
$ busybox sh ./rec_radiko_ts_busybox.sh -s RN1 -f 201705020825 -t 201705020835 -o "/hoge/2017-05-02 日経電子版NEWS(朝).m4a"
```
オプション等の実行例は元プロジェクト (https://github.com/uru2/rec_radiko_ts) に準じます。

## 元プロジェクト
rec_radiko_ts
https://github.com/uru2/rec_radiko_ts

## ライセンス
元プロジェクトに準じた MIT License で配布しています。
