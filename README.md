# MySkills

## travel-plan

目录：[`travel-plan`](./travel-plan/)

用于从零规划或细化旅行攻略，覆盖多目的地比较、逐日路径规划图、具体交通班次、景点和餐厅营业时间、酒店价格与优缺点、预算以及响应式 HTML 攻略。

当前版本：`1.2.0`（以 [`travel-plan/VERSION`](./travel-plan/VERSION) 为准）。

### 安装

推荐克隆仓库后创建软链接。这样后续在仓库中执行 `git pull`，My Skills 中的版本也会同步更新：

```bash
git clone https://github.com/hienao/MySkills.git ~/MySkills
mkdir -p ~/.agents/skills
ln -s ~/MySkills/travel-plan ~/.agents/skills/travel-plan
```

如果不希望使用软链接，也可以直接复制：

```bash
git clone https://github.com/hienao/MySkills.git
mkdir -p ~/.agents/skills
cp -R MySkills/travel-plan ~/.agents/skills/travel-plan
```

如果目标目录已经存在，请先确认其中是否有需要保留的修改，不要直接覆盖。安装完成后，重新打开 Codex 或刷新 Skills 列表，应能看到 `travel-plan`；可用下面的指令测试：

```text
使用 $travel-plan，帮我制定一份 5 天 4 晚的旅行攻略。
```

### 依赖配置

`travel-plan` 依赖 [`xpzouying/x-mcp`](https://github.com/xpzouying/x-mcp) 搜索和读取小红书内容。安装本 skill 后，还需要打开 x-mcp 仓库，按照其 README 添加 MCP、配置连接并完成小红书登录。

若小红书未登录或 x-mcp 连接异常，skill 会停止小红书调研并提示上述配置地址；只有用户明确选择跳过，才会改用公开网络并标记“小红书实测未核验”。

### 版本检查

每次调用时，skill 会读取本地 `VERSION`，并与 GitHub `main` 分支上的 [`travel-plan/VERSION`](https://github.com/hienao/MySkills/blob/main/travel-plan/VERSION) 比较。仅当远端版本更新时提示用户，不会自动更新或中断当前任务；检查失败时会继续正常工作。

使用示例：

```text
使用 $travel-plan，比较霞浦、潮汕和洛阳。
杭州出发，5 天 4 晚，不爬山；列出高铁和飞机方案、每日安排、
餐厅地址与营业时间，以及每个住宿地 2–3 家酒店的价格和优缺点。
```

更完整的使用方式和配置步骤见 [`travel-plan/README.md`](./travel-plan/README.md)。
