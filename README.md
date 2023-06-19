# Github Pages 使用指南
## fork 此仓库

更名为 “[github_user].github.io”，其中 [github_user] 就是你的 github 用户名；

这个仓库的名字 [github_user].github.io 就是访问网站的网址。

## 修改配置文件 _config.yml

网站设置
```yml
title   : 我的的个人博客
description: > # this means to ignore newlines until "Language & timezone"
  学习笔记 & 存档 & 想法
```
Github 仓库
``` yml
## => GitHub Repository (if the site is hosted by GitHub)
##############################
repository: github_user
repository_tree: master

```

个人信息
```yml
## => Author and Social
##############################
author:
  type      : # "person" (default), "organization"
  name      : 我的名字
  ...

```

Markdown Enhancements 全局设置：
- 用于渲染$\LaTeX$ 公式的 mathjax 渲染器
- mathjax_autoNumber 公式自动编号
- Mermaid 流程图
- Chart 交互表格
```yml
## => Markdown Enhancements
##############################
## Mathjax
mathjax: # false (default), true
mathjax_autoNumber: # false (default), true

## Mermaid
mermaid: # false (default), true

## Chart
chart: # true false (default), true
```

## 撰写博客

将 markdown 文档放置在 `_posts/` 文件夹，按照 `year-month-date-Tilte.md` 的格式命名，例如：

`/_posts/2023-05-10-Beat-Track-Algorithm.md`

并且在文档开头做以下声明 layout 和 title 
```markdown
---
layout: article
title: 节拍跟踪算法概述
---
```
如果需要覆盖全局设置也可以添加内容

```markdown
---
layout: article
title: 节拍跟踪算法概述
mathjax: true
mathjax_autoNumber: true
---
```

## 插入图片

用户图片存放在 `pictures/` 文件夹

插入图片必须采用绝对路径，例如

```
![](/pictures/example.jpg)
```

这样才能在 Github Pages 上正确显示。

## 使用 Google Drive 作为图床

打开 [Google Drive](https://drive.google.com/drive/)，登入账户。选择其中一张图片，右键，选择“共享”，把“常规访问权限”从“受限”改为“知道链接的任何人”，然后点“复制链接”，得到类似这样的链接

```url
https://drive.google.com/file/d/[CODE]/view?usp=sharing
```

复制 `[CODE]`, 把链接改为

```url
https://drive.google.com/uc?export=view&id=[CODE]
```

或者

```url

https://drive.google.com/uc?export=download&id=[CODE]
```

可以作为图片插入文档

![](https://drive.google.com/uc?export=view&id=1Ws4FvCSoNttkISGhz3Etrx3Q8jj7pxeY)

![](https://drive.google.com/uc?export=view&id=1E_ocZdPUoX1LBo7Fc1nT6hubdxloWhi1) 

## 插入 PDF

```markdown
<center><embed src="/assets/example.pdf" width="850" height="600" ></center>
```

<center><embed src="/assets/下肢外骨骼调研.pdf" width="850" height="600" ></center>

## 发布

Push 到 [github_user].github.io 仓库

等待几分钟 Github 自动编译即可。

## License
基于 [TeXt Theme](https://github.com/kitian616/jekyll-TeXt-theme) (MIT License)