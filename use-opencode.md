# OpenCode 사용방법 완벽 정리
OpenCode는 오픈소스 AI 코딩 에이전트로, 터미널 인터페이스(TUI), 데스크톱 앱, 또는 IDE 확장으로 사용할 수 있습니다. 다양한 LLM 프로바이더를 지원하며, 코드 작성부터 분석까지 전방위적인 개발 지원을 제공합니다.

## **기본 실행 방법**
OpenCode를 시작하는 방법은 매우 간단합니다. 터미널에서 다음 명령어를 실행하면 현재 디렉토리를 기준으로 TUI가 실행됩니다.

```bash
opencode
```

특정 디렉토리에서 실행하고 싶다면 경로를 지정할 수 있습니다.

```bash
opencode /path/to/project
```
  

## **LLM 프로바이더 설정**
OpenCode는 75개 이상의 LLM 프로바이더를 지원합니다. 처음 사용할 때는 프로바이더를 연결해야 합니다.

### **프로바이더 연결하기**
TUI에서 `/connect` 명령어를 실행하면 사용 가능한 프로바이더 목록이 표시됩니다. 원하는 프로바이더를 선택하고 API 키를 입력하면 됩니다.

```bash
/connect
```

### **주요 프로바이더 설정 예시**
**OpenCode Zen (추천)**: OpenCode 팀이 테스트하고 검증한 모델 목록으로, 초보자에게 권장됩니다. `opencode.ai/auth`에서 인증 후 API 키를 발급받을 수 있습니다.

**Anthropic (Claude)**: Anthropic을 선택하고 Claude Pro/Max 계정으로 브라우저 인증을 진행합니다.

**OpenAI**: `platform.openai.com/api-keys`에서 API 키를 생성한 후 입력합니다.

**Ollama (로컬 모델)**: 로컬에서 모델을 실행하려면 Ollama를 설치하고 연결합니다.

설정된 프로바이더의 자격 증명은 `~/.local/share/opencode/auth.json`에 저장됩니다.
  
### **모델 선택하기**
연결된 프로바이더에서 사용 가능한 모델 목록을 확인하려면 다음 명령어를 사용합니다.

```bash
/models
```

원하는 모델을 선택하면 해당 모델로 대화를 시작할 수 있습니다.
  

## **프로젝트 초기화**
새로운 프로젝트에서 OpenCode를 사용하기 전에 초기화를 진행하는 것이 좋습니다.

```bash
/init
```

이 명령어는 프로젝트를 분석하고 프로젝트 루트에 `AGENTS.md` 파일을 생성합니다. 이 파일은 OpenCode가 프로젝트 구조와 코딩 패턴을 이해하는 데 도움을 줍니다. 키바인드는 `Ctrl+X I`입니다.
  

## **기본 사용법**

### **질문하기**
OpenCode에게 코드베이스에 대해 질문할 수 있습니다. 특정 부분을 이해하고 싶을 때 유용합니다.

```
이 함수가 어떻게 동작하는지 설명해줘
```

### **파일 참조하기**
메시지에서 `@` 기호를 사용하면 파일을 참조할 수 있습니다. 파일 이름을 입력하면 퍼지 검색이 실행되고, 해당 파일의 내용이 자동으로 대화에 추가됩니다.

```
@config.json 이 파일의 설정을 설명해줘
```

### **Bash 명령어 실행**
메시지를 `!`로 시작하면 셸 명령어를 실행할 수 있습니다. 명령어 출력이 대화에 툴 결과로 추가됩니다.

```
!npm test
```

### **기능 추가하기**
새로운 기능을 추가할 때는 먼저 계획을 세우는 것이 좋습니다.

**1단계: Plan 모드로 전환**   
`Tab` 키를 눌러 Plan 모드로 전환합니다. 화면 오른쪽 하단에 모드 표시가 나타납니다. Plan 모드에서는 OpenCode가 실제로 변경을 수행하지 않고 어떻게 구현할지 제안만 합니다.

```
로그인 기능을 추가해줘. JWT 토큰을 사용하고 Redis에 세션을 저장해야 해.
```

**2단계: 계획 검토 및 피드백**  
OpenCode가 제안한 계획을 검토하고 필요하면 추가 피드백을 줍니다. 이미지를 드래그 앤 드롭으로 추가하여 UI 디자인을 참조할 수도 있습니다.

