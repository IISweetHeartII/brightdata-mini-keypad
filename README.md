# 미니 매크로 키패드 (Bright Data 3키 + 노브)

USB `1189:8890` — WCH **CH57x** 계열. 설정은 기기 **온보드 메모리**에 저장되므로
한 번 구우면 드라이버 없이 어느 컴퓨터에서나 동작한다.

## 쓰는 법

```bash
./kp                 # 프리셋 목록 (→ 가 마지막으로 구운 것)
./kp mac-shottr      # 그 프리셋을 기기에 굽는다
./kp keys volume     # 기기가 지원하는 키 검색
```

처음 쓰는 기계라면 도구부터: `cargo install ch57x-keyboard-tool`

## 프리셋

| 이름 | 키1 / 키2 / 키3 | 용도 |
|---|---|---|
| `mac-shottr` | `⌥2` / `⌥⇧S` / `⌃⇧Power` | 맥북 기본. Shottr 전체·영역 캡처 + 화면만 끄기 |
| `mac-lock` | `⌥2` / `⌥⇧S` / `⌃⌘Q` | 키3만 화면 잠금(암호 요구). 자리 비울 때 |
| `portable` | `prev` / `play` / `next` | Shottr·macOS 비의존. Windows·Linux 에서도 동작 |
| `identify` | `1` / `2` / `3` | 새 기계에서 위·중간·아래 중 뭐가 키1인지 확인용 |

노브는 전 프리셋 공통: 좌 = 볼륨 down / 누름 = 음소거 / 우 = 볼륨 up.

## 커스텀하려면

`presets/` 에 `.yaml` 을 하나 더 만들면 `./kp` 목록에 자동으로 뜬다.
기존 걸 복사해서 `buttons` 줄만 바꾸는 게 제일 빠르다.

```yaml
model: ch57x-2
orientation: normal
rows: 1          # 3키 1노브는 1행 3열 + 노브1
columns: 3
knobs: 1
layers:
  - buttons:
      - ["키1", "키2", "키3"]
    knobs:
      - ccw: "volumedown"
        press: "mute"
        cw: "volumeup"
```

- 쓸 수 있는 키 이름은 `./kp keys` 로 확인 (문자·F1~F24·미디어키·마우스 클릭/휠/드래그).
- 모디파이어: `ctrl` `shift` `alt`/`opt` `cmd`/`win` — `ctrl-cmd-q` 처럼 하이픈으로 연결.
- 상위 모델(3x4 + 노브2 + 3레이어) 문법 예시는 `reference/example-mapping.yaml`.

## 알아둘 것

**키패드는 «키 조합»을 보낼 뿐, «동작»을 보내지 않는다.**
그래서 `⌥2` 는 Shottr 가 깔려 같은 핫키를 쓰는 맥에서만 캡처가 된다.
Shottr 가 없으면 그냥 `™` 가 입력된다. 노브(미디어키)만 OS 를 안 가린다.

**기기에서 설정을 되읽을 수 없다.** 도구에 download 기능이 없어서, 지금 뭐가 굽혀
있는지는 `.current` 파일 기록으로만 추적한다(git 미추적).

**C-to-C 케이블은 안 될 수 있다.** USB-C 장치는 CC 핀에 5.1kΩ 풀다운이 있어야
호스트가 인식하는데, 저가 기기는 이걸 생략하는 경우가 흔하다. 안 붙으면 고장이 아니라
저항이 없는 것 — A-to-C 로 쓰면 된다.

### ⚠️ 매핑하면 안 되는 것

- `ctrl-cmd-power` — **저장 없이 강제 재시동.** 실행 중인 작업이 전부 죽는다.
- `shift-cmd-q` — **로그아웃.** 세션의 모든 프로세스가 죽는다.
  화면 잠금은 `ctrl-cmd-q` 다. 끝이 똑같이 `Q` 라 헷갈리기 쉬우니 주의.

화면 잠금(`⌃⌘Q`)이나 화면 끄기(`⌃⇧Power`)는 **프로세스를 멈추지 않는다.**
작업을 멈추는 건 시스템 잠자기뿐이다.

## 함정 (다른 AI 답변 따라갔다가 시간 날린 것들)

1. **Python 툴이 아니라 Rust다.** `pip install -r requirements.txt` 나
   `ch57x-keyboard-tool.py` 는 존재하지 않는 파일이다(업스트림 404 확인).
2. **`minikeyboard.top` / `sayodevice.com` / `key.soku.cc` 로는 영원히 안 잡힌다.**
   SayoDevice 는 다른 회사의 다른 프로토콜이다. 이 기기는 raw USB control transfer 라
   WebHID 설정기와 무관하다.
3. **`upload` 는 파일 인자를 안 받는다 — stdin 전용.** `upload < mapping.yaml`
4. 3키 모델은 **시퀀스의 첫 키에만** 모디파이어가 붙는다.
5. macOS "키보드 식별 마법사"(ANSI 고르라는 창)는 그냥 닫아도 된다. 하드웨어와 무관.

## 출처

- https://github.com/kriomant/ch57x-keyboard-tool
