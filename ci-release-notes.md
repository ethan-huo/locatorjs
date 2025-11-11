# LocatorJS Extension Release Checklist

## 1. 安装 + 依赖构建
```
pnpm install

# 这些顺序跑完 extension 才编译得过
pnpm --filter @locator/shared build
pnpm --filter @locator/runtime build
pnpm --filter @locator/babel-jsx build
pnpm --filter @locator/react-devtools-hook exec -- tsc --outDir dist --declaration --noEmit false
```
> 最后一条必须加 `--noEmit false`，否则 `@locator/react-devtools-hook/dist` 不会生成，webpack 会报 “Can't resolve '@locator/react-devtools-hook'”。

## 2. 构建扩展
```
pnpm --filter locatorjs-extension build
pnpm --filter locatorjs-extension pack:chrome
```
- Unpacked 目录：`apps/extension/build/production_chrome`
- Zip：`apps/extension/build/chrome.zip`（Release 附件直接用这个）

## 3. 版本与源码
- 当前改动涉及：
  - `apps/extension/src/pages/Content/index.ts`
  - `packages/shared/src/sharedOptionsStore.ts`
  - `packages/runtime/src/components/LinkOptions.tsx`
- 这几个文件必须用最新 commit；否则修复自定义链接丢失的逻辑缺失。
- 如需 bump 版本，改 `apps/extension/package.json`，manifest 会自动跟随。

## 4. Release 产物
- GitHub Release 上传 `apps/extension/build/chrome.zip`
- 如需提供 unpacked 版本，可再 `zip/tar` 一份 `production_chrome`

按上面流程跑，CI 就不会因为 workspace 包没 build 好而失败。
