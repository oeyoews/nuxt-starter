---
title: "再探 Nuxt3"
date: 2024/3/1 11:30:28
---

## 接触 Nuxt3

* 首次接触 Nuxt3 的第一个障碍就是网络问题。nuxt cli 使用 tar.gz 的模版地址，其实模版没有什么东西。最直接的解决办法就是手动下载那个 tar.gz, 或者手动安装 nuxt npm package, 然后复制一些配置文件 就行了.

* 看了一些 nuxt 的文档后就没有深入使用了.

## 再次接触 Nuxt3

* 最近在使用 nuxt3 将 Next.js 的 blog 重写一下.

## 为什么不直接用 vue 呢

blog 为了 seo, 必须要使用 ssr, nuxt 在 vue 的基础上对 ssr 有很好的支持

## Nuxt 环境变量

* 虽然已经有了 Next.js 的环境变量的使用基础. 但是 nuxt3 使用环境变量的方法又稍微不同, 网上的配置环境变量的方法有很多种，但是有很多局限，甚至限制了 nuxt 版本. 以下是官方文档推荐使用的方法 useRuntimeConfig (nuxt 3.10.x)

nuxt 的的服务端环境变量都要写在 config 里面，默认值可以为空，运行时会被 .env 的 NUXT_xxx 的环境变量覆盖 xxx 的真实环境变量. 部署环境下则需要在后台将.env 的变量填写到后台的系统环境变量中.

客户端环境变量 同理，只不过是被 Nuxt_PUBLIC_xxx 重写

以上只是配置变量，使用上也不是使用传统的 process.env，而是使用 useRuntimeconfig 来访问环境变量

```ts
// config
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  runtimeConfig: {
    GITHUB_TOKEN: '', // server env
    public: {
      apiBase: '/api', // client env
    },
  },
});

// read env

const env = useRuntimeConfig();
console.log(env.GITHUB_TOKEN)
```