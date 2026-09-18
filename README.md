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

推 `main` 分支即可，Pages 已配置为 `main` 分支根目录（`/`）发布：

```bash
git add -A
git commit -m "update site"
git push
```

约 1 分钟后生效，地址形如 `https://<用户名>.github.io/formpilot-site/`。

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

- [x] 页脚联系邮箱：已填 `285600131@qq.com`（`index.html` 页脚 + `privacy.html` 联系段落）
- [ ] 商店链接：把 `YOUR-ID` 换成 Edge / Chrome 商店详情页真实 ID（`index.html` 的 `#download` 区块）
