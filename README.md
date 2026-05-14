# d3-cloud-es

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

An ES module fork of the popular [d3-cloud](https://github.com/jasondavies/d3-cloud) library by [Jason Davies](https://www.jasondavies.com/). This module generates word clouds in JavaScript, using HTML5 canvas and sprite masks for high-performance collision detection.

The layout algorithm is based on a paper by [Jonathan Feinberg](http://static.mrfeinberg.com/bv_ch03.pdf).

## Demo

View the [live examples](https://code4fukui.github.io/d3-cloud-es/examples/).

## Features

- Asynchronous layout algorithm that does not block the UI.
- High performance via sprite-based collision detection.
- Highly customizable word attributes: font, size, rotation, and padding.
- Supports both browser and Node.js environments.
- Events for tracking word placement and layout completion.

## Installation

In a browser, you can load the module directly from a CDN like Skypack:

```html
<script type="module">
  import { cloud } from "https://cdn.skypack.dev/d3-cloud-es";
  // ... your code here
</script>
```

For server-side use in Node.js, you will also need to install a canvas implementation, such as `canvas`.

```bash
npm install d3-cloud-es canvas
```

## Usage

### Browser

This example fetches a list of words and renders the resulting cloud as an SVG using D3.

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