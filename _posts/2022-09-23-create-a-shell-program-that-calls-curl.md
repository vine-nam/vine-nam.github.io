---
title: curl 호출 shell 프로그램 만들기
categories: [dev]
comments: true
tags: [shell, osgi, bwce]
---

## Intro
사용하는 프로그램 중에 command 환경에서 http 호출로 명령을 전달할 수 있는 기능이 있다.

url은 `http://localhost:8090/bw/framework.json/osgi`이며, command 파라미터에 실행할 명령어를 전달하여 사용한다.

또한, command 파라미터 값으로 추가 파라미터들을 함께 적어줄 수 있는데, 이때 값 구분은 스페이스 공백에 해당하는 url escape code `%20`을 사용한다.

예를 들면, `http://localhost:8090/bw/framework.json/osgi?command=help%20setloglevel`와 같이 사용한다.

<br/>

## 문제
일단 url이 길다. 기억해서 사용하기에는 길어서 매번 복사해서 사용하였다. 

그리고 스페이스 공백을 `%20`으로 적어주어야 한다. 가독성도 떨어지고 상당히 번거로운 작업이다.

<br/>

## 그래서
스페이스 공백을 `%20`으로 치환해주고, curl 호출도 해주는 쉘 프로그램이 있으면 좋겠다는 생각을 하게 되었다.

다음은 그 프로그램의 전문이다.

```bash
input=$@

space="%20"

replaced=${input// /$space}

url="http://localhost:8090/bw/framework.json/osgi?command=$replaced"

curl ${url}
```

<br/>

## 코드 설명
`$@`는 입력 받은 모든 인자의 목록이다.

예를 들어, `111 222 333` 를 인자로 넘기면 `$@`의 값은 `111 222 333` 이 된다.

command.sh
```bash
echo $@
```

```bash
$ ./command.sh 111 222 333
111 222 333
```

<br/>
<br/>

입력 받은 모든 인자의 목록은 문자열로 사용하기 위해 input 변수에 저장한다.
```bash
input=$@
```

---

입력 받은 인자에서 스페이스 공백을 `%20`으로 치환한다.
```bash
space="%20"

replaced=${input// /$space}
```

---

url 변수에 command 파라미터 값을 포함한 완전한 url을 저장한 후, <br/>
curl 명령으로 url을 호출다.
```bash
url="http://localhost:8090/bw/framework.json/osgi?command=$replaced"

curl ${url}
```

<br/>

## 사용
**before**
```bash
$ curl http://localhost:8090/bw/framework.json/osgi?command=help%20setloglevel 

$ curl http://localhost:8090/bw/framework.json/osgi?command=setloglevel%20-l%20Debug%20Root
```

**after**
```bash
$ ./command.sh help setloglevel

setloglevel - Set log level of given logger
   scope: frwk
   options:
      -l, --level   Log level to set. The valid values are {Trace, Debug, Info, Error, Warn}. If not provided, log level is set to parent log level. [optional]
   parameters:
      String   Name of the Logger
```

```bash
$ ./command.sh setloglevel -l Debug Root
Log level of Logger [Root] is set to [DEBUG]
```

<br/>

상당히 사용하기 편해졌고, 보기에도 깔끔해졌다. ☺️