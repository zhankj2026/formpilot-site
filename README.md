# 智填 FormPilot · 官网站点（GitHub Pages）

对外公开的静态站点，只放「落地页 + 隐私政策」两个页面，**不含扩展源码**。
GitHub Pages 免费版要求仓库公开，而扩展主仓（`F:\aigen\chajian`）含源码与发布脚本，
因此单独用这个仓库承载官网，避免源码公开。

## 文件

| 文件 | 对应关系 | 用途 |
|---|---|---|
| `index.html` | 主仓 `landing/index.html`（同步副本） | 产品落地页 |
| `privacy.html` | 主仓 `docs/privacy-policy.md` + `src/privacy.html` | **商店审核要查的隐私政策 URL** |
| `.nojekyll` | — | 关闭 Jekyll 处理，避免下划线开头的文件被忽略 |

## 部署

远程：`git@github.com:zhankj2026/formpilot-site.git`（SSH），Pages 发布源为 `main` 分支根目录（`/`）。

```bash
git add -A
git commit -m "update site"
git push          # 注意下面的 insteadOf 坑，可能需带 GIT_CONFIG_GLOBAL
```

约 1 分钟后生效，地址：`https://zhankj2026.github.io/formpilot-site/`。

### 坑：全局 insteadOf 会把 SSH 改写成 HTTPS

本机 `~/.gitconfig` 有一条历史遗留规则：

```ini
[url "https://github.com/"]
    insteadOf = git@github.com:
```

它会把所有 `git@github.com:` 远程**强制改写为 HTTPS**。而 HTTPS 走本机代理时
schannel 握手失败（`SSL/TLS connection failed`），且 `GIT_TERMINAL_PROMPT=0` 下无法输密码，
表现为 `git push` 直接报 `unable to access 'https://github.com/...'`。

绕开办法（不改全局配置，用一份空的全局配置覆盖本条命令）。注意 Windows 版 git 不认 `/tmp`，
要写 Windows 风格路径：

```bash
: > "C:/Users/zhankj/.workbuddy/.gitconfig-nourl"        # 只需建一次；内容为空即可
cd /f/aigen/formpilot-site && \
GIT_CONFIG_GLOBAL="C:/Users/zhankj/.workbuddy/.gitconfig-nourl" \
GIT_SSH_COMMAND="ssh -o BatchMode=yes" \
git push -u origin main
```

彻底修复：删掉 `~/.gitconfig` 里的 `[url "https://github.com/"]` 段（该规则对当前网络无用）。
验证 SSH 通道：`ssh -T git@github.com` 应回 `Hi zhankj2026! You've successfully authenticated`。

## 与主仓同步

站点内容的主源是扩展主仓，改动顺序：先改主仓 → 再同步到这里。

```bash
# 1) 改主仓（F:\aigen\chajian）的 landing/index.html 或 docs/privacy-policy.md，提交
# 2) 同步到本站点仓库
cp /f/aigen/chajian/landing/index.html /f/aigen/formpilot-site/index.html
# 隐私政策有差异（HTML 版把 md 排版过），改动后手动同步 privacy.html
```

> 注意：本仓 `index.html` 里隐私政策链接必须是**相对路径** `privacy.html`。
> 项目型 Pages 站点（`用户名.github.io/仓库名/`）用绝对路径 `/privacy-policy` 会 404。

## 待办（上架前后各一次）

- [x] 建公开仓并完成首次推送：`main` = `8c9152e`（2026-09-18）
- [ ] **在仓库 Settings → Pages 里开启发布**（Source 选 `Deploy from a branch` → `main` / `(root)`）；
      未开启前 `https://zhankj2026.github.io/formpilot-site/` 返回 404
- [x] 页脚联系邮箱：已填 `285600131@qq.com`（`index.html` 页脚 + `privacy.html` 联系段落）
- [ ] 商店链接：把 `YOUR-ID` 换成 Edge / Chrome 商店详情页真实 ID（`index.html` 的 `#download` 区块）
