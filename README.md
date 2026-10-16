# Muse 邀请码接力池 · 部署说明

产物：`index.html` 单文件（样式与脚本全部内联，无后端、无依赖），直接用浏览器打开就能跑。

机制：「拿一个码、留一个码」。站长码 **RXQT1I** 置顶首位默认展示，池子初始只内置这一个码（未编造其他码）。
留码与取用计数全部走浏览器 `localStorage`，页面多处已注明「演示版：数据只存本机浏览器，正式版接共享池」——不假装有后端统计。

## 路线一：GitHub Pages

1. 在 GitHub 新建一个公开仓库（例如 `muse-relay-pool`），把 `index.html` 传到仓库根目录并 push。
2. 打开仓库 → Settings → Pages。
3. Source 选 `Deploy from a branch`，Branch 选 `main`（或 master），目录选 `/ (root)`，点 Save。
4. 等 1～2 分钟，页面会给出地址：`https://<你的用户名>.github.io/muse-relay-pool/`。
5. 以后改文案：本地改完 `index.html` 再 push，Pages 自动重新发布。

注意：公开仓库意味着页面源码（含默认展示的站长码）任何人可见——这是接力池本来就要的效果。

## 路线二：Cloudflare Pages

A. 直接上传（不用 Git）：
1. 登录 Cloudflare Dashboard → 左侧 Workers & Pages → Create → Pages → **Upload assets**。
2. 项目名自定（如 `muse-relay`），把 `index.html` 所在文件夹整个拖进去上传（确保根目录有 index.html），点 Deploy。
3. 部署完成即得 `https://muse-relay.pages.dev` 形地址；可在 Custom domains 里绑自己的域名。

B. 接 Git 仓库（推荐长期维护）：
1. 同路线一把 `index.html` 推到 GitHub 仓库。
2. Cloudflare Pages → Create → **Connect to Git**，授权并选中该仓库。
3. Build settings：Framework preset 选 None，Build command 留空，Build output directory 填 `/`，点 Save and Deploy。
4. 之后每次 push 自动重新部署。

## 正式版接共享池时的改动点（备忘）

- 把 `load()/save()` 的 localStorage 换成后端 API（GET 取池、POST 留码、POST 取用计数+1）。
- 取用计数改为服务端原子递增；「剩余次数」由后端维护真实值。
- 其余展示逻辑（置顶站长码、按剩余次数/最新排序）可原样保留。

## 文件清单

- `index.html` —— 接力池单页（含全部样式与脚本）
- `README.md` —— 本文件
