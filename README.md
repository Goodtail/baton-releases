# Baton Releases

> Baton의 **공개 배포 저장소**입니다. 앱 소스 코드는 이 저장소에 없습니다.

Baton은 할 일을 앱 창, Chrome 탭, tmux 페인처럼 실제 작업하던 화면과 연결하고, 클릭 한 번으로 그 자리로 돌아가게 해주는 macOS 메뉴바 앱입니다. 플로팅 HUD, 빠른 할 일 추가, 자동 시간 기록, 로컬 MCP 서버를 함께 제공합니다.

이 저장소는 Baton 설치 파일과 자동 업데이트에 필요한 공개 파일만 보관합니다. 제품 개발은 비공개 `Goodtail/Baton` 저장소에서 진행하고, 웹사이트는 비공개 `Goodtail/baton-landing` 저장소에서 관리합니다.

## 설치

1. [최신 Baton.dmg](https://github.com/Goodtail/baton-releases/releases/latest/download/Baton.dmg)를 내려받습니다.
2. DMG를 열고 Baton을 `Applications` 폴더로 옮깁니다.
3. 앱의 온보딩 안내에 따라 손쉬운 사용, 화면 기록, 자동화 권한을 허용합니다.

지원 환경은 macOS 14 Sonoma 이상입니다.

> GitHub가 자동으로 만드는 `Source code (zip/tar.gz)` 파일에는 앱이 아니라 이 저장소의 배포 메타데이터만 들어 있습니다. 설치할 때는 반드시 `Baton.dmg`를 사용하세요.

## 이 저장소가 제공하는 것

| 항목 | 역할 |
| --- | --- |
| GitHub Releases | 공증된 `Baton.dmg` 설치 파일과 Sparkle용 버전별 ZIP 배포 |
| `appcast.xml` | 설치된 앱이 새 버전을 확인하는 Sparkle 업데이트 피드 |
| `policy.json` | 실행을 허용할 최소 빌드 번호(`minBuild`) 정책 |

`appcast.xml`의 ZIP에는 EdDSA 서명이 포함되며, 설치된 Baton은 Sparkle을 통해 서명을 검증합니다. 릴리스 파이프라인은 앱과 DMG를 Developer ID로 서명하고 Apple 공증까지 마친 뒤 배포합니다.

## 저장소 구성

| 실제 저장소 | 내부에서 부르는 이름 | 책임 | 공개 여부 |
| --- | --- | --- | --- |
| `Goodtail/Baton` | `baton` | macOS 앱과 내장 `baton-mcp` 소스, 빌드·릴리스 스크립트 | 비공개 |
| `Goodtail/baton-landing` | `baton-fe` | 제품 소개, 가격, 다국어 페이지, 다운로드 연결 | 비공개 |
| `Goodtail/baton-releases` | `baton-release` | 설치 파일, Sparkle 피드, 최소 지원 빌드 정책 | 공개 |

요청이나 작업에서 `baton-release`라고 부르더라도 실제 GitHub 저장소명은 **`baton-releases`(복수형)** 입니다.

## 배포 흐름

```text
Goodtail/Baton
  └─ scripts/release.sh --publish
       ├─ 앱 빌드·서명·공증
       ├─ Baton.dmg와 업데이트 ZIP 생성
       ├─ GitHub Release 업로드
       └─ appcast.xml 갱신

Goodtail/baton-landing ── 다운로드 버튼 ──> 최신 Baton.dmg
설치된 Baton ── Sparkle ──> appcast.xml
설치된 Baton ── 지원 정책 확인 ──> policy.json
```

## 운영 시 주의사항

- 일반 릴리스는 앱 소스 저장소의 `scripts/release.sh --publish`로 발행합니다.
- `appcast.xml`은 릴리스 스크립트가 서명 정보와 파일 크기를 계산해 생성하므로 수동 편집하지 않습니다.
- `policy.json`의 `minBuild`를 올리면 이전 빌드 사용자가 앱을 계속 사용할 수 없게 될 수 있습니다. 긴급 차단이 필요한 경우에만 변경합니다.
- Sparkle 피드 URL은 이미 설치된 앱에 포함되어 있으므로 저장소를 이동하거나 파일 경로를 바꾸지 않습니다.
- 이 저장소는 공개 저장소입니다. 소스 코드, 인증서, 서명 개인키, 공증 자격 증명, 환경 파일을 커밋하지 않습니다.
