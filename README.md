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
| | |
| ---------- | ---------------------------------------------- |
| ![기능1](https://github.com/user-attachments/assets/174ef1dc-72c1-4204-9723-91a98716bb7e) | 기존 브라우저와 비슷한 환경 제공 <br /> 앞으로 가기/ 뒤로 가기 및 새로고침을 구현되어 있습니다. |
| ![기능2](https://github.com/user-attachments/assets/1d5e6a10-8856-434f-8d4c-76714ac4936a) | 사용자 상호작용 기록 <br /> 사용자의 클릭, 입력 요소에 대해 tagName, id, class, url을 기록하고 저장합니다. |
| ![기능3](https://github.com/user-attachments/assets/6fcd87a9-4181-486a-b8c5-b900a0581830) | 기록된 매크로를 실행 <br /> DOM이 다시 로드 되거나, 페이지가 이동해도 순서는 보장됩니다. |

<br />

## 3. 기술 스택
| 프레임워크 | 프론트엔드 | 빌드 | 테스트 | 배포 |
| ---------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | 
| <img src="https://img.shields.io/badge/Electron-3B4250?style=flat-square&logo=Electron&logoColor=#47848F"/> | <img src="https://img.shields.io/badge/React-3B4250?style=flat-square&logo=React&logoColor=#61DAFB"/> <br /> <img src="https://img.shields.io/badge/Zustand-3B4250?style=flat-square&logo=React&logoColor=#3B4250"/> <br /> <img src="https://img.shields.io/badge/Tailwind-3B4250?style=flat-square&logo=tailwindcss&logoColor=#06B6D4"/> | <img src="https://img.shields.io/badge/Vite-3B4250?style=flat-square&logo=vite&logoColor=#646CFF"/> | <img src="https://img.shields.io/badge/Vitest-3B4250?style=flat-square&logo=vitest&logoColor=#6E9F18"/> | <img src="https://img.shields.io/badge/Netlify-3B4250?style=flat-square&logo=netlify&logoColor=#00C7B7"/> <br /> <img src="https://img.shields.io/badge/Electron Builder-3B4250?style=flat-square&logo=electronbuilder&logoColor=#000000"/> |

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

![스크린샷 2025-03-01 오후 7 44 17](https://github.com/user-attachments/assets/b8057adc-58c7-4a09-a062-595a1c88b6ba)

### 2. 전역 상태는 왜 사용헀나?
Electron은 main, renderer 두 개의 프로세스가 존재하고, 두 프로세스 간 ipc[Inter-Process Communication] 통신을 사용합니다.<br />
통신 채널을 설정하고 이벤트 리스너로 등록 시켜놓으면 양방향 통신이 가능해지는데, 이 때 특정 이벤트끼리의 순서가 보장되어야 하는 의존성이 생깁니다, 따라서 의존성이 생긴 이벤트들의 순서를 보장해 주기 위해선 "부모 컨트롤러"가 존재해야 한다는 생각이 들었습니다.<br />
다만, Auto Page프로젝트에서는 의존성이 생긴 이벤트들이 프로젝트의 중심 기능이고 모든 이벤트의 흐름을 파악하고 이벤트에 맞는 다음 로직을 이어나가야 했어서 "부모 컨트롤러"로써 전역상태를 쓰기로 마음 먹었습니다.<br />
Auto Page에서는 전역상태를 이용해 모든 이벤트의 결과를 수집하고 그 결과에 따라 화면 캡쳐, 에러 팝업, 매크로 실행 등 앱의 흐름을 관리합니다.


<br />

## 5. 개발 중 문제 해결

### 5-1. DOM 이벤트를 어떻게 기록할까?

- **문제점**: 모든 DOM 이벤트를 기록하는 것은 어려운 과제였습니다.
- **해결 방법**: document.addEventListener를 통해 모든 DOM 이벤트를 기록하였으며, 이벤트 캡쳐링을 통해 클릭 이벤트가 막힌 요소들도 감지할 수 있었습니다.
- **파일**: `src/preload/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 사용자 동작을 매크로로 기록하는 데 많은 도전 과제가 있었습니다. 특히, 다양한 사용자 동작을 정확히 기록하는 것이 매우 까다로웠습니다.

**알고리즘 및 기술**: `MutationObserver` 사용, DOM 변화 감지 및 기록, 단계별 기록 및 메타데이터 저장

**적용 방법**:

- `MutationObserver`를 사용하여 DOM의 변화를 감지하였습니다. 이를 통해 사용자의 동작을 실시간으로 기록할 수 있었습니다.
- DOM 변화가 감지되면 해당 변화를 매크로로 기록하고, 각 동작을 단계별로 저장하였습니다.
- 필요한 메타데이터를 함께 저장하여, 매크로 재생 시 정확히 동작하도록 하였습니다.

### 5-2. **기록한 매크로를 어떻게 다시 실행시킬까?**

- **문제점**: 모든 DOM 이벤트에 대해 작동하도록 설정하는 것은 복잡한 작업이었습니다.
- **해결 방법**: document에 이벤트를 걸고 InputEvent를 감시하여 이벤트가 발생할 때 값을 감지하도록 하였습니다. 관련 자료는 MDN 문서와 Stack Overflow 답변을 참고하였습니다.
**파일**: `src/preload/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 기록된 매크로를 정확히 재생하는 것이 어려웠습니다. 특히, DOM 구조나 페이지 상태가 예상과 다를 경우 발생하는 오류를 처리하는 것이 까다로웠습니다.

**알고리즘 및 기술**: DOM 요소 동적 탐색, 오류 처리 로직, 단계별 실행

**적용 방법**:

- 매크로 재생 시 각 동작을 순차적으로 실행하였습니다. 이를 위해 DOM 요소를 동적으로 탐색하고, 필요한 경우 페이지 이동 등을 처리하였습니다.
- 예상치 못한 상황이 발생할 경우를 대비하여 오류 처리 로직을 추가하였습니다. 이를 통해 매크로 재생 중 발생할 수 있는 오류를 효과적으로 처리하였습니다.
- 각 동작을 단계별로 실행하여, 매크로가 정확히 재생되도록 하였습니다.

### 5-3. **이벤트 발생 직후의 이미지 캡쳐**

- **문제점**: 로드가 완료된 후 이미지를 캡처하는 것은 어려운 과제였습니다.
- **해결 방법**: 세션 스토리지를 활용하여 로드 완료 후 이미지를 캡처하였습니다.

**파일**: `src/main/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 웹 페이지의 특정 부분을 캡처하고 이를 저장하는 기능을 구현하는 데 어려움이 있었습니다. 특히, 웹뷰에서 페이지 로딩이 완료된 시점을 정확히 파악하고 캡처를 수행하는 것이 까다로웠습니다.

**알고리즘 및 기술**: `did-stop-loading` 이벤트 활용, `capturePage` 메서드 사용, 상태 관리 로직 구현

**적용 방법**:
- `did-stop-loading` 이벤트를 활용하여 웹 페이지의 로딩이 완료된 시점을 정확히 파악하였습니다. 이 이벤트를 통해 페이지 로딩이 완료되면 캡처 작업을 수행하도록 하였습니다.
- `capturePage` 메서드를 사용하여 웹 페이지의 특정 부분을 캡처하고, 이를 이미지로 저장하였습니다.
- 캡처된 이미지를 관리하기 위해 추가적인 상태 관리 로직을 구현하여, 캡처된 이미지가 적절히 저장되고 필요할 때 불러올 수 있도록 하였습니다.

### 5-4. **브라우저의 뒤로가기, 새로고침, 탭 관리**

- **문제점**: canGoBack은 원시값이기 때문에 객체에 넣어 전달이 가능했지만, goBack() 같은 함수는 다른 컨텍스트에서 실행 시 오류가 발생하였습니다.
- **해결 방법**: goBack()을 콜백 함수에 담아 this를 고정시켜 실행 권한을 위임하였습니다.
**파일**: `src/renderer/src/stores/useTabStore.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 여러 브라우저 탭을 효율적으로 관리하고, 각 탭의 상태를 유지하는 것이 어려웠습니다.

**알고리즘 및 기술**: Zustand를 사용한 탭 상태 관리, 탭 간 전환 시 데이터 저장 및 불러오기 로직

**적용 방법**:

- Zustand를 사용하여 여러 브라우저 탭의 상태를 효율적으로 관리하였습니다. 각 탭의 상태 변화를 추적하고, 필요한 경우 상태를 업데이트하였습니다.
- 탭 간 전환 시 필요한 데이터를 적절히 저장하고, 전환 시 데이터를 불러오는 로직을 구현하여 사용자 경험을 향상시켰습니다.

### 5-5. **irame은 어떻게 지울까**

- **문제점**: 비동기적으로 로드되는 웹뷰를 처리하는 것은 어려운 과제였습니다.
- **해결 방법**: 기존의 setTimeout 방법 대신 mutationObserver를 사용하여 DOM의 변화를 감지하고, iframe을 즉시 제거하는 방식으로 UX를 개선하였습니다.

### 5-6. **BrowserRouter와 HashRouter의 차이**

- **문제점**: BrowserRouter는 URL을 기준으로 라우팅하고, HashRouter는 URL의 해시 부분을 기준으로 라우팅합니다. Electron에서 BrowserRouter를 사용할 경우 흰 화면이 뜨는 문제가 발생하였습니다.
- **해결 방법**: HashRouter를 사용하여 라우팅 문제를 해결하였습니다.

## 앞으로의 개선 사항


## 6. 결과
