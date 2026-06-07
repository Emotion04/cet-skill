# Reading Mode Reference

> **难度校准**: 生成任何阅读题目前，必须先完成 [difficulty-calibration.md](difficulty-calibration.md) 中规定的**生成前难度锚点**和**生成后难度自检**。本文件中的所有难度描述均以该文件中的量化指标为准。

## Goal

Generate full CET-style reading simulations that are closer to the texture of real CET reading: not only correct in format, but also closer in article density, option ambiguity, paraphrase depth, and distractor design.

The reading module should especially improve CET-6 difficulty. CET-6 reading passages should feel like adapted mid-length articles from sources such as BBC Future, Scientific American, The Guardian, The Conversation, National Geographic, or The Economist: information-rich, moderately abstract, argument-driven, and written with journalistic or science-communication texture rather than plain AI essay style.

For CET-4, keep the topic and logic more accessible, but avoid oversimplified textbook prose.

## Required Reading Subtype Choice

When the user chooses Reading Mode but does not specify the reading subtype, ask this required follow-up question in Chinese before generating any passage:

> 你想练哪一种阅读题型？
> 1. 选词填空（Banked Cloze / 15选10）
> 2. 长篇阅读 / 信息匹配（Paragraph Matching）
> 3. 仔细阅读（Careful Reading）
>
> 我会按完整仿真题型生成；题目风格基于 2015–2025 年真题趋势分析。

Do not generate a generic reading exercise before the subtype is clear.

Recognize common names and route them as follows:

- "选词填空", "十五选十", "15选10", "banked cloze" → Banked Cloze;
- "长篇阅读", "信息匹配", "段落匹配", "paragraph matching" → Paragraph Matching;
- "仔细阅读", "传统阅读", "选择题阅读", "careful reading" → Careful Reading.

## Reading Difficulty Control

难度控制以 [difficulty-calibration.md](difficulty-calibration.md) 中的量化指标为准。核心要求摘要：

- **词汇**: CET-4 80%+ 四级词表内；CET-6 须含 15–25% 六级词汇
- **句长**: CET-4 平均 14–20 词；CET-6 平均 20–30 词，含 3+ 种复杂句式
- **信息密度**: CET-4 每段 1–2 信息点；CET-6 每段 2–4 信息点，无"水分句"
- **推理深度**: CET-4 主力 L1；CET-6 L2/L3 占 50%+，配推理层级分布
- **干扰项**: CET-4 无 G1 项，至少 1 个 G3；CET-6 至少 2 个 G3/G4，主旨题含 G5
- **文本真实感**: CET-6 须含 2+ 项必选特征（让步/多重因果/研究引用/对比视角/反直觉发现），禁止五段式 AI 论文体

### Difficulty Anchor (Pre-Generation — Mandatory)

生成任何阅读文章和题目**之前**，必须先在内部列出 3 个难度锚点：

> 本题难度锚点：
> 1. {锚点 1：具体说明难度来源}
> 2. {锚点 2}
> 3. {锚点 3}

如果列不出 3 个具体锚点，**不生成，重新设计题目方向**。锚点不允许泛泛而谈（如"词汇较难"），必须具体（如"选项 B 的 partial truth 需要读者区分 study's finding vs author's own claim"）。

## Simulation Principle

For all reading subtypes, follow the exam mechanics and difficulty style:

- follow the section instruction, task format, item count, numbering, answer marking logic, text density, answer evidence, and distractor categories;
- align topic selection, sentence density, information distribution, and distractor logic with 2015–2025 trend patterns;
- default to delayed answer reveal: generate the task first, wait for the user's answers, then provide answer key and explanations.

---

## Banked Cloze Rules: 选词填空 / 15选10

Use when the user chooses 选词填空 / 十五选十 / 15选10 / Banked Cloze.

### Mandatory CET-style Task Instruction

Every Banked Cloze task must begin with this instruction exactly or with only minimal harmless formatting changes:

> In this section, there is a passage with ten blanks. You are required to select one word for each blank from a list of choices given in a word bank following the passage. Read the passage through carefully before making your choices. Each choice in the bank is identified by a letter. Please mark the corresponding letter for each item on Answer Sheet 2 with a single line through the centre. You may not use any of the words in the bank more than once.

Do not describe or generate this as ordinary fill-in-the-blank, grammar completion, multiple-choice cloze, or sentence completion.

### Mandatory Full Simulation Format

Generate exactly this structure:

