# STEP 6: 画像を切り抜こう

## strconvパッケージ

文字列から数値に変換するには、`strconv`パッケージの関数を使います。
`string`から`int`型に変換する(10進数として解釈)には、`strconv.Atoi`関数を使うのが手っ取り早いです。

`int`型以外に、または10進数以外として数値に変換したい場合は、`Parse`で始まる関数を使います。
なお、`int`と`int64`は別の型であり、別の型同士の演算は必ずキャストがいります。
キャストは、`int(f)`や`float64(n)`のようにできます。

## image/drawパッケージ

`image/draw`パッケージは、画像の描画を行う機能を提供するパッケージです。
`draw.Image`は`image.Image`に`Set`メソッドを追加したインタフェースで、画像に対して書き込みができるようになっています。

`draw.Draw`を使えば、指定した画像対して、別の画像を描画する事ができます。
もちろん、一部分を描画することもできます。

## 埋込み
構造体には、匿名フィールドとして指定した型の値を埋め込むことができます。
匿名フィールドは、以下のように、名前を持たず型情報だけを指定します。

```
type Image struct {
    image.Image
}
```

埋め込まれた構造体は、埋め込んだ型の持つメソッドや構造体の場合はフィールドをあたかも自分のメソッドやフィールドのように呼び出すことができます。

しかし、これはあくまで処理を埋込んだ値に委譲しているだけで、決して継承をしているわけではありません。埋込みでJavaに出てくるような型階層を作ろとしても失敗します。

なお、埋め込んだ値の持つメソッドもインタフェースを実装するためのメソッドリストとしてカウントします。
さらに、インタフェースも匿名フィールドとして埋め込むことができるため、上記の例だと`Image`構造体が`image.Image`インタフェースを実装していることになります。

匿名フィールドには、以下のように型名でアクセスできます。
当然ながら同じ型の値を2つ以上埋め込むことはできません。
フィールドであることには変わらないので、埋め込んだ値を後で変更することも可能です。

```
img := &Image{_img} // _imgはimage.Image型
img.Image = _img2   // _img2もimage.Image型
```