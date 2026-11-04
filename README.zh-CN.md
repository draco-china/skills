# skills

[English](README.md) | **简体中文**

Claude Code / Codex 通用的 skills 集合。每个 skill 是 `skills/<name>/` 下的一个独立目录（目录名和 frontmatter 的 `name` 一致），入口是 `SKILL.md`，参考文件按需加载。整个仓库本身也是一个 Claude Code 插件。

## Skills

| Skill | 说明 | 来源 | 许可证 |
|---|---|---|---|
| [design-md](skills/design-md/) | 维护仓库根目录的 DESIGN.md 视觉契约：建立、从风格目录采纳、更新、lint、审计、导出 Tailwind/DTCG。内置 Google 格式规范和 84 份风格参考 | **自写**，引用 [google-labs-code/design.md](https://github.com/google-labs-code/design.md) 和 [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) | MIT（引用的内容各自沿用 Apache-2.0 / MIT） |
| [impeccable](skills/impeccable/) | 前端界面设计、评审、打磨、审计 | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | Apache-2.0 |
| [design-taste-frontend](skills/design-taste-frontend/) | 落地页和网站的美术方向，避免千篇一律的 AI 风格 | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) | MIT |
| [archify](skills/archify/) | 架构图、流程图、时序图、ERD 等，输出可交互的 HTML | [tt-a1i/archify](https://github.com/tt-a1i/archify) | Apache-2.0 |
| [brooks-review](skills/brooks-review/) | PR 代码审查 | [hyhmrright/brooks-lint](https://github.com/hyhmrright/brooks-lint) | MIT |
| [brooks-audit](skills/brooks-audit/) | 架构和模块依赖审计 | 同上 | MIT |
| [brooks-debt](skills/brooks-debt/) | 技术债评估和重构优先级 | 同上 | MIT |
| [brooks-test](skills/brooks-test/) | 测试套件质量评审 | 同上 | MIT |
| [brooks-health](skills/brooks-health/) | 以上四个维度的综合评分 | 同上 | MIT |
| [brooks-sweep](skills/brooks-sweep/) | 全面扫描并直接修复 | 同上 | MIT |

`skills/_shared/` 不是 skill，是 brooks 系列共用的参考文件（通过 `../_shared/` 相对路径引用）。

## 安装

**Claude Code**（整套安装）：

```
/plugin marketplace add draco-china/skills
/plugin install draco-skills@draco-skills
```

**Codex / 其他 agent**（可以挑着装）：

```bash
npx skills add draco-china/skills
```

```bash
npx skills add draco-china/skills --skill design-md
```

**手动**：clone 后把 skill 目录软链到 `~/.claude/skills/`（Codex 是 `~/.agents/skills/`）：

```bash
ln -s "$PWD/skills/design-md" ~/.claude/skills/design-md
```

**在 Claude Code 以外使用 brooks 系列**：`npx skills add` 只安装带 `SKILL.md` 的目录，会漏掉 `_shared`，导致 brooks 读不到参考文件。Codex 请手动软链，并把 `_shared` 一起链上：

```bash
for d in _shared brooks-audit brooks-debt brooks-health brooks-review brooks-sweep brooks-test; do
  ln -s "$PWD/skills/$d" ~/.agents/skills/$d
done
```

也可以用[上游的安装脚本](https://github.com/hyhmrright/brooks-lint#installation)。上面的 Claude Code 插件安装方式已经包含 `_shared`。

## 上游同步

除了 `design-md` 的自写部分（`SKILL.md`、`assets/`、`agents/`），其余内容都是上游的逐字节镜像，由 [sync.yml](.github/workflows/sync.yml) 每天（UTC 02:00）自动同步并直接提交到 main：

- [upstreams.json](upstreams.json)：要同步什么（上游仓库、分支、`上游路径 → 本仓库路径`，可选的 `include` 文件过滤）。
- [skills.lock.json](skills.lock.json)：当前同步到的上游 SHA，由 CI 生成。
- 上游仓库的 LICENSE 会复制进每个镜像目录。
- 某个上游同步失败时，它保留上一个版本，其他上游照常同步；这次运行会被标记为失败，方便发现问题。
- 每次同步提交前都会跑检查，其中包括用最新的 DESIGN.md 规范 lint design-md 的模板。
- 插件不写固定的 `version`，Claude Code 会把每个 commit 当作新版本，`/plugin update` 能拿到每次同步的内容。

**镜像目录不要在本地改**，下次同步会被覆盖。要改进就提给上游。

新增上游：往 `upstreams.json` 加一项，push 以后会自动触发同步。也可以在 Actions 页面手动运行 `sync-upstream`。

## 维护约定

- 目录名 == frontmatter 的 `name`（由 [check.yml](.github/workflows/check.yml) 校验）。
- 不写机器特定路径（`/Users/...`）。
- 自写 skill 里的命令和 API 必须验证过，不凭记忆写参数。

## License

仓库自身和自写的 skill 使用 [MIT](LICENSE)。镜像目录遵循各自上游的许可证（见目录里的 LICENSE）。
