---
title: chrome extension 만들기
categories: [dev]
comments: true
tags: [chrome_extension]
---

## 첫 chrome extension 만들기
간단하게 `Hello` 를 출력하는 확장 프로그램을 만들어 보겠습니다.

### 새 프로젝트 만들기
새 폴더를 만들고, 그 안에 manifest.json 파일과 popup.xml 파일을 만듭니다.
- extension에 대한 정보는 manifest.json에 작성합니다.

manifest.json
```json
{
  "name": "Hello World",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_popup": "popup.html"
  }
}
```

popup.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
  </head>
  <body>
    Hello
  </body>
</html>
```

<br/>

### 확장 프로그램 업로드 하기
확장 프로그램 관리 페이지(chrome://extensions/)에서 
**개발자 모드**를 켜면 확장 프로그램을 올릴 수 있습니다.

![](../images/dev/Pasted%20image%2020220824213126.png)

**압축해제된 확장 프로그램을 로드합니다.** 를 클릭한 후, 파일을 업로드 합니다.

![](../images/dev/Pasted%20image%2020220824214825.png)

확장 프로그램이 추가되었습니다.

![](../images/dev/Pasted%20image%2020220824214146.png)

확장 프로그램 아이콘을 클릭하면 `Hello`가 출력 되는 것을 확인할 수 있습니다.

![](../images/dev/Pasted%20image%2020220828125444.png)

<br/>

## 웹 페이지에서 데이터 복사하는 프로그램 만들기
저는 보통 특정 페이지에서 데이터를 확인하고 특정 형식으로 전달하는 반복 작업을 할 때, 크롬 확장 프로그램을 만들어서 사용합니다. 

여기서는 간단하게, 제 블로그에서 글 제목과 날짜 데이터를 복사하는 기능을 만들어보겠습니다.

### 현재 페이지의 데이터를 가져오는 기능 만들기
popup.html에 버튼을 사용하여 데이터를 복사하고, 그 결과를 보여줄 메시지 영역을 만들어줍니다.

popup.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <button id="copyBtn">복사</button>
    <div id="msg" class="hide"></div>
  </body>
  <script src="./popup.js"></script>
</html>
```

style.css
```css
body {
  margin: 3px;
}

#copyBtn {
  height: 20px;
  width: 45px;
  outline: none;
  border: none;
  border-radius: 2px;
}

#msg {
  height: 20px;
  width: 45px;
  text-align: center;
  vertical-align: middle;
}

.hide { 
  display: none;
}

.show {
  display: table-cell;
}

.success {
  background: chartreuse;
}

.fail {
  background: orangered;
}
```

<br/>

**복사** 버튼을 클릭하면 페이지에서 post_title과 post_date 정보를 가져오는 코드를 popup.js파일에 작성해줍니다.

popup.js
```javascript
let copyBtn = document.getElementById("copyBtn");

copyBtn.addEventListener("click", async () => {
  let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });

  chrome.scripting.executeScript({
    target: { tabId: tab.id }, // 작업을 수행할 탭 지정
    func: copyPageData // 실행할 함수
  },
  (result) => { // 실행 결과
    console.log(result);
  });
});

function copyPageData() {
  let title = document.querySelector('.post_title').innerHTML;
  let date = document.querySelector('.post_date').innerHTML;
  return `(${date}) ${title}`;
}
```

<br/>

메니페스트 파일에 확장 프로그램이 현재 페이지에 임시로 액세스할 수 있는 권한과 열려있는 페이지에서 접근하기 위해 Scripting API의 executeScript 메서드를 사용할 수 있는 권한을 추가해줍니다.

manifest.json
```json
{
  "name": "Hello World",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_popup": "popup.html"
  },
  "permissions": ["activeTab", "scripting"]
}
```

<br/>

이제 확장 프로그램을 새로고침하고,  블로그 페이지도 새로고침한 다음 팝업을 우클릭하여 콘솔창을 열어줍니다.

![](../images/dev/Pasted%20image%2020220825214800.png)

![](../images/dev/Pasted%20image%2020220825214920.png)

그 다음 **복사** 버튼 클릭하여 실행 결과를 확인합니다.

![](../images/dev/Pasted%20image%2020220825215035.png)


<br/>