1. section instruction in English;
2. one passage;
3. ten blanks embedded in the passage, numbered **26–35**;
4. one word bank after the passage;
5. fifteen lettered choices labeled **A–O**;
6. each word-bank choice must be a single English word unless a standard hyphenated word is unavoidable;
7. ask the user to submit answers in the form `26A 27F 28C...`;
8. do not reveal the answer key or explanations until the user submits answers.

### Mandatory Length and Completeness

- CET-4: **220–280 words** in the passage;
- CET-6: **250–320 words** in the passage;
- exactly **10 blanks**;
- exactly **15 choices**.

### Word Bank Design

The word bank must include:

- a realistic mixture of nouns, verbs, adjectives, adverbs, and participles;
- fifteen choices that are plausible at first glance;
- exactly ten correct choices and five distractors;
- distractors that fail for clear reasons such as wrong part of speech, wrong collocation, wrong semantic polarity, wrong tense/form, or wrong local logic;
- no repeated word forms that make the answer ambiguous;
- no word may be used more than once.

For CET-6, avoid making the correct word obvious through one-word collocation alone. At least some blanks should require sentence-level or paragraph-level semantic judgment.

---

## Paragraph Matching Rules: 长篇阅读 / 信息匹配

Use when the user chooses 长篇阅读 / 信息匹配 / 段落匹配 / Paragraph Matching.

### Mandatory CET-style Task Instruction

Every Paragraph Matching task must begin with this instruction exactly or with only minimal harmless formatting changes:

> In this section, you are going to read a passage with ten statements attached to it. Each statement contains information given in one of the paragraphs. Identify the paragraph from which the information is derived. You may choose a paragraph more than once. Each paragraph is marked with a letter. Answer the questions by marking the corresponding letter on Answer Sheet 2.

Do not generate this as ordinary reading comprehension, heading matching, paragraph summary matching, paragraph ordering, or title matching.

### Mandatory Full Simulation Format

Generate exactly this structure:

1. section instruction in English;
2. one long passage with a title;
3. paragraphs marked with letters **[A] through [O]**;
4. ten statements numbered **36–45**;
5. each statement contains information derived from one paragraph;
6. users answer by marking the paragraph letter, e.g. `36C 37A 38F...`;
7. do not reveal the answer key or explanations until the user submits answers.

### Mandatory Length and Completeness

- CET-4: **900–1200 words total**, exactly **15 lettered paragraphs [A]–[O]**;
- CET-6: **1200–1500 words total**, exactly **15 lettered paragraphs [A]–[O]**;
- each paragraph must contain **35–100 words**;
- exactly **10 statements**, numbered 36–45;
- paragraph letters may be used more than once;
- some paragraphs may be unused.

### Statement Design

Statements must:

- be paraphrases, compressions, abstractions, or logical restatements of information in one paragraph;
- not copy sentences directly from the passage;
- test information location, synonym recognition, paragraph gist, and logical equivalence;
- avoid appearing in the same order as the passage whenever possible;
- include enough semantic overlap across paragraphs to feel realistic, while keeping one best paragraph answer.

CET-6 statements should not be simple keyword matches. They should often compress a paragraph's claim, implication, contrast, or example-function into one statement.

---

## Careful Reading Rules: 仔细阅读

Use when the user chooses 仔细阅读 / 传统阅读 / 选择题阅读 / Careful Reading.

### Mandatory Full Simulation Format

Generate exactly this structure:

1. **two passages**, Passage One and Passage Two;
2. each passage has five multiple-choice questions;
3. four options per question, labeled A–D;
4. Passage One questions are numbered **46–50**;
5. Passage Two questions are numbered **51–55**;
6. ask the user to submit answers, e.g. `46A 47C 48D 49B 50A 51C 52D 53A 54B 55C`;
7. do not reveal answers until the user submits.

### Mandatory Length and Completeness

- CET-4: **350–450 words** per passage;
- CET-6: **450–550 words** per passage;
- exactly **2 passages**;
- exactly **5 questions per passage**;
- exactly **4 options per question**.

### CET-6 Careful Reading Article Standards

For CET-6 careful reading, apply all of the following:

- the passage should resemble an adapted quality-media article, not a tidy classroom essay;
- the argument should contain at least one concession, tension, qualification, or shift in perspective;
- avoid making the thesis fully explicit in the first or last sentence;
- include at least one example, study, institutional practice, historical reference, or concrete scenario that supports a broader point;
- use denser phrasing and more abstract vocabulary than CET-4;
- avoid excessive transparent connectors;
- make some relationships inferable rather than fully spelled out.

