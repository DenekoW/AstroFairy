# AstroFairy

还在每次面对一个望远镜/巡天数据库时都会被一大堆藏在各种角落里的文档搞得晕转向吗？看到一个有趣的天体却要花时间翻一大堆数据库找它有什么波段的数据？试试这个AstroFairy Skill吧
AstroFairy是面向observer的研究型 skill，可以用来调查某个巡天/望远镜数据的基础信息、数据产品获取方式与注意事项，也可以用于调查某一坐标下的天体被什么数据/proposal所覆盖。

**版本**: 0.3

## 快速开始

```bash
# 方式一：从 GitHub 直接安装
hermes skills install https://raw.githubusercontent.com/denekow/astrofairy/main/SKILL.md

# 方式二：添加 tap 仓库后安装
hermes skills tap add https://github.com/denekow/astrofairy
hermes skills install astrofairy

# 方式三：手动clone
git clone https://github.com/denekow/astrofairy.git ~/.hermes/skills/astrofairy/
```

然后就可以直接开始使用了！

## AstroFairy的三种模式

在`examples/`中有针对这三种模式生成的示例调查报告。

### Survey Mode（巡天模式）
用户给出巡天名称 + release，AstroFairy会去调查相关的数据公开网站和文档，最后总结为md报告，报告内容包括：基础信息、公开的星表、图像及PSF/mask/variance获取方式、数据使用注意事项。

使用示例：
```
> "调查一下 Gaia DR3"
> "怎么下载 HSC-SSP PDR3 的 cutout，连带 variance 和 mask？"
```

### Position Mode（坐标模式）
用户给定坐标或天体名，AstroFairy可以帮忙通过各个数据库的api收集信息，总结该位置上所有可用的数据（星表/图像/光谱，可给出数据来源及可以直接执行的 API/curl/SQL 命令。

使用示例：
```
> "RA=149.8226 Dec=1.7284 有什么多波段数据？"
> "查一下这个坐标在 MAST/IRSA/CDS/NED 的数据"
```

### Proposal Check（观测提案检索模式）
检查 JWST/HST/ALMA 是否有已批准的观测项目覆盖该目标。（目前只支持这三个望远镜）

使用示例：
```
> "TOI-270 有没有被 JWST proposal 覆盖？"
> "这个天体有没有还在 proprietary period 的 HST 数据？"
```

使用的数据源：STScI 已批准项目列表、MAST 观测元数据、arXiv 文献、以及第三方 JWST 提案搜索工具（`https://jwst-search.zhechenghu.com/`）。

## References（参考文件）

在每一轮收集数据后，AstroFairy会在`references/` 目录下总结收集到的信息，每个 `.md` 文件都是基于官方文档验证过的具体访问结论——包含 URL、表名、API 命令和已知注意事项。skill 加载它们即可复用，无需每次从零开始调查。



## 运行环境

本 skill 为 **Hermes Agent** 设计，也可在其他 AI 编程助手中使用，部署方式略有不同。

### Hermes Agent（推荐）

```bash
hermes skills install https://raw.githubusercontent.com/denekow/astrofairy/main/SKILL.md
```

需要：`terminal`、`write_file`、`patch`、`browser_*` 工具，`curl`，`python3`（astroquery/pyvo 可选）

### Claude Code

将 SKILL.md 内容放入项目的 `CLAUDE.md`，或在启动时注入：

```bash
claude --append-system-prompt "$(cat SKILL.md)"
```

或将整个 `astrofairy/` 目录放入项目，在 `CLAUDE.md` 中添加：

```markdown
# AstroFairy Skill
当用户询问巡天数据、观测提案覆盖、或某个坐标的多波段数据时，
遵循 astrofairy/SKILL.md 中的协议。参考文件位于 astrofairy/references/。
```

限制：Claude Code 不支持 `skill_view()` 延迟加载，所有 reference 会一次性载入。

### OpenAI Codex

同 Claude Code。放入 `.codex.md` 或启动时注入：

```bash
codex --instructions "$(cat SKILL.md)"
```

或添加至项目的 `CONTEXT.md`。

### 其他 Agent（Cursor、Windsurf、Aider 等）

任何接受 system prompt 或指令文件的 agent 都可以使用 AstroFairy。将其指向 `SKILL.md` 作为任务协议，并确保其可以访问 `curl`、`python3`，以及读写 `references/` 和输出报告的权限。

### 运行时依赖

- `curl` — SIMBAD、NED、MAST、VizieR 等 API 访问
- `python3` — 推荐，用于 astroquery、pyvo 及 footprint 验证脚本


## 如果真的对你的研究有帮助
点个Star吧！