**3단계: Build 모드로 전환하여 구현**  
다시 `Tab` 키를 눌러 Build 모드로 전환한 후 실제 구현을 요청합니다.

```
이 계획대로 구현해줘
```

### **직접 변경하기**
간단한 변경 사항은 Plan 모드 없이 바로 Build 모드에서 요청할 수 있습니다.

```
사용자 인증 미들웨어에 rate limiting을 추가해줘. 분당 10회로 제한해야 해.
```

### **변경사항 되돌리기 (Undo/Redo)**
OpenCode가 수행한 변경사항이 마음에 들지 않으면 언제든 되돌릴 수 있습니다.

```bash
/undo
```

이 명령어는 가장 최근의 사용자 메시지와 모든 후속 응답, 그리고 파일 변경사항을 되돌립니다. 키바인드는 `Ctrl+X U`입니다.

되돌린 변경사항을 다시 적용하려면:

```bash
/redo
```

키바인드는 `Ctrl+X R`입니다.

OpenCode는 내부적으로 Git을 사용하여 파일 변경을 관리하므로, 프로젝트가 Git 저장소여야 합니다.
  

## **주요 슬래시 명령어**
OpenCode TUI에서는 `/`로 시작하는 다양한 명령어를 사용할 수 있습니다.

### **세션 관리**

- `/new` 또는 `/clear`: 새로운 세션을 시작합니다. (키바인드: `Ctrl+X N`)
- `/sessions`, `/resume`, `/continue`: 세션 목록을 보고 전환합니다. (키바인드: `Ctrl+X L`)
- `/exit`, `/quit`, `/q`: OpenCode를 종료합니다. (키바인드: `Ctrl+X Q`)

### **컨텍스트 관리**
- `/compact` 또는 `/summarize`: 현재 세션을 압축합니다. 대화가 길어져 컨텍스트 제한에 도달할 때 유용합니다. (키바인드: `Ctrl+X C`)

### **설정 및 도구**
- `/models`: 사용 가능한 모델 목록을 표시합니다. (키바인드: `Ctrl+X M`)
- `/themes`: 사용 가능한 테마 목록을 표시합니다. (키바인드: `Ctrl+X T`)
- `/details`: 툴 실행 상세 정보를 토글합니다. (키바인드: `Ctrl+X D`)
- `/help`: 도움말 대화상자를 표시합니다. (키바인드: `Ctrl+X H`)

### **공유 및 내보내기**
- `/share`: 현재 세션을 공유합니다. 팀원과 협업하거나 도움을 요청할 때 유용합니다. (키바인드: `Ctrl+X S`)
- `/unshare`: 세션 공유를 해제합니다.
- `/export`: 현재 대화를 마크다운으로 내보내고 기본 에디터에서 엽니다. (키바인드: `Ctrl+X X`)
- `/editor`: 외부 에디터를 열어 메시지를 작성합니다. `EDITOR` 환경 변수에 설정된 에디터를 사용합니다. (키바인드: `Ctrl+X E`)

### **고급 기능**
- `/thinking`: 추론/사고 블록의 표시 여부를 토글합니다. 확장 사고를 지원하는 모델에서 모델의 추론 과정을 볼 수 있습니다.
  


## **에이전트(Agents) 사용하기**
OpenCode는 특정 작업에 특화된 에이전트 시스템을 제공합니다.

### **주요 에이전트**
**Build (기본)**: 모든 툴이 활성화된 기본 에이전트로, 파일 작업과 시스템 명령에 대한 전체 액세스 권한을 가집니다.

**Plan**: 계획 및 분석을 위한 제한된 에이전트입니다. 파일 편집과 bash 명령에 대한 권한이 기본적으로 `ask`로 설정되어 있어, 실제 변경 없이 코드 분석이나 변경 제안을 받을 수 있습니다.

**General (서브에이전트)**: 복잡한 질문을 연구하고 다단계 작업을 실행하는 범용 에이전트입니다. 필요시 파일 변경이 가능하며, 여러 작업 단위를 병렬로 실행할 수 있습니다.

