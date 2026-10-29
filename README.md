# 个人主页 · Mingyu Zhou（周明彧）

给 **OpenReview 注册**用的单页学术主页。纯静态、无外部依赖（不引 Google Fonts / CDN，国内直接打开也不卡），一个 `index.html` 就是全部。

| 文件 | 作用 |
|---|---|
| `index.html` | 主页本体（样式内嵌，改一处即可） |
| `favicon.svg` | 标签页图标（MZ 字母标） |
| `README.md` | 本说明 |

---

## 一、上线：GitHub Pages（约 3 分钟，不用装 git）

1. 登录 GitHub 账号 **zmy1234567890** → 右上角 **+ → New repository**
2. 仓库名**必须**是 `zmy1234567890.github.io`（`<用户名>.github.io` 才会挂在根路径），选 **Public**，不要勾 "Add a README"
3. 进入新仓库 → **Add file → Upload files** → 把 `index.html`、`favicon.svg` 拖进去 → **Commit changes**
4. **Settings → Pages → Build and deployment**：Source 选 `Deploy from a branch`，Branch 选 `main` + `/ (root)` → **Save**
5. 等 1–2 分钟，访问 **https://zmy1234567890.github.io/** 确认能打开
6. OpenReview 注册页的 **Homepage** 字段填这个地址

> 仓库名用别的也行（站点会变成 `https://zmy1234567890.github.io/仓库名/`），但要记得同步改下面第二节第 1 条里的三处 URL。

### 备选部署（国内打开更快）

- **Cloudflare Pages**：dash.cloudflare.com → Workers & Pages → 直接拖拽这个文件夹上传，免费，给 `xxx.pages.dev` 域名
- **Netlify / Vercel**：同样支持拖拽整个文件夹

⚠️ 无论用哪个：**链接必须是公开可访问的**。语雀 / 飞书 / Notion 私有页、需要登录的链接，OpenReview 审核打开会失败。

---

## 二、上线前要改的 3 处

1. **域名**（走 GitHub Pages 则不用改）
   全文搜索 `https://zmy1234567890.github.io/`，共 3 处：`<link rel="canonical">`、`og:url`、JSON-LD 里的 `"url"`。换了部署方式就一起换。

2. **头像**（可选，但审核时真人照片更容易确认是本人）
   现在是 "MZ" 字母圆形。放照片的做法：
   - 新建 `assets/photo.jpg`（正方形，建议 400×400 以上，正面、光线均匀）
   - 把 `<div class="avatar" aria-hidden="true">MZ</div>` 整行替换为
     `<img class="avatar" src="assets/photo.jpg" alt="Portrait of Mingyu Zhou" />`

3. **个人信息核对**
   姓名拼写、邮箱、学院/专业、入学年份 —— OpenReview 会拿这些跟注册资料比对，**写错比不写更麻烦**。

---

## 三、OpenReview 注册要点（对着填）

| 字段 | 值 | 说明 |
|---|---|---|
| First name | `Mingyu` | 与 TGRS 论文署名一致（论文里是 `Mingyu Zhou`） |
| Last name | `Zhou` |  |
| Alternate name | `周明彧` | Profile 里有 "Alternate names"，加上便于中文场景匹配 |
| Email | `2024010904031@std.uestc.edu.cn` | **优先学校邮箱**；机构域名若已在 OpenReview 库中会走机构验证 |
| Backup email | `272401137@qq.com` | QQ 属公共域名，会被标 unverified，需人工审核，通常等几天 |
| Homepage | `https://zmy1234567890.github.io/` | 本页面，**必须能打开** |
| Institution | University of Electronic Science and Technology of China | Domain: `uestc.edu.cn` |
| Department | School of Information and Communication Engineering |  |

**加分项：Publications → 用 DOI 添加 `10.1109/TGRS.2026.3701189`**
OpenReview 会自动抓取题录，`Mingyu Zhou` 落在作者第 2 位 —— 这是最难伪造的核验材料，比主页本身更有说服力。加完后主页上的 `[J1]` 与 profile 里的条目就对齐了。

> 新人注册要走人工审核（几天是常态）。**先把主页跑起来、再加 DOI**，别等审核被退回才补。

---

## 四、以后改内容去哪找

在 `index.html` 里搜这些关键词：

| 想改 | 搜索 |
|---|---|
| 新闻 | `id="news"` |
| 研究项目（两段） | `id="research"` |
| 论文列表 | `id="publications"` |
| 开源 / 项目卡片 | `id="open-source"` |
| 学历 | `id="education"` |
| 联系方式 | `id="contact"` |
| 个人简介 | `id="about"` |
| 研究方向关键词 | `Research interests:` |
| 颜色主色 | `--accent:#0e5a68`（深色模式在下面几行的 `@media (prefers-color-scheme: dark)` 里） |

改完直接覆盖仓库里的 `index.html`，Pages 会自动重新部署（约 1 分钟）。

---

## 五、写作口径提醒（双盲）

- `[M1]` 目前只写 "under double-blind review"，**没有写标题** —— 保 ICME 的匿名。
- 注意：HF 数据集卡（`_release/hf_README.md`）的"引用"一节其实已经把标题公开写了。真要严格匿名，那边也该改。
- 主页上的所有数字都来自本项目实测档（+3–4 dB 区域项、0.27 M / 1.51 GFLOPs 等）。**改数字前回服务器 `eval/` 核对**，不要凭记忆写。

---

## 六、设计说明

- 单文件、零 JS、零外部字体/CDN：断网、国内网络都能完整渲染
- 自动深色模式（跟随系统）、移动端自适应、`Ctrl+P` 可直接打印成 PDF 版简介
- 内嵌 `schema.org/Person` 结构化数据（姓名 / 中文名 / 单位 / 邮箱 / GitHub / HF 互相 `sameAs` 关联），搜索引擎与身份核验更容易把"这个人 = 这个单位 = 这个主页"对上
- 语义化标签 + 跳转链接 + 焦点样式，屏读器可用

---

*最后更新：2026-10-29*
