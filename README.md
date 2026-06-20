# 变量命名规范工具 Variable Naming Skill

规范代码变量命名 + 文档标题命名审查。内置 **600+ 条**中英文术语词典，覆盖通用编程、雷达/信号处理、气象/大气科学、激光雷达/LiDAR 领域。

## 功能

### 🖥️ 代码变量命名

| 检测项 | 示例 → 修正 |
|--------|------------|
| 拼音命名 | `yonghu` → `user` |
| 中文命名 | `let 名称 = "foo"` → `let name = "foo"` |
| 拼音缩写 | `hs := getList()` → `func getList()` |
| 无意义缩写 | `tmp`, `aaa`, `data1` → 描述性命名 |
| 风格不一致 | 混用 camelCase/snake_case → 按语言统一 |

### 📝 文档标题命名

检测图片/表格/公式/算法标题中的：

| 检测项 | 示例 → 修正 |
|--------|------------|
| 编号格式 | `图1` vs `Fig.1` → 全文统一 |
| 无空格断句 | `Fig.1DataFusionFlow` → `Fig.1 Data Fusion Flow` |
| 拼音/中文残留 | `Fig.1 yonghu liucheng` → `Fig.1 User Flow` |
| 大小写不当 | `Fig.1 analysis of data` → `Fig.1 Analysis of Data` |
| 术语不一致 | 标题与正文用词不统一 → 对照词典统一 |

## 用法命令

| 命令 | 功能 |
|------|------|
| `/variable-naming review [路径]` | 审查代码命名 |
| `/variable-naming fix [路径]` | 审查并自动修复命名 |
| `/variable-naming dict [关键词]` | 查看术语词典 |
| `/variable-naming add <中文> <英文>` | 添加术语条目 |
| `/variable-naming translate <中文>` | 查询术语翻译 |
| `/variable-naming caption review [文件]` | 审查文档标题命名 |
| `/variable-naming caption fix [文件]` | 审查并修复标题命名 |
| `/variable-naming caption format` | 查看标题命名规范 |

## 安装方法

### Claude Code（当前环境）

Skill 已安装在 `~/.claude/skills/variable-naming/`，直接使用：

```bash
# 在 Claude Code 会话中输入
/variable-naming review
```

**手动安装到其他机器的 Claude Code：**

```bash
# 将技能文件复制到目标机器
scp -r ~/Documents/variable-naming-skill user@host:~/.claude/skills/variable-naming/

# 或通过文稿目录备份进行安装
cp -r ~/Documents/variable-naming-skill ~/.claude/skills/variable-naming/
```

### Cursor IDE

