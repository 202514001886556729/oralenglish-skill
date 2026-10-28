# oralenglish-skill

专四（TEM-4）英语口语思辨陪练 Skill。AI 逐轮给出英文思辨话题与一正一反参考句型，用户口述作答后，AI 原样转写、给出 1 版口语化优化句（核心改动加粗）并简短点评中式英语，通过【继续】/【重新回答】循环推进。纯文本对话工作流，无脚本、无外部依赖。

## 项目解决什么问题

英语专业学生在备考 TEM-4 口语（及日常英语辩论）时，普遍缺少一种即时、结构化的思辨训练：

- **话题难找、观点没抓手**——面对一个话题，不知道从哪个立场说、用什么句型起手；
- **说错没人指**——口述里的中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移）无法逐句定位问题，往往只能靠自我感觉；
- **反馈太笼统**——"多练多说"式的建议不解决"我到底哪里说错了、该怎么改"。

本项目把「出题 → 口述作答 → 原样转写 → 口语化改写 → 简短点评」做成一个纯文本闭环：AI 负责出题和提供参考句型，用户负责开口说；AI 再针对每一句中式英语给出可对比的优化句与具体点评，并用【继续】/【重新回答】在同一话题下反复打磨，直到表达到位。

## 主要功能

- **逐轮训练**：每轮先输出 `Topic`，再给 2 句参考句型（`For` 肯定立场 / `Against` 否定立场，纯英文），等待用户口述作答。
- **三段式反馈**：作答后固定输出
  - ①【我的原句】原样转写，保留原始表达与错误；
  - ②【优化句】仅 1 版适配专四口语的改写，优先修正中式英语（直译语序、中式搭配、逻辑断层、母语负迁移），核心改动加粗，拒绝浅层同义词替换，不过度书面化；
  - ③【简短点评】点明本次中式英语问题，并说明逻辑连贯性、句式与用词地道度。
- **循环推进**：反馈结束后给出两个选项——
  - 【继续】：基于当前话题做深度追问（让步反驳、前提质疑、具体化、权衡取舍等），开启新一轮作答；
  - 【重新回答】：不换话题，重新作答后再次转写、优化、点评，循环直到选择【继续】。
- **纯文本、零依赖**：不调用脚本、不联网、无图形界面，支持文字对话的客户端即可直接运行。

## 安装方法

本项目采用开源 Agent Skills 目录格式（`SKILL.md` + `agents/openai.yaml`），安装即"把文件夹复制进客户端的技能目录"：

1. 将本仓库中的 `oralenglish-skill/` 整个目录（含 `SKILL.md` 与 `agents/`）复制到你的客户端技能目录。常见位置示例：
   - Claude Code / Anthropic Skills：`~/.claude/skills/`
   - OpenAI Codex / Agent Skills：`~/.codex/skills/` 或 `~/.agents/skills/`
   - 豆包（Doubao）工作区：`workspace/.user_skills/`
   - 其他支持 Agent Skills 的客户端：放入其约定的技能目录即可。SKILL.md 所在目录名需与技能名一致，即 `oralenglish-skill/`
2. 重启或新建一个对话会话，让客户端重新加载技能列表。
3. 验证安装：在对话中输入启动口令"开始练习"，若 AI 按「出题轮」格式输出 `Topic:` 与 `For:` / `Against:` 参考句型，即安装成功。

> 说明：本项目目前以本地目录形式分发，未发布到任何技能市场或注册表，因此没有 `npx skills add` 之类的远程安装命令；后续若发布到市场，再补充对应安装方式。

## 使用方法

1. 在对话中输入启动口令"开始练习"即开始训练；也可以使用"帮我练专四口语思辨"、"出个思辨话题，我来说"、"口语陪练"等触发语。
2. 会话按以下流程推进：
   - **出题轮**：AI 输出 `Topic:` 与 `For:` / `Against:` 两句参考句型，等待你作答；
   - **口述作答**：你口头回答（打字或语音转文字均可，允许夹杂少量中文，AI 会原样转写）；
   - **反馈轮**：AI 输出【我的原句】→【优化句】→【简短点评】；
   - **选路**：输入【继续】进入同话题深度追问，或输入【重新回答】重答本题；输入其它内容一律视为对当前话题的新一轮作答。
3. 想换话题时，直接告诉 AI"换个话题"或指定话题，AI 会重新出题。

## 输入输出示例

用户输入启动口令：

```
开始练习
```

AI 出题：

```
Topic: Should college students take part-time jobs?

For: I believe taking part-time jobs helps college students build practical skills that classrooms alone can't provide.
Against: In my view, part-time jobs can easily distract students from their studies, especially during exam seasons.
```

用户口述：

```
I think college students should do part-time job, because it can let us learn many things from society. Also can earn some money to reduce my family burden.
```

AI 反馈：

```
①【我的原句】
I think college students should do part-time job, because it can let us learn many things from society. Also can earn some money to reduce my family burden.

②【优化句】
I think college students should **take part-time jobs**, because they **let us gain real-world experience** and **help us earn some money to ease our family's financial burden**.

③【简短点评】
中式英语主要在搭配与句法：do part-time job 是"做兼职"的直译，应改为 take part-time jobs；learn many things from society 表达空泛且偏直译，可落为 gain real-world experience；第二句 Also can earn 缺主语，是中文式省略。两句是并列补充关系，用 and 衔接更顺。用词整体偏直译，句式简单但意思清楚，先解决搭配与主语问题再求地道度。

【继续】｜【重新回答】
```

用户输入【继续】后，AI 基于同一话题深度追问（不换话题）：

```
Topic: Some argue that part-time jobs force students to sacrifice their GPA. How would you respond?

For: That concern is fair, but I think students can manage their time well enough to keep both their grades and their part-time work.
Against: I actually agree with that worry, because many students are not mature enough to balance work and study.
```

## 目录结构

```
oralenglish-skill/
├── SKILL.md              # 技能指令：触发条件 + 完整工作流（出题轮 / 反馈轮 / 继续与重新回答）
└── agents/
    └── openai.yaml       # Agent 平台元数据（display_name / short_description / default_prompt）
```

## 适用场景与边界

- 适用：TEM-4 口试准备、英语辩论与思辨训练、中式英语纠错、口语表达升级。
- 不支持：网页抓取、PDF 处理、图形界面；本 Skill 为纯文本对话工作流。

## License

本项目采用 MIT License，详见 [LICENSE](LICENSE)。
