<a href="https://github.com/VoltAgent/voltagent">
    <img width="1500" height="500" alt="codex" src="https://github.com/user-attachments/assets/35f56654-e3e7-4023-a7d5-acd5215455de" />
</a>

<br />
<br />

<div align="center">
    <strong>10개 카테고리에 걸친 136개 이상의 Codex 서브에이전트를 모아둔 멋진 컬렉션.</strong>
    <br />
    <br />
</div>

   
<div align="center">
    
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![Subagent Count](https://img.shields.io/badge/subagents-136-blue?style=classic)
[![Last Update](https://img.shields.io/github/last-commit/VoltAgent/awesome-codex-subagents?label=Last%20update&style=classic)](https://github.com/VoltAgent/awesome-codex-subagents)
[![Discord](https://img.shields.io/discord/1361559153780195478.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://s.voltagent.dev/discord)

<br />


<div align="center">
    <strong>개발자를 위한 또 다른 멋진 컬렉션</strong>
    <br />
    <br />
</div>

[![Agent Skills](https://img.shields.io/static/v1?label=%E2%9A%A1%20Agent&message=Skills%2012k&color=black&style=classic)](https://github.com/VoltAgent/awesome-agent-skills)
[![Claude Code Subagents](https://img.shields.io/static/v1?label=Claude&message=Code%20Subagents%2014k&color=D97757&style=classic&logo=claude&logoColor=D97757)](https://github.com/VoltAgent/awesome-claude-code-subagents)
[![OpenClaw Skills](https://img.shields.io/static/v1?label=%F0%9F%A6%9E%20OpenClaw&message=Skills%2040k&color=f53e36&style=classic)](https://github.com/VoltAgent/awesome-openclaw-skills)
[![AI Agent Papers](https://img.shields.io/static/v1?label=arxiv&message=Agent%20Papers%20328&color=b31b1b&style=classic&logo=arxiv)](https://github.com/VoltAgent/awesome-ai-agent-papers)
</div>


# Awesome Codex Subagents

이 저장소는 특정 개발 작업을 위해 설계된 전문 AI 도우미인 [Codex Subagents](https://developers.openai.com/codex/subagents)의 결정판 컬렉션입니다. Codex 전용으로 작성되었으며 공식 문서에 맞춰 구성되어 있습니다.

## 설치

문서에 안내된 대로 Codex 커스텀 에이전트 디렉터리를 사용하세요:

- `~/.codex/agents/` 는 전역 에이전트용입니다 (모든 프로젝트에서 사용 가능)
- `.codex/agents/` 는 프로젝트 전용 에이전트용입니다 (해당 저장소에서 더 높은 우선순위)

1. 이 저장소를 클론합니다.
2. 원하는 `.toml` 에이전트 파일을 위 디렉터리 중 하나로 복사합니다.
3. 필요하다면 Codex 세션을 재시작하거나 새로고침합니다.
4. 프롬프트에서 명시적으로 위임합니다 (Codex는 커스텀 서브에이전트를 자동으로 생성하지 않습니다).

예시:
```bash
mkdir -p ~/.codex/agents
cp categories/01-core-development/backend-developer.toml ~/.codex/agents/
```

```bash
mkdir -p .codex/agents
cp categories/04-quality-security/reviewer.toml .codex/agents/
```

Codex에서 에이전트 설정을 사용할 경우, 공식 문서에 설명된 대로 `.codex/config.toml` 의 `[agents]` 아래에 유지하세요.


### 서브에이전트 저장 위치

| 유형 | 경로 | 사용 범위 | 우선순위 |
|------|------|-----------|----------|
| 프로젝트 서브에이전트 | `.codex/agents/` | 현재 프로젝트에서만 | 높음 |
| 전역 서브에이전트 | `~/.codex/agents/` | 모든 프로젝트 | 낮음 |

참고: 이름 충돌이 발생하면 프로젝트 전용 서브에이전트가 전역 서브에이전트를 덮어씁니다.


## 서브에이전트 구조

각 서브에이전트는 Codex 네이티브 `.toml` 형식을 사용합니다:

```toml
name = "subagent-name"
description = "When this agent should be invoked"
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"

[instructions]
text = """
You are a [role description and expertise areas]...

[Agent-specific checklists, patterns, and guidelines]...
"""
```

### 스마트 모델 라우팅

각 서브에이전트에는 `model` 필드가 포함되어 있으며, 품질과 비용의 균형을 맞추면서 적절한 모델로 자동 라우팅됩니다:

| 모델 | 사용 시점 | 예시 |
|-------|----------------|----------|
| `gpt-5.4` | 심층 추론 -- 아키텍처 리뷰, 보안 감사, 금융 로직 | `security-auditor`, `architect-reviewer`, `fintech-engineer` |
| `gpt-5.3-codex-spark` | 빠른 스캔, 종합, 비교적 가벼운 리서치 작업 | `search-specialist`, `docs-researcher`, `agent-installer` |

### Sandbox Mode 철학

각 서브에이전트의 `sandbox_mode` 필드는 파일시스템 접근 범위를 제어합니다:
- **읽기 전용 에이전트** (reviewer, auditor): `sandbox_mode = "read-only"` - 수정 없이 분석
- **작업공간 쓰기 에이전트** (developer, engineer): `sandbox_mode = "workspace-write"` - 파일 생성 및 수정


## 카테고리

### [01. 핵심 개발](categories/01-core-development/)

일상적인 코딩 작업을 위한 필수 개발 서브에이전트입니다.

- [**api-designer**](categories/01-core-development/api-designer.toml) - REST 및 GraphQL API 아키텍트
- [**backend-developer**](categories/01-core-development/backend-developer.toml) - 확장 가능한 API를 위한 서버사이드 전문가
- [**code-mapper**](categories/01-core-development/code-mapper.toml) - 코드 경로 매핑 및 소유권 경계 분석
- [**electron-pro**](categories/01-core-development/electron-pro.toml) - 데스크톱 애플리케이션 전문가
- [**frontend-developer**](categories/01-core-development/frontend-developer.toml) - React, Vue, Angular용 UI/UX 전문가
- [**fullstack-developer**](categories/01-core-development/fullstack-developer.toml) - 엔드투엔드 기능 개발
- [**graphql-architect**](categories/01-core-development/graphql-architect.toml) - GraphQL 스키마 및 페더레이션 전문가
- [**microservices-architect**](categories/01-core-development/microservices-architect.toml) - 분산 시스템 설계자
- [**mobile-developer**](categories/01-core-development/mobile-developer.toml) - 크로스 플랫폼 모바일 전문가
- [**ui-designer**](categories/01-core-development/ui-designer.toml) - 비주얼 디자인 및 인터랙션 전문가
- [**ui-fixer**](categories/01-core-development/ui-fixer.toml) - 재현된 UI 문제에 대한 가장 작은 안전 패치
- [**websocket-engineer**](categories/01-core-development/websocket-engineer.toml) - 실시간 통신 전문가

### [02. 언어별 전문가](categories/02-language-specialists/)

프레임워크에 대한 깊은 지식을 갖춘 언어 특화 전문가들입니다.
- [**angular-architect**](categories/02-language-specialists/angular-architect.toml) - Angular 15+ 엔터프라이즈 패턴 전문가
- [**cpp-pro**](categories/02-language-specialists/cpp-pro.toml) - C++ 성능 전문가
- [**csharp-developer**](categories/02-language-specialists/csharp-developer.toml) - .NET 생태계 전문가
- [**django-developer**](categories/02-language-specialists/django-developer.toml) - Django 4+ 웹 개발 전문가
- [**dotnet-core-expert**](categories/02-language-specialists/dotnet-core-expert.toml) - .NET 8 크로스 플랫폼 전문가
- [**dotnet-framework-4.8-expert**](categories/02-language-specialists/dotnet-framework-4.8-expert.toml) - .NET Framework 레거시 엔터프라이즈 전문가
- [**elixir-expert**](categories/02-language-specialists/elixir-expert.toml) - Elixir 및 OTP 내결함성 시스템 전문가
- [**erlang-expert**](categories/02-language-specialists/erlang-expert.toml) - Erlang/OTP 및 rebar3 엔지니어링 전문가
- [**flutter-expert**](categories/02-language-specialists/flutter-expert.toml) - Flutter 3+ 크로스 플랫폼 모바일 전문가
- [**golang-pro**](categories/02-language-specialists/golang-pro.toml) - Go 동시성 전문가
- [**java-architect**](categories/02-language-specialists/java-architect.toml) - 엔터프라이즈 Java 전문가
- [**javascript-pro**](categories/02-language-specialists/javascript-pro.toml) - JavaScript 개발 전문가
- [**kotlin-specialist**](categories/02-language-specialists/kotlin-specialist.toml) - 현대적 JVM 언어 전문가
- [**laravel-specialist**](categories/02-language-specialists/laravel-specialist.toml) - Laravel 10+ PHP 프레임워크 전문가
- [**nextjs-developer**](categories/02-language-specialists/nextjs-developer.toml) - Next.js 14+ 풀스택 전문가
- [**php-pro**](categories/02-language-specialists/php-pro.toml) - PHP 웹 개발 전문가
- [**powershell-5.1-expert**](categories/02-language-specialists/powershell-5.1-expert.toml) - Windows PowerShell 5.1 및 전체 .NET Framework 자동화 전문가
- [**powershell-7-expert**](categories/02-language-specialists/powershell-7-expert.toml) - 크로스 플랫폼 PowerShell 7+ 자동화 및 최신 .NET 전문가
- [**python-pro**](categories/02-language-specialists/python-pro.toml) - Python 생태계 전문가
- [**rails-expert**](categories/02-language-specialists/rails-expert.toml) - Rails 8.1 신속 개발 전문가
- [**react-specialist**](categories/02-language-specialists/react-specialist.toml) - React 18+ 현대적 패턴 전문가
- [**rust-engineer**](categories/02-language-specialists/rust-engineer.toml) - 시스템 프로그래밍 전문가
- [**spring-boot-engineer**](categories/02-language-specialists/spring-boot-engineer.toml) - Spring Boot 3+ 마이크로서비스 전문가
- [**sql-pro**](categories/02-language-specialists/sql-pro.toml) - 데이터베이스 쿼리 전문가
- [**swift-expert**](categories/02-language-specialists/swift-expert.toml) - iOS 및 macOS 전문가
- [**typescript-pro**](categories/02-language-specialists/typescript-pro.toml) - TypeScript 전문가
- [**vue-expert**](categories/02-language-specialists/vue-expert.toml) - Vue 3 Composition API 전문가


### [03. 인프라](categories/03-infrastructure/)

DevOps, 클라우드, 배포 전문가들입니다.

- [**azure-infra-engineer**](categories/03-infrastructure/azure-infra-engineer.toml) - Azure 인프라 및 Az PowerShell 자동화 전문가
- [**cloud-architect**](categories/03-infrastructure/cloud-architect.toml) - AWS/GCP/Azure 전문가
- [**database-administrator**](categories/03-infrastructure/database-administrator.toml) - 데이터베이스 관리 전문가
- [**deployment-engineer**](categories/03-infrastructure/deployment-engineer.toml) - 배포 자동화 전문가
- [**devops-engineer**](categories/03-infrastructure/devops-engineer.toml) - CI/CD 및 자동화 전문가
- [**devops-incident-responder**](categories/03-infrastructure/devops-incident-responder.toml) - DevOps 인시던트 관리
- [**docker-expert**](categories/03-infrastructure/docker-expert.toml) - Docker 컨테이너화 및 최적화 전문가
- [**incident-responder**](categories/03-infrastructure/incident-responder.toml) - 시스템 인시던트 대응 전문가
- [**kubernetes-specialist**](categories/03-infrastructure/kubernetes-specialist.toml) - 컨테이너 오케스트레이션 전문가
- [**network-engineer**](categories/03-infrastructure/network-engineer.toml) - 네트워크 인프라 전문가
- [**platform-engineer**](categories/03-infrastructure/platform-engineer.toml) - 플랫폼 아키텍처 전문가
- [**security-engineer**](categories/03-infrastructure/security-engineer.toml) - 인프라 보안 전문가
- [**sre-engineer**](categories/03-infrastructure/sre-engineer.toml) - 사이트 신뢰성 엔지니어링 전문가
- [**terraform-engineer**](categories/03-infrastructure/terraform-engineer.toml) - Infrastructure as Code 전문가
- [**terragrunt-expert**](categories/03-infrastructure/terragrunt-expert.toml) - Terragrunt 오케스트레이션 및 DRY IaC 전문가
- [**windows-infra-admin**](categories/03-infrastructure/windows-infra-admin.toml) - Active Directory, DNS, DHCP, GPO 자동화 전문가

<details>
<summary><b>04. 품질 및 보안</b> — 테스트, 보안, 코드 품질 전문가 (16개 에이전트)</summary>

### [04. 품질 및 보안](categories/04-quality-security/)

- [**accessibility-tester**](categories/04-quality-security/accessibility-tester.toml) - 접근성(A11y) 규정 준수 전문가
- [**ad-security-reviewer**](categories/04-quality-security/ad-security-reviewer.toml) - Active Directory 보안 및 GPO 감사 전문가
- [**architect-reviewer**](categories/04-quality-security/architect-reviewer.toml) - 아키텍처 리뷰 전문가
- [**browser-debugger**](categories/04-quality-security/browser-debugger.toml) - 브라우저 기반 재현 및 클라이언트 사이드 디버깅
- [**chaos-engineer**](categories/04-quality-security/chaos-engineer.toml) - 시스템 복원력 테스트 전문가
- [**code-reviewer**](categories/04-quality-security/code-reviewer.toml) - 코드 품질 수호자
- [**compliance-auditor**](categories/04-quality-security/compliance-auditor.toml) - 규제 준수 전문가
- [**debugger**](categories/04-quality-security/debugger.toml) - 고급 디버깅 전문가
- [**error-detective**](categories/04-quality-security/error-detective.toml) - 오류 분석 및 해결 전문가
- [**penetration-tester**](categories/04-quality-security/penetration-tester.toml) - 윤리적 해킹 전문가
- [**performance-engineer**](categories/04-quality-security/performance-engineer.toml) - 성능 최적화 전문가
- [**powershell-security-hardening**](categories/04-quality-security/powershell-security-hardening.toml) - PowerShell 보안 강화 및 규정 준수 전문가
- [**qa-expert**](categories/04-quality-security/qa-expert.toml) - 테스트 자동화 전문가
- [**reviewer**](categories/04-quality-security/reviewer.toml) - 정확성, 보안, 회귀를 점검하는 PR 스타일 리뷰
- [**security-auditor**](categories/04-quality-security/security-auditor.toml) - 보안 취약점 전문가
- [**test-automator**](categories/04-quality-security/test-automator.toml) - 테스트 자동화 프레임워크 전문가

</details>

<details>
<summary><b>05. 데이터 및 AI</b> — 데이터 엔지니어링, ML, AI 전문가 (12개 에이전트)</summary>

### [05. 데이터 및 AI](categories/05-data-ai/)

- [**ai-engineer**](categories/05-data-ai/ai-engineer.toml) - AI 시스템 설계 및 배포 전문가
- [**data-analyst**](categories/05-data-ai/data-analyst.toml) - 데이터 인사이트 및 시각화 전문가
- [**data-engineer**](categories/05-data-ai/data-engineer.toml) - 데이터 파이프라인 아키텍트
- [**data-scientist**](categories/05-data-ai/data-scientist.toml) - 분석 및 인사이트 전문가
- [**database-optimizer**](categories/05-data-ai/database-optimizer.toml) - 데이터베이스 성능 전문가
- [**llm-architect**](categories/05-data-ai/llm-architect.toml) - 대규모 언어 모델 아키텍트
- [**machine-learning-engineer**](categories/05-data-ai/machine-learning-engineer.toml) - 머신러닝 시스템 전문가
- [**ml-engineer**](categories/05-data-ai/ml-engineer.toml) - 머신러닝 전문가
- [**mlops-engineer**](categories/05-data-ai/mlops-engineer.toml) - MLOps 및 모델 배포 전문가
- [**nlp-engineer**](categories/05-data-ai/nlp-engineer.toml) - 자연어 처리 전문가
- [**postgres-pro**](categories/05-data-ai/postgres-pro.toml) - PostgreSQL 데이터베이스 전문가
- [**prompt-engineer**](categories/05-data-ai/prompt-engineer.toml) - 프롬프트 최적화 전문가

</details>

<details>
<summary><b>06. 개발자 경험</b> — 툴링 및 개발자 생산성 전문가 (13개 에이전트)</summary>

### [06. 개발자 경험](categories/06-developer-experience/)

- [**build-engineer**](categories/06-developer-experience/build-engineer.toml) - 빌드 시스템 전문가
- [**cli-developer**](categories/06-developer-experience/cli-developer.toml) - 명령줄 도구 제작자
- [**dependency-manager**](categories/06-developer-experience/dependency-manager.toml) - 패키지 및 의존성 전문가
- [**documentation-engineer**](categories/06-developer-experience/documentation-engineer.toml) - 기술 문서화 전문가
- [**dx-optimizer**](categories/06-developer-experience/dx-optimizer.toml) - 개발자 경험 최적화 전문가
- [**git-workflow-manager**](categories/06-developer-experience/git-workflow-manager.toml) - Git 워크플로 및 브랜칭 전문가
- [**legacy-modernizer**](categories/06-developer-experience/legacy-modernizer.toml) - 레거시 코드 현대화 전문가
- [**mcp-developer**](categories/06-developer-experience/mcp-developer.toml) - Model Context Protocol 전문가
- [**powershell-module-architect**](categories/06-developer-experience/powershell-module-architect.toml) - PowerShell 모듈 및 프로필 아키텍처 전문가
- [**powershell-ui-architect**](categories/06-developer-experience/powershell-ui-architect.toml) - WinForms, WPF, Metro 프레임워크, TUI용 PowerShell UI/UX 전문가
- [**refactoring-specialist**](categories/06-developer-experience/refactoring-specialist.toml) - 코드 리팩터링 전문가
- [**slack-expert**](categories/06-developer-experience/slack-expert.toml) - Slack 플랫폼 및 @slack/bolt 전문가
- [**tooling-engineer**](categories/06-developer-experience/tooling-engineer.toml) - 개발자 도구 전문가

</details>

<details>
<summary><b>07. 특화 도메인</b> — 도메인 특화 기술 전문가 (12개 에이전트)</summary>

### [07. 특화 도메인](categories/07-specialized-domains/)

- [**api-documenter**](categories/07-specialized-domains/api-documenter.toml) - API 문서화 전문가
- [**blockchain-developer**](categories/07-specialized-domains/blockchain-developer.toml) - Web3 및 크립토 전문가
- [**embedded-systems**](categories/07-specialized-domains/embedded-systems.toml) - 임베디드 및 실시간 시스템 전문가
- [**fintech-engineer**](categories/07-specialized-domains/fintech-engineer.toml) - 핀테크 전문가
- [**game-developer**](categories/07-specialized-domains/game-developer.toml) - 게임 개발 전문가
- [**iot-engineer**](categories/07-specialized-domains/iot-engineer.toml) - IoT 시스템 개발자
- [**m365-admin**](categories/07-specialized-domains/m365-admin.toml) - Microsoft 365, Exchange Online, Teams, SharePoint 관리 전문가
- [**mobile-app-developer**](categories/07-specialized-domains/mobile-app-developer.toml) - 모바일 애플리케이션 전문가
- [**payment-integration**](categories/07-specialized-domains/payment-integration.toml) - 결제 시스템 전문가
- [**quant-analyst**](categories/07-specialized-domains/quant-analyst.toml) - 정량 분석 전문가
- [**risk-manager**](categories/07-specialized-domains/risk-manager.toml) - 리스크 평가 및 관리 전문가
- [**seo-specialist**](categories/07-specialized-domains/seo-specialist.toml) - 검색 엔진 최적화 전문가

</details>

<details>
<summary><b>08. 비즈니스 및 제품</b> — 제품 관리 및 비즈니스 분석 (11개 에이전트)</summary>

### [08. 비즈니스 및 제품](categories/08-business-product/)

- [**business-analyst**](categories/08-business-product/business-analyst.toml) - 요구사항 전문가
- [**content-marketer**](categories/08-business-product/content-marketer.toml) - 콘텐츠 마케팅 전문가
- [**customer-success-manager**](categories/08-business-product/customer-success-manager.toml) - 고객 성공 전문가
- [**legal-advisor**](categories/08-business-product/legal-advisor.toml) - 법률 및 규정 준수 전문가
- [**product-manager**](categories/08-business-product/product-manager.toml) - 제품 전략 전문가
- [**project-manager**](categories/08-business-product/project-manager.toml) - 프로젝트 관리 전문가
- [**sales-engineer**](categories/08-business-product/sales-engineer.toml) - 기술 영업 전문가
- [**scrum-master**](categories/08-business-product/scrum-master.toml) - 애자일 방법론 전문가
- [**technical-writer**](categories/08-business-product/technical-writer.toml) - 기술 문서 전문가
- [**ux-researcher**](categories/08-business-product/ux-researcher.toml) - 사용자 리서치 전문가
- [**wordpress-master**](categories/08-business-product/wordpress-master.toml) - WordPress 개발 및 최적화 전문가

</details>

<details>
<summary><b>09. 메타 및 오케스트레이션</b> — 에이전트 조정 및 메타 프로그래밍 (12개 에이전트)</summary>

### [09. 메타 및 오케스트레이션](categories/09-meta-orchestration/)

- [**agent-installer**](categories/09-meta-orchestration/agent-installer.toml) - GitHub를 통해 이 저장소의 에이전트를 탐색하고 설치
- [**agent-organizer**](categories/09-meta-orchestration/agent-organizer.toml) - 멀티 에이전트 코디네이터
- [**context-manager**](categories/09-meta-orchestration/context-manager.toml) - 컨텍스트 최적화 전문가
- [**error-coordinator**](categories/09-meta-orchestration/error-coordinator.toml) - 오류 처리 및 복구 전문가
- [**it-ops-orchestrator**](categories/09-meta-orchestration/it-ops-orchestrator.toml) - IT 운영 워크플로 오케스트레이션 전문가
- [**knowledge-synthesizer**](categories/09-meta-orchestration/knowledge-synthesizer.toml) - 지식 통합 전문가
- [**multi-agent-coordinator**](categories/09-meta-orchestration/multi-agent-coordinator.toml) - 고급 멀티 에이전트 오케스트레이션
- [**performance-monitor**](categories/09-meta-orchestration/performance-monitor.toml) - 에이전트 성능 최적화
- [**pied-piper**](https://github.com/sathish316/pied-piper/) - 반복적인 SDLC 워크플로를 위한 AI 서브에이전트 팀 오케스트레이션
- [**task-distributor**](categories/09-meta-orchestration/task-distributor.toml) - 작업 할당 전문가
- [**workflow-orchestrator**](categories/09-meta-orchestration/workflow-orchestrator.toml) - 복잡한 워크플로 자동화

</details>

<details>
<summary><b>10. 리서치 및 분석</b> — 리서치, 검색, 분석 전문가 (7개 에이전트)</summary>

### [10. 리서치 및 분석](categories/10-research-analysis/)

- [**competitive-analyst**](categories/10-research-analysis/competitive-analyst.toml) - 경쟁 정보 분석 전문가
- [**data-researcher**](categories/10-research-analysis/data-researcher.toml) - 데이터 발굴 및 분석 전문가
- [**docs-researcher**](categories/10-research-analysis/docs-researcher.toml) - 문서 기반 API 및 프레임워크 검증
- [**market-researcher**](categories/10-research-analysis/market-researcher.toml) - 시장 분석 및 소비자 인사이트
- [**research-analyst**](categories/10-research-analysis/research-analyst.toml) - 종합 리서치 전문가
- [**search-specialist**](categories/10-research-analysis/search-specialist.toml) - 고급 정보 검색 전문가
- [**trend-analyst**](categories/10-research-analysis/trend-analyst.toml) - 신흥 트렌드 및 예측 전문가

</details>

## 서브에이전트 이해하기

서브에이전트는 작업 특화 전문성을 제공함으로써 Codex의 역량을 확장하는 전문 AI 도우미입니다. 특정 유형의 작업을 만났을 때 Codex가 호출할 수 있는 전담 도우미 역할을 합니다.

### 서브에이전트의 특별한 점은 무엇인가요?

**독립적인 컨텍스트 윈도**
각 서브에이전트는 서로 격리된 자체 컨텍스트 공간에서 동작하므로, 서로 다른 작업 간 오염을 방지하고 주 대화 스레드의 명확성을 유지합니다.

**도메인 특화 지능**
서브에이전트에는 각 전문 영역에 맞춘 정교한 지침이 포함되어 있어, 특화 작업에서 더 우수한 성능을 냅니다.

**프로젝트 간 공유**
서브에이전트를 만든 뒤에는 다양한 프로젝트에서 활용할 수 있으며, 일관된 개발 방식을 유지하기 위해 팀원들과 공유할 수 있습니다.

**명시적 위임**
Codex는 서브에이전트를 자동으로 생성하지 않습니다. 어떤 에이전트를 생성할지, 작업을 어떻게 나눌지, 결과물을 어떤 형태로 받을지를 명시하는 위임 프롬프트를 사용하세요.

### 핵심 장점

- **메모리 효율성**: 격리된 컨텍스트 덕분에 주 대화가 작업별 세부사항으로 어수선해지지 않습니다
- **정확도 향상**: 특화된 프롬프트와 설정으로 특정 도메인에서 더 나은 결과를 얻을 수 있습니다
- **워크플로 일관성**: 팀 전체가 서브에이전트를 공유함으로써 공통 작업에 대해 일관된 접근 방식을 유지할 수 있습니다
- **Codex 네이티브**: 공식 Codex 서브에이전트 문서에 맞춘 `.toml` 에이전트 파일을 사용합니다

### 예시 워크플로

**PR 리뷰 워크플로:**
```text
Review this branch with parallel subagents. Have reviewer look for correctness, security, and missing tests. Have docs_researcher verify the framework APIs this patch depends on. Wait for both and summarize the findings with file references.
```

**버그 조사 워크플로:**
```text
Investigate the broken settings flow. Have code_mapper trace the owning code paths, browser_debugger reproduce the bug in the browser, and frontend_developer propose the smallest fix after the failure is understood. Wait for the read-heavy agents first, then continue.
```

**저장소 탐색 및 계획 수립 워크플로:**
```text
Use search_specialist to locate the code related to payment retries, knowledge_synthesizer to summarize the current design, and refactoring_specialist to propose a minimal refactor plan. Return a concrete action list.
```
## 기여하기

기여를 환영합니다! 가이드라인은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참고하세요.

- 새로운 서브에이전트를 PR로 제출
- 기존 정의 개선
- 이슈 및 버그 제보


## 라이선스

MIT 라이선스 - [LICENSE](LICENSE) 참조

이 저장소는 유지관리자와 커뮤니티가 함께 기여한 서브에이전트 정의를 선별해 모아둔 컬렉션입니다. 모든 서브에이전트는 어떠한 보증도 없이 "있는 그대로" 제공됩니다. 우리는 어떤 서브에이전트의 보안성이나 정확성도 감사하거나 보증하지 않습니다. 사용 전에 반드시 검토해야 하며, 유지관리자는 사용으로 인해 발생하는 어떠한 문제에 대해서도 책임을 지지 않습니다.

목록에 올라온 서브에이전트에 문제가 있거나 자신의 기여분 삭제를 원한다면, 이 저장소에 이슈를 열어 주세요. 신속히 대응하겠습니다.
