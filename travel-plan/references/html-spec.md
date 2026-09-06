# Travel Planner HTML 规范

生成或大幅更新旅行攻略 HTML 前必须完整阅读本文件。

## 整体风格

- 主色 `#8B6914`，辅色 `#D4A84B`，背景 `#FDF8F0`，卡片 `#FFFFFF`。
- Day 标题背景使用 `#F5E6D0` 到 `#F0DCC0` 的浅暖棕渐变。
- 推荐标签绿底，预约/费用标签橙底，免费/交通标签蓝底，摄影提醒可用粉底。
- 字体：`"PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`。
- 卡片圆角约 12px，标签圆角约 6px，阴影 `0 2px 12px rgba(0,0,0,0.08)`。
- 内容区默认最大宽度 800–1080px，居中。多方案宽表可使用更宽上限，但正文行长需可读。
- Hero 默认使用 CSS 渐变和几何形状模拟风景；只有用户明确要求图片 Hero 时才使用经过核验的本地资源。
- 不使用 emoji 作为页面图标；使用 CSS 圆点、线条、色块和文字标签。

## 页面结构

### 1. Hero

包含路线标题、一句话描述、日期范围、天数/晚数、人数、旅行主题和最重要限制。多方案页面在 Hero 下直接给首选及综合评分。

### 2. 快速跳转

为推荐组合、每条备选、住宿、预算、订票/复核和来源建立锚点。锚点必须指向实际内容，而不是只跳到排名表。

### 3. 路线总览

使用圆点与连接线展示 `出发地 → 住宿基地 → 住宿基地 → 返程`。下方用卡片说明每段主题、停留时间、跨城方式和主要亮点。

### 4. 每日行程卡片

每张卡片包含：

- `Day X · M 月 D 日 · 周几 · 当日主题`。
- 左侧约 70% 的时间线：时间、地点、门票/标签和说明。
- 右侧约 30% 的亮点与提醒：体力、预约、天气、排队和替代项。
- 当日推荐餐厅、住宿基地和必要交通缓冲。

推荐 CSS：

```css
.day-header {
  background: linear-gradient(135deg, #F5E6D0, #F0DCC0);
  border-left: 4px solid #D4A84B;
  border-radius: 12px 12px 0 0;
  padding: 16px 24px;
}
.timeline-item::before {
  content: '';
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #D4A84B;
  border: 2px solid #FDF8F0;
}
.timeline-item::after {
  content: '';
  width: 2px;
  background: #E8D5B5;
}
.tag-recommend { background: #E8F5E9; color: #2E7D32; }
.tag-free { background: #E3F2FD; color: #1565C0; }
.tag-booking { background: #FFF3E0; color: #E65100; }
.tag-photo { background: #FCE4EC; color: #C62828; }
.highlights-sidebar {
  background: #FFFAF3;
  border-left: 3px solid #D4A84B;
  border-radius: 8px;
  padding: 16px;
}
```

### 5. 交通明细

使用表格列出路段、优先班次、备选班次、出发/到达站、时间、票价和选择逻辑。飞机、高铁、自驾要用同一门到门口径。对尚未开售的日期添加醒目的“运行图样本”提示。

### 6. 酒店明细

每个住宿基地建立表格，列：酒店、参考价/晚、区域/地址、优点、缺点、适合人群。用标签标识“位置首选、性价比、景观、高端”。长表格允许横向滚动，但首列与标题应清晰。

### 7. 景点与餐厅

景点表列地址/区域、开放时间、停止入场、门票、建议停留和预约。餐厅表列准确分店、行程安排、地址、营业时间、建议菜品和避坑。

用户要求景点参考图时，在对应方案的景点表附近增加响应式图库。每张卡片至少包含景点名、实景图、准确 `alt`、一句景观说明和来源；详细检索、授权、压缩与失败回退规则见 [image-spec.md](image-spec.md)。同一景点在多个方案出现时复用同一资源。

### 8. 预约与行前准备

使用两到三列卡片展示：预约购票、交通前往、预约提醒、天气 B 计划、携带清单和最后复核日期。移动端改为单列。

### 9. 预算

表格列出：项目、单位价格、小计和说明。至少包括大交通、酒店、门票、餐饮、当地交通、保险/签证（如有）和机动费用。下方展示经济、舒适、宽松三档总计及省钱提示。

### 10. 来源

小红书体验与官方/平台来源分组展示。链接文字说明该来源支持的内容。注明调研日期以及车次、房价、营业时间可能变化。

## 响应式

- `max-width: 800px`：允许每日卡片保持左右布局，但减少间距。
- `max-width: 600px`：每日卡片改为上下布局；三列卡片改为单列；Hero 指标改为 2×2；导航允许换行。
- 宽表格外包 `.table-wrap { overflow-x: auto; }`，不要让页面整体横向溢出。
- 手机端正文不得小于约 14px；按钮和可点击摘要应有足够高度。

## 打印

```css
@media print {
  body { background: white; }
  .day-card, .section, table { break-inside: avoid; }
  .hero { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
  a { color: inherit; text-decoration: none; }
}
```

展开式备选方案使用 `<details open>`，确保打印时内容默认可见。打印前检查分页，避免标题单独落在页尾。

## 数据与实现约束

- 所有数据直接写入 HTML，不使用运行时接口请求。
- 不引用外部 CSS、JavaScript或字体。无图片时保持单文件；有景点图时使用本地相对路径，不依赖临时外链，并把资源目录随 HTML 一起交付。
- 对用户已有 HTML 进行增量更新，保留风格和无关内容。
- 页面必须显示价格口径、调研日期和不确定性标签。
- 生成后浏览器检查：锚点、折叠区、表格滚动、中文换行、日期/周几、闭馆日、酒店小计与总预算。