**Explore (서브에이전트)**: 코드베이스를 탐색하는 빠른 읽기 전용 에이전트입니다. 파일을 수정할 수 없으며, 패턴으로 파일을 찾거나 키워드로 코드를 검색하거나 코드베이스에 대한 질문에 답할 때 사용합니다.

### **에이전트 전환 및 호출**
**주요 에이전트 전환**: `Tab` 키를 눌러 주요 에이전트 간 전환할 수 있습니다.

**서브에이전트 호출**: 메시지에서 `@` 멘션을 사용하여 서브에이전트를 수동으로 호출할 수 있습니다.

```
@explore src 디렉토리에서 인증 관련 파일을 찾아줘
```

**세션 간 탐색**: 서브에이전트가 자체 자식 세션을 생성할 때, `<Leader>+Right` (기본값: `Ctrl+X`를 누른 후 오른쪽 화살표) 또는 `<Leader>+Left`를 사용하여 부모 세션과 자식 세션 간 이동할 수 있습니다.

### **커스텀 에이전트 생성**
대화형 명령어로 새 에이전트를 쉽게 생성할 수 있습니다.

```bash
opencode agent create
```

이 명령어는 에이전트를 저장할 위치, 설명, 사용 가능한 툴 등을 단계별로 물어보고 마크다운 파일로 에이전트 구성을 생성합니다.
  


## **커스텀 명령어 만들기**
반복적인 작업을 위한 커스텀 명령어를 생성할 수 있습니다.

### **마크다운 파일로 명령어 정의**
`.opencode/commands/` 디렉토리에 마크다운 파일을 생성합니다.

**예시: `.opencode/commands/test.md`**

```markdown
---
description: "Run tests for a component"
---

Run all tests for the $ARGUMENTS component and show me the results.
```

이제 다음과 같이 사용할 수 있습니다.

```
/test Button
```

`$ARGUMENTS`는 "Button"으로 대체됩니다.

### **위치 매개변수 사용**
개별 인수에 액세스하려면 `$1`, `$2`, `$3` 등을 사용합니다.

```markdown
---
description: "Analyze specific file"
---

Analyze $1 file in the $2 directory with options: $3
```

사용 예:

```
/analyze config.json src {"verbose": true}
```

### **셸 명령어 출력 주입**
_!`command`_ 구문을 사용하여 bash 명령어 출력을 프롬프트에 주입할 수 있습니다.

```markdown
---
description: "Review recent changes"
---

Review the following git changes:

!`git diff HEAD~1`
```

### **파일 참조**
`@` 뒤에 파일명을 사용하여 파일을 포함할 수 있습니다.

```markdown
---
description: "Update README based on package.json"
---

Update the README.md based on the dependencies in @package.json
```
  


## **설정 파일(Config) 관리**
OpenCode는 JSON 또는 JSONC(주석이 있는 JSON) 형식의 설정 파일을 지원합니다.

### **설정 파일 위치 및 우선순위**
설정 파일은 여러 위치에 배치할 수 있으며, 다음 순서로 병합됩니다 (나중 설정이 이전 설정을 덮어씀).

1. **원격 설정** (`.well-known/opencode`) - 조직 기본값
2. **전역 설정** (`~/.config/opencode/opencode.json`) - 사용자 기본 설정
3. **커스텀 설정** (`OPENCODE_CONFIG` 환경 변수) - 커스텀 오버라이드
4. **프로젝트 설정** (프로젝트 내 `opencode.json`) - 프로젝트별 설정
5. **`.opencode` 디렉토리** - 에이전트, 명령어, 플러그인
6. **인라인 설정** (`OPENCODE_CONFIG_CONTENT` 환경 변수) - 런타임 오버라이드

### **기본 설정 예시**

**전역 설정: `~/.config/opencode/opencode.json`**

```json
{
  "theme": "opencode",
  "autoupdate": true,
  "model": "opencode/claude-sonnet-4-5"
}
```

**프로젝트 설정: `opencode.json`**

```json
{
  "model": "opencode/gpt-5.1-codex",
  "formatter": {
    "*.js": "prettier --write",
    "*.py": "black"
  }
}
```

### **환경 변수 및 파일 참조**
설정 파일에서 변수 치환을 사용할 수 있습니다.

**환경 변수**: `{env:VARIABLE_NAME}` 사용

