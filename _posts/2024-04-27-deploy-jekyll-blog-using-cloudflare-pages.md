---
categories: [计算机网络, 建站]
date: 2024-04-27 14:00:00 +0800
last_modified_at: 2024-04-27 16:34:00 +0800
tags:
- Cloudflare
- Pages
- Jekyll
- Chirpy
- 搬瓦工
title: 使用 Cloudflare Pages 免费部署 Jekyll 博客
---

## 简介

此前个人博客站点和代理服务一直共用 443 端口，猜测是国内审查机构发现 443 端口不仅用于正常的 HTTPS 通信，而且被用来传输用于翻墙的数据，所以封锁或限制了该端口的流量。

![443 端口被墙](/img/image-20240427160327782.webp){: .shadow }

使用搬瓦工提供的工具检测如下：

![IP 被阻断](/img/image-20240427164741162.webp){: .shadow }

> 只能将科学上网工具从服务器上移除，静等 GFW 自行清除了😅。
{: .prompt-info }

之所以有本篇文章，是因为刚购买不到一个月的搬瓦工主机 443 端口被墙了，在不更换主机 IP 或新增主机的情况下，想到的可行方式有：

- 将博客站点部署到 Cloudflare Pages、GitHub Pages、Vercel 等 Serverless 平台。
- 使用 Cloudflare CDN 代理，绕过主机 443 端口无法访问的问题。

本文将一步一步指导你，使用 Cloudflare Pages 部署 Chirpy 主题的 Jekyll 博客。操作步骤如下：

1. 创建 GitHub 仓库
2. 创建 Cloudflare Pages
3. 配置环境变量
4. 绑定自定义域名

### 1.创建 GitHub 仓库

访问仓库地址，创建一个新仓库：

![创建新仓库](/img/image-20240427161459991.webp){: .shadow }

### 2.创建 Cloudflare Pages

在 Workers & Pages 创建一个 Pages 应用，连接上一步创建的新仓库。

### 3.配置环境变量

在未作任何配置的情况下，上一步将提示以下错误：

```
13:31:45.370	bundler: failed to load command: jekyll (/opt/buildhome/.asdf/installs/ruby/3.2.2/bin/jekyll)
13:31:45.371	/opt/buildhome/.asdf/installs/ruby/3.2.2/lib/ruby/3.2.0/bundler/definition.rb:524:in `materialize': Could not find html-proofer-4.4.3, nokogiri-1.16.4-x86_64-linux, parallel-1.24.0, rainbow-3.1.1, typhoeus-1.4.1, yell-2.2.2, zeitwerk-2.6.13, racc-1.7.3, ethon-0.16.0 in locally installed gems (Bundler::GemNotFound)
```

出现该问题的原因是 Cloudflare Pages 的构建系统中，设置了默认的环境变量 `BUNDLE_WITHOUT` 值，旨在通过避免安装不需要的 gem 来优化构建时间，致使其排除了 Jekyll 需要的 gem。解决办法很简单，就是给 `BUNDLE_WITHOUT` 添加一个默认的空字串值，如下：

| 变量名称         | 值   |
| ---------------- | ---- |
| `BUNDLE_WITHOUT` | `""` |

### 4.绑定自定义域名

切换到自定义域选项卡，添加自定义域名：

![image-20240427163304835](/img/image-20240427163304835.webp){: .shadow }
