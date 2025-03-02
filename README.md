# 🤖 Auto Page - Copy Cat your Web Surfing
<div align="center">
  
  ![icon](https://github.com/user-attachments/assets/0862acdb-ea22-4319-9d16-ca15a04b4fbd)
  <br />
  📸 [시연 영상 보러가기](https://www.youtube.com) | 
  📦 [다운로드 바로가기](https://www.auto-page.site)
  
</div>

Auto Page는 사용자의 웹 서핑 기록을 기억하고, 그 기록을 매크로처럼 다시 실행 시켜주는 프로젝트입니다.

브라우저와 비슷한 디자인으로 사용자에게 편안한 웹 서핑 환경을 제공하고, 매크로를 통해 반복적인 작업을 관리하여 사용자에게 편리함을 제공합니다.


<br />

## 목차

<br />

## 1. 개발 동기
매번 같은 작업을 반복하고 있나요?

평상 시 웹 서핑을 하다 보면 반복되는 작업을 매번 진행해야 하는 불편함이 존재합니다, Auto Page는 이러한 불편함을 개선해 시간을 절약해 보자 라는 생각으로 시작된 프로젝트입니다.

[시중에 매크로 프로그램은 많이 배포](https://kmong.com/search?type=gigs&keyword=%EB%A7%A4%ED%81%AC%EB%A1%9C+%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8)되어 있습니다. 하지만 특수 목적으로 제작 된 매크로들과, 일반 사용자가 설명서 없이 사용하기에는 어려운 매크로 등록, 또한 주문 제작을 통한 가격의 상승 등 이미 시중에 있는 프로그램들도 충분한 개선사항이 존재한다는 것을 알게 되었고, 이런 개선사항을 하나 씩 해결하다 보면 자연스럽게 브라우저와 DOM의 이해, 비동기 순서제어, 다양한 예외 케이스 처리 능력 등 개발 실력도 같이 증가 시킬 수 있을 것 같다는 생각이 들었습니다.

“웹 서핑 하듯 사용하세요! Auto Page가 기록하고 실행해 줄게요.” 를 목표로 최대한 기존 브라우저와 비슷한 환경에서 사용자가 매크로를 기록하고, 실행하기 위해 개발을 시작하게 되었습니다.

<br />

## 2. 기능
<div align="center">
  
| | |
| ---------- | ---------------------------------------------- |
| ![기능1](https://github.com/user-attachments/assets/174ef1dc-72c1-4204-9723-91a98716bb7e) | 기존 브라우저와 비슷한 환경 제공 <br /> 앞으로 가기/ 뒤로 가기 및 새로고침을 구현되어 있습니다. |
| ![기능2](https://github.com/user-attachments/assets/1d5e6a10-8856-434f-8d4c-76714ac4936a) | 사용자 상호작용 기록 <br /> 사용자의 클릭, 입력 요소에 대해 tagName, id, class, url을 기록하고, 이벤트 후의 변경사항이 반영된 화면을 캡쳐하여 저장합니다. |
| ![기능3](https://github.com/user-attachments/assets/6fcd87a9-4181-486a-b8c5-b900a0581830) | 기록된 매크로를 실행 <br /> DOM이 다시 로드 되거나, 페이지가 이동해도 순서는 보장됩니다. |

</div>

<br />

## 3. 기술 스택
<div align="center">
  
| 프레임워크 | 프론트엔드 | 빌드 | 테스트 | 배포 |
| ---------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | 
| <img src="https://img.shields.io/badge/Electron-3B4250?style=flat-square&logo=Electron&logoColor=#47848F"/> | <img src="https://img.shields.io/badge/React-3B4250?style=flat-square&logo=React&logoColor=#61DAFB"/> <br /> <img src="https://img.shields.io/badge/Zustand-3B4250?style=flat-square&logo=React&logoColor=#3B4250"/> <br /> <img src="https://img.shields.io/badge/Tailwind-3B4250?style=flat-square&logo=tailwindcss&logoColor=#06B6D4"/> | <img src="https://img.shields.io/badge/Vite-3B4250?style=flat-square&logo=vite&logoColor=#646CFF"/> | <img src="https://img.shields.io/badge/Vitest-3B4250?style=flat-square&logo=vitest&logoColor=#6E9F18"/> | <img src="https://img.shields.io/badge/Netlify-3B4250?style=flat-square&logo=netlify&logoColor=#00C7B7"/> <br /> <img src="https://img.shields.io/badge/Electron Builder-3B4250?style=flat-square&logo=electronbuilder&logoColor=#000000"/> |

</div>

<br />

## 4. 기술 스택에 대한 이해
### 4-1. 왜 Electron인가?
사용자가 클릭한 요소, 입력한 값에 대해서는 사용자가 발생시키는 이벤트에 접근할 수 있어야 했고, 매크로를 실행하기 위해서 외부의 DOM을 조작해야 했습니다. <br /><br />

#### 1. 첫 시도
처음은 iframe을 통한 접근이었습니다. ```iframeElement.contentWindow``` 를 통해 iframe 속 외부 DOM에서 사용자의 이벤트를 감지하려 했지만
간과한 부분은 CORS에러로 사용자가 원하는 도메인을 띄울 수 없기 때문에 다시 방향을 틀어보고자 하였습니다. <br />

#### 2. 두 번째 시도
프록시 서버를 사용해 CORS를 우회하고자 하였습니다. ```Express.js```를 통해 크롤링 서버를 구축하였고,
크롤링을 통해 사용자가 요청한 도메인의 DOM이 로드 되면 해당 DOM을 문자열 형태로 반환받아 화면단에 뿌려주는 것을 목표로 하였고 CORS를 우회하여 사용자가 원했던 도메인을 호출하는데에 성공했습니다. <br />
하지만 이 방법의 한계로 사용자가 다음 페이지로 넘어가거나, 페이지 전환 시마다 요청을 막고 크롤링 서버로 보내다 보니 응답을 기다리는 시간이 길어졌고 사용자 친화적이지 않다는 생각을 하게 되었습니다.

#### 3. 세 번째 시도
앞의 방법들을 시도해 보면서 CORS, 사용자 친화성, 이벤트 감지 및 전달이 최우선이라고 느꼈고, 문제들을 해결하기 위해 Electron이 최적이라고 생각했습니다.<br />
Electron은 main 및 renderer라는 두 가지 유형의 독립된 프로세스가 있고, renderer프로세스에 preload라는 node스크립트를 주입해 두 프로세스를 연결합니다.<br />
이를 통해 외부 DOM을 직접 조작하고, 사용자의 이벤트를 감지하는 것이 가능해졌고 즉, Electron을 선택한 이유는 다음과 같습니다.<br />
1. CORS 문제 해결: Electron은 브라우저 환경에서 실행되지만, Node.js의 기능을 활용할 수 있어 CORS 문제를 우회할 수 있었습니다.
2. 사용자 친화성: 페이지 전환 시 크롤링과는 다르게 기다릴 필요가 없고 Electron의 ```webview```태그를 통해 기존 브라우저와 같은 환경을 제공할 수 있었습니다.
3. 이벤트 감지 및 전달: preload를 ```webview```에 주입해 renderer프로세스에서 이벤트를 감지하고 main 프로세스로 전달하기 용이했습니다.  
<div align="center">
  
![스크린샷 2025-03-01 오후 7 44 17](https://github.com/user-attachments/assets/b8057adc-58c7-4a09-a062-595a1c88b6ba)

</div>

### 2. 전역 상태는 왜 사용헀나?
Electron은 main, renderer 두 개의 프로세스가 존재하고, 두 프로세스 간 ipc[Inter-Process Communication] 통신을 사용합니다.<br />
통신 채널을 설정하고 이벤트 리스너로 등록 시켜놓으면 양방향 통신이 가능해지는데, 이 때 특정 이벤트끼리의 순서가 보장되어야 하는 의존성이 생깁니다, 따라서 의존성이 생긴 이벤트들의 순서를 보장해 주기 위해선 "부모 컨트롤러"가 존재해야 한다는 생각이 들었습니다.<br />
다만, Auto Page프로젝트에서는 의존성이 생긴 이벤트들이 프로젝트의 중심 기능이고 모든 이벤트의 흐름을 파악하고 이벤트에 맞는 다음 로직을 이어나가야 했어서 "부모 컨트롤러"로써 전역상태를 쓰기로 마음 먹었습니다.<br />
Auto Page에서는 전역상태를 이용해 모든 이벤트의 결과를 수집하고 그 결과에 따라 화면 캡쳐, 에러 팝업, 매크로 실행 등 앱의 흐름을 관리합니다.


<br />

## 5. 개발 중 문제 해결

### 5-1. DOM 이벤트를 어떻게 기록할까?
사용자의 웹 서핑 흐름을 방해하지 않고 클릭, 입력, URL이동 등 사용자가 웹 페이지에서 진행하는 이벤트들을 정확히 기록해야 했습니다. 만약 이를 제대로 처리하지 않는다면 매크로 실행에 큰 오류를 야기할 수 있기 때문입니다.<br />
우선 DOM에서 사용자와 상호작용할 수 있는 요소를 찾았습니다. button, textarea, select, input, a 요소들이 있고 요소들의 이벤트를 감지해야 했습니다.<br />
다양한 예외를 처리해야 헀기 때문에 하나하나 테스트 해보며 태그들이 어떤 이벤트에 반응하는지 찾았고, 이벤트를 감지하기 위해서는 총 3개 "CLICK", "CHANGE", "KEYDOWN" 이벤트가 필요했습니다.
<div align="center">
  
| 이벤트 | 감지되는 태그 |
| ---------- | ---------------------------------------------- |
| CLICK | ```button```, ```a```, ```input[type=button]``` |
| CHANGE | ```textarea```, ```select```, ```button```, ```input[type=text,password,number,email, 등 그 외]``` |
| KEYDOWN | ```Enter입력``` |

</div>

위 표와 같이 대부분의 이벤트는 CLICK, CHANGE로 잡아낼 수 있었으며, 보통 Enter키 이벤트가 걸려있는 검색창들은 KEYDOWN으로 대응했습니다.

- iframe CORS 문제<br />
아쉽게도 아무리 웹뷰를 사용해 CORS를 회피했다고 하더라도, 웹뷰 속 iframe까지의 CORS를 피하기에는 한계가 있다는 것을 알게됐습니다.
Electron의 preload스크립트를 웹뷰에 주입했지만 그 스크립트가 웹뷰 속 iframe까지 전해지는 것이 아니었습니다. 따라서 iframe속 내용은 이벤트를 수집할 수 없었고, 사용자 오인을 방지하기 위해 iframe을 지워야 할 필요가 있었습니다.<br />
사용자가 웹 페이지를 사용할 수 있기 전 부터 iframe을 볼 수 없어야 했기 때문에 MutationObserver를 활용하여 동적으로 로드되는 iframe을 모두 삭제해 사용자가 iframe과 상호작용할 수 없도록 막았습니다.
```js
const observer = new MutationObserver(() => {
  const iframe = document.getElementsByTagName("iframe");
  Array.from(iframe).forEach((frame) => {
    frame.remove();
  });
});
```
<br />

- 이벤트를 감지했으니, 이제 어떻게 기록해야 할까?<br />
각 요소 별로 중복되지 않는 Id가 하나씩 할당되어 있다면, 정말 편하겠지만 현실은 그렇지 않았습니다. Id가 없는 경우, Class가 중복되는 경우 등 다양한 예외가 존재할 수 있어 이벤트 요소에 대해 많은 것을 수집해야 했습니다.<br />
DOM 상에서 위치를 기반으로 하는 xPath의 경우 DOM의 구조가 변경되면 매크로 실행에 있어 치명적일 수 있지만 Id, Class, TagName같은 경우 이름이 의미하는 바가 있기 때문에 구조가 변경되더라도, 비교적 더 안정적일 수 있어 매크로 실행에 유연성을 제공할 것이라고 판단했고, 이벤트명, 이벤트가 발생한 url, href, 입력값을 포함하여 이벤트에 대한 추가정보를 수집해 JSON형태로 저장했습니다.
```js
{
  method: "change",
  id: "input_item_id",
  tagName: "INPUT",
  tagIndex: "0",
  class: [
    {className: "input_item_id", classIndex, 0}
  ],
  url: "https://www.naver.com",
  href: "",
  value: "my id",
}
```
<br />

### 5-2. 기록한 매크로를 어떻게 다시 실행시킬까?
초기에는 기록된 매크로를 순회하며 타겟 요소를 찾고 저장한 이벤트를 그대로 다시 실행시켜주는 방식의 구현이었지만 테스트를 해보며 요소를 찾지 못하거나, 매크로가 일찍 종료되거나, 순서가 보장되지 않는 등 예상과는 다르게 동작하는 것을 발견했습니다.<br />
SPA페이지일 경우 ```dom-ready```이벤트에 감지되고나서 컨텐츠들이 동적으로 생기며 구조가 변경될 수 있고, ```a tag```를 통해 페이지 상태가 바뀌면 실행 중인 preload스크립트가 재실행되었습니다, 이를 해결하기 위해 기록된 매크로 요소를 DOM에서 동적으로 찾아 실행하고, 실행 중이던 preload스크립트가 초기화 되더라도 중단된 부분부터 순서를 보장하고, 다시 실행시켜야 핟다고 판단했습니다.

- SPA일 경우 어떻게 동적으로 DOM요소를 찾을까?

- 페이지가 변경될 때마다 preload스크립트가 재실행되는데 어떻게 실행을 이어서 할까?












### 5-3. **이벤트 발생 직후의 이미지 캡쳐**
웹 페이지의 특정 시점에서 이미지를 캡처해야 했습니다. 하지만 페이지 로딩이 완료된 후 이미지를 정확히 캡처하는 시점을 맞추는 것이 매우 어려웠습니다. 이 문제를 해결하기 위해 did-stop-loading 이벤트를 사용해 페이지가 완전히 로드된 시점을 감지하고, 그 후에 이미지를 캡처하도록 했습니다. capturePage 메서드를 사용해 웹 페이지의 특정 부분을 캡처하고 이를 저장하는 방식으로 구현했습니다. 어려운 점은 페이지 로딩 시점과 이미지 캡처 타이밍을 정확히 맞추는 것이었고, 이를 위해 로딩 완료 이벤트를 세밀하게 처리해야 했습니다. 이 작업을 통해 페이지 로딩이 완료된 후 정확히 이미지를 캡처하고 저장할 수 있었습니다.

### 5-4. **BrowserRouter와 HashRouter의 차이**
Electron에서 BrowserRouter를 사용할 경우 URL을 기준으로 라우팅이 이루어지는데, 이때 발생하는 흰 화면 문제를 해결하기 위해 라우팅 방식을 변경해야 했습니다. 이를 해결하기 위해 HashRouter를 사용하여 라우팅을 처리했습니다. HashRouter는 URL의 해시 부분을 기준으로 라우팅하기 때문에, Electron 환경에서 발생하는 라우팅 문제를 해결할 수 있었습니다. 문제는 BrowserRouter 사용 시 발생하는 라우팅 문제였고, 이를 해결하기 위해 HashRouter로 변경하는 방법을 선택하여 해결했습니다.


## 앞으로의 개선 사항


## 6. 결과
