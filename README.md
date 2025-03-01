# 🤖 Auto Page
<div align="center">
  
  ![icon](https://github.com/user-attachments/assets/0862acdb-ea22-4319-9d16-ca15a04b4fbd)
  <br />
  📸 [시연 영상 보러가기](https://www.youtube.com) | 
  📦 [다운로드 바로가기](https://www.auto-page.site)
  
</div>

Auto Page는 사용자의 웹 서핑 기록을 기억하고, 그 기록을 매크로처럼 다시 실행 시켜주는 프로젝트입니다.

브라우저와 비슷한 디자인으로 사용자에게 편안한 웹 서핑 환경을 제공하고, 매크로를 통해 반복적인 작업을 관리하여 사용자에게 편리함을 제공합니다.



## 목차

## 개발 동기
매번 같은 작업을 반복하고 있나요?

평상 시 웹 서핑을 하다 보면 반복되는 작업을 매번 진행해야 하는 불편함이 존재합니다, Auto Page는 이러한 불편함을 개선해 시간을 절약해 보자 라는 생각으로 시작된 프로젝트입니다.

[시중에 매크로 프로그램은 많이 배포](https://kmong.com/search?type=gigs&keyword=%EB%A7%A4%ED%81%AC%EB%A1%9C+%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8)되어 있습니다. 하지만 특수 목적으로 제작 된 매크로들과, 일반 사용자가 설명서 없이 사용하기에는 어려운 매크로 등록, 또한 주문 제작을 통한 가격의 상승 등 이미 시중에 있는 프로그램들도 충분한 개선사항이 존재한다는 것을 알게 되었고, 이런 개선사항을 하나 씩 해결하다 보면 자연스럽게 브라우저와 DOM의 이해, 비동기 순서제어, 다양한 예외 케이스 처리 능력 등 개발 실력도 같이 증가 시킬 수 있을 것 같다는 생각이 들었습니다.

“웹 서핑 하듯 사용하세요! Auto Page가 기록하고 실행해 줄게요.” 를 목표로 최대한 기존 브라우저와 비슷한 환경에서 사용자가 매크로를 기록하고, 실행하기 위해 개발을 시작하게 되었습니다.

## 기능
| | |
| ---------- | ---------------------------------------------- |
| ![기능1](https://github.com/user-attachments/assets/174ef1dc-72c1-4204-9723-91a98716bb7e) | 기존 브라우저와 비슷한 매크로 기록 환경을 제공하기 위해 앞으로 가기/ 뒤로 가기 및 새로고침을 구현했습니다. |
| ![기능2](https://github.com/user-attachments/assets/1d5e6a10-8856-434f-8d4c-76714ac4936a) | 사용자 상호작용 기록 사용자의 클릭, 입력 요소에 대해 tagName, id, class, url을 기록하고 저장합니다. |
| ![기능3](https://github.com/user-attachments/assets/6fcd87a9-4181-486a-b8c5-b900a0581830) | 기록된 매크로를 실행합니다. DOM이 다시 로드 되거나, 페이지가 이동해도 순서는 보장됩니다. |

## 기술 스택
| 프레임워크 | 프론트엔드 | 빌드 | 테스트 | 배포 |
| ---------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | 
| <img src="https://img.shields.io/badge/Electron-3B4250?style=flat-square&logo=Electron&logoColor=#47848F"/> | <img src="https://img.shields.io/badge/React-3B4250?style=flat-square&logo=React&logoColor=#61DAFB"/> <br /> <img src="https://img.shields.io/badge/Zustand-3B4250?style=flat-square&logo=React&logoColor=#3B4250"/> <br /> <img src="https://img.shields.io/badge/Tailwind-3B4250?style=flat-square&logo=tailwindcss&logoColor=#06B6D4"/> | <img src="https://img.shields.io/badge/Vite-3B4250?style=flat-square&logo=vite&logoColor=#646CFF"/> | <img src="https://img.shields.io/badge/Vitest-3B4250?style=flat-square&logo=vitest&logoColor=#6E9F18"/> | <img src="https://img.shields.io/badge/Netlify-3B4250?style=flat-square&logo=netlify&logoColor=#00C7B7"/> <br /> <img src="https://img.shields.io/badge/Electron Builder-3B4250?style=flat-square&logo=electronbuilder&logoColor=#000000"/> |

## 기술 스택에 대한 이해
1. 왜 Electron인가?
    - main 프로세스인 node와 renderer 프로세스인 크로미움 환경의 클라이언트 두 프로세스로 이루어져 있고 preload 프로세스를 통해 두 프로세스 간 IPC통신을 한다는 것을 이해하게 되었습니다.
2. Zustand 전역 상태는 왜 사용헀나?
    - 전역 상태관리로 Redux와 다르게 보일러 플레이트의 양이 적고 사용이 간편하고 성능도 좋다는 것을 이해하게 되었습니다.
    - 전역 상태관리는 main프로세스에서 전달받은 값을 통해 다른 컴포넌트에서도 화면의 상태 관리를 쉽게 하기 위함이었습니다.

## 개발 중 문제 해결
## **executeJavaScript를 이용한 eval 변환 문제 해결**

- **문제점**: 각 webview마다 Electron의 preload 스크립트를 삽입하여 eval 변환 문제를 해결하려 했지만, 새 탭을 열거나 새로고침 시 preload 스크립트가 재삽입되면서 문제가 발생하였습니다.
- **해결 방법**: 추가적인 스크립트 영역을 관리하여 문제를 해결하였습니다. 매 웹뷰마다 preload 스크립트를 삽입하고, 새로고침 시에도 스크립트가 유지되도록 설정하였습니다.

### **console-message 이벤트 대신 값을 전달하는 방법**

- **문제점**: console-message 이벤트를 사용하는 대신, 이벤트 여부에 따라 값을 가져올 수 있는 방법이 필요했습니다.
- **해결 방법**: ipcRenderer의 sendToHost를 통해 웹뷰 renderer로 직접 값을 전달하여 상태를 업데이트하였습니다. 또한, ipc 통신의 여러 메서드(send, sendToHost, invoke, on, handle)를 활용하여 문제를 해결하였습니다.

### **모든 DOM 이벤트 기록 방법**

- **문제점**: 모든 DOM 이벤트를 기록하는 것은 어려운 과제였습니다.
- **해결 방법**: document.addEventListener를 통해 모든 DOM 이벤트를 기록하였으며, 이벤트 캡쳐링을 통해 클릭 이벤트가 막힌 요소들도 감지할 수 있었습니다.

### **모든 DOM 이벤트 작동 방법**

- **문제점**: 모든 DOM 이벤트에 대해 작동하도록 설정하는 것은 복잡한 작업이었습니다.
- **해결 방법**: document에 이벤트를 걸고 InputEvent를 감시하여 이벤트가 발생할 때 값을 감지하도록 하였습니다. 관련 자료는 MDN 문서와 Stack Overflow 답변을 참고하였습니다.

### **이벤트 유지 및 동기화**

- **문제점**: 각 이벤트가 실행되고 새로 로드될 때마다 이벤트가 끊기거나 남은 이벤트가 진행되지 않는 문제가 발생하였습니다.
- **해결 방법**: 세션 스토리지를 활용하여 이벤트가 끊기거나 남은 이벤트를 유지하고 동기화하였습니다.

### **로드 완료 후 이미지 캡처 방법**

- **문제점**: 로드가 완료된 후 이미지를 캡처하는 것은 어려운 과제였습니다.
- **해결 방법**: 세션 스토리지를 활용하여 로드 완료 후 이미지를 캡처하였습니다.

### **브라우저의 뒤로가기, 새로고침, 탭 관리**

- **문제점**: canGoBack은 원시값이기 때문에 객체에 넣어 전달이 가능했지만, goBack() 같은 함수는 다른 컨텍스트에서 실행 시 오류가 발생하였습니다.
- **해결 방법**: goBack()을 콜백 함수에 담아 this를 고정시켜 실행 권한을 위임하였습니다.

### **비동기적으로 로드되는 웹뷰 처리**

- **문제점**: 비동기적으로 로드되는 웹뷰를 처리하는 것은 어려운 과제였습니다.
- **해결 방법**: 기존의 setTimeout 방법 대신 mutationObserver를 사용하여 DOM의 변화를 감지하고, iframe을 즉시 제거하는 방식으로 UX를 개선하였습니다.

### **preload 재삽입 시 매크로 실행 문제**

- **문제점**: preload가 다시 삽입될 시 매크로가 중단되는 문제가 발생하였습니다.
- **해결 방법**: 매크로 시작 시 sessionStorage에 값을 저장하고, webview가 실행될 때 해당 값을 기반으로 매크로를 재실행하도록 하였습니다. 또한, preload의 unload 이벤트를 감지하여 sessionStorage를 업데이트하고, 새로운 webview에서 이를 감지하여 실행하였습니다.

### **이미지 콘텐츠 촬영과 콘텐츠 로딩 문제 해결**

- **문제점**: 최신 웹 페이지에서 콘텐츠를 로딩하는 데는 지연 로딩 메커니즘, 네트워크 지연 등을 포함한 많은 과제가 있습니다.
- **해결 방법**: 느린 네트워크 속도, 동적 콘텐츠, 단일 페이지 애플리케이션, 무거운 리소스 렌더링 등의 문제를 해결하기 위해 다양한 방법을 사용하였습니다. 예를 들어, DOMContentLoaded와 onload 이벤트를 활용하여 로딩 상태를 감지하고, mutationObserver를 통해 동적으로 로드되는 콘텐츠를 감지하였습니다.

### **BrowserRouter와 HashRouter의 차이**

- **문제점**: BrowserRouter는 URL을 기준으로 라우팅하고, HashRouter는 URL의 해시 부분을 기준으로 라우팅합니다. Electron에서 BrowserRouter를 사용할 경우 흰 화면이 뜨는 문제가 발생하였습니다.
- **해결 방법**: HashRouter를 사용하여 라우팅 문제를 해결하였습니다.

### 앞으로의 개선 사항

---

### 6. 결과

---

# Auto Page - Copy Cat your Web Surfing

Auto Page는 사용자가 웹 서핑 중 수행하는 작업을 자동/수동으로 기록하고 재생할 수 있는 애플리케이션입니다.

## 주요 기능

- 웹 페이지의 특정 동작을 기록하고 재생
- 매크로 저장 및 관리
- 다양한 브라우저 탭 관리

## 어려웠던 부분과 해결 방법

### 1. 웹 페이지 캡처 기능 구현

**파일**: `src/main/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 웹 페이지의 특정 부분을 캡처하고 이를 저장하는 기능을 구현하는 데 어려움이 있었습니다. 특히, 웹뷰에서 페이지 로딩이 완료된 시점을 정확히 파악하고 캡처를 수행하는 것이 까다로웠습니다.

**알고리즘 및 기술**: `did-stop-loading` 이벤트 활용, `capturePage` 메서드 사용, 상태 관리 로직 구현

**적용 방법**:

- `did-stop-loading` 이벤트를 활용하여 웹 페이지의 로딩이 완료된 시점을 정확히 파악하였습니다. 이 이벤트를 통해 페이지 로딩이 완료되면 캡처 작업을 수행하도록 하였습니다.
- `capturePage` 메서드를 사용하여 웹 페이지의 특정 부분을 캡처하고, 이를 이미지로 저장하였습니다.
- 캡처된 이미지를 관리하기 위해 추가적인 상태 관리 로직을 구현하여, 캡처된 이미지가 적절히 저장되고 필요할 때 불러올 수 있도록 하였습니다.

### 2. 매크로 기록

**파일**: `src/preload/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 사용자 동작을 매크로로 기록하는 데 많은 도전 과제가 있었습니다. 특히, 다양한 사용자 동작을 정확히 기록하는 것이 매우 까다로웠습니다.

**알고리즘 및 기술**: `MutationObserver` 사용, DOM 변화 감지 및 기록, 단계별 기록 및 메타데이터 저장

**적용 방법**:

- `MutationObserver`를 사용하여 DOM의 변화를 감지하였습니다. 이를 통해 사용자의 동작을 실시간으로 기록할 수 있었습니다.
- DOM 변화가 감지되면 해당 변화를 매크로로 기록하고, 각 동작을 단계별로 저장하였습니다.
- 필요한 메타데이터를 함께 저장하여, 매크로 재생 시 정확히 동작하도록 하였습니다.

### 3. 매크로 재생

**파일**: `src/preload/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 기록된 매크로를 정확히 재생하는 것이 어려웠습니다. 특히, DOM 구조나 페이지 상태가 예상과 다를 경우 발생하는 오류를 처리하는 것이 까다로웠습니다.

**알고리즘 및 기술**: DOM 요소 동적 탐색, 오류 처리 로직, 단계별 실행

**적용 방법**:

- 매크로 재생 시 각 동작을 순차적으로 실행하였습니다. 이를 위해 DOM 요소를 동적으로 탐색하고, 필요한 경우 페이지 이동 등을 처리하였습니다.
- 예상치 못한 상황이 발생할 경우를 대비하여 오류 처리 로직을 추가하였습니다. 이를 통해 매크로 재생 중 발생할 수 있는 오류를 효과적으로 처리하였습니다.
- 각 동작을 단계별로 실행하여, 매크로가 정확히 재생되도록 하였습니다.

### 4. 상태 관리

**파일**: `src/renderer/src/stores/useTabStore.js`, `src/renderer/src/stores/useModalStore.js`

**어려움**: 다양한 상태를 효율적으로 관리하고, 상태 변화에 따른 UI 업데이트를 처리하는 것이 어려웠습니다.

**알고리즘 및 기술**: Zustand 라이브러리 사용, 상태 구독 및 자동 업데이트

**적용 방법**:

- Zustand 라이브러리를 사용하여 애플리케이션의 상태 관리를 단순화하였습니다. 이를 통해 상태 변화에 따른 UI 업데이트를 효율적으로 처리할 수 있었습니다.
- 각 컴포넌트는 필요한 상태만 구독하고, 상태 변화에 따라 자동으로 UI가 업데이트되도록 하였습니다.

### 5. Electron 환경 설정

**파일**: `electron-builder.yml`, `build/entitlements.mac.plist`

**어려움**: Electron 애플리케이션의 빌드 및 배포 환경을 설정하는 데 어려움이 있었습니다. 특히, MacOS에서의 권한 설정과 빌드 설정이 까다로웠습니다.

**알고리즘 및 기술**: `electron-builder` 사용, 권한 설정 파일 (`entitlements.mac.plist`) 구성

**적용 방법**:

- `electron-builder`를 사용하여 빌드 설정을 관리하였습니다. 이를 통해 다양한 플랫폼에서 애플리케이션을 쉽게 빌드하고 배포할 수 있었습니다.
- `entitlements.mac.plist` 파일을 통해 MacOS에서 필요한 권한을 명시적으로 설정하였습니다. 이를 통해 MacOS에서 애플리케이션이 정상적으로 실행되도록 하였습니다.

### 6. 브라우저 탭 관리

**파일**: `src/renderer/src/stores/useTabStore.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 여러 브라우저 탭을 효율적으로 관리하고, 각 탭의 상태를 유지하는 것이 어려웠습니다.

**알고리즘 및 기술**: Zustand를 사용한 탭 상태 관리, 탭 간 전환 시 데이터 저장 및 불러오기 로직

**적용 방법**:

- Zustand를 사용하여 여러 브라우저 탭의 상태를 효율적으로 관리하였습니다. 각 탭의 상태 변화를 추적하고, 필요한 경우 상태를 업데이트하였습니다.
- 탭 간 전환 시 필요한 데이터를 적절히 저장하고, 전환 시 데이터를 불러오는 로직을 구현하여 사용자 경험을 향상시켰습니다.

### 7. UI 컴포넌트의 스타일링

**파일**: `src/renderer/src/assets/tailwind.css`, `src/renderer/src/components/Button/Button.jsx`

**어려움**: 다양한 UI 컴포넌트의 스타일링을 일관성 있게 유지하는 것이 어려웠습니다.

**알고리즘 및 기술**: Tailwind CSS 사용, 공통 스타일 분리 및 관리

**적용 방법**:

- Tailwind CSS를 사용하여 다양한 UI 컴포넌트의 스타일링을 일관성 있게 유지하였습니다. Tailwind 클래스를 사용하여 필요한 스타일을 적용하고, 공통 스타일은 별도의 CSS 파일로 분리하여 관리하였습니다.

### 8. 사용자 입력 처리

**파일**: `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 사용자 입력을 정확히 처리하고, 각 입력 이벤트를 적절히 반영하는 것이 어려웠습니다.

**알고리즘 및 기술**: WebView 입력 이벤트 처리, 추가 입력 이벤트 생성

**적용 방법**:

- WebView 컴포넌트에서 입력 이벤트를 처리하는 로직을 구현하였습니다. 각 입력 이벤트는 WebView의 이벤트 리스너를 통해 처리되었습니다.
- 필요한 경우 추가적인 입력 이벤트를 생성하여 사용자 입력을 정확히 반영하였습니다.

### 9. 데이터 저장 및 로드

**파일**: `src/main/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 매크로 데이터와 이미지 데이터를 효율적으로 저장하고 로드하는 것이 어려웠습니다.

**알고리즘 및 기술**: 파일 시스템을 사용한 데이터 저장 및 로드, JSON 형식 데이터 저장

**적용 방법**:

- 파일 시스템을 사용하여 데이터를 저장하고 로드하는 로직을 구현하였습니다. 각 데이터는 JSON 형식으로 저장되며, 필요한 경우 데이터 변환 로직을 추가하여 데이터를 적절히 처리하였습니다.

### 10. 보안 및 권한 관리

**파일**: `electron-builder.yml`, `build/entitlements.mac.plist`

**어려움**: 애플리케이션의 보안과 권한을 적절히 관리하는 것이 어려웠습니다. 특히, MacOS에서의 권한 설정이 까다로웠습니다.

**알고리즘 및 기술**: Electron 보안 가이드라인 준수, MacOS 권한 설정

**적용 방법**:

- Electron의 보안 가이드라인을 따르고, 필요한 권한을 명시적으로 설정하였습니다. `entitlements.mac.plist` 파일을 통해 MacOS에서 필요한 권한을 설정하고, 애플리케이션이 안전하게 실행되도록 하였습니다.

### 11. 비동기 데이터 처리

**파일**: `src/main/index.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 비동기 작업을 효율적으로 처리하고, 데이터가 준비되기 전에 발생할 수 있는 문제를 방지하는 것이 어려웠습니다.

**알고리즘 및 기술**: 비동기/대기 (`async/await`) 패턴 사용, 비동기 데이터 처리 로직 구현

**적용 방법**:

- 비동기/대기 (`async/await`) 패턴을 사용하여 비동기 작업을 처리하였습니다. 이를 통해 비동기 작업이 완료되기 전에 발생할 수 있는 문제를 방지하였습니다.
- 비동기 데이터 처리 로직을 구현하여 데이터를 효율적으로 처리하였습니다.

### 13. 사용자 정의 단축키

**파일**: `src/renderer/src/constants/textConstants.js`, `src/renderer/src/components/WebView/WebView.jsx`

**어려움**: 사용자 정의 단축키를 지원하고, 사용자 인터페이스에서 쉽게 설정할 수 있도록 하는 것이 어려웠습니다.

**알고리즘 및 기술**: 단축키 설정 로직 구현, 사용자 입력 이벤트 처리

**적용 방법**:

- 사용자 정의 단축키를 설정할 수 있는 로직을 구현하였습니다. 이를 통해 사용자가 원하는 단축키를 설정할 수 있었습니다.
- 사용자 입력 이벤트를 처리하여 단축키가 올바르게 작동하도록 하였습니다.

### 14. 어떻게 iframe을 지웠나

mutationObserver 사용