```json
{
  "provider": {
    "openai": {
      "apiKey": "{env:OPENAI_API_KEY}"
    }
  }
}
```

**파일 내용**: `{file:path/to/file}` 사용

```json
{
  "instructions": ["{file:~/.config/opencode/rules.md}"]
}
```
   


## **툴(Tools) 관리**
OpenCode는 LLM이 코드베이스에서 작업을 수행할 수 있도록 다양한 내장 툴을 제공합니다.

### **주요 내장 툴**

**bash**: 프로젝트 환경에서 셸 명령어를 실행합니다.

**edit**: 정확한 문자열 대체를 사용하여 기존 파일을 수정합니다. LLM이 코드를 수정하는 주요 방법입니다.

**write**: 새 파일을 생성하거나 기존 파일을 덮어씁니다.

**read**: 코드베이스에서 파일 내용을 읽습니다. 대용량 파일의 경우 특정 줄 범위 읽기를 지원합니다.

**grep**: 정규 표현식을 사용하여 파일 내용을 검색합니다.

**glob**: 패턴 매칭으로 파일을 찾습니다. 예: `**/*.js` 또는 `src/**/*.ts`

**list**: 지정된 경로의 파일과 디렉토리를 나열합니다.

**webfetch**: 웹 콘텐츠를 가져옵니다. 문서를 찾거나 온라인 리소스를 연구할 때 유용합니다.

**question**: 실행 중 사용자에게 질문합니다. 요구 사항을 수집하거나 구현 선택에 대한 결정을 받을 때 유용합니다.

### **툴 권한 설정**
기본적으로 모든 툴이 활성화되어 있고 실행 권한이 필요하지 않습니다. 권한 시스템을 통해 툴 동작을 제어할 수 있습니다.

```json
{
  "permission": {
    "edit": "ask",
    "bash": "ask",
    "webfetch": "allow"
  }
}
```

권한 값:
- `"allow"`: 승인 없이 모든 작업 허용
- `"ask"`: 실행 전 승인 요청
- `"deny"`: 툴 비활성화

### **특정 bash 명령어 권한 설정**

```json
{
  "permission": {
    "bash": {
      "*": "ask",
      "git *": "allow",
      "rm -rf *": "deny"
    }
  }
}
```
  

## **MCP 서버 통합**
Model Context Protocol(MCP)을 사용하여 외부 툴과 서비스를 통합할 수 있습니다.

### **로컬 MCP 서버 추가**

```json
{
  "mcp": {
    "everything": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-everything"],
      "env": {
        "EXAMPLE_VAR": "value"
      }
    }
  }
}
```

### **원격 MCP 서버 추가**

```json
{
  "mcp": {
    "my-remote-server": {
      "type": "remote",
      "url": "https://api.example.com/mcp",
      "headers": {
        "Authorization": "Bearer {env:API_TOKEN}"
      }
    }
  }
}
```

### **MCP 서버 인증**

OAuth가 필요한 MCP 서버의 경우 수동으로 인증을 트리거할 수 있습니다.

```bash
opencode mcp auth <server-name>
```

인증 상태 확인:

```bash
opencode mcp list
```

### **MCP 서버 예시**

**Context7 (문서 검색)**

```json
{
  "mcp": {
    "context7": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@upstash/context7"],
      "env": {
        "CONTEXT7_API_KEY": "{env:CONTEXT7_API_KEY}"
      }
    }
  }
}
```

**Sentry (에러 모니터링)**

```json
{
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev"
    }
  }
}
```

설정 후 인증:

```bash
opencode mcp auth sentry
```
 

### 설치해야할 MCP ?
**꼭 설치해야 하는 MCP는 없습니다.** OpenCode는 MCP 없이도 완벽하게 작동하며, 이미 강력한 내장 툴들을 제공합니다.

OpenCode의 내장 툴만으로도 대부분의 코딩 작업을 충분히 수행할 수 있습니다. 내장 툴에는 다음이 포함됩니다:

- **bash**: 셸 명령어 실행
- **edit/write**: 파일 수정 및 생성
- **read**: 파일 읽기
- **grep**: 코드 검색
- **glob**: 파일 패턴 매칭
- **list**: 디렉토리 탐색

