# ivpod.pages.dev · 面试 ASR 助手知识点精讲（Cloudflare Pages 站点）

这是 [`https://ivpod.pages.dev`](https://ivpod.pages.dev) 的**发布产物仓库**，
由本机流水线生成，**不要手工改这里的文件**。

## 它是什么

- 10 期「知识点精讲」播客的单文件播放页（e01–e10）+ 索引页 + 字幕 + 音频 + 配乐
- 播放器增强：倍速、上下期页内接续、播完后三选一、敲敲鱼配乐三选一、Media Session
- **音频双源**：主源指向 Cloudflare R2（支持 Range，可拖进度）；主源失败或 8 秒不出元数据时，
  自动切同目录的 mp3 副本（Pages 不支持 Range，兜底源不能拖进度）

## 谁生成它

```
podcast/site/                      # 真源（R2 站与 Pages 站共用同一份页面）
  ↓ tools/podcast_render.py --all
  ↓ tools/podcast_pages.py         # 打包 + 自检
podcast/_site-pages/               # ← 本仓库内容
  ↓ wrangler pages deploy podcast/_site-pages --project-name ivpod --branch main
https://ivpod.pages.dev
```

发布命令与已知限制见项目内 `podcast/发布说明.md`；微信实测清单也在那里。

## 为什么要有个仓库

Cloudflare Pages 现在用的是 **Direct Upload**（每次发版跑一次 `wrangler pages deploy`），
不依赖 Git。但留一个镜像仓库有两个用处：内容有异地备份；将来想改成 Git 集成自动部署，
直接在这个仓库上开就行，不用重新准备产物。

## 注意

- 音频是二进制且有 27MB，每次内容更新都会让仓库变大；如果将来要长期用 Git 集成，
  建议改成只版本化 HTML/JSON，音频继续走 R2 + Pages 直传。
- 站点内容只含播客成品，不含任何面试记录、简历或密钥。
