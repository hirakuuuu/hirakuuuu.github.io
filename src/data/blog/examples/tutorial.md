---
title: 数式環境の導入について
author: Hiraku Morimoto
pubDatetime: 2025-06-29T12:00:00Z
slug: tutorial
featured: false
draft: false
tags:
  - markdown
description:
  テンプレートにあった数式環境の導入方法の記事を日本語訳したものです．
---


astro paper に数式環境を導入する方法のメモです（テンプレートにあった英語の記事を日本語訳したものです）


1. 必要なプラグインのインストール:

   ```bash
   npm install rehype-katex remark-math katex
   ```

2. プラグインに関する設定の変更:

   ```ts file=astro.config.ts
   // ...
   import remarkMath from "remark-math";
   import rehypeKatex from "rehype-katex";

   export default defineConfig({
     // ...
     markdown: {
       remarkPlugins: [
         remarkMath, // [!code ++]
         remarkToc,
         [remarkCollapse, { test: "Table of contents" }],
       ],
       rehypePlugins: [rehypeKatex], // [!code ++]
       shikiConfig: {
         // For more themes, visit https://shiki.style/themes
         themes: { light: "min-light", dark: "night-owl" },
         wrap: false,
       },
     },
     // ...
   });
   ```

3. katex の CSS を読み込む記述を追加: 

   ```astro file=src/layouts/Layout.astro
   ---
   import { SITE } from "@config";

   // astro code
   ---

   <!doctype html>
   <!-- others... -->
   <script is:inline src="/toggle-theme.js"></script>

   <!-- [!code highlight:4] -->
   <link
     rel="stylesheet"
     href="https://cdn.jsdelivr.net/npm/katex@0.15.2/dist/katex.min.css"
   />

   <body>
     <slot />
   </body>
   ```

4. katex の text-color の設定: 

   ```css file=src/styles/typography.css
   @plugin '@tailwindcss/typography';

   @layer base {
     /* other classes */

     /* Katex text color */
     /* [!code highlight:3] */
     .katex-display {
       @apply text-foreground;
     }

     /* ===== Code Blocks & Syntax Highlighting ===== */
     /* other classes */
   }
   ```

こうすると `$...$`，`$$...$$` で囲んだときに数式として表示される．

こんな感じ

$X$

$$ \int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi} $$

$$
\begin{aligned}
\nabla \cdot \mathbf{E} &= \frac{\rho}{\varepsilon_0} \\
\nabla \cdot \mathbf{B} &= 0 \\
\nabla \times \mathbf{E} &= -\frac{\partial \mathbf{B}}{\partial t} \\
\nabla \times \mathbf{B} &= \mu_0\left(\mathbf{J} + \varepsilon_0 \frac{\partial \mathbf{E}}{\partial t}\right)
\end{aligned}
$$

