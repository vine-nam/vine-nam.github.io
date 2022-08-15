---
title: 1. 지킬(Jekyll) 블로그 만들기
categories: [blog]
comments: true
tags: [blog, jekyll]
---

### 1. 루비 개발 환경 설치하기
 Windows에서 루비를 설치하기 위해서는 [RubyInstaller 다운로드 페이지](https://rubyinstaller.org/downloads/)에서  **Ruby+Devkit** 버전을 다운로드 받아 설치합니다.
 
설치 시 옵션들은 default로 사용하면 됩니다. 

![](../images/blog/Pasted%20image%2020220814160921.png)

설치 마법사의 마지막 단계에서 `ridk install` 절차를 수행합니다.

명령어 `ruby -v`, `gem -v` 로 루비와 루비젬이 설치되었는지 확인합니다.

![](../images/blog/Pasted%20image%2020220814162028.png)

### 2. Jekyll과 Bundler 젬 설치하기

```
gem install jekyll bundler
```

### 3. 블로그 만들고 실행하기
1.  `./myblog` 에 새 Jekyll 사이트를 생성한다.
    
	```
	jekyll new myblog
	```
    
2.  생성된 디렉토리로 이동한다.
    
	```
	cd myblog
	```
    
3.  사이트를 빌드하고 로컬 서버에 적용한다.
    
    ```
    bundle exec jekyll serve
    ```

	이 때 `require': cannot load such file -- webrick (LoadError)` 에러가 발생하면 `bundle add webrick` 명령어를 실행해준 뒤 다시 위의 명령어를 실행해줍니다.

	![](../images/blog/Pasted%20image%2020220814162514.png)

4.  이제 브라우저로 [http://localhost:4000](http://localhost:4000/) 에 접속합니다.

	![](../images/blog/Pasted%20image%2020220814162556.png)


<br/>

**다음글 보러가기** ➞ [2. 지킬(Jekyll) 테마 변경하기](../2022-08/002-change-jekyll-theme.html)

---
[참고]
- jekyll 설치 : [https://jekyllrb-ko.github.io/docs/](https://jekyllrb-ko.github.io/docs/)
- 루비 설치: [ https://jekyllrb-ko.github.io/docs/installation/windows/]( https://jekyllrb-ko.github.io/docs/installation/windows/)
- require': cannot load such file -- webrick (LoadError) 오류 : [https://junho85.pe.kr/1850](https://junho85.pe.kr/1850)