[Cursor](https://cursor.sh) 支持 `.cursorrules` 项目规则文件，可将技能的术语词典和规范说明注入 AI 上下文。

**方法一：项目级规则（推荐）**

将术语词典和规范说明写入项目根目录的 `.cursorrules`：

```bash
# 从词典中提取关键条目，添加到 .cursorrules
cat >> .cursorrules << 'EOF'
## 变量命名规范

### 命名风格
- Go: camelCase 变量, PascalCase 类型, ALL_CAPS 常量
- Python: snake_case 变量/函数, PascalCase 类, ALL_CAPS 常量
- JS/TS: camelCase 变量/函数, PascalCase 类型, kebab-case 文件

### 禁止项
- 禁止拼音命名（如 yonghu, xitong）
- 禁止中文命名
- 禁止无意义缩写（tmp, aaa, val, data1）
- 禁止中英文混写

### 术语词典（节选）
- 用户 → user/account
- 管理员 → admin
- 登录 → login/signIn
- 密码 → password
- 雷达 → radar
- 信号 → signal
- 目标检测 → targetDetection
- 多普勒 → doppler
- 激光雷达 → lidar
- 点云 → pointCloud
- 气溶胶 → aerosol
- ...（更多术语参考 SKILL.md）
EOF
```

**方法二：全局规则**

在 `~/.cursorrules` 中配置全局规则，对所有项目生效。

### Continue.dev

[Continue](https://continue.dev) 是 VS Code / JetBrains 的开源 AI 编码助手。

**方法：项目级 instructions**

在项目根目录创建 `.continue/instructions.md`：

```bash
mkdir -p .continue
cat >> .continue/instructions.md << 'EOF'
# 变量命名规范

## 命名风格规则
- Go: camelCase / PascalCase / ALL_CAPS
- Python: snake_case / PascalCase / ALL_CAPS
- JS/TS: camelCase / PascalCase / ALL_CAPS
- 文件名: snake_case（Go/Python）、kebab-case（JS/TS）

## 禁止模式（自动审查）
1. ❌ 拼音命名 → 替换为对应英文
2. ❌ 中文命名 → 替换为对应英文
3. ❌ 无意义缩写（tmp, aaa, data1, val）→ 描述性命名
4. ❌ 中英文混写 → 统一为英文

## 术语对照示例
- 用户管理 → userManagement
- 数据采集 → dataAcquisition
- 信号处理 → signalProcessing
- 目标检测 → targetDetection

## 文档标题规范（Caption）
- 英文标题使用 Title Case：实词大写，冠词/介词/连词小写
- 首词和末词必须大写
- 词与词之间必须有空格
- 格式：`Fig.1 Analysis of Radar Data`

（完整词典 600+ 条，参考 SKILL.md）
EOF
```

### Hermes Agent

[Hermes](https://github.com/NousResearch/Hermes) 是 Nous Research 的本地 AI 代理系统。

**方法：System Prompt 配置**

在 Hermes 的 prompt 配置或系统提示词中添加：

```bash
# 在 hermes 配置中添加以下系统提示
cat >> hermes_prompt.txt << 'PROMPT'
## 命名规范指令
当你审查代码或文档时，请遵循以下命名规范规则：

### 代码变量
1. 检测并修正拼音命名 → 替换为英文
2. 检测并修正中文命名 → 替换为英文
3. 检测无意义缩写 → 替换为描述性命名
4. 按语言规范统一命名风格

### 文档标题（图片/表格）
1. 英文标题使用 Title Case（实词大写，虚词小写，首末词必大写）
2. 确保词间有空格
3. 统一编号格式（Fig.1 / 图1）
4. 标题用词与正文保持一致

### 术语词典
- 用户 → user/account
- 管理员 → admin
- 雷达 → radar
- 信号 → signal
- 目标检测 → targetDetection
- 多普勒 → doppler
- 激光雷达 → lidar
- 点云 → pointCloud
- 气溶胶 → aerosol
- 消光系数 → extinctionCoefficient
- 湍流强度 → turbulenceIntensity
- 退偏振比 → depolarizationRatio
（更多术语见 SKILL.md）
PROMPT
```

### OpenAI Codex CLI

[Codex CLI](https://developers.openai.com/codex) 是 OpenAI 的终端 AI 编程助手，使用 `AGENTS.md` 文件注入自定义规则。**Codex 没有斜杠命令系统，安装规则后通过 Prompt 来执行审查。**

#### 使用方法

安装规则后，在 Codex 会话中通过自然语言 prompt 触发审查：

```bash
# 审查当前目录下的变量命名
codex "审查当前目录的代码变量命名，检测拼音、中文、无意义缩写等问题，并给出修正建议"

# 审查并自动修复
codex "检查 src/ 下所有 Python 文件的变量命名，将拼音命名替换为英文"

# 审查图片标题
codex "审查论文中 Fig.1 到 Fig.10 的标题，确保使用 Title Case、词间有空格、术语规范"

# 查询术语翻译
codex "将以下中文翻译为推荐的变量名：用户管理、数据采集、信号处理、目标检测"

# 批量检查特定问题
codex "找出项目中所有包含拼音的变量名，列出文件和行号"
```

**推荐做法：** 创建一个项目的命名审查 `Makefile` 或 `justfile` 脚本：

```makefile
# Makefile
review:
	codex "审查 src/ 下所有代码的变量命名，列出拼音/中文/无意义缩写问题"

review-fix:
	codex "修复 src/ 下代码中的拼音变量命名，逐个确认修改"

caption:
	codex "审查 docs/ 中图片标题，检查 Title Case 和空格"
```

然后在终端直接运行：

```bash
make review
make review-fix
make caption
```

#### 安装方法

**方法一：全局安装（所有项目生效）**

将命名规则写入全局 `~/.codex/AGENTS.md`：

```bash
mkdir -p ~/.codex
cat > ~/.codex/AGENTS.md << 'EOF'
# 变量命名规范

## 代码命名
- 禁止拼音命名（yonghu, xitong, shezhi 等 → 替换为英文）
- 禁止中文命名
- 禁止无意义缩写（tmp, aaa, val, data1, test1 → 使用描述性命名）
- 按语言统一风格：Go(camelCase) / Python(snake_case) / JS/TS(camelCase)
- 代码中的中英文对照请使用以下术语词典

## 文档标题（Caption）
- 英文使用 Title Case：实词大写，冠词(a/an/the)/介词(in/on/at/of/for/to/with/by/from)/连词(and/or/but)小写
- 首词和末词必须大写
- 词与词之间必须有空格（如 `Fig.1 Data Fusion Flow`，不是 `Fig.1DataFusionFlow`）
- 同一文档内编号格式统一（图1 / 图 1 / Fig.1 选其一）

## 常用术语对照（节选）
### 通用编程
- 用户 → user/account
- 密码 → password
- 登录/注册 → login/register
- 配置 → config/configuration
- 缓存 → cache
- 队列 → queue

### 雷达/信号处理
- 雷达 → radar
- 目标检测 → targetDetection
- 多普勒 → doppler
- 信号 → signal
- 杂波 → clutter
- 信噪比 → signalToNoiseRatio / snr

### 气象/大气
- 气象 → meteorology
- 降水 → precipitation
- 气溶胶 → aerosol
- 湍流 → turbulence
- 风速 → windSpeed
- 温度 → temperature

### 激光雷达/LiDAR
- 激光雷达 → lidar
- 点云 → pointCloud
- 飞行时间 → timeOfFlight / tof
- 视场角 → fieldOfView / fov
EO F
```

**方法二：项目级安装**

在项目根目录创建 `AGENTS.md`（Codex 会自动从项目根目录到当前目录发现合并）：

```bash
# 项目根目录
cat > AGENTS.md << 'EOF'
# 项目命名规范

## 语言栈
本项目使用 Go + TypeScript

## 命名风格
### Go
- 变量/函数: camelCase
- 导出函数/类型: PascalCase
- 常量: ALL_CAPS
- 文件: snake_case

### TypeScript
- 变量/函数: camelCase
- 类型/接口: PascalCase
- 常量: ALL_CAPS
- 文件: kebab-case

## 项目术语
- 本项目涉及雷达信号处理
- 不使用拼音命名
- 所有中文注释需附带英文术语对照
EOF
```

**方法三：使用 AGENTS.override.md 临时覆盖**

```bash
# 在特定子目录添加覆盖规则
cat > services/processor/AGENTS.override.md << 'EOF'
# 本目录覆盖规则
- 信号处理相关变量统一使用 _sig 后缀
- 使用缩写遵循现有代码风格
EOF
```

**验证安装：**

```bash
codex --ask-for-approval never "Show which instruction files are active."
```

## 术语词典覆盖领域

| 章节 | 条目数 | 覆盖范围 |
|------|--------|---------|
| 通用编程 | 100+ | 变量命名、系统架构、设计模式 |
| 业务/管理 | 25+ | 订单、支付、会员、审批 |
| 运维/系统 | 18+ | 部署、监控、集群、环境 |
| 网络/通信 | 15+ | 协议、地址、网关、防火墙 |
| 雷达/信号处理 | 120+ | SAR/ISAR/CFAR/STAP、电子对抗、检测跟踪 |
| 激光雷达/LiDAR | 85+ | 点云、TOF、SLAM、MEMS |
| 气象/大气科学 | 125+ | 天气系统、锋面、风场、气候现象 |
| 补充（标准PDF） | 110+ | 测风雷达、气溶胶激光雷达、气象产品 |

## 自定义扩展

### 添加术语

```bash
/variable-naming add 中文术语 englishTerm
```

### 手动编辑

编辑 `SKILL.md` 中对应表格，添加行即可。

### 文件结构

```
variable-naming-skill/
├── SKILL.md          # 主技能文件（术语词典 + 规范说明）
├── README.md         # 本文件
└── pdf_terms_review.md  # 规范PDF术语提取记录
```

## 相关文件

- `~/Documents/variable-naming-skill/` — 文稿备份，可在此编辑后复制到 skill 目录
- `~/.claude/skills/variable-naming/` — Claude Code 技能目录
- `~/Documents/pdf_专业术语中英文对照表.md` — 来自规范PDF的完整术语表（352 条）
- `~/Documents/pdf_terms_review.md` — 提取记录与校验
