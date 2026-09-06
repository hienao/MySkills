# travel-plan — 旅行规划与攻略细化

`travel-plan` 是一个可独立运行的完整旅行规划 skill。它已经把原 `Travel Planner Skill` 的需求确认、小红书与公开网络研究、路线设计、逐日攻略、预算、行前准备和精美 HTML 生成功能合并进来，并补上多方案比较、具体车次、餐厅分店与营业时间、酒店价格及优缺点等执行细节。

当前版本：`1.1.0`（以 [`VERSION`](./VERSION) 为准）。

不需要先调用或另行依赖 `travel-planner`；从一句“十一从杭州出发，5 天去哪玩”开始即可完成从研究到 HTML 交付的全过程。

## 与 Travel Planner Skill 的整合范围

- 从零开始确认日期、人数、交通、体力、兴趣、预算和住宿偏好。
- 优先通过 x-mcp 研究小红书攻略、评论区纠错、实际花费和避坑经验。
- 再用官方及预订平台核验车次、开放时间、地址、票价、房价和预约规则。
- 规划路线总览、逐日时间线、交通、住宿、餐饮、预算和行前清单。
- 生成暖色旅行主题的响应式 HTML，并支持手机和打印版式；需要景点参考图时自动生成本地静态资源目录和可部署产物。
- 在以上能力上增加所有备选方案的完整详情，而非只给排名或摘要。

完整页面规范随 skill 一起存放在 `references/html-spec.md`，生成或大幅更新 HTML 时会强制读取，因此它是本 skill 的内置内容，不是外部依赖。

## 能规划和补充什么

- 所有候选目的地的同口径比较与排序。
- 每条方案独立的逐日时间表，而不只是优缺点摘要。
- 高铁车次样本、飞机/高铁门到门成本和选择阈值。
- 景点开放时间、停止入场、票价、预约和闭馆日。
- 每条路线的景点参考图库，包括准确替代文本、来源、作者/许可信息和季节差异提醒。
- 餐厅准确分店、地址、营业时间、建议菜品和排队提醒。
- 每个实际住宿地 2–3 家酒店，包括间夜价格区间、位置、优点、缺点和适用人群。
- 按人数计算的预算、天气 B 计划、订票及最终复核清单。
- 更新已有 HTML，或生成单文件响应式旅行攻略 HTML。

## 使用方式

在 Codex 中显式调用：

```text
使用 $travel-plan，比较霞浦、潮汕和洛阳。
杭州出发，9 月 25 日到 29 日，2 人，5 天 4 晚，不爬山。
每个方案列具体车次、每日行程、餐厅地址与营业时间，
每个住宿城市给 2–3 家酒店和价格、优缺点，最后生成 HTML。
```

也可以用它继续完善已有文件：

```text
使用 $travel-plan 更新这份攻略 HTML。
保留现有设计，补齐全部备选方案、交通班次、景点和餐厅时间，
并给每个实际过夜地点增加 2–3 家酒店及间夜价格和优缺点。
```

也可以显式要求补充图片和部署包：

```text
使用 $travel-plan 更新这份旅行攻略，给各方案的主要景点增加参考图。
图片要与景点匹配、标明来源并下载为本地 WebP，使用相对路径；
最后把 index.html 和图片资源一起构建为可远端静态部署的 ZIP。
```

配图时优先使用用户提供素材和 Wikimedia Commons 等许可明确的图片。没有可复用的精确景点图时，会改用官方图集链接或标注“同区域景观参考”，不会把生成图伪装成现场照片。详细规则位于 `references/image-spec.md`。

输入越完整，结果越稳定。推荐提供：出发地、日期、天数/晚数、人数、候选目的地、预算、交通偏好、体力限制和已有攻略文件。

## 版本更新提示

每次调用时，skill 会将本地 `VERSION` 与 GitHub `main` 分支中的 [`travel-plan/VERSION`](https://github.com/hienao/MySkills/blob/main/travel-plan/VERSION) 比较。仅在远端版本更高时提示更新，不会自动修改本地文件，也不会打断当前旅行任务。网络或版本检查异常时会直接继续执行任务。

## 必需依赖：x-mcp

本 skill 依赖 [`xpzouying/x-mcp`](https://github.com/xpzouying/x-mcp) 提供的小红书搜索和详情能力，主要使用：

- `check_login_status`
- `search_feeds`
- `get_feed_detail`

仓库中的 skill 位于 `x-mcp/x-mcp`。可将该目录安装到个人 skills 目录：

```bash
git clone https://github.com/xpzouying/x-mcp.git
cp -R x-mcp/x-mcp ~/.codex/skills/x-mcp
```

还需按照项目说明安装 Chrome 扩展、在 [aredink.com](https://aredink.com) 创建连接并取得 Token，然后把 MCP 服务接入 Codex：

```bash
export CODEX_XMCP_TOKEN=sk_xxxxxxx
codex mcp add x-mcp \
  --url https://mcp.aredink.com/mcp \
  --bearer-token-env-var CODEX_XMCP_TOKEN
```

不要把 Token 写进 `SKILL.md`、README、代码仓库或聊天内容。配置后先确认浏览器已登录小红书，再测试 `check_login_status`。

如果小红书未登录，或 x-mcp 未安装、连接失败、登录检查报错、持续超时，skill 会停止小红书调研并提示用户打开 [`xpzouying/x-mcp`](https://github.com/xpzouying/x-mcp)，按照仓库说明添加或排查 MCP、配置连接并完成登录。只有用户明确选择跳过，才会退回公开网络，并标注“小红书实测未核验”。

原 `travel-planner` 已经内置合并，不再作为上游依赖。本 skill 既可以直接从目的地或候选清单开始，也可以接手已有 HTML。

## 数据口径

- 车票未进入预售期时，只能给当前运行图样本，最终以 12306 为准。
- 酒店价格按指定日期、1 间 2 成人的相近房型比较；早餐、税费和取消政策必须同口径。
- 餐厅和景点可能临时调整，建议出发前 3–7 天再核验一次。
- 小红书用于真实体验和避坑；班次、开放时间及价格仍需官方或预订平台交叉验证。

## 安装位置

从 GitHub 克隆仓库，并把 skill 链接到个人 Skills 目录：

```bash
git clone https://github.com/hienao/MySkills.git ~/MySkills
mkdir -p ~/.agents/skills
ln -s ~/MySkills/travel-plan ~/.agents/skills/travel-plan
```

也可以直接复制安装：

```bash
git clone https://github.com/hienao/MySkills.git
mkdir -p ~/.agents/skills
cp -R MySkills/travel-plan ~/.agents/skills/travel-plan
```

仓库内的 skill 源目录为：

```text
/Volumes/Data/Code/GitHub/MySkills/travel-plan
```

上面的绝对路径仅是本仓库维护者当前机器上的位置；其他用户以实际克隆目录为准。不要直接覆盖已有同名目录。重新打开 Codex 或刷新 Skills 列表后，可在 My Skills 中看到“travel-plan”。
