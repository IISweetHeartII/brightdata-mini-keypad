# 검증 기록

검증일: **2026-09-09 (Asia/Seoul)**. 아래 결과는 작성자의 한 대의 맥과 한 대의 키패드에 해당합니다.

## 환경

| 항목 | 확인한 값 |
|---|---|
| 기기 | Bright Data 3키 + 노브 1개, WCH CH57x, USB `1189:8890` |
| macOS | `26.6.2` (`25G83`) |
| 업로드 도구 | `ch57x-keyboard-tool 1.8.0` |
| Raycast | `1.104.28` |
| Chrome | `152.0.7977.84` |
| 최종 프리셋 | `mac-work` |
| 설정 커밋 | `02af87f` 클립보드 추가 → `7033a18` 키2 재생/일시정지 추가 |

## 실제 동작과 확인 범위

| 기능 | 확인 결과 | 근거 |
|---|---|---|
| 키1: 클립보드 기록 | 확인 | 사용자가 실제 버튼으로 기록을 열어 사용. Raycast의 `⌃⌥⌘V` 호출도 UI 확인 |
| 키2: 재생/일시정지 | 확인 | 사용자가 Chrome의 YouTube에서 버튼으로 정지·재개 확인 |
| 키3: 디스플레이 끄기 | 기존 사용 확인 | 사용자가 퇴근할 때 사용하는 기능으로 확인. 이번 매핑 변경에서는 동일한 값 유지 |
| 노브: 볼륨·음소거 | 확인 | 사용자가 회전·누름 동작 확인. 이번 매핑 변경에서는 동일한 값 유지 |
| 키패드의 위·중간·아래 순서 | 미확정 | 문서에는 논리적 키1·키2·키3만 사용 |
| YouTube Music / Netflix / Disney+ / 치지직 | 미검증 | 다른 사이트·플레이어에서 미디어키 동작을 직접 확인하지 않음 |
| Windows / Linux | 미검증 | 업스트림의 지원 범위와 이 저장소의 실측 범위를 구분 |

## 실행 증거

모든 저장소 명령은 이 저장소의 기본 체크아웃에서 실행했습니다. 별도 표시한 명령만
`/tmp`를 현재 디렉터리로 사용했습니다. 이번 README 정리에서는 기기를 다시 굽지 않았습니다.

| 명령 / 검사 | 종료 코드 | 관찰 결과 |
|---|---|---|
| `./kp mac-clipboard` (설정 작업 당시) | 0 | 프리셋 업로드 성공 |
| `./kp mac-work` (설정 작업 당시) | 0 | 프리셋 업로드 성공 |
| `mac-work`와 `mac-clipboard` 설정값 비교 | 0 | 키2만 `alt-shift-s` → `play`, 나머지 설정 동일 |
| `./kp` | 0 | 프리셋 6개, 마지막 업로드 기록 `mac-work` 표시 |
| `./kp keys play` | 0 | `play` 지원 키 표시 |
| `./kp nonexistent` | 1 (예상 결과) | `없는 프리셋: nonexistent`와 목록 표시, 업로드하지 않음 |
| `/tmp`에서 이 저장소 `kp`의 절대 경로 실행 | 0 | 프리셋 목록과 `mac-work` 기록 정상 표시 |
| `bash -n kp` | 0 | Bash 문법 검사 통과 |
| 각 `presets/*.yaml`에 `ch57x-keyboard-tool validate` 실행 | 0 | 6개 모두 `config is valid` |
| `git diff --check` | 0 | 공백 오류 없음 |
| `cargo info ch57x-keyboard-tool@1.8.0` | 0 | crates.io의 `1.8.0` 배포 확인 |
| `ch57x-keyboard-tool validate --help` / `upload --help` | 0 | `[CONFIG_PATH]` 선택 인자 지원, 생략하면 stdin |
| `ch57x-keyboard-tool validate presets/mac-work.yaml` | 0 | 파일 인자로도 `config is valid` 확인 |
| README 로컬 링크·이미지·목차 앵커 검사 | 0 | 대상 파일과 섹션 존재 확인 |
| README YAML과 `mac-work.yaml`의 설정값 비교 | 0 | 예제와 실제 설정 일치 |
| GitHub Markdown API (`gh api markdown`, GFM) | 0 | 제목 1개, 배지 3개, 사진·캡처 2개, 표 5개 렌더링 확인 |

온라인 GitHub Latest 표시는 `v1.7.0`이었지만 crates.io는 `1.8.0`을 제공했다.
실제 설치 도구와 crates.io를 근거로 README의 버전을 유지했다. 기존의 "stdin 전용"
설명은 현재 `1.8.0`의 도움말·파일 인자 검증 결과에 맞춰 README와 에이전트 문서에서 수정했다.

`validate`는 문법 검사이며 기기 수용이나 실제 기능 실행을 증명하지 않습니다.
`upload`는 쓰기 성공이며 실제 기능 실행은 위의 사용자 버튼 확인으로 별도 검증했습니다.
`.current`는 마지막 업로드 기록으로, 기기에서 되읽은 값이 아닙니다.

## 공개 이미지

- [`keypad.jpg`](keypad.jpg): 기존 저장소의 기기 실물 사진.
- [`raycast-clipboard-settings.png`](raycast-clipboard-settings.png): 2026-09-09 Raycast 설정 창만 캡처.
  `⌃⌥⌘V` 및 `7 Days` 값을 확인했고, 공개 전 이미지에서 개인 클립보드 내용·대화·알림이
  포함되지 않았음을 시각적으로 확인했습니다.