이것만으로도 파일 관리, 코드 편집, 검색, 실행 등 기본적인 개발 작업을 모두 수행할 수 있습니다.

#### **MCP 서버는 언제 필요할까?**
MCP 서버는 특정 워크플로우나 외부 서비스와의 통합이 필요할 때 추가하는 것이 좋습니다. 다음과 같은 경우에 유용합니다:

#### **외부 서비스 통합이 필요할 때**
**GitHub MCP**: GitHub 이슈, PR, 리포지토리를 직접 관리하고 싶을 때. 브라우저를 열지 않고 터미널에서 GitHub 작업을 하고 싶다면 유용합니다.

**Sentry MCP**: 에러 모니터링 데이터를 코딩 중에 바로 확인하고 싶을 때.

**Jira/Linear MCP**: 프로젝트 관리 도구와 연동하여 작업을 추적할 때.

#### **특수한 기능이 필요할 때**

**Sequential Thinking MCP**: 복잡한 문제를 단계별로 분석하고 아키텍처 결정을 내릴 때. 디버깅이나 시스템 설계에 도움이 됩니다.

**Playwright/Puppeteer MCP**: 웹 자동화, UI 테스팅, 스크린샷 캡처 등이 필요할 때.

**Context7 MCP**: 온라인 문서와 API 레퍼런스를 검색하여 최신 정보를 가져올 때. API 키 없이 사용 가능합니다.

#### **프로젝트 컨텍스트 관리가 중요할 때**

**Memory Bank MCP**: 대규모 프로젝트에서 프로젝트 구조와 목표를 계층적으로 정리하여 AI가 더 잘 이해하도록 할 때.

**Knowledge Graph Memory MCP**: 세션 간 프로젝트 컨텍스트를 유지하고 반복을 방지할 때.
  

### **MCP 사용 시 주의사항**

#### **컨텍스트 토큰 소비**
MCP 서버를 추가하면 각 서버의 툴 목록과 설명이 컨텍스트에 포함됩니다. 너무 많은 MCP를 설치하면 컨텍스트 윈도우를 빠르게 소진하고 토큰 비용이 증가합니다.

특히 GitHub MCP 같은 경우 많은 토큰을 소비하는 것으로 알려져 있어 주의가 필요합니다.

#### **선택적 활성화 추천**
필요한 MCP만 선택적으로 활성화하는 것이 좋습니다. 전역으로 비활성화하고 특정 에이전트에서만 활성화하는 방식을 권장합니다.

```json
{
  "permission": {
    "tool": {
      "github-*": "deny"
    }
  },
  "agent": {
    "github-agent": {
      "description": "GitHub 작업 전용 에이전트",
      "tools": {
        "github-*": true
      }
    }
  }
}
```

  

## **공유 기능**
OpenCode 대화를 공개 링크로 공유하여 팀원과 협업하거나 도움을 받을 수 있습니다.

### **공유 모드**

**Manual (기본)**: 명시적으로 `/share` 명령어를 사용해야 합니다.

```bash
/share
```

**Auto**: 모든 새 대화가 자동으로 공유됩니다.

```json
{
  "share": "auto"
}
```

**Disabled**: 공유 기능을 완전히 비활성화합니다.

```json
{
  "share": "disabled"
}
```

### **공유 해제**

```bash
/unshare
```

공유 링크를 제거하고 대화 관련 데이터를 삭제합니다.
  

## **코드 포매터 설정**

특정 파일 유형에 대한 코드 포매터를 설정할 수 있습니다.

```json
{
  "formatter": {
    "*.js": "prettier --write",
    "*.ts": "prettier --write",
    "*.py": "black",
    "*.go": "gofmt -w"
  }
}
```

OpenCode는 파일을 편집한 후 자동으로 지정된 포매터를 실행합니다.
  
  

## **키바인드 커스터마이징**
OpenCode의 키바인드를 커스터마이징할 수 있습니다.

```json
{
  "keybinds": {
    "submit": "ctrl+enter",
    "cancel": "ctrl+c",
    "new_session": "ctrl+n",
    "switch_agent": "ctrl+space"
  }
}
```

기본 리더 키는 `ctrl+x`이며, 대부분의 명령어는 리더 키와 다른 키의 조합으로 실행됩니다.
  


## **테마 변경**
OpenCode는 다양한 테마를 지원합니다.

