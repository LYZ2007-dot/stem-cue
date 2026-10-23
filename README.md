# stem-cue

**减少“看懂解析就以为自己会做”的错觉，学会独立想到解法。**

当前发布版本：**v0.1.0**，首个公开开发版本。功能与输出规范仍可能调整；版本变更见 [CHANGELOG.md](CHANGELOG.md)。根目录旧文档文件名中的 `v1.4`、`v2` 属于历史草稿编号，不对应 GitHub 发布版本。

`main` 分支包含尚未发布的更新；下文固定到 `v0.1.0` 的安装链接获取该版本快照。若需要本次更新，可从 `main` 分支下载项目并手动安装。

`stem-cue` 是一个面向大学理工科题目的 AI 解题辅导 skill，适用于高等数学、大学物理、概率统计、电路分析，以及模拟与数字电子技术等课程。

STEM 指 Science、Technology、Engineering、Mathematics（科学、技术、工程与数学）；cue 指提示或触发线索。项目通过解释“条件如何提示方法选择”，帮助学习者理解解法的由来，并逐步形成独立解题能力。

## 1. 项目解决什么问题

向 AI 询问习题后，学习者可能很快看懂完整解析，觉得“这道题我会了”。但顺着现成步骤理解答案，并不等于在没有提示时，能够自己想出同样的解法：为什么先选这个模型？这个条件为什么提示这条公式？推到这里又该如何继续？这些方法选择的依据，往往没有真正被理解。

结果是，解析一旦被拿走，或下次遇到同类题目、相同条件，学习者仍然难以独立完成解答，却曾产生“已经掌握”的错觉。

**stem-cue 的本质目的是减少这种学习错觉，帮助学习者理解解法为什么能够被想到，逐步获得独立解题与迁移能力。**

“建立条件与策略之间的联系”是实现这一目的的具体方法。`stem-cue` 沿着完整的解题过程，解释在尚未看到后续答案时，可以依据哪些线索选择下一步：

> 题干或图形条件 → 初始模型与公式 → 中间条件或算式特征 → 下一步策略 → 答案

例如：

- **电容器断开电源** → 电荷量保持不变 → 优先使用含电荷量的能量公式。
- **积分式中多个几何量同时变化** → 需要统一变量 → 利用几何关系进行代换。
- **要求统计满足限制的序列** → 识别可拆分的起始状态 → 考虑分类讨论与递推。

判断是否学会，应看学习者离开解析后，能否自行识别关键条件、说明选法理由并继续解答同类题目。`stem-cue` 旨在支持这一过程；是否真正掌握，需要通过独立尝试验证。

## 2. 主要功能

| 功能 | 说明 |
| --- | --- |
| 条件与策略映射 | 按解题顺序提取关键触发节点，解释未看到答案时可依据哪些条件选择方法，以及公式或模型的适用理由。 |
| 中间推导接力 | 将推导中出现的新条件、算式结构或矛盾作为新的触发器，解释下一步的选择。 |
| 中文详细解析 | 给出变量定义、公式代入、推导与计算步骤，落实前面提出的策略。 |
| 易错点与路径选择 | 指出常见误区；存在合适的多种解法时，并列展示通用方法与简便方法。 |
| 英文考场作答 | 原题为英文时，额外提供以公式为主、文字精简的英文作答。 |
| 手写过程诊断 | 根据上传的手写步骤，区分模型、公式适用条件和计算错误，并回溯出错节点。 |

支持输入题目文本、题目截图和手写解题图片。图片输入需要所用 AI 客户端与模型支持图像理解。

## 3. 安装方法

### 安装到 Codex

本项目以 Markdown 指令文件提供 skill，无需安装 Python 或 Node.js 依赖。需要先准备可加载本地 skills 的 Codex 环境。

正式 skill 入口为 [`skills/stem-cue/SKILL.md`](skills/stem-cue/SKILL.md)。版本归档文件保留在项目根目录；安装时使用正式入口，不需要手动改名或修改 frontmatter。

```text
项目根目录/
└── skills/
    └── stem-cue/
        └── SKILL.md
```

**从 GitHub 安装**：在 Codex 中发送以下提示词，将安装来源固定到本次发布的版本：

```text
$skill-installer 请从 https://github.com/LYZ2007-dot/stem-cue/tree/v0.1.0/skills/stem-cue 安装 stem-cue。
```

**手动安装**：

