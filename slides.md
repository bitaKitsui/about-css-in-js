---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# CSS in JS について

---

# 本日のひとこと

- ポケモンの環境おさらい
  - キノガッサ、ミミッキュ、ドラパルトなどの下馬評高めのポケモンが強烈メタと新ポケモンのスペックに押されてやや評価down
  - 新ポケモンで評価高め
    - キョジオーン（ゴースト or 水 or 飛行） しおづけと特性と数値
    - ラウドボーン（フェアリー）　フレアソングと特性と数値
    - コノヨザル（水 or 毒 or 飛行）　ふんどのこぶしと数値

---

# Agenda

- フロントエンドでの主な選択肢
- CSS in JS の説明
- 主なライブラリ紹介
- まとめ

---

# 主なスタイルの付け方

フロントエンド界隈（主に React ）では、どのようにスタイルを付けるかが悩みどころ

- 扱いやすさ（情報量、簡単、書き心地良し, etc... ）
- 人によって書き方がブレない

---

# そもそも何故必要？

- CSS がグローバルに適用されてしまうため
  - Vue であれば Scoped CSS があったり
- 「予測可能性」「再利用性」「保守性」「拡張性」とかを担保するため
  - BEM や SMACSS などの設計手法にも通ずる

---

# React は選択肢が多い？🤔

- `className` へ文字列を渡す
- インラインスタイル
- その他ライブラリ

---

# React は選択肢が多い？🤔

`className` へ文字列を渡す

名前被りに気を使う必要あり

```tsx
render() {
  return <span className="menu navigation-menu">Menu</span>
}
```

```tsx
render() {
  let className = 'menu';
  if (this.props.isActive) {
    className += ' menu-active';
  }
  return <span className={className}>Menu</span>
}
```

---

# React は選択肢が多い？🤔

インラインスタイル

※ 非推奨（毎回 css のオブジェクト生成、擬似要素などが使えない、etc...）

```tsx
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

---

# CSS Modules

https://github.com/css-modules/css-modules

- `.module.css` というファイルを用意し、それを読み込むことでスタイルを適用する
- 自動的にユニークなクラス名を作成することでコンポーネントに閉じることができる
- 衝突を気にすることなく、異なるファイルで同じ CSS クラス名を使用できる
- バンドル時に `css` ファイルが書き出される

---

# CSS Modules

- 現状の詳しい状況は Hatena のブログに記載あり
  - https://developer.hatenastaff.com/entry/2022/09/01/093000
  - 仕様が一つ、実装が複数

---

# Tailwind CSS

https://tailwindcss.com/

- ユーティリティーファーストを謳ったフレームワーク
- `flex pt-4 text-center` のような汎用クラスを当てていく

```html
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space-x-4">
  <div class="shrink-0">
    <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo">
  </div>
  <div>
    <div class="text-xl font-medium text-black">ChitChat</div>
    <p class="text-slate-500">You have a new message!</p>
  </div>
</div>
```

---

# CSS in JS とは？

- JavaScript を使って CSS を記述する技術
  - Linter が使えたり、コンパイル時にエラーが出たり
- 各コンポーネントに閉じた状態でスタイルを適用できる
- パフォーマンスに難あり
  - `js` ファイルが書き出され、 `css` 記述を解析し...

---

# styled-components

https://styled-components.com/

- React で幅広く使われている CSS in JS の先駆け的存在
- 後発のライブラリに機能面で劣っている部分もあったが今は追いついている
- `styled` API を使いスタイルを書いていく

---

# styled-components

```ts
const Button = styled.a`
  /* This renders the buttons above... Edit me! */
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;
  width: 11rem;
  background: transparent;
  color: white;
  border: 2px solid white;

  /* The GitHub button is a primary button
   * edit this to target it specifically! */
  ${props => props.primary && css`
    background: white;
    color: black;
  `}
`
```

```tsx
render(<Button>Click</Button>)
```

---

# styled-components

ぱっと見で `button` タグが使われてそうだけど、実際は `a` タグ🤔

```tsx
render(<Button>Click</Button>)
```

---

# Emotion

https://emotion.sh/docs/introduction

- `@emotion/css` と `@emotion/react` の二つがある
  - 前者はフレームワークに依存しない
- `React.createElement()` ではなく独自のコンパイラーを通している
  - `tsx` ファイルの上一行目に `/** @jsx jsx */` を書く必要あり
- `css` 属性に定義した CSS を渡してスタイルを適用する
  - `className` に文字列を渡す場合と比較して、間違いに気付きやすくなる
- styled-components より若干パフォーマンスが上？
  - https://techblog.zozo.com/entry/zozotown-css-in-js

---

# Emotion

```tsx
export const App = () => {
  return (
    <div css={wrapperStyle}>
      <p css={mainTextStyle}>重要なお知らせ</p>
      <span css={subTextStyle}>確認してください</span>
    </div>
  )
}
```

---

# ゼロランタイムとは？

- JavaScript で CSS を記述する技術
- 出力を `.css` ファイルにすることでブラウザ側での負荷を軽減することを目的としている

---

# Linaria

https://linaria.dev/

- ゼロランタイム CSS in JS
- `styled` と `css` という二つの API でスタイルを付けられる

---

# Linaria

`css`

テンプレートリテラル記法 -> 一意のクラス名へ

```ts
import { css } from '@linaria/core';

