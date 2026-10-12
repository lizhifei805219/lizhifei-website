# 邵飞商贸官网（lizhifei.com.cn）维护说明

本目录就是网站线上运行的**全部源码**（静态站，HTML + 图片 + 视频）。
2026-10-12 已完成 SEO 修复（301 重定向 / canonical / robots.txt / sitemap.xml / JSON-LD 域名），并部署到 Netlify。

---

## 一、关键信息（照抄即可）

- **线上域名**：https://lizhifei.com.cn
- **Netlify 站点 ID**：`f0e8ea64-9a7d-49f4-87fe-8abe256e110b`
- **Netlify 站点名**：resonant-licorice-dca3dd
- **部署方式**：当前为 CLI 手动部署；目标改为 Git 仓库自动部署（见第三节）。

---

## 二、以后改网站，改哪些文件？

| 想改的东西 | 改哪个文件 |
|---|---|
| 首页内容（文案/JSON-LD/图） | `index.html` |
| 6 个产品页内容 | `light-calcium-carbonate-for-*.html` |
| 新增/删除页面 | 增删对应 `.html`，并同步改 `sitemap.xml` |
| 301 重定向规则 | `_redirects`（每行一条，末尾加 `!` 强制） |
| 收录入口 | `robots.txt` + `sitemap.xml` |
| 图片 / 视频 | `images/`、`videos/` 目录 |

> ⚠️ 重要：改完**务必顺手更新 `sitemap.xml`**（新增页就加一行 `<url>`），否则搜索引擎发现不了。
> ⚠️ 每个产品页的 `<link rel="canonical">` 一定要指向**不带 .html 的干净地址**，否则重复内容会复发。

---

## 三、接入 Git（推荐，长期最稳）—— 你需要做 3 步

### 第 1 步：在 GitHub 建一个空仓库
1. 打开 https://github.com → 右上角 “+” → New repository
2. 仓库名随便起，比如 `lizhifei-website`
3. **不要**勾选 “Add a README”（保持空仓库）→ Create repository
4. 建好后，GitHub 会显示一串地址，形如：
   `https://github.com/你的用户名/lizhifei-website.git`
   复制它（下面第 3 步要用）。

### 第 2 步：把本仓库推上去（在本地终端跑）
```bash
cd "D:\word\邵飞商贸\SEO审计\site_deploy"
git remote add origin https://github.com/你的用户名/lizhifei-website.git
git branch -M main
git push -u origin main
```
> 第一次 push 会弹窗要你登录 GitHub（或填 token），按提示登一下即可。

### 第 3 步：Netlify 连 GitHub（以后自动部署）
1. 打开 https://app.netlify.com → 进入站点 `resonant-licorice-dca3dd`
2. Site settings → Build & deploy → 找到 “Connect to Git provider” → 选 GitHub → 授权 → 选刚才那个仓库
3. 在部署设置里确认：
   - **Build command**：留空（这是纯静态站，不需要构建）
   - **Publish directory**：`.`（点号，表示仓库根目录就是发布内容）
4. 保存。以后**改完文件 → `git push` → Netlify 自动上线**，不用再手动跑命令。

---

## 四、兜底：万一没连 Git，用 CLI 手动部署

如果暂时没接 GitHub，改完文件后在这目录跑一条命令也能上线：
```bash
netlify deploy --prod --site f0e8ea64-9a7d-49f4-87fe-8abe256e110b --dir "D:\word\邵飞商贸\SEO审计\site_deploy"
```
（Netlify CLI 已在本机登录 lzhifei352@gmail.com，可直接用。）

---

## 五、验证上线是否成功（每次部署后看一眼）

- 访问 `https://lizhifei.com.cn/robots.txt` 和 `/sitemap.xml` 能打开
- 访问任意 `https://lizhifei.com.cn/xxx.html` 应 **301** 跳到不带 `.html` 的地址
- 首页 view-source 搜 `netlify.app` 应为 **0 次**
