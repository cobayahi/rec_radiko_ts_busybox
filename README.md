# rec_radiko_ts_busybox
[@uru2](https://github.com/uru2) 氏の rec_radiko_ts (https://github.com/uru2/rec_radiko_ts) を、BusyBox 等のより小さなコマンド環境で動作させることを目的としたフォークです。[busybox-w32 (BusyBox for Windows)](https://frippery.org/busybox/) での動作をメインターゲットとしつつ、より少/省コマンドな環境も見据えて、オリジナルで使用されているロジックを awk ベースの処理やシェル組込み機能へと置換しています。

## 動作確認環境
[zlatkovic.com 配布の xmllint](https://www.zlatkovic.com/libxml.en.html) では `-l` オプションの表示に不具合を生じるため、MSYS2 Packages 等のモダンなビルドの利用を推奨します。
- Windows 11 24H2
    - busybox64u (https://frippery.org/busybox/) 1.38.0
    - xmllint (https://packages.msys2.org/)
        - libxml 2.15.1 (mingw64)
        - libiconv 1.18 (mingw64)
        - zlib 1.3.1 (mingw64)
    - ffmpeg 5.1.8

BusyBox を用いずに OS のネイティブシェルで実行可能なことも、以下の環境で確認しています。
- Artix (rolling release)
    - curl 8.18.0
    - xmllint using libxml version 21303
    - ffmpeg 7.0.2

## オリジナルからの変更点

### コマンドの置換
[元プロジェクト](#元プロジェクト) のスクリプト内に存在するコマンドを下表のように置き換えています。

| Commands | Replaced with |
|---|---|
| `cut`, `dd`, `head`, `printf` (shell built-in), `realpath`, `tr` | `awk` |
| `grep` | `awk` or `case` |
| `basename`, `touch` | shell built-ins |

### 乱数生成方式を変更
Windows 等 `/dev/random` が利用できない環境に対応するため、乱数生成器を置換しています。
- `lsid` の生成方法が異なります。
- 一時ファイルの保存先である一時ディレクトリの生成を `mktemp` にて実装しています。
    - 環境変数 `TMPDIR` 未指定時の挙動も異なります（[使い方](#使い方) を参照ください）。

### CRLF で生じる問題に対応
オリジナルのコードそのままでは xmllint を用いたタイムフリープレイリスト群の抽出処理等、改行コードの違いに由来する不具合を複数確認しているため、CRLF/LF 両環境での動作性を担保できるロジックへと変更しています。

### FFmpeg のログレベル
chunk ダウンロード時のログレベルを、オリジナルの `error` から `info` に変更しています。必要に応じて他のレベルへ変更してください。

## 使い方
- BusyBox ほかのパスや一時ファイルの保存先（環境変数 `TMPDIR` にて設定）は予め適当に設定してください。
    - `TMPDIR` 未指定の場合はカレントディレクトリに一時ディレクトリを作成するようにしています。
- オプション等の実行例は [元プロジェクト](#元プロジェクト) に準じます。
```
$ busybox sh ./rec_radiko_ts_busybox.sh -s RN1 -f 201705020825 -t 201705020835 -o "/hoge/2017-05-02 日経電子版NEWS(朝).m4a"
```

## ライセンス
[元プロジェクト](#元プロジェクト) に準じた MIT License で配布しています。

## 元プロジェクト
rec_radiko_ts (https://github.com/uru2/rec_radiko_ts)
- 作者である [@uru2](https://github.com/uru2) 氏には本フォークへの助言も頂きました。感謝しております。
