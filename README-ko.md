# AI-DLC ToolSet with Subagents and Custom Skills

> 🌏 **번역**: [English](README.md)

[AI-DLC (AI-Driven Development Life Cycle)](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) 실행 환경을 Main-Subagent 아키텍처, MCP Server 연결, Custom Skill로 구성한 샘플입니다. 팀 기반 병렬 개발 워크플로우를 지원합니다.

> **이 ToolSet은 [Kiro CLI](https://kiro.dev) 전용입니다.**

> ⚠️ **Disclaimer**: 이 프로젝트는 샘플 코드이며, 프로덕션 용도가 아닙니다. 배포 전 소속 조직의 보안/법무 팀과 협업하여 보안·규제·컴플라이언스 요구사항을 충족시켜야 합니다.

## 개요

AI-DLC ToolSet은 다음을 하나로 묶은 실행 환경입니다:

- **Agent 구조** — Main Agent (오케스트레이터) + 2 Subagent (Reverse Engineering, Code Generation)
- **MCP Server 연결** — context7, aws-knowledge-mcp-server, tavily
- **Custom Skill** — requirements-generator, git-merge, drawio
- **Steering Rules** — AI-DLC core workflow 및 rule details

## 디렉토리 구조

```
.kiro/
├── agents/                    # Agent 정의 및 프롬프트
│   ├── aidlc-main.json
│   ├── aidlc-code-generation.json
│   ├── aidlc-reverse-engineering.json
│   └── prompts/
├── skills/                    # Custom Skill
│   ├── requirements-generator/
│   ├── git-merge/
│   ├── drawio/
│   └── skill-creator/        # → 아래 "skill-creator 추가" 참조
├── steering/                  # Steering rules
│   └── aws-aidlc-rules/
└── aws-aidlc-rule-details/    # 상세 rule 레퍼런스
docs/
└── skills/                    # Skill 문서
    ├── en/                    # English
    └── ko/                    # 한국어
```

## 시작하기

### 사전 요구사항

- [Kiro CLI](https://kiro.dev) 설치
- MCP Server 설정 (context7, aws-knowledge-mcp-server, tavily)

### 사용 방법

1. `.kiro/` 디렉토리를 프로젝트 루트에 복사
2. Kiro CLI 설정에서 MCP Server 연결 구성
3. Kiro CLI 세션 시작 — AI-DLC 워크플로우 사용 가능

### skill-creator 추가

`skill-creator` 스킬(Code Review 스킬 등 새로운 Custom Skill을 생성하는 데 사용)은 Anthropic이 관리하며 이 저장소에는 **포함되어 있지 않습니다**. 추가 방법:

```bash
# Anthropic skills 저장소에서 클론
git clone https://github.com/anthropics/skills.git /tmp/skills

# 대상 디렉토리를 보장한 후 skill-creator를 .kiro/skills/에 복사
mkdir -p .kiro/skills
cp -r /tmp/skills/skills/skill-creator .kiro/skills/

# 정리
rm -rf /tmp/skills
```

자세한 내용은 [skill-creator 문서](https://github.com/anthropics/skills/tree/main/skills/skill-creator)를 참조하세요.

## Custom Skill

| Skill | 설명 |
|-------|------|
| **requirements-generator** | 소스 문서(PDF, 마크다운, 이미지)를 분석하여 구조화된 요구사항 정의서와 제약사항 문서를 자동 생성 |
| **git-merge** | 여러 개발자가 서로 다른 unit을 병렬로 개발할 때 발생하는 git merge conflict 해결 |
| **drawio** | draw.io 다이어그램을 `.drawio` 파일로 생성, PNG/SVG/PDF 내보내기 지원 |

각 스킬의 상세 문서는 [docs/skills/ko/](docs/skills/ko/)를 참조하세요.

## Agent 아키텍처

```
┌─────────────────────────────────────────┐
│            aidlc-main                   │
│     (오케스트레이터 + 상태 관리)          │
└──────────┬──────────────┬───────────────┘
           │              │
           ▼              ▼
┌──────────────────┐  ┌──────────────────────┐
│ aidlc-reverse-   │  │ aidlc-code-          │
│ engineering      │  │ generation           │
│ (Brownfield      │  │ (Unit별 코드         │
│  코드 분석)       │  │  생성)               │
└──────────────────┘  └──────────────────────┘
```

- **aidlc-main**: 전체 AI-DLC 워크플로우 오케스트레이션, 상태 관리, 사용자 승인 처리
- **aidlc-reverse-engineering**: 기존 코드베이스 분석 (Brownfield 프로젝트), 8개 artifact 문서 생성
- **aidlc-code-generation**: 승인된 Code Generation Plan 실행, unit별 코드 생성

각 Subagent는 자체 context window에서 독립 실행되며, Main Agent에게는 structured summary만 반환하여 context 효율성을 유지합니다.

## 관련 리소스

- [AI-DLC 블로그](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [Open-Sourcing Adaptive Workflows for AI-DLC](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/)
- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)
- [Kiro CLI](https://kiro.dev)

## Security

자세한 내용은 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)을 참조하세요.

## License

이 프로젝트는 MIT-0 License로 제공됩니다. [LICENSE](LICENSE) 파일을 참조하세요.
