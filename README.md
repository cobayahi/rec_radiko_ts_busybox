# rec_radiko_ts_busybox
このスクリプトは [@uru2](https://github.com/uru2) 氏の rec_radiko_ts (https://github.com/uru2/rec_radiko_ts) を BusyBox 上で動作させるために、最小限の修正を加えたフォークです。

## 動作確認環境
- Windows 11 24H2
    - busybox64u (https://frippery.org/busybox/) 1.38.0
    - xmllint (https://www.zlatkovic.com/pub/libxml/64bit/)
        - libxml 2.9.3 (64bit)
        - libiconv 1.14 (64bit)
        - zlib 1.2.8 (64bit)
    - ffmpeg 5.1.8


## オリジナルからの変更点
動作確認に用いた環境で生じた問題に関連して、以下の変更を行っています。

### 乱数生成方式を変更
Windows 等 `/dev/random` が利用できない環境に対応するため、`lsid` の生成方法を置換しています。

### xmllint 出力の改行問題に対応
xmllint を用いたタイムフリープレイリスト群の抽出処理に不具合を生じたため、処理内容を調整しています。

### FFmpeg のログレベル
chunk ダウンロード時のログレベルを、オリジナルの `error` から `info` に変更しています。必要に応じて他のレベルへ変更してください。

## 使い方
- BusyBox ほかのパスや一時ファイルの保存先（環境変数 `TMPDIR` にて設定）は予め適当に設定してください。
    - `TMPDIR` 未指定の場合はカレントディレクトリに一時ディレクトリを作成するようにしています。
- オプション等の実行例は元プロジェクトに準じます。
```
$ busybox sh ./rec_radiko_ts_busybox.sh -s RN1 -f 201705020825 -t 201705020835 -o "/hoge/2017-05-02 日経電子版NEWS(朝).m4a"
```

## ライセンス
元プロジェクトに準じた MIT License で配布しています。

## 元プロジェクト
rec_radiko_ts (https://github.com/uru2/rec_radiko_ts)
- 作者である [@uru2](https://github.com/uru2) 氏には本フォークへの助言も頂きました。感謝しております。
