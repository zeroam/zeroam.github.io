---
title: tmux 사용법
date: 2021-03-19 22:00:00
categories:
- linux
- terminal
---

## Tmux?
{% blockquote %}
**Tmux(Termial MUltipleXer)**
하나의 창에서 동시에 여러개의 세션에 접근할 수 있도록 도와주는 command-line 프로그램
해당 세션으로부터 종료 없이 벗어날 수 있고(detach), 외부 세션을 활성화 시킨 채로 놔둘 수 있다.
{% endblockquote %}
<br/>

## Why tmux?

1. 작업을 하다보면 한 번에 여러개의 터미널에 접속해 리소스를 모니터링하거나, 코드를 테스트 해보고, 서버를 실행시키는 등의 여러가지 작업을 동시에 할 필요가 있다. 이럴 때 내 컴퓨터에서 지원하는 터미널에서 split을 하게되면 쉘에서 나갈때마다 매번 쪼개는 작업을 다시 해야하고, 번거로운데 tmux를 이용해 윈도우 창을 커스터마이징 해놓으면 필요할 때마다 해당 세션을 불러와 활용할 수 있다.
2. ssh로 외부 컴퓨터에 붙어서 작업을 하다보면 서버나 크롤링 같은 작업들을 계속 실행시켜야할 때가 있다. 이럴 때 ssh 쉘은 장시간 입력없이 두거나 네트워크 문제로 접속이 끊기면 진행 중인 작업이 종료되거나 실시간으로 로그를 확인하던 화면에서 벗어나 새로 접속해 프로그램을 다시 실행해야 한다. 이럴 때 서버측의 tmux에서 세션을 생성해두면 접속이 끊기더라도 실행 중인 프로세스를 그대로 유지할 수 있다.
3. 하나의 세션에 동시에 여러명이 접속 가능하다. 이는 개발 서버를 띄워서 실시간으로 로그를 확인하고 재실행할 때 여러명이 같이 작업을 할 수 있도록 한다.
<br/>
<br/>

## tmux 설치하기

### Linux(Ubuntu)

```bash
sudo apt install tmux
```

### MacOs

```bash
brew install tmux
```
<br/>

## 용어 정리

![Tmux Interface](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/21d04661-3012-4421-940e-39bb2f2a3b00/Screen_Shot_2021-03-18_at_9.58.48_AM.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210319%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210319T135851Z&X-Amz-Expires=86400&X-Amz-Signature=3312c6f1a71570dedb000cf5e6cc74563d50e693906fbaa9ffa95c9fc407058a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screen_Shot_2021-03-18_at_9.58.48_AM.png%22)

- `세션(session)`
    - tmux의 실행 단위이다. 여러개의 세션을 생성하고 접근할 수 있으며, 하나의 세션에서는 여러개의 window를 생성할 수 있다.
- `윈도우(window)`
    - 터미널에서 한 번에 볼 수 있는 화면으로, 하나의 세션에 여러개의 window를 추가할 수 있다.
    - ctrl + b, w 으로 해당 세션에 있는 window 목록을 확인할 수 있다.
    - `*` 가 붙은 윈도우가 현재 활성화된 윈도우를 뜻한다.
- `pane`
    - 하나의 window에서는 화면을 분할 할 수 있는데, 분할된 화면 하나를 pane 이라고 한다.
<br/>
<br/>


## 자주 사용되는 커맨드
{% blockquote %}
tmux에서는 기본 prefix으로 `Ctrl + b` 를 입력한 후 단축키를 입력해야 한다.
{% endblockquote %}

### 세션 관련
- 세션 생성 : `tmux`
- 세션 생성 (세션명 지정) : `tmux new -s <session-name>`
- 세션 빠져나오기(detach) : `ctrl + b, d`
- 세션 목록 조회 : `tmux ls`
- 세션 다시 시작 : `tmux attach -t <name>`
- 세션 종료 : `exit`
<br/>

### 윈도우 관련
- 새 윈도우 생성 : `ctrl + b, c`
- 윈도우 목록 조회 : `ctrl + b, w`
- 윈도우 이동
    - 윈도우 번호로 이동 : `ctrl + b, <num>`
    - 다음 윈도우로 이동 : `ctrl + b, n`
    - 이전 윈도우로 이동 : `ctrl + b, p`
- 윈도우 종료 : `ctrl + b, &`
<br/>

### Pane 관련
- 횡 분할 : `ctrl + b, %`
- 종 분할 : `ctrl + b, "`
- pane 이동
    - 화면에 나오는 숫자로 이동 : `ctrl + b, q`
    - 순서대로 이동 : `ctrl + b, o`
    - 방향키로 이동 : `ctrl + b, arrow`
- pane 삭제 : `ctrl + b, x`
<br/>

### copy mode
- 해당 모드에 진입하면 콘솔 내용을 스크롤하거나 복사할 수 있다
- copy mode 진입 : `ctrl + b, [`
- copy mode 빠져나오기 : `q`, `esc`
- copy mode 이동 : `arrow key`
<br/>

## 알아두면 좋은 커맨드
- 단축키 목록 조회 : `ctrl + b, ?`
- 세션 생성 (세션명, 윈도우명 지정) : `tmux new -s <session-name> -n <window-name>`
- 세션명 수정 : `ctrl + b, $`
- 윈도우명 수정 : `ctrl + b, ,`
- 활성화된 pane 확대 및 축소 : `ctrl + b, z`
<br/>
<br/>

*만약 내 컴퓨터에서 tmux를 설치해 이용할 경우, 재부팅을하면 설정해두었던 세션정보들이 모두 날라가므로 주의해야 한다.*
<br/>

---
## References
- [tmux - wikipedia](https://en.wikipedia.org/wiki/Tmux)
- [Getting Started with Tmux [Beginner's Guide]](https://linuxhandbook.com/tmux/)
- [tmux 입문자 시리즈 요약](https://edykim.com/ko/post/tmux-introductory-series-summary/)