A CET-6 passage should not read as "topic sentence + explanation + example + conclusion" in every paragraph.

### Question Design

Across the two careful-reading passages, include a realistic mix of:

- main idea / best title;
- detail with paraphrase;
- inference;
- author attitude or tone;
- example function;
- vocabulary in context;
- cause-effect;
- paragraph function;
- author purpose.

Do not make all questions simple location questions.

### Option and Distractor Design

This is critical. Options must force best-answer judgment.

Correct options:

- should be semantically supported by the passage;
- should usually be paraphrased rather than copied from the evidence sentence;
- should avoid direct reuse of rare or distinctive words from the passage when possible;
- should express the underlying claim, not merely repeat a phrase.

Distractors:

- must not be obviously absurd, unrelated, or the simple opposite of the passage;
- should often contain partial truths from the passage but distort the focus, scope, cause, subject, condition, attitude, or conclusion;
- should include "reasonable but not best" choices, especially for main idea, inference, and example-function questions;
- should avoid extreme words such as only, always, never, entirely, unless the trap is deliberate and plausible;
- should be similar in length and style to the correct option.

For CET-6, at least two distractors in each question should be plausible enough that a student must return to the passage and compare details.

### Anti-Obviousness Check (Post-Generation — Mandatory, Pass/Fail)

生成后，逐项验证。**任一 Fail 项必须修正后方可输出**。

#### 选项匹配测试
- [ ] **Pass**: 正确选项不包含原文 evidence 句中的标志性词语（除非该词语不可替代）
- [ ] **Fail**: 读者只需匹配一个明显短语即可选出正确答案 → 重写正确选项为 paraphrase
- [ ] **Fail**: 两个以上错误选项与文章话题明显无关 → 给每个错误选项加入部分正确元素

#### 干扰项排除测试
- [ ] **Pass (CET-4)**: 每题至少 1 个干扰项需要读者比对原文才能排除
- [ ] **Pass (CET-6)**: 每题至少 2 个干扰项需要读者仔细比对原文细节才能排除
- [ ] **Fail**: 存在 "obviously wrong" 干扰项（极端词/反义词/话题无关项） → 重写为带有 partial truth 的陷阱

#### 推理深度测试
- [ ] **Pass (CET-4)**: L0 直接定位题 ≤ 4 题（总共 10 题），至少 1 题 L2+
- [ ] **Pass (CET-6)**: L2/L3 推理题 ≥ 5 题（总共 10 题），包括至少 1 题 L3 隐含推理
- [ ] **Fail**: CET-6 题目全部可通过对原文单句改写定位 → 增加推断题和主旨题

#### 选项风格测试
- [ ] **Pass**: 正确选项与干扰项在长度、语气、用词学术度上大致均衡
- [ ] **Fail**: 正确选项明显更长、更温和、更"学术" → 平衡选项风格
- [ ] **Fail**: 所有干扰项格式雷同（如全是原文词但逻辑错） → 多样化干扰策略

#### 文章真实感测试 (CET-6)
- [ ] **Pass**: 文章包含至少 2 项 CET-6 必选文本特征（让步/多重因果/研究引用/对比视角/反直觉发现）
- [ ] **Pass**: 文章不呈现整齐的五段式结构，论点不完全在首尾句
- [ ] **Fail**: 文章读起来像 AI 教科书范文 → 重写为更接近媒体文章的风格
- [ ] **Fail**: 每段都是 "topic sentence + explanation + example" → 打乱结构

#### 全量指标对照
- [ ] 参照 [difficulty-calibration.md](difficulty-calibration.md) 第七章 "生成后难度自检"，逐项勾选通过

After the user answers, explain:

- correct answer;
- evidence location and paraphrased evidence;
- reasoning path;
- why each wrong option is wrong;
- error type and follow-up drill.

---

## Reading Distractor Taxonomy

Use these categories when generating and explaining reading questions:

- source detail but wrong focus;
- concept substitution;
- scope too broad or too narrow;
- reversed causality;
- mismatched subject;
- attitude polarity reversal;
- time or condition mismatch;
- over-inference;
- example mistaken for conclusion;
- concession mistaken for author's final stance;
- keyword repetition without logical support;
- reasonable but not best.

## Reading / Listening Feedback Template

```markdown
## 正确答案
{answer key}

## 定位依据 / 听力线索
{evidence or listening cue}

## 推理路径
{reasoning}

## 干扰项分析
{distractor analysis}

## 错因诊断
{error types}

## 下一题训练方向
{targeted drill}
```
