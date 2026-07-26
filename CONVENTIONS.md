# CONVENTIONS — 路径引用规范

> L2 按需加载。定义所有 skill 文件中跨空间路径引用的统一规范。

---

## 三种文件空间

| 空间 | 位置 | 引用方式 |
|------|------|---------|
| **Skill 内部** | `~/.trae/skills/{skill-name}/` 目录 | `./FILENAME.md` |
| **项目工作区** | 当前项目根目录 | `{workspace}/config/FILENAME.md` |
| **用户 Home** | `~` 开始 | `~/PATH/FILENAME.md` |

---

## 规则一：Skill 内部引用 → `./FILENAME.md`

仅限同 skill 目录下的文件。不跨越 skill 边界。

```
[WORKFLOW.md](./WORKFLOW.md)
[EXAMPLES.md](./EXAMPLES.md)
```

## 规则二：项目工作区引用 → `{workspace}/config/FILENAME.md`

`{workspace}` 代表当前项目工作区根目录。每个使用 `{workspace}` 的文件必须在其顶部声明此行：

```
> {workspace} = 当前项目工作区根目录
```

禁止裸 `config/` 和裸文件名。

```
# 正确
[{workspace}/config/RULES.md]({workspace}/config/RULES.md)

# 禁止
config/RULES.md
RULES.md
```

## 规则三：用户 Home 引用 → `~/PATH/FILENAME.md`

用于引用 `~/.trae-cn/` 及其他用户目录下的文件。

```
~/.trae-cn/user_profile.md
~/.trae-cn/github-credentials.md
```

## 规则四：跨 skill 引用 → 名称引用 + 首次加注释

保持名称引用格式（如 `Use Skill: teaching-gate`），或文中提及的 skill 名称。

首次出现时加注释说明物理位置：

```
Use Skill: teaching-gate  <!-- skill 位置：~/.trae/skills/teaching-gate/ -->
```

```
不知道学什么 → `career-advisor`  <!-- skill 位置：~/.trae/skills/career-advisor/ -->
```

同一文件内同一 skill 多次出现，仅首次加注释。

## 规则五：`{workspace}` 声明行

每个使用 `{workspace}` 占位符的 SKILL.md 或 L2 文件，必须在文件顶部（标题后、正文前）加声明行：

```
> {workspace} = 当前项目工作区根目录
```

声明行放在 L2 提示行（如有）之后。
