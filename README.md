# 亦然小窝 · 赞赏单页

一个**纯静态**的赞赏（打赏）单页：拟态（Neumorphism）风格，支持微信 / 支付宝收款码切换与昼夜模式。
不需要服务器、不需要 PHP / 数据库，直接托管到 **GitHub Pages** 即可。

---

## 一、目录结构

```
index.html          页面本体（样式 + 逻辑都在这一个文件里）
assets/images/      头像、背景、支付图标
assets/qr/          收款码图片（需要你自己放进去）
.nojekyll           让 GitHub Pages 跳过 Jekyll 处理
```

---

## 二、放收款码（必做）

把两张收款码图片放进 `assets/qr/` 目录，**文件名必须一致**：

| 渠道 | 文件名 |
| --- | --- |
| 微信 | `assets/qr/wechat.png` |
| 支付宝 | `assets/qr/alipay.png` |

> 建议只截取**中间的二维码部分**（去掉顶部"推荐使用微信支付 / 支付宝"的横幅），
> 因为页面自己已经有标题和渠道 Tab，整张海报放进去会重复。
>
> 没放图片时页面会显示"未找到收款码图片"的占位提示，不会报错、也不影响部署。

---

## 三、改文案 / 换头像

打开 `index.html`，改最上方脚本里的 `CONFIG` 即可（不用动下面的逻辑）：

```js
var CONFIG = {
  avatar : 'assets/images/avatar.jpg',   // 头像
  bg     : 'assets/images/dream.webp',   // 顶部模糊背景
  name   : '亦然小窝',                    // 昵称
  sign   : '我热爱你所热爱的一切！',        // 签名
  subtitle: '如果这里的内容对你有帮助……',   // 副标题
  declare: '赞赏完全自愿，感谢每一份心意',   // 页脚声明
  email  : '3176210846@qq.com',          // 反馈邮箱
  channels: [ /* 渠道，改这里增删微信/支付宝 */ ]
};
```

- **换头像**：直接覆盖 `assets/images/avatar.jpg` 即可（保持文件名）。
- **换背景**：覆盖 `assets/images/dream.webp`。
- **改邮箱**：改 `CONFIG.email`，页脚会自动生成可点击的 `mailto:` 链接。

---

## 四、部署到 GitHub Pages

### 方式 A：网页上传（最简单，不用命令行）

1. 打开 GitHub，右上角 **+ → New repository**，仓库名填 `zanshu`，可见性选 **Public**，不要勾选任何初始化文件，点 **Create repository**。
2. 进入新仓库，点 **Add file → Upload files**，把解压出来的**全部文件**（`index.html`、`assets` 文件夹、`.nojekyll`）一起拖进去，点 **Commit changes**。
   > 注意 `assets` 文件夹要一起拖，目录结构要保留。
3. 点仓库 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 **main** / **(root)**，点 **Save**。
4. 等 1～2 分钟，访问 `https://<你的用户名>.github.io/zanshu/`。

### 方式 B：命令行推送

```bash
cd zanshu
git init -b main
git add .
git commit -m "init: 亦然小窝赞赏页"
git remote add origin https://github.com/<你的用户名>/zanshu.git
git push -u origin main
```

推送成功后，同样到 **Settings → Pages** 里把 Source 设为 `main` / `(root)`。

> ⚠️ 免费版 GitHub Pages 只支持 **Public（公开）** 仓库，私有仓库无法开启 Pages。

---

## 五、本地预览（可选）

直接双击 `index.html` 就能看；或者起个本地服务：

```bash
python3 -m http.server 8080
```

然后浏览器访问 `http://localhost:8080/`。

---

## 六、常见问题

**Q：页面显示"未找到收款码图片"？**
A：说明 `assets/qr/wechat.png` 或 `alipay.png` 没放对。检查文件名大小写、扩展名是否为 `.png`，以及是否在 `assets/qr/` 目录下。

**Q：部署后打开是 404？**
A：① 确认文件名是 `index.html`（不能是 `index.htm`）；② 确认 Pages 的 Source 已设为 `main` / `(root)`；③ 刚开启需要等 1～2 分钟。

**Q：想加回 QQ 渠道？**
A：在 `CONFIG.channels` 里加一项即可，例如：

```js
{ id:'qq', label:'QQ', color:'#12B7F5', icon:'assets/images/qq-pay.svg',
  tip:'打开 QQ「扫一扫」进行赞赏', img:'assets/qr/qq.png' }
```

再把对应图片放进 `assets/qr/`。