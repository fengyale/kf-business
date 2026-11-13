<div align="center">

# kf-business

单页企业官网静态站点模板，面向制造业 / 材料行业。

![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?logo=tailwindcss&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP-88CE02?logo=greensock&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow)

</div>

## 项目定位

一套可直接改内容上线单页企业官网模板：全部区块、样式与动效都写在 `index.html` 内，无构建步骤、无后端依赖、无 CDN 依赖，把 `assets/` 和 `index.html` 传到任何静态托管或 IIS 即可访问。

适合用作制造业、铸造 / 材料行业的官网起步模板：保留结构与动效，替换公司信息、产品图片和文案即可成站。

## 技术栈

| 技术 | 用途 |
| --- | --- |
| HTML5 + 原生 JavaScript | 页面结构、交互逻辑（导航、移动端菜单、表单校验、返回顶部） |
| Tailwind CSS（本地构建产物） | 全站样式，`assets/css/tailwind.min.css` 已内置，不依赖 CDN |
| Font Awesome（本地字体） | 图标，字体文件在 `assets/fonts/` |
| GSAP + ScrollTrigger | 滚动进场、区块渐显等动效 |
| three.js | Hero 区 `#hero-canvas` 的粒子背景 |
| Noto Sans SC（本地 woff2） | 中文字体，避免外部字体请求 |

## 页面区块

| 区块 | 锚点 | 说明 |
| --- | --- | --- |
| 导航 | — | 桌面端吸顶导航 + 移动端汉堡菜单（`#mobile-menu`） |
| Hero | `#home` | 主标题、行动按钮、three.js 粒子画布 |
| 产品优势 | — | 卡片式罗列核心卖点 |
| 产品中心 | `#products` | 产品网格，图片与名称由 `#products-container` 渲染 |
| 成功案例 | — | 按行业（汽车 / 电力 / 钢铁）展示应用场景 |
| AI 产品应用场景分析器 | `#ai-tool` | 纯前端演示表单（工艺类型、材质、温度）→ 本地规则输出推荐结果，不发请求、不接后端 |
| 关于我们 | `#about` | 公司介绍、车间与产线图片 |
| 联系我们 | `#contact` | 联系方式 + 留言表单（前端校验，无提交接口） |

全站响应式，按 `md` 断点切换移动端 / 桌面端布局。

## 目录结构

```text
.
├── index.html               # 单页站点全部结构、样式与脚本
├── web.config               # IIS 静态缓存与 rewrite 配置
└── assets
    ├── css
    │   ├── tailwind.min.css     # Tailwind 构建产物
    │   ├── font-awesome.min.css # 图标样式
    │   └── noto-sans-sc.css     # 字体声明
    ├── fonts                  # Font Awesome / Noto Sans SC woff2
    ├── images                 # 产品图、案例图、车间图、Logo
    └── js                     # gsap.min.js、ScrollTrigger.min.js、three.min.js
```

## 本地预览

方式一，直接双击 `index.html` 用浏览器打开（站点未使用模块化脚本，可正常运行）。

方式二，起一个本地静态服务器，更贴近线上环境：

```bash
python -m http.server 8080
# 浏览器访问 http://localhost:8080
```

## 部署（IIS）

`web.config` 提供两项配置，部署在 IIS 时自动生效：

- `staticContent/clientCache`：静态资源统一缓存 30 天
- `rewrite/outboundRules`：为 `.css` / `.js` / `.woff2` / `.jpg` / `.jpeg` / `.png` / `.gif` / `.svg` 的响应补充 `Cache-Control: public, max-age=2592000`

使用时把仓库根目录作为站点物理路径，确保 `web.config` 与 `index.html` 同级。部署到 Nginx / 对象存储等其它静态托管时，`web.config` 会被忽略，缓存策略需在对应平台单独配置。

## 关于示例数据

> 仓库为脱敏示例版本，站点内的公司名称（示例铸造材料有限公司）、地址（河南省郑州市示例产业园 12 号楼）、电话（+86 000-0000 0000）、邮箱（info@example-casting.com）均为示例值，表单也不含任何后端提交接口。正式使用前请全文替换为真实信息，并自行接入表单收集方案。

## License

MIT
