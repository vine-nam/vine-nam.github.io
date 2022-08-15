---
title: 2. 지킬(Jekyll) 테마 변경하기
categories: [blog]
comments: true
tags: [blog, jekyll]
---

여러 사이트에서 지킬 테마를 다운 받을 수 있습니다.

아래의 사이트에서 마음에 드는 테마를 선택합니다.

-   [GitHub.com #jekyll-theme repos](https://github.com/topics/jekyll-theme)
-   [jamstackthemes.dev](https://jamstackthemes.dev/ssg/jekyll/)
-   [jekyllthemes.org](http://jekyllthemes.org/)
-   [jekyllthemes.io](https://jekyllthemes.io/)
-   [jekyll-themes.com](https://jekyll-themes.com/)

<br/>

저는 [Catbook](http://jekyllthemes.org/themes/CATbook/) 으로 하겠습니다.

**Demo** 를 클릭하면 미리보기가 가능합니다.

**Download** 를 클릭하여 파일을 다운 받습니다.

![](../images/blog/Pasted%20image%2020220814164034.png)

다운 받은 파일을 자신의 지킬 블로그 폴더에 넣어줍니다.

![](../images/blog/Pasted%20image%2020220814164447.png)

중복되는 파일이 있으면 파일은 덮어써 줍니다.

![](../images/blog/Pasted%20image%2020220814164438.png)


지킬을 실행시켜 줍니다.

```
 bundle exec jekyll serve
```

파일 충돌이 있는 것 같습니다.

![](../images/blog/Pasted%20image%2020220814164659.png)

**index.markdown** 과 **about.markdown** 파일을 삭제해 줍니다.

![](../images/blog/Pasted%20image%2020220814164813.png)

이제 [http://localhost:4000/](http://localhost:4000/) 로 접속해보면 바뀐 테마를 확인할 수 있습니다.

![](../images/blog/Pasted%20image%2020220814165129.png)


<br/>

**다음글 보러가기** ➞ [3. github blog 만들고 지킬(Jekyll) 올리기](../2022-08/003-create-github-blog-and-upload-jekyll.html)