const flower = css`
  display: inline;
  color: violet;
`;

// flower === flower__9o5awv –> with babel plugin
```

---

# Linaria

`styled`

動的な値を渡すことができる

```ts
import { styled } from '@linaria/react';
import colors from './colors.json';

const Container = styled.div`
  background-color: ${colors.background};
  color: ${(props) => props.color};
  width: ${100 / 3}%;
  border: 1px solid red;

  &:hover {
    border-color: blue;
  }
`;
```

---

# Linaria

`css` で動的に値を渡すには CSS Custom Properties を使うことに

```ts
const textCss = css`
  font-size: var(--font-size);
`;

export const Post = ({ title, body }) => {
  return (
    <article>
      <h1 style={{ "--font-size": fontSizeMap["h1"] }} className={textCss}>{title}</h1>
      <div style={{ "--font-size": fontSizeMap["body2"] }} className={textCss}>{body}</div>
    </article>
  );
}
```

---

# vanilla-extract

https://vanilla-extract.style/

- ゼロランタイム CSS in TS
- TypeScript で型安全に書ける
- `.css.ts` というファイルに書いてスタイルを適用する
  - TypeScript 版の CSS Modules のような立ち位置
  - Vite などのバンドラーが `.css` ファイルを出力してくれる

---

# vanilla-extract

`className` にスタイルを渡す

```ts
// MyComponent.css.ts
const COLOR = 'blue'

export const MyComponent = style({
	color: 'red',
	':hover': {
		color: COLOR
	}
})
```

```tsx
import { MyComponent } from './MyComponent.css'

render(
	<div className={MyComponent}>Hover</div>
)
```

---

# vanilla-extract

- オブジェクト記法しか対応していない
  - デザイナーがCSSを触る案件などが懸念点
  - プロパティがケバブケースになってしまうが、型定義はされている
  - 存在しないプロパティを書いた際はコンパイル時点でエラーになる
- `calc` に型定義がされている
  - https://vanilla-extract.style/documentation/packages/css-utils/

```ts
import { calc } from '@vanilla-extract/css-utils';

const styles = {
  height: calc.multiply('var(--grid-unit)', 2)
};
```

---

# Stitches

https://stitches.dev/

- CSS in TS (near zero runtime)
- デザインシステムを構築する際に使えるライブラリ
- オブジェクト記法のみサポート

---

# Stitches

`variants` というプロパティでスタイルの分岐ができる

```ts
const Button = styled('button', {
  variants: {
    color: {
      primary: {
          backGroundColor: 'blue'
      },
      error: {
          backGroundColor: 'red'
      }
    }
  }
})

() => (
	<Button color="primary">button</Button>
)
```

---

# Stitches

`config.ts` にブレークポイントを定義しグローバル管理もできる

プロジェクト全体で利用する値を管理するのが楽にできそう🤔

```ts
// stiches.config.ts
import { createStitches } from '@stitches/react'

export const { config } = createStitches({
  media: {
    bp1: '(min-width: 480px)'
  },
  utils: {
    marginX: (value) => ({ marginLeft: value, marginRight: value }),
  },
})
```

---

# Stitches

レスポンシブ対応の例

```tsx
const Button = styled('button', {
  // base styles

  variants: {
    color: {
      violet: {},
      gray: {},
    },
  },
});

() => (
  <Button
    color={{
      '@initial': 'gray',
      '@bp2': 'violet'
    }}
  >
    Button
  </Button>
);
```

---

# Vue は？

- 標準で Scoped CSS がサポートされている
- `v-bind()` 関数で動的に値を渡すこともできる

```vue
<script setup>
const theme = {
  color: 'red'
}
</script>

<template>
  <p>hello</p>
</template>

<style scoped>
p {
  color: v-bind('theme.color');
}
</style>
```

---

# pinceau

https://github.com/Tahul/pinceau

- ゼロランタイム CSS in TS
- JS の出力は 0kb
- `css()` という API の中でオブジェクト記法を使いスタイルを当てていく

```vue
<style scoped lang="ts">
css({
  div: {
    color: '{color.primary.900}',
    backgroundColor: '{color.orange.50}'
  }
})
</style>
```

---

# pinceau

- 絶賛開発中とのこと
  - `styled-dictionary-esm`, `@stitches/stringify` に依存している部分もある
  - Stitches のように `utils` プロパティに使いまわせる式を書けたりできるようになる予定
  - Style Dictionary の `transforms`, `formats`, `actions` も使えるようになる予定

---

# まとめ

- パフォーマンス面、メンテナンス状況、書き心地など様々な面でメリット/デメリットあり
- 個人的には Stitches が面白そうだなと思いました
- 使った結果もアウトプットしたいです