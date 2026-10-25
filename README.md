# 顺丰越南区业务门户（SF Vietnam Portal）

一个纯静态的内部业务门户，适合 **GitHub 托管 + Cloudflare 加速** 部署。

## 页面内容

- 顶部品牌条 + 当前日期
- **公告模块**（纯文字，按日期列出）
- 5 个业务模块：**供应链 / 快递电商 / 销售 / 人资 / 财务**，每模块 9 个图标入口
- 所有入口目前指向占位地址 `https://www.baidu.com/`

## 部署到 GitHub Pages

1. 在 GitHub 新建仓库（如 `sfvn-portal`），把 `index.html` 上传到仓库根目录。
2. 仓库 **Settings → Pages → Source** 选择 `main` 分支 / `/(root)`，保存。
3. 等待 1-2 分钟，访问 `https://<用户名>.github.io/sfvn-portal/` 即可。
4. （可选）在 Cloudflare 添加自定义域名：**Workers & Pages → 你的 Pages 项目 → Custom domains**，或在 DNS 里加一条 CNAME 指向 `<用户名>.github.io`，并在 GitHub 仓库 Settings → Pages → Custom domain 填写该域名。

> 提示：仓库可以是 Private + 仅自己可见，但 GitHub Pages 对私有仓库需要 Pro 账号；公共仓库免费。

## 如何修改内容

所有可编辑内容集中在 `index.html` 底部的 `<script>` 内：

### 1. 修改某个入口的网址

找到 `MODULES` 数组，给对应条目加上 `url` 字段即可：

```js
{t:"仓储管理", d:"仓库出入库与库存", i:"warehouse", url:"https://wms.example.com"},
```

没写 `url` 的条目默认打开 `PLACEHOLDER`（当前为 baidu.com），可统一改 `PLACEHOLDER` 变量。

### 2. 修改入口名称 / 描述 / 图标

- `t`：显示名称　`d`：一行小描述（可留空 `""`）
- `i`：图标名，可选值见同文件 `ICONS` 对象（box / truck / ship / warehouse / chart / users 等 40 余个），写错名字会自动回退为默认图标。

### 3. 增减模块或入口数量

在 `MODULES` 数组里复制/删除一段对象即可，页面和顶部导航自动跟随，无需改其他代码。

### 4. 修改公告

直接编辑 HTML 中 `<section class="notice">` 里的 `<li>` 列表，每条格式：

```html
<li><time>10-24</time><span>公告正文……</span></li>
```

## 技术说明

- 单文件 `index.html`，无外部依赖（图标全部为内嵌 SVG），离线可用。
- 响应式布局，手机端自动换行为两列/单列。
- 建议通过 Cloudflare Pages 直接连 GitHub 仓库自动部署（推送即更新）。
