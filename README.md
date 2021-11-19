# nextjs-sb-demo
next.js + storybook + antd 构建的 React 应用模板

### 创建 Next.js 应用

```base
npx create-next-app my-app

cd my-app

yarn dev
```

### 安装 Storybook

```base
npx sb init --builder webpack5

yarn storybook
```

> `--builder webpack5`，由于当前版本的 `Next.js` 是基于 `webpack5` 构建的，所以在安装 Storybook 的时候要指定 webpack 版本。

### 配置全局样式

```base
// .storybook/preview.js

+ import '../styles/globals.css';

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
}

```

### 配置图片加载

```base
// .storybook/preview.js 在 Storybook 中为 Next.js Images 使用未优化的 prop

+ import * as NextImage from "next/image";

+ const OriginalNextImage = NextImage.default;

+ Object.defineProperty(NextImage, "default", {
  configurable: true,
  value: (props) => (
    <OriginalNextImage
      {...props}
      unoptimized
    />
  ),
});
```

```base
// package.json 在 Storybook 中提供公共目录

{
  ...
  
  "scripts": {
    - "storybook": "start-storybook -p 6006",
    - "build-storybook": "build-storybook"
    + "storybook": "start-storybook -p 6006 -s ./public",
    + "build-storybook": "build-storybook -s public"
  },
  
 ...

}
```

> [Use the Next.js Image Component in Storybook](https://dev.to/jonasmerlin/how-to-use-the-next-js-image-component-in-storybook-1415)

### Storybook 与 [Mock Service Worker](https://mswjs.io/) 集成


### 安装 antd 主题

```base
yarn add antd
yarn add -D storybook-addon-customize-antd-theme

// .storybook/main.js
module.exports = {
  stories: ['storybook-addon-customize-antd-theme/dist/esm/stories/index.js'],
  addons: ['storybook-addon-customize-antd-theme'],
};

// .storybook/preview.js
export const parameters = {
  customizeAntdTheme: {
    modifyVars: {
      'primary-color': '#ff1771',
      'border-radius-base': '20px',
    },
  },
};
```

> [Storybook 插件可直观地自定义蚂蚁设计主题](https://storybook.js.org/addons/storybook-addon-customize-antd-theme/)


> [Next.js with Storybook 视频](https://www.youtube.com/watch?v=i5tvZ9f7gJw)

> [Next.js with Storybook 文档](https://storybook.js.org/blog/get-started-with-storybook-and-next-js/)
