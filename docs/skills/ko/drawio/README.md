# Draw.io Diagram Skill

draw.io 다이어그램을 `.drawio` 파일로 생성하고, 필요 시 PNG/SVG/PDF로 내보내는 스킬입니다.

## 언제 사용하나요?

다이어그램이 필요할 때 사용합니다:
- 시스템 아키텍처 다이어그램
- 플로우차트
- ER 다이어그램
- 시퀀스 다이어그램
- 네트워크 토폴로지
- 기타 draw.io로 그릴 수 있는 모든 다이어그램

## 사용 방법

### 기본 사용 (`.drawio` 파일 생성)

```
drawio로 로그인 플로우차트 만들어줘
```

→ `login-flow.drawio` 파일이 생성되고 draw.io에서 열립니다.

### 이미지로 내보내기

```
drawio png으로 아키텍처 다이어그램 만들어줘
```

→ `architecture-diagram.drawio.png` 파일이 생성됩니다. PNG/SVG/PDF 모두 draw.io XML이 내장되어 있어 다시 편집할 수 있습니다.

### 포맷 지정 예시

| 요청 | 출력 파일 |
|------|----------|
| `drawio 플로우차트 만들어줘` | `flowchart.drawio` |
| `drawio png 로그인 플로우` | `login-flow.drawio.png` |
| `drawio svg ER 다이어그램` | `er-diagram.drawio.svg` |
| `drawio pdf 아키텍처 개요` | `architecture-overview.drawio.pdf` |

## 사전 요구사항

- 내보내기(PNG/SVG/PDF)를 사용하려면 [draw.io Desktop](https://github.com/jgraph/drawio-desktop/releases)이 설치되어 있어야 합니다
- `.drawio` 파일만 생성하는 경우에는 설치 없이도 사용 가능합니다

## 특징

- draw.io 네이티브 XML 형식으로 생성하여 완전한 편집 가능
- 내보낸 PNG/SVG/PDF에도 XML이 내장되어 draw.io에서 다시 열면 편집 가능
- 컨테이너, 스윔레인, 그룹 등 draw.io의 고급 기능 지원
