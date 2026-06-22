# project-resume-builder — 代码项目转求职材料生成器

> ⚡ Claude Code 技能，将代码项目反推为求职材料：精炼简历、面试弹药库、项目全景介绍  
> 诚信优先 / 零真实源码 / 可讲性验证 / 中文输出

## 为什么需要它

技术人准备面试时，常卡在"做过的项目写不出亮点"或"简历写得漂亮但面试讲不清"。本 skill 通过扫描代码仓库，自动提炼技术选型、架构设计、复杂度攻克点，产出**候选人讲得清、抗得住追问**的求职材料——不编数字、不编经历、只输出代码可证的内容。

## 快速开始

```bash
# 在 Claude Code 中调用（需先安装此 skill）
/project-resume-builder /path/to/your/project

# 可选参数
/project-resume-builder /path/to/project --role 前端
/project-resume-builder /path/to/project --module auth
```

执行后在 `<项目路径>/career-report/` 生成三份文件：
- `简历素材.md` — 给面试官看的精炼版（3-5 条 bullet）
- `面试稿.md` — 给自己备战的完整弹药库
- `项目介绍.md` — 重建项目全景认知的技术文档

## 核心特性

- **诚信红线机制** — 四层真实性判定，拒绝编造量化指标和虚构技术难点
- **可讲性止损** — 每条选型论证必须能口头复述其一句话内核，推断不出的降级为事实陈述
- **深度优先扫描** — 优先识别 1-2 个最有技术含量的亮点，而非罗列所有功能
- **岗位方向自适应** — 启发式推断前端/后端/AI 方向，可通过 `--role` 覆盖
- **零源码泄露** — 产物中不贴项目真实代码，仅在必要时使用伪代码 + 标注
- **模块深挖支持** — 通过 `--module` 参数对指定模块做更深入分析

## 安装

### 作为 Claude Code Skill 安装

1. 将本仓库克隆或下载到 Claude Code 的 skills 目录：
```bash
git clone git@github.com:Lslong/project-resume-builder.git ~/.claude/skills/project-resume-builder
```

2. 在 Claude Code 中验证安装：
```bash
/skills  # 查看已安装的 skills 列表
```

3. 调用 skill：
```bash
/project-resume-builder /path/to/your/project
```

## 使用方法

### 基本调用

```bash
# 分析当前项目
/project-resume-builder .

# 分析指定项目
/project-resume-builder ~/workspace/my-awesome-project
```

### 高级参数

- `--module <模块名>` — 对指定模块做深度挖掘
  ```bash
  /project-resume-builder . --module payment-gateway
  ```

- `--role <岗位方向>` — 覆盖自动推断的岗位方向
  ```bash
  /project-resume-builder . --role 前端
  /project-resume-builder . --role AI
  ```

### 产物说明

生成的三份文件各有分工：

| 文件 | 用途 | 特点 |
|------|------|------|
| `简历素材.md` | 给面试官看 | 精炼（3-5 条 bullet），动词开头，深度优先 |
| `面试稿.md` | 给自己备战 | 求全，包含所有技术点追问弹药库和亮点设计专区 |
| `项目介绍.md` | 重建全景认知 | 类似 CLAUDE.md，技术栈、架构、数据流、亮点索引 |

## 工作原理

```
阶段 1: 扫描与推断
└─ 只读分析代码 → 识别技术栈 + 架构 + 亮点 + 选型理由

阶段 2: 简历成稿
└─ 按 resume-template.md 产出精炼 bullet + 可讲性标注

阶段 3: 面试备战弹药库
└─ 按 interview-script.md 产出完整技术点追问库

附加产物: 项目全景介绍
└─ 复用阶段 1 扫描结果，零额外成本生成 CLAUDE.md 式文档
```

## 产物示例

### 简历素材示例片段
```markdown
- 设计基于 Redis Stream 的消息队列系统，采用 Consumer Group 实现多消费者负载均衡，解决原 Kafka 方案在小规模场景下的运维成本问题（技术选型论证：可由代码推断出 Stream 配置与消费逻辑）
- 实现 OAuth2 授权服务器，支持 Authorization Code / Client Credentials 两种授权模式，通过 JWT 令牌 + Redis 黑名单机制保障安全性（可讲性：能口头复述 JWT 失效机制）
```

### 面试稿示例片段
```markdown
#### 技术点：Redis Stream 消息队列
- **选型对比**: Kafka vs Redis Stream，权衡点：运维复杂度 vs 吞吐量
- **可能追问**: "为什么不用 Kafka?" → 回答：消息量级小（日均 10w 条），Kafka 三节点集群运维成本高...
- **白板准备**: Consumer Group ACK 机制伪代码（标注：这是伪代码，真实实现在 message-queue/consumer.go）
```

## 设计原则

1. **诚信优先于表现力** — 宁可说"这个我不确定"，不编造面试时讲不清的内容
2. **深度优先于广度** — 1-2 个讲透的亮点 > 10 个浅尝辄止的功能点
3. **可讲性止损** — 每条内容必须能由候选人口头复述其内核，推断不出的降级或删除
4. **零源码泄露** — 不贴项目真实代码，仅必要时用伪代码 + 显式标注

## 常见问题

**Q: 会不会生成虚假的项目经历？**  
A: 不会。本 skill 严格遵守"诚信红线"，所有技术选型、架构设计、复杂度攻克点必须能从代码中推断出来。对于无法从代码验证的内容，会标注"可讲性存疑，建议删除"。

**Q: 生成的量化指标（如"性能提升 50%"）是真实的吗？**  
A: 生成的产物中，量化指标**仅以占位符形式出现**（如【待测量：性能提升 XX%】），并附测量建议。绝不编造任何数字，需要候选人自行测量后填入。

**Q: 产物中会包含项目源代码吗？**  
A: 不会。简历素材中完全不含代码；面试稿中仅在"可能被要求白板手写"处使用伪代码 + 显式标注；项目介绍中默认用自然语言描述。

**Q: 如何处理团队项目中"不是我写的"功能？**  
A: 本 skill 不区分模块归属，所有有价值的功能点都默认按用户主导产出。简历末尾会附自查提示，点出明显是框架/库自带能力的条目，采纳权交用户。

**Q: 支持导出为 PDF / DOCX 吗？**  
A: 当前版本产物为 Markdown 格式，用户可自行粘贴进任何简历模板。未来版本可能支持直接导出。

## 贡献指南

欢迎贡献：
- 完善 `references/` 中的扫描规则、模板格式
- 提交真实项目的测试用例（脱敏后）
- 改进岗位方向推断启发式规则

## 许可证

MIT