1. 下载 [v0.1.0 源码](https://github.com/LYZ2007-dot/stem-cue/archive/refs/tags/v0.1.0.zip)并解压，进入项目根目录。
2. 将 `skills/stem-cue` 整个文件夹复制到用户 skills 目录 `~/.agents/skills`。若只希望在某个项目中使用，则复制到该项目的 `.agents/skills`。
3. Codex 会自动检测 skill；如果没有出现，重启 Codex。

Windows 用户可以在项目根目录运行以下 PowerShell 命令完成安装。命令会保留仓库中的源文件；如果目标已存在，则停止，避免覆盖已有安装。

```powershell
$skillsDir = Join-Path $HOME '.agents/skills'
$skillDir = Join-Path $skillsDir 'stem-cue'
if (Test-Path -LiteralPath $skillDir) {
    throw "目标目录已存在，请先检查已有安装：$skillDir"
}

New-Item -ItemType Directory -Path $skillsDir -Force | Out-Null
Copy-Item -LiteralPath './skills/stem-cue' -Destination $skillsDir -Recurse
```

不同版本的客户端可能使用不同的发现路径，相关配置可参考 [Codex 官方 Skills 文档](https://developers.openai.com/codex/skills/)。

### 在其他 AI 对话工具中使用

如果客户端支持自定义 skills，按其规则导入 [`skills/stem-cue/SKILL.md`](skills/stem-cue/SKILL.md)。也可以将该文件的正文粘贴到对话中，要求 AI 按该规则讲题，再发送题目。这种方式仅在当前对话中提供指令，不等同于安装本地 skill。

## 4. 使用方法

安装并加载后，在 Codex 中通过 `$stem-cue` 显式调用，再输入题目或附上图片。例如：

```text
$stem-cue 请讲解这道题，帮我建立条件与解题策略之间的联系：
计算定积分 ∫₀¹ x/(1+x²) dx。
```

也可以在 skill 已加载的环境中使用自然语言请求：

```text
请用 stem-cue 对这道题做触发词拆解，并给出完整推导。
```

上传手写过程时，可附上：

```text
$stem-cue 请检查我上传的手写步骤，指出最早出错的位置，
说明错误类型，并解释该条件下应该选择什么策略。
```

发送题目时，请包含完整题干、已知量、所求量和相关图形；需要纠错时，同时提供原题与自己的步骤。

输出随输入内容变化：

| 输入情况 | 输出结构 |
| --- | --- |
| 中文题目 | 条件触发与策略映射 → 中文详细解析 |
| 英文题目 | 条件触发与策略映射 → 中文详细解析 → 英文考场极简作答 |
| 附有手写过程 | 在对应语言的输出结构上，增加手写诊断与纠错 |

策略映射部分聚焦方法选择与理由，具体代数和计算过程放在详细解析部分。简单题目无需人为增加中间节点。

## 5. 输入输出示例

### 示例一：有限长载流直导线的磁场

**输入**

```text
$stem-cue Please map the key conditions to the solution strategies, then give a detailed explanation and a concise exam-style solution in English.
A straight wire of finite length L carries current I to the right. Point P lies below the wire at perpendicular distance a. The endpoint angles β₁ and β₂ are measured from the perpendicular through P to the wire, with signed β increasing from the left endpoint to the right endpoint (β₁ < β₂). Find the magnetic field at P. All contributions point into the page (⊗).
```

**输出示例**

积分变量 β 按输入中的有符号角度约定，从 β₁ 增至 β₂。

#### 第一部分：条件触发与处理策略映射

##### 触发词/触发条件 1（初始条件 a₀）：“finite straight wire” + “magnetic field at point P”

**条件反射（a₀ → 初始公式）**：有限长电流分布不能直接套用无限长导线结果 → 从毕奥–萨伐尔定律写出电流元产生的磁场：

$$
d\mathbf{B}=\frac{\mu_0 I\,d\mathbf{l}\times\mathbf{e}_r}{4\pi r^2}.
$$

**推出的中间条件 a₁ → 下一步策略**：电流元位置 l、场点距离 r 和夹角 α 都随电流元变化 → 利用题图中的直角三角形，把它们统一改写成角度 β 的函数，再进行积分。

##### 触发词/触发条件 2（已知量与图形）：“given I, a, β₁, β₂”

题目以两端的有符号张角和垂直距离作为已知量 → 选择 β 作为积分变量，并用几何关系完成统一代换：

- cos β = a/r，因此 r = a/cos β。
- l = a tan β，因此 dl = a sec² β dβ。
- α = π/2 + β，因此 sin α = cos β。

##### 触发词/触发条件 3（图形方向符号）：“P is into the page (⊗)”

**条件反射**：用右手定则判断磁场方向。按题图，所有电流元在 P 点产生的磁场都垂直纸面向里且方向一致 → 对磁场大小作标量积分，最后注明方向。

---

#### 第二部分：中文详细解析与推导过程

**解析：**

1. **写出磁场微元**

   对导线上任一电流元 Idl，它到 P 点的距离为 r，电流方向与指向 P 的矢径夹角为 α。由毕奥–萨伐尔定律：

   $$
   dB=\frac{\mu_0 I\,dl\sin\alpha}{4\pi r^2}.
   $$

   所有微元磁场都指向纸面内（⊗），因此可以直接对大小积分。

2. **将变化量统一表示为 β**

由题设几何关系，垂直距离 a 为常量：

   $$
   \cos\beta=\frac{a}{r}
   \quad\Longrightarrow\quad
   r=\frac{a}{\cos\beta}.
   $$

   又因为 l = a tan β，对 β 求微分：

   $$
   dl=a\sec^2\beta\,d\beta
   =\frac{a}{\cos^2\beta}\,d\beta.
   $$

   角度关系 α = π/2 + β 给出：

   $$
   \sin\alpha=\sin\left(\frac{\pi}{2}+\beta\right)=\cos\beta.
   $$

3. **代入并积分**

   将三个代换式代入磁场微元公式：

   $$
   dB=
   \frac{\mu_0 I\left(\frac{a}{\cos^2\beta}\,d\beta\right)\cos\beta}
   {4\pi\left(\frac{a}{\cos\beta}\right)^2}
   =\frac{\mu_0 I}{4\pi a}\cos\beta\,d\beta.
   $$

按题设角度约定，从 β₁ 积分到 β₂：

   $$
   B=\int_{\beta_1}^{\beta_2}dB
   =\frac{\mu_0 I}{4\pi a}\int_{\beta_1}^{\beta_2}\cos\beta\,d\beta
   =\frac{\mu_0 I}{4\pi a}
   \left(\sin\beta_2-\sin\beta_1\right).
   $$

   因此磁感应强度大小为 μ₀I(sin β₂ − sin β₁)/(4πa)，方向垂直纸面向里（⊗）。

---

#### 第三部分：英文考场极简作答（Exam-Standard Solution）

By the Biot–Savart law,

$$
dB=\frac{\mu_0 I\,dl\sin\alpha}{4\pi r^2}
\quad(\text{into the page }\otimes).
$$

From the geometry,

$$
\begin{aligned}
r&=\frac{a}{\cos\beta},\\
l&=a\tan\beta
\implies dl=\frac{a}{\cos^2\beta}\,d\beta,\\
\alpha&=\frac{\pi}{2}+\beta
\implies \sin\alpha=\cos\beta.
\end{aligned}
$$

Substituting,

$$
dB=\frac{\mu_0 I}{4\pi a}\cos\beta\,d\beta.
$$

Integrating from β₁ to β₂,

$$
\mathbf{B}=
\frac{\mu_0 I}{4\pi a}
\left(\sin\beta_2-\sin\beta_1\right)
\quad(\text{into the page }\otimes).
$$

### 示例二：手写过程纠错

**输入**

```text
$stem-cue 请检查我附图中的手写步骤。
原题：平行板电容器充电后断开电源，将极板间距增大为原来的两倍。
忽略边缘效应，问电场能量如何变化？
我的手写过程：C'=C/2，因为 U 不变，所以 W'=C'U²/2=W/2。
```

**输出节选：手写诊断与纠错**

- **错误类型**：物理模型误用。断开电源后，电容器的电荷量 $Q$ 保持不变；“电压不变”的判断不适用于此处。
- **最早出错节点**：“断开电源”应触发“电荷量不变”，手写步骤在此处选择了错误的不变量。
- **正确策略**：由 $C=\varepsilon A/d$ 得 $C'=C/2$，结合 $W=Q^2/(2C)$，得到 $W'=Q^2/(2C')=2W$，即电场能量增大为原来的两倍。

该题原文为中文，因此完整输出包含策略映射、中文详解与手写诊断，无需英文作答部分。
