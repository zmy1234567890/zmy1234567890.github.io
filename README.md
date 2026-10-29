# 个人主页 · Mingyu Zhou（周明彧）

**线上地址：https://zmy1234567890.github.io/** ← OpenReview 注册的 Homepage 就填这个。

纯静态、无外部依赖（不引 Google Fonts / CDN，国内直接打开也不卡），一个 `index.html` 就是全部。

| 文件 | 作用 |
|---|---|
| `index.html` | 主页本体（样式内嵌，改一处即可） |
| `favicon.svg` | 标签页图标（MZ 字母标） |
| `.github/workflows/pages.yml` | 自动部署：push 到 `main` 就发布，无需手动操作 |
| `preview.png` / `preview_live.png` | 本地截图，**已 gitignore，不发布** |
| `README.md` | 本说明 |

---

## 一、部署状态（2026-10-29 已完成）

| 项 | 值 |
|---|---|
| 远端仓库 | `git@github.com:zmy1234567890/zmy1234567890.github.io.git`（public） |
| 默认分支 | `main` |
| 部署方式 | **GitHub Actions**（`.github/workflows/pages.yml`），不是传统的"分支发布" |
| 认证方式 | SSH 密钥（`~/.ssh/id_ed25519`），这台机器已可免密 push |
| 本地路径 | `D:\c_course\personal-site`（已从 `paper-writing\personal-site` 移出，避免嵌在论文仓库里） |

> 为什么用 Actions 而不是「Settings → Pages → Deploy from a branch」：
> 这个仓库的 Pages 站点一开始没启用，`configure-pages` 带 `enablement: true` 让工作流自己把站点建了起来。
> 好处是以后**只要 push 就自动发布**，不用再进设置页。

---

## 二、以后改内容怎么发布

```powershell
cd D:\c_course\personal-site
# 改 index.html ……
git add -A
git commit -m "更新 About"
git push
```

push 后 Actions 自动跑（约 40–60 秒），进度看：
`https://github.com/zmy1234567890/zmy1234567890.github.io/actions`

刷新 https://zmy1234567890.github.io/ 即可（CDN 有几十秒缓存，属正常）。

不想用命令行也行：GitHub 网页上仓库 → `index.html` → 铅笔图标直接改 → Commit，同样会触发部署。

---

## 三、要改的地方

1. **头像**（可选，但审核时真人照片更容易确认是本人）
   现在是 "MZ" 字母圆形。放照片：
   - 新建 `assets/photo.jpg`（正方形，建议 400×400 以上，正面、光线均匀）
   - 把 `<div class="avatar" aria-hidden="true">MZ</div>` 整行替换为
     `<img class="avatar" src="assets/photo.jpg" alt="Portrait of Mingyu Zhou" />`

2. **个人信息核对**
   姓名拼写、邮箱、学院/专业、入学年份 —— OpenReview 会拿这些跟注册资料比对，**写错比不写更麻烦**。

3. **换域名时**（只有换域名才需要）
   全文搜索 `https://zmy1234567890.github.io/`，共 3 处：`<link rel="canonical">`、`og:url`、JSON-LD 的 `"url"`。

---

## 四、OpenReview 注册要点（对着填）

| 字段 | 值 | 说明 |
|---|---|---|
| First name | `Mingyu` | 与 TGRS 论文署名一致（论文里是 `Mingyu Zhou`） |
| Last name | `Zhou` |  |
| Alternate name | `周明彧` | Profile 里有 "Alternate names"，加上便于中文场景匹配 |
| Email | `2024010904031@std.uestc.edu.cn` | **优先学校邮箱**；机构域名若已在 OpenReview 库中会走机构验证 |
| Backup email | `272401137@qq.com` | QQ 属公共域名，会被标 unverified，需人工审核，通常等几天 |
| Homepage | `https://zmy1234567890.github.io/` | 已上线，能直接打开 |
| Institution | University of Electronic Science and Technology of China | Domain: `uestc.edu.cn` |
| Department | School of Information and Communication Engineering |  |

**加分项：Publications → 用 DOI 添加 `10.1109/TGRS.2026.3701189`**
OpenReview 会自动抓取题录，`Mingyu Zhou` 落在作者第 2 位 —— 这是最难伪造的核验材料，比主页本身更有说服力。加完后主页上的 `[J1]` 与 profile 里的条目就对齐了。

> 新人注册要走人工审核（几天是常态）。主页已经能打开了，DOI 补上就齐了。

---

## 五、页面内容在哪（搜索这些关键词）

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
| 主色 | `--accent:#0e5a68`（深色模式在下方 `@media (prefers-color-scheme: dark)` 里） |

---

## 六、写作口径提醒（双盲）

- `[M1]` 目前只写 "under double-blind review"，**没有写标题** —— 保 ICME 的匿名。
- 注意：HF 数据集卡（`reflection_removal/_release/hf_README.md`）的"引用"一节其实已经把标题公开写了。真要严格匿名，那边也该改。
- 主页上的数字都来自本项目实测档（+3–4 dB 区域项、0.27 M / 1.51 GFLOPs 等）。**改数字前回服务器 `eval/` 核对**，不要凭记忆写。

---

## 七、设计说明

- 单文件、零 JS、零外部字体/CDN：断网、国内网络都能完整渲染
- 自动深色模式（跟随系统）、移动端自适应（390 px 实测无横向溢出）、`Ctrl+P` 可直接打印成 PDF 版简介
- 内嵌 `schema.org/Person` 结构化数据（姓名 / 中文名 / 单位 / 邮箱 / GitHub / HF 互相 `sameAs` 关联），搜索引擎与身份核验更容易把"这个人 = 这个单位 = 这个主页"对上
- 语义化标签 + 跳转链接 + 焦点样式，屏读器可用

---

*最后更新：2026-10-29（已部署上线）*
