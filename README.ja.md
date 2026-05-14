# d3-cloud-es

[Jason Davies](https://www.jasondavies.com/)による人気ライブラリ[d3-cloud](https://github.com/jasondavies/d3-cloud)のESモジュールフォークです。このモジュールは、HTML5 Canvasとスプライトマスクを使用した高性能な衝突判定により、JavaScriptでワードクラウドを生成します。

レイアウトアルゴリズムは、[Jonathan Feinberg](http://static.mrfeinberg.com/bv_ch03.pdf)の論文に基づいています。

## デモ

[ライブデモ](https://code4fukui.github.io/d3-cloud-es/examples/)をご覧ください。

## 特徴

- UIをブロックしない非同期レイアウトアルゴリズム。
- スプライトベースの衝突判定による高いパフォーマンス。
- フォント、サイズ、回転、パディングなど、単語の属性を高度にカスタマイズ可能。
- ブラウザとNode.js環境の両方をサポート。
- 単語の配置やレイアウトの完了をトラッキングするためのイベント。

## インストール

ブラウザでは、SkypackなどのCDNから直接モジュールを読み込むことができます。

```html
<script type="module">
  import { cloud } from "https://cdn.skypack.dev/d3-cloud-es";
  // ... your code here
</script>
```

Node.jsでのサーバーサイド利用には、`canvas`などのCanvas実装もインストールする必要があります。

```bash
npm install d3-cloud-es canvas
```

## 使い方

### ブラウザ

この例では、単語のリストを取得し、D3を使用して生成されたクラウドをSVGとしてレンダリングします。

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.skypack.dev/d3@7"></script>
  <script type="module">
    import { cloud } from "https://cdn.skypack.dev/d3-cloud-es";

    const words = ["Hello", "world", "normally", "you", "want", "more", "words", "than", "this"]
      .map(d => ({text: d, size: 10 + Math.random() * 90}));

    const layout = cloud()
      .size([500, 500])
      .words(words)
      .padding(5)
      .rotate(() => Math.floor(Math.random() * 2) * 90)
      .font("Impact")
      .fontSize(d => d.size)
      .on("end", draw);

    layout.start();

    function draw(words) {
      d3.select("body").append("svg")
          .attr("width", layout.size()[0])
          .attr("height", layout.size()[1])
        .append("g")
          .attr("transform", `translate(${layout.size()[0] / 2},${layout.size()[1] / 2})`)
        .selectAll("text")
          .data(words)
        .enter().append("text")
          .style("font-size", d => `${d.size}px`)
          .style("font-family", "Impact")
          .attr("text-anchor", "middle")
          .attr("transform", d => `translate(${[d.x, d.y]})rotate(${d.rotate})`)
          .text(d => d.text);
```
