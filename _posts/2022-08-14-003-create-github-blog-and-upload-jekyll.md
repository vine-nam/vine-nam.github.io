---
title: 3. github blog 만들고 지킬(Jekyll) 올리기
categories: [blog]
comments: true
---

### 1.  gitHub에서 새 레파지토리를 생성해줍니다.

이름은 `유저이름.github.io` 와 같이 만들어 주어야 합니다.

![](../images/blog/Pasted%20image%2020220814155359.png)

### 2. [이전에 만든 지킬 블로그](../2022-08/001-create-jekyll-blog)를 github 레파지토리와 연동합니다.

다음 명령어들은 지킬 블로그 파일이 있는 경로에서 실행해 줍니다.
ex) D:\\_JIA\\WorkSpaces\\myblog

1. git repository로 초기화 합니다.

	```sh
	git init
	```

2. 파일을 스테이징에 추가하고 커밋합니다.

	```sh
	git add --all
	git commit -m "블로그 생성"
	```

3. 브랜치를 만들고 github에 파일을 올려줍니다.

	```sh
	git branch -M main
	git remote add origin https://github.com/유저이름/유저이름.github.io.git
	git push -u origin main
	```

4. 이제 브라우저로 **https://유저이름.github.io/** 에 접속합니다. 

	(처음에 접속하면 바로 접속이 되지 않을 수 있습니다. 몇 분 후 다시 시도해보세요.)

	![](../images/blog/Pasted%20image%2020220814171149.png)

