# 示例铸造材料有限公司官网（静态单页）

一个响应式企业官网静态单页示例，包含产品展示、滚动动效与静态部署配置。

> **声明**：仓库中的公司名称、地址、电话、邮箱等均为示例值，不含任何真实企业信息，仅用于演示静态站点结构、响应式布局与页面动效。

## 页面结构

`index.html` 是唯一页面，顶部导航包含 5 个锚点，正文共 7 个 `<section>`：

| 顺序 | 区块 | 锚点 | 主要内容 |
| --- | --- | --- | --- |
| 1 | 首屏 | `#home` | 主标题、行动按钮、Three.js 粒子背景 canvas、向下滚动提示 |
| 2 | 我们的产品优势 | 无 | 3 张优势卡片（高温稳定性、化学成分、质量管控等） |
| 3 | 产品中心 | `#products` | 4 个分类筛选项（铝矾土 / 莫来石粉 / 铸造涂料 / 耐火材料），5 张产品卡片 |
| 4 | 成功案例 | 无 | 钢铁、电力、汽车制造 3 个行业案例卡片 |
| 5 | AI 应用方案 | `#ai-tool` | 工艺/材料/温度等表单，提交后由前端规则匹配输出推荐结果 |
| 6 | 关于我们 | `#about` | 公司简介、资质标签、6 张厂区实景图 |
| 7 | 联系我们 | `#contact` | 留言表单（纯前端，无提交接口）与地址、电话、邮箱、工作时间 |
| — | 页脚 | — | 产品/快速链接、社交图标、版权信息 |

页面还包含移动端折叠菜单、返回顶部按钮、图片懒加载（`data-src` + `IntersectionObserver`）、产品分类筛选，以及键盘输入 Konami 码触发的烟花彩蛋。

## 技术栈

| 资源 | 版本 | 用途 |
| --- | --- | --- |
| Tailwind CSS | 3.4.1 | 本地构建版样式，负责布局、响应式与工具类（无 CDN 依赖） |
| Font Awesome Free | 6.4.0 | 页面图标（`fas` / `fab`），含本地 woff2 字体 |
| Three.js | r128 | 首屏 `#hero-canvas` 粒子背景 |
| GSAP | 3.11.4 | 粒子旋转、彩蛋烟花等 JavaScript 动画 |
| GSAP ScrollTrigger | 3.11.4 | 已引入，但当前脚本未调用；滚动进场动画实际由 `IntersectionObserver` + CSS 实现 |
| Noto Sans SC | — | 本地 `@font-face` 字体，woff2 文件目前为占位内容 |

项目为纯静态实现，无构建工具、无 npm 依赖、无后端服务；AI 分析器仅为前端写死的规则匹配演示，不调用任何模型 API。

## 本地预览

方式一：直接双击 `index.html`，用浏览器打开。

方式二：启动本地静态服务器（便于验证相对路径与懒加载）：

```powershell
cd D:\workspace\zhengzhou-yijinjing
python -m http.server 8080
```

然后访问 <http://localhost:8080>。

## 目录结构

```text
zhengzhou-yijinjing/
├── index.html                  # 单页站点全部结构、样式与脚本
├── web.config                  # IIS 静态站点缓存配置
├── .gitignore
└── assets/
    ├── css/
    │   ├── tailwind.min.css    # Tailwind CSS 3.4.1
    │   └── font-awesome.min.css
    ├── fonts/                  # Font Awesome 与 Noto Sans SC 字体
    ├── js/
    │   ├── three.min.js        # Three.js r128
    │   ├── gsap.min.js         # GSAP 3.11.4
    │   └── ScrollTrigger.min.js
    └── images/
        ├── about/              # 厂区、设备、办公场景图
        ├── cases/              # 行业案例图
        ├── products/           # 产品图（含同名 SVG 占位图）
        ├── ai-analysis.jpg
        ├── company-logo.svg
        ├── company-logo-light.svg
        └── endjpg.png          # 首屏背景图
```

## 部署说明

`web.config` 面向 IIS 静态站点，包含两项配置：

- `staticContent/clientCache`：使用 `UseMaxAge`，默认缓存 30 天。
- `rewrite/outboundRules`：为 `css/js/woff2/jpg/jpeg/png/gif/svg` 响应追加 `Cache-Control: public, max-age=2592000`（30 天）。
- 未配置 URL 重写或跳转规则，不依赖伪静态。

将整个目录上传到 IIS 站点根目录即可（`index.html` 作为默认文档）。其它静态托管方式：

- GitHub Pages / Netlify / Vercel：直接发布仓库根目录，`web.config` 不会生效，需在平台侧配置缓存头。
- Nginx：将根目录指向站点路径，缓存策略需在 `nginx.conf` 中另行配置。

## 自定义说明

- **产品数据**：产品卡片直接写在 `index.html` 的 `#products-container` 中，修改卡片标题、描述、标签和图片即可；`data-category` 需与分类按钮的 `bauxite` / `mullite` / `coating` / `refractory` 对应。
- **图片替换**：产品图放在 `assets/images/products/`，案例图放在 `assets/images/cases/`，厂区图放在 `assets/images/about/`；`<img>` 使用 `data-src` 懒加载，替换时同步修改路径。
- **联系方式**：全局搜索 `示例铸造材料有限公司`、`河南省郑州市示例产业园 12 号楼`、`+86 000-0000 0000`、`info@example-casting.com`，同时更新 `<title>`、导航 logo 与页脚。
- **AI 分析结果**：`#ai-tool` 的推荐内容由脚本内写死的 `recommendations` / `advantages` 数组生成，可直接替换文案与图片路径。
- **字体**：`assets/fonts/NotoSansSC-*.woff2` 当前为 27 字节占位文件，正式使用前需替换为真实字体文件。
- **留言表单**：`#contact` 表单没有 `action` 与提交处理逻辑，需自行接入后端接口或第三方表单服务。
