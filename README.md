# Oral skill — 专四口语思辨陪练

一个可开源的 Agent Skill：以"话题 → 参考句型 → 用户口述作答 → 三段反馈 → 继续/重答"的循环，陪练专四口试级别的英语即席思辨表达，重点纠正中式英语。

## 1. 项目解决什么问题

专四口试的即席思辨环节要求对话题快速给出"有立场、有理由"的口语表达，但自练时存在三个痛点：

- **不知从何开口**：没有话题与句型支架，面对陌生题目难以起手；
- **中式英语无人指出**：中文直译语序、中式搭配、逻辑断层、母语负迁移等问题自己很难察觉；
- **练完没有即时反馈**：自说自话式练习得不到针对性改写与点评，进步慢。

本 Skill 把"出题 → 搭支架 → 转写 → 优化 → 点评 → 多轮追问"做成一条完整陪练闭环，让每次口头作答都能得到即时、具体、可执行的反馈。

## 2. 主要功能

- **出题 + 句型支架**：每轮用英文给出一个思辨 Topic，并附带【肯定立场】【否定立场】两句纯英文参考句型（立场 + 可展开理由），可直接套用也可改造。
- **启用口令**：说出口令"**开启思辨口语训练**"即强制开启一轮全新训练；会话中途说出口令会结束当前话题、从头开始。口令之外的相关请求（如"来个话题"）照常触发。
- **忠实转写**：①【我的原句】原样保留用户口述表达，不纠正、不修饰；中英混杂时英文原样转写、中文部分用括号标注。
- **针对性优化**：②【优化句】仅生成 1 版，适配专四口语；优先修正四类中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移），拒绝浅层同义词替换，核心改动加粗，保留用户原立场。
- **简短点评**：③【简短点评】用中文 2–4 句，先点明本次最典型的中式英语问题，再说明逻辑连贯性、句式自然度与用词地道度。
- **双选项循环**：反馈后给出【继续】（同话题深度追问：挑战立场、要求证据、引入反例/对比/新情境）与【重新回答】（同话题同问题再答，循环直到输入【继续】）。
- **灵活交互**：直接开口说话视为对当前问题的新作答；说"换个话题"即开启全新话题。
- **边界处理**：作答为空或无效时提示重新作答；整段中文作答会被标注并提醒用英文。
- **零依赖**：纯对话式训练，不涉及网页抓取、PDF、图形界面或任何外部工具。

## 3. 安装方法

**前置条件**：使用支持 SKILL.md 标准的 Agent 环境（如豆包办公、Codex CLI、Claude Code 等）。

1. 将本项目的 `oral-skill/` 文件夹（含 `SKILL.md` 与 `agents/`）整体复制到任一 skills 根目录，例如 `workspace/.user_skills/`，最终路径形如 `<skills 根目录>/oral-skill/SKILL.md`；
2. 重启或刷新 Agent 会话，使其重新扫描技能；
3. 无需安装任何第三方依赖、无需配置环境变量、无脚本需执行。

## 4. 使用方法

1. **触发**：直接提出练习需求即可，例如"帮我练专四口语思辨""来个话题""口语陪练""Chinglish 纠错"；也可直接说出口令"**开启思辨口语训练**"强制开启一轮全新训练（会话中途说出口令会结束当前话题、从头开始）。
2. **作答**：AI 给出 Topic 和两句参考句型后，**用英文口头作答**；中英混杂会被原样转写（中文部分括号标注），并提示尽量用英文。
3. **反馈**：每次作答后收到三段固定反馈（①我的原句 → ②优化句 → ③简短点评）。
4. **推进**：反馈后二选一——
   - 【继续】：AI 基于当前话题提出深度追问，开启新一轮作答；
   - 【重新回答】：不换话题、不换问题，重答一次后再次获得反馈，可循环。
5. **其他指令**：想开新话题时说"换个话题"；反馈之外直接说话视为对当前问题的新作答。

## 5. 输入输出示例

**AI（出题 + 参考句型）：**
> Topic: Should college students take part-time jobs?
>
> - 【肯定立场】Taking a part-time job can be a real learning experience, because it teaches students responsibility and how to manage their time.
> - 【否定立场】I'd say college students should focus on their studies first, since a job might leave them too tired to learn well.

**用户（口头作答）：** `I think take part-time job is very good, because can earn money and learn many social experience.`

**AI（三段反馈）：**
> ①【我的原句】
> I think take part-time job is very good, because can earn money and learn many social experience.
>
> ②【优化句】
> I think **taking a part-time job is very good**, because **it lets us earn some money and gain a lot of real social experience**.
>
> ③【简短点评】
> 最典型的是中式搭配：英语里不说 learn experience，要说 gain experience，且 experience 不可数不能用 many；另外 because can earn 缺主语，take part-time job 作主语要用动名词形式。逻辑上理由能撑起观点；句式偏简单但口头可用；用词 learn experience 不地道。

**AI（选项）：** 【继续】｜【重新回答】

## 目录结构

```
Oral skill/
├── README.md                # 本文件
└── oral-skill/              # Skill 本体（机器名 oral-skill，显示名 Oral skill）
    ├── SKILL.md             # 元数据 + 完整工作流指令
    └── agents/
        └── openai.yaml      # UI 元数据（display_name / short_description / default_prompt）
```

## 校验

已通过官方 `quick_validate.py` 校验（frontmatter 格式、必填字段、命名规范均符合）。