```bash
/themes
```

설정 파일에서 테마를 지정할 수 있습니다.

```json
{
  "theme": "opencode"
}
```
  


## **컨텍스트 관리**
대화가 길어지면 컨텍스트 창이 가득 찰 수 있습니다. OpenCode는 이를 관리하는 여러 방법을 제공합니다.

### **자동 압축**

```json
{
  "compaction": {
    "auto": true,
    "prune": true
  }
}
```

- `auto`: 컨텍스트가 가득 차면 자동으로 세션을 압축합니다.
- `prune`: 토큰을 절약하기 위해 오래된 툴 출력을 제거합니다.

### **수동 압축**

```bash
/compact
```

또는 키바인드 `Ctrl+X C`를 사용합니다.
  


## **에디터 설정**
`/editor`와 `/export` 명령어는 `EDITOR` 환경 변수에 지정된 에디터를 사용합니다.

**Linux/macOS**:

```bash
export EDITOR="code --wait"
```

영구적으로 설정하려면 `~/.bashrc` 또는 `~/.zshrc`에 추가합니다.

**Windows (PowerShell)**:

```powershell
$env:EDITOR = "code --wait"
```

**인기 있는 에디터 옵션**:
- `code --wait`: Visual Studio Code
- `cursor --wait`: Cursor
- `windsurf --wait`: Windsurf
- `nvim`: Neovim
- `vim`: Vim
  


## **고급 기능**

### **규칙(Rules) 설정**
`AGENTS.md` 파일이나 별도의 규칙 파일을 통해 모델에 지침을 제공할 수 있습니다.

```json
{
  "instructions": [
    ".opencode/rules/*.md",
    "~/.config/opencode/global-rules.md"
  ]
}
```

### **LSP 서버 통합 (실험적)**
Language Server Protocol 서버를 통합하여 코드 인텔리전스 기능을 사용할 수 있습니다.

```bash
OPENCODE_EXPERIMENTAL_LSP_TOOL=true opencode
```

LSP 툴은 정의로 이동, 참조 찾기, 호버 정보, 호출 계층 등을 지원합니다.

### **파일 감시 제외 패턴**

```json
{
  "watcher": {
    "ignore": [
      "**/node_modules/**",
      "**/.git/**",
      "**/dist/**"
    ]
  }
}
```

### **자동 업데이트 제어**

```json
{
  "autoupdate": false
}
```

또는 알림만 받으려면:

```json
{
  "autoupdate": "notify"
}
```
  

## **CLI 모드**
TUI 외에도 CLI 모드로 OpenCode를 사용할 수 있습니다.

```bash
opencode run "프로젝트에 Docker 설정을 추가해줘"
```

이 모드는 스크립트나 CI/CD 파이프라인에서 유용합니다.
  
   
   
## **유용한 팁**

**1. 충분한 세부 정보 제공**: OpenCode에게 작업을 요청할 때는 팀의 주니어 개발자에게 설명하듯이 충분한 세부 정보를 제공하세요.

**2. 이미지 드래그 앤 드롭**: 터미널에 이미지를 드래그 앤 드롭하여 UI 디자인이나 다이어그램을 참조할 수 있습니다.

**3. Git 저장소 필수**: Undo/Redo 기능을 사용하려면 프로젝트가 Git 저장소여야 합니다.

**4. MCP 서버 주의**: MCP 서버는 컨텍스트에 추가되므로, 너무 많은 서버를 사용하면 컨텍스트 제한에 빠르게 도달할 수 있습니다.

**5. 에이전트별 툴 관리**: 많은 MCP 서버가 있다면 전역적으로 비활성화하고 특정 에이전트에서만 활성화하는 것이 좋습니다.

**6. 프로젝트별 설정**: 팀 전체에 적용할 설정은 프로젝트 루트의 `opencode.json`에 추가하고 Git에 커밋하세요.

**7. Plan 모드 활용**: 큰 변경을 수행하기 전에 Plan 모드에서 먼저 계획을 검토하면 실수를 줄일 수 있습니다.

이제 OpenCode를 효과적으로 사용할 준비가 되었습니다! 추가로 궁금한 사항이 있으면 `/help` 명령어를 사용하거나 공식 문서를 참조하세요.