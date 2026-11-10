# 我的静态网页（Astro）

一个「放文案和照片」的个人主页。没有框架知识也能改：文字集中在一个文件里，照片丢进一个文件夹就行。

---

## 1. 在电脑上看效果

**方法一（推荐）**：双击 **`启动网站.bat`**，浏览器会自动打开
`http://localhost:4321`

**方法二**：如果窗口里已经开着 http://localhost:4321，直接看就行——
改完文件保存，浏览器会自己刷新，不用重启。

关掉：在黑色窗口里按 `Ctrl + C`，或直接关窗口。

---

## 2. 改文字：两个地方都行

**方式 A：在网页里改（推荐，手机也能改）**
登录 <https://app.pagescms.org> → 选仓库 → 「网站内容」→ 表格式界面里改文字、传照片 → 保存。
保存后 Cloudflare 会自动重新构建，1~2 分钟线上生效。

**方式 B：在电脑上改文件**
打开 **`src/data/site.json`**（这是纯文本内容，格式是 JSON）：

| 想改什么 | 找哪一行 |
| --- | --- |
| 浏览器标签标题 | `pageTitle` |
| 左上角名字 | `nav.brand` |
| 大标题 | `hero.title`（`<br>` 是换行） |
| 开头那段介绍 | `hero.lead` |
| 关于我的两段话 | `about.paragraphs` 数组 |
| 右侧小档案 | `about.facts` |
| 照片列表 | `photos.items` |
| 邮箱、署名 | `footer.email`、`footer.copyright` |

规则：**只改引号里的字**，逗号、括号别删，逗号别多也别少。改完 `Ctrl + S`。
写在 `.pages.yml` 里的中文标签，就是网页 CMS 上显示的名字。

---

## 3. 换照片

现在放了 18 张：封面 `cover.jpg` + `01.jpg`～`17.jpg`，全部来自
`E:\yahyz\来自手机-oneplus PKG110\相簿`（一加 Ace 5 拍的，已缩到 1600px、去掉了手机水印）。

1. 把照片复制到 **`public/photos/`** 文件夹；
2. **最省事的做法**：直接覆盖原来的 `cover.jpg`、`01.jpg`……`17.jpg`（同名替换，不用改任何代码）；
3. 想新增照片：把图命名为 `18.jpg` 放进去，然后在 `src/data/site.ts` 的 `photos.items` 里照着加一行：

```js
{ src: '/photos/18.jpg', caption: '一句话说明' },
```

- `caption` 是点开大图时显示的那句话，也是图片的替代文字
- 竖图横图都不用管，照片墙会自动按原比例排（不裁切）
- 想删照片：把对应的整行 `{ ... },` 删掉
- 注意 `about.facts` 里还有一行「这里放了 18 张」，改数量时顺手改一下

> 照片建议：长边 1600px 左右就够了，格式 jpg，单张别超过 1MB。
> 网站文件里写的是 `/photos/xxx.jpg`，指向的就是 `public/photos/xxx.jpg`（`public` 这个名字不会出现在网址里）。

---

## 4. 换个气质（可选）

打开 `src/styles/global.css`，最上面几行：

```css
--paper:#faf8f4;   /* 页面底色 */
--accent:#2f4a3f;  /* 主色：按钮、小标题 */
```

把 `#faf8f4` 改成 `#f4f1ea`（更暖）、`#ffffff`（纯白）之类，整站就跟着变。

---

## 5. 发到网上（免费，不用买服务器）

1. 双击 **`打包发布.bat`**，它会生成一个 `dist` 文件夹并自动弹出来；
2. 打开 <https://pages.cloudflare.com/> 注册（用邮箱即可）；
3. 选择 **Upload assets / 直接上传**，把整个 **`dist` 文件夹**拖进去；
4. 得到一个 `xxx.pages.dev` 的网址，手机电脑都能打开。

想用自己的域名，在 Cloudflare Pages 的 Custom domain 里加一条就行；国内访问为主的话，域名需要备案。

---

## 6. 文件都是干什么的

```
src/data/site.json    ← 所有文字都在这（网页 CMS 改的就是它）
.pages.yml            ← 网页 CMS 的配置：定义哪些字段可编辑
src/pages/index.astro ← 页面结构（不用动）
src/layouts/Base.astro← 页面外壳（不用动）
src/styles/global.css ← 样式/配色（想换色改这里）
public/photos/        ← 照片放这
上传到GitHub/          ← 首次上传 GitHub 用的干净副本（不含 node_modules）
dist/                 ← 打包出来的成品
启动网站.bat           ← 本地预览
打包发布.bat           ← 生成 dist（手动上传时用）
```

---

## 7. 卡住了怎么办

- **双击 bat 闪一下就没了**：右键 →「以管理员身份运行」试试；或者告诉我，我帮你看。
- **端口被占用**：Astro 会自动换成 4322，看窗口里打印的地址。
- **照片不显示**：检查文件名和后缀（`.jpg` / `.jpeg` / `.png` 要一致），以及 `site.ts` 里写的是 `/photos/文件名`。
- **改坏了想还原**：直接告诉我改坏了什么，我用 Git 或者备份帮你恢复。
