# abook

![GitHub Action status](https://github.com/hentaigirls/abook/workflows/build%20abook%20template/badge.svg)

## 使い方

### 統括を行う人

1. `./info.txt` を編集します。
1. `./works.txt` に完成した本文の `input` 命令を追加します。 例: `\input{./work/author/title/main.tex}`
1. アンケートページが必要な場合は、 `./figure/form.pdf` にQRコードを追加します。
1. あとがきページが必要な場合は、 `./figure/afterwords.png` にあとがき画像を追加します。

### 本文を追加する人

1. `./work/example-author/example-title/` を `./work/<author>/<title>/` にコピーします。
1. `./work/<author>/<title>/main.tex` を編集します。
1. `./work/<author>/<title>/LICENSE` に適切なライセンス表示を追加します。
1. `./work/input.tex` に `\input{./work/<author>/<title>/main.tex}` を追加するか、 `./script/make_story_list.sh` を実行します。なお、 `./work/input.tex` はgitで管理されていません。
1. `llmk book.tex` または `docker run --rm -v .:/data amane/lualatex-latexmk llmk book.tex` を実行するか、push後にCIの結果を確認します。

### 表紙を追加する人

1. `./figure/cover1.pdf` に表1を追加します。
1. `./figure/cover4.pdf` に表4を追加します。
1. `llmk book.tex` を実行するか、push後にCIの結果を確認します（ `printmode` がtrueの場合は表紙が出力されません）。

## Tips

### `./figure/form.pdf`

#### A5

おおよそ1181x1181（600dpi/5cm）の画像を含んだPDFの作り方のサンプルです。

```sh
qrencode "https://goo.gl/forms/XXXXXXXXXX" -m 0 -o /tmp/qrcode.png
WIDTH=$(identify /tmp/qrcode.png | cut -f3 -d" " | cut -f1 -d "x")
mogrify -scale $(echo "(1181/$WIDTH+1)*$WIDTH" | bc) /tmp/qrcode.png
convert -density 600x600 -units PixelsPerInch /tmp/qrcode.png ./figure/form.pdf
```

#### A6

おおよそ827x827（600dpi/3.5cm）の画像を含んだPDFの作り方のサンプルです。

```sh
qrencode "https://goo.gl/forms/XXXXXXXXXX" -m 0 -o /tmp/qrcode.png
WIDTH=$(identify /tmp/qrcode.png | cut -f3 -d" " | cut -f1 -d "x")
mogrify -scale $(echo "(827/$WIDTH+1)*$WIDTH" | bc) /tmp/qrcode.png
convert -density 600x600 -units PixelsPerInch /tmp/qrcode.png ./figure/form.pdf
```

### `./figure/afterwords.png`

#### A5

おおよそ2634x4050（600dpi / 112mm x 171mm）の画像の作り方のサンプルです。

```sh
convert -size 2634x4050 canvas:white ./figure/afterwords.png
```

#### A6

おおよそ1937x2634（600dpi / 82mm x 112mm）の画像の作り方のサンプルです。

```sh
convert -size 1937x2634 canvas:white ./figure/afterwords.png
```
