# AI 에이전트 세팅 프롬프트

Claude Code·Codex 등에 **아래 블록을 그대로 붙여넣으면** 된다.
이 기기에서 실제로 시간을 날렸던 함정들을 미리 박아둬서 엉뚱한 길로 새지 않는다.

레포를 이미 클론했다면 `AGENTS.md` 가 자동으로 읽히므로
"이 레포 클론하고 `./kp identify` 부터 해줘" 한 줄이면 충분하다.
아래는 **레포 없이 맨바닥에서** 세팅할 때 쓴다.

---

```text
Bright Data 굿즈로 받은 3키 1노브 미니 매크로 키패드를 맥북에 세팅하고 싶어.
지금 아무 키나 누르면 c(한글 상태에선 ㅊ)만 연타로 입력돼.

[1. 기기 확인 — 추측하지 말고 실제로 봐줘]
  ioreg -p IOUSB -w0 -l | grep -B20 idProduct
VID/PID 가 1189:8890 (십진수 4489:34960) 이면 WCH CH57x 계열이야.

[2. 도구]
kriomant/ch57x-keyboard-tool (Rust):
  cargo install ch57x-keyboard-tool          # Rust 없으면 rustup 먼저
  ch57x-keyboard-tool show-keys              # 쓸 수 있는 키 전체
  ch57x-keyboard-tool validate < mapping.yaml
  ch57x-keyboard-tool upload   < mapping.yaml   # sudo 불필요

[3. 미리 알려주는 함정 — 검색으로 나오는 안내 상당수가 틀렸어]
1) Python 툴이 아니라 Rust야. pip install -r requirements.txt 나
   ch57x-keyboard-tool.py 는 업스트림에 존재하지 않는 파일이다(404 확인됨).
2) minikeyboard.top / sayodevice.com / key.soku.cc 같은 웹 설정기로는
   영원히 안 잡힌다. SayoDevice 는 다른 회사의 다른 프로토콜이고, 이 기기는
   raw USB control transfer 라 WebHID 와 무관해. 여기서 시간 쓰지 마.
3) 이 레포는 validate/upload 에 stdin으로 설정을 전달해 → upload < mapping.yaml
   확인한 1.8.0은 파일 경로 인자도 지원하므로 다른 버전은 --help로 확인해.
4) 3키 모델은 시퀀스의 "첫 키에만" 모디파이어가 붙는다.
5) macOS "키보드 식별 마법사"(ANSI 고르라는 창)는 하드웨어와 무관해. 닫아도 돼.

[4. 설정 파일 형식 — 3키 + 노브 1개]
  model: ch57x-2
  orientation: normal
  rows: 1
  columns: 3
  knobs: 1
  layers:
    - buttons:
        - ["키1", "키2", "키3"]
      knobs:
        - ccw: "volumedown"
          press: "mute"
          cw: "volumeup"

[5. 매핑을 정하기 전에 내 환경부터 조사해줘 — 추측 금지]
- 내가 이미 쓰는 스크린샷/유틸 앱이 있는지 확인하고
    ps -axo comm= | grep -iE "shottr|cleanshot|monosnap|xnip"
  있으면 그 앱 설정 파일에서 실제 핫키 조합을 읽어줘.
  (Shottr 라면 defaults read cc.ffitch.shottr 의 KeyboardShortcuts_* —
   carbon modifier 비트마스크는 2048=⌥, 512=⇧, 4096=⌃, 256=⌘)
- macOS 기본 단축키는 com.apple.symbolichotkeys plist 에서 확인.
- 내 마우스가 이미 커버하는 기능(미션 컨트롤·데스크탑 전환)은 중복이니 빼줘.
- 키가 3개뿐이야. 홈포지션에서 치는 게 더 빠른 cmd-c 류는 자리 낭비니까 반대해줘.

[6. 키 순서 확정]
위·중간·아래 중 뭐가 키1인지는 문서에 없어. 먼저 ["1","2","3"] 으로 구워서
내가 눌러보고 알려주게 해줘. 그 다음에 최종 매핑을 구워.

[7. 하드웨어 안전 — 절대 매핑하면 안 되는 것]
- ctrl-cmd-power : 저장 없이 강제 재시동. 실행 중인 작업이 전부 죽는다.
- shift-cmd-q    : 로그아웃. 세션의 모든 프로세스가 죽는다.
화면 잠금은 ctrl-cmd-q 야. 끝 글자가 같은 shift-cmd-q(로그아웃)와 헷갈리지 마.

[8. 완료 기준]
upload 의 exit=0 은 "기기에 썼다"까지만 증명해. "동작한다"는 증명하지 않아.
완료라고 말하기 전에 내가 실제로 눌러본 결과를 받아서 확인해줘.
확인을 못 받았으면 무엇이 미검증인지 명시해줘.
```

---

## 결과물을 이 레포처럼 정리하고 싶다면

이어서 이렇게 요청한다.

```text
이걸 나중에 재사용·수정·커스텀 가능하게 정리해줘.
프리셋을 파일로 쪼개고, 프리셋 이름만 주면 굽는 스크립트를 만들고,
자체 git 레포로 만들어서 커밋해줘. README 에 함정도 남겨줘.
```