### 가져온 데이터를 클립보드에 저장하기
웹 페이지에서 가져온 데이터가 있을 때, 클립보드에 저장하는 로직을 추가합니다.

이 때, 실행 결과가 성공이면 `성공` 메시지를, 실패일 때 `실패` 메시지를 출력하는 메소드를 추가합니다.

popup.js
```javascript
...
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    func: copyPageData
  },
  (result) => {
    let data = result[0].result;
    if(!!data) {
      dataToClipboard(data) ? successMsg() : failMsg();
    } else {
      failMsg();
    }
  });
});

...

function dataToClipboard(data) {
  let tempElem = document.createElement('textarea');

  tempElem.value = data;
  document.body.appendChild(tempElem);
  tempElem.select();
  let copyResult = document.execCommand('copy');
  document.body.removeChild(tempElem);

  return copyResult;
}

function successMsg() {
  let msg = document.getElementById("msg");
  msg.innerHTML = "성공";
  msg.className = "show success";
}

function failMsg() {
  let msg = document.getElementById("msg");
  msg.innerHTML = "실패";
  msg.className = "show fail";
}
```

<br/>

다시 확장 프로그램을 새로고침하고 , 팝업의 **복사** 버튼을 클릭합니다.

![](../images/dev/Pasted%20image%2020220825220717.png)

`성공` 메시지가 출력 되었습니다.

이 상태에서 빈 영역에 `Ctrl + v`  해보면 복사 된 내용을 확인할 수 있습니다.
```
(Aug 14, 2022) 1. 지킬(Jekyll) 블로그 만들기
```

<br/>

## 정리
- extension에 대한 정보는 manifest.json에 작성한다.
- 현재 열려 있는 탭에 대한 권한을 갖기 위해서는 `"permissions": ["activeTab"]`을 사용한다.
- 웹 페이지에 javascript, css를 삽입(실행)하기 위해서는 `chrome.scripting` API를 사용 한다.

<br/>

## 추가 내용
### 아이콘 등록하기
프로젝트 폴더에 아이콘 이미지를 넣고, 매니페스트 파일에 아이콘 이미지 경로를 작성해 줍니다.
```json
{
  "name": "Hello World",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_popup": "popup.html",
    "default_icon": "icon.png"
  },
  "icons": {
    "128": "icon.png"
  },
  "permissions": ["activeTab", "scripting"]
}
```

`action.default_icon`에 등록한 이미지는 확장 프로그램 아이콘 이미지로 표시됩니다.

![](../images/dev/Pasted%20image%2020220828131201.png)

`icons`에 등록한 이미지는 확장 관리 페이지에 이미지를 표시합니다.

![](../images/dev/Pasted%20image%2020220828131247.png)


<br/>

### 단축키 등록하기

```json
{
  ...
  "commands": {
    "_execute_action": {
      "suggested_key": {
        "default": "Ctrl+Shift+Y",
        "mac": "Command+Shift+Y"
      }
    }
  }
}
```

최종 사용자는 `chrome://extensions/shortcuts` 에서 원하는 키로 다시 매핑 할 수 있습니다.

![](../images/dev/Pasted%20image%2020220828133712.png)

<br/>

단축키를 누르면 **복사** 버튼을 누르는 것 대신 바로 복사가 되면 좋을 것 같다는 생각이 드네요.

popup.js 파일을 다음과 같이 수정한 후, 웹 페이지에서 단축키를 눌러 보세요.
```javascript
//let copyBtn = document.getElementById("copyBtn");

//copyBtn.addEventListener("click", async () => {
window.addEventListener('load', async () => {
  let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });
  
...
```
<br/>

## 참고
- [Chrome Developers 시작하기](https://developer.chrome.com/docs/extensions/mv3/getstarted/)
- [매니페스트 파일 형식](https://developer.chrome.com/docs/extensions/mv3/manifest/)
- [권한 선언](https://developer.chrome.com/docs/extensions/mv3/declare_permissions/)
- [ActiveTab 권한](https://developer.chrome.com/docs/extensions/mv3/manifest/activeTab/)
- [chrome.scripting](https://developer.chrome.com/docs/extensions/reference/scripting/)
- [chrome.commands](https://developer.chrome.com/docs/extensions/reference/commands/)


