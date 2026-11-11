# 🌐 Web-Programming-Study: Chapter 10. 윈도우 및 브라우저 관련 객체 (BOM)

  > 본 문서에서는 자바스크립트를 이용해 웹 브라우저 환경 및 윈도우 창을 제어하는 **BOM(Browser Object Model)**의 핵심 개념과 주요 객체들의 활용법을 다룹니다.

---

## 📌 목차 (Table of Contents)
1. [BOM (Browser Object Model) 개요](#1-bom-browser-object-model-개요)
2. [window 객체 (Window Object)](#2-window-객체-window-object)
   - [윈도우 창 열기 및 닫기](#-윈도우-창-열기-및-닫기)
   - [타이머 활용 (Timer Functions)](#-타이머-활용-timer-functions)
   - [웹 페이지 프린트 제어](#-웹-페이지-프린트-제어)
3. [location 객체 (Location Object)](#3-location-객체-location-object)
4. [navigator 객체 (Navigator Object)](#4-navigator-객체-navigator-object)
5. [screen 객체 (Screen Object)](#5-screen-객체-screen-object)
6. [history 객체 (History Object)](#6-history-객체-history-object)

---

## 1. BOM (Browser Object Model) 개요

**BOM(브라우저 객체 모델)**은 자바스크립트로 웹 브라우저 환경과 윈도우 창을 제어하기 위해 제공되는 객체들의 집합입니다.

### 💡 DOM vs BOM 비교
- **DOM (Document Object Model)**: HTML 문서의 구조와 콘텐츠(`<body>`, `<div>`, `<p>` 등)를 제어하는 객체 트리의 집합
- **BOM (Browser Object Model)**: HTML 문서 내용과 무관하게 브라우저 창, 히스토리, 화면 해상도, URL 등 브라우저 자체 환경을 제어하는 객체

```text
┌────────────────────────────────────────────────────────┐
│                        window                          │
├──────────────┬─────────────┬────────────┬──────────────┤
│  navigator   │   history   │  location  │    screen    │
└──────────────┴─────────────┴────────────┴──────────────┘
                               │
                           document (DOM)
```

---

## 2. window 객체 (Window Object)

`window` 객체는 열려 있는 브라우저 윈도우나 탭 윈도우를 나타내는 최상위 객체입니다.

### 📍 생성 및 접근
- **생성 조건**: 브라우저가 새 페이지를 로드할 때, `<iframe>` 태그당 1개씩, 또는 `window.open()` 호출 시 생성
- **접근 키워드**: `window`, `window.self`, 또는 `self`

### 📍 주요 크기 및 좌표 프로퍼티
- `innerWidth`, `innerHeight`: 브라우저 내 문서가 실제 출력되는 영역의 너비와 높이
- `outerWidth`, `outerHeight`: 툴바 및 테두리를 포함한 브라우저 창 전체의 너비와 높이
- `screenX`, `screenY`: 모니터 화면 기준 브라우저 창의 왼쪽 상단 X, Y 위치

### 🔑 윈도우 창 열기 및 닫기
```javascript
// window.open(URL, windowName, windowFeatures)
let win = window.open("http://www.naver.com", "myWin", "left=300,top=300,width=400,height=300");

// 윈도우 닫기
win.close();        // 새로 연 자식 윈도우 닫기
window.close();     // 현재 윈도우 닫기
```

### ⏱️ 타이머 활용 (Timer Functions)

| 구분 | 1회성 실행 | 반복 실행 |
| :--- | :--- | :--- |
| **타이머 설정** | `setTimeout(code, delayMs)` | `setInterval(code, delayMs)` |
| **타이머 해제** | `clearTimeout(timerID)` | `clearInterval(timerID)` |

```javascript
// 5초 후 Naver 사이트로 이동하는 타이머 설정
let timerID = setTimeout(() => {
  window.location.href = "https://www.naver.com";
}, 5000);

// 특정 조건 발생 시 타이머 중단
function cancelTimer() {
  if (timerID !== null) {
    clearTimeout(timerID);
  }
}
```

### 🖨️ 웹 페이지 프린트 제어
```javascript
// 인쇄 다이얼로그 호출
window.print();
```

- `onbeforeprint`: 인쇄 직전 호출되는 리스너 (예: 인쇄 전 전용 로고 출력)
- `onafterprint`: 인쇄 완료/취소 직후 호출되는 리스너 (예: 변경된 스타일 복원)

```javascript
window.onbeforeprint = function() {
  document.getElementById("print-logo").style.display = "block";
};

window.onafterprint = function() {
  document.getElementById("print-logo").style.display = "none";
};
```

---

## 3. location 객체 (Location Object)

`location` 객체는 현재 윈도우에 로드된 문서의 **URL 정보**를 관리하고 페이지 이동을 제어합니다.

### 📍 URL 구조 및 프로퍼티
예시 URL: `http://www.mysite.com:8080/content/index.html#section1`

- `protocol`: `http:`
- `hostname`: `www.mysite.com`
- `port`: `8080`
- `host`: `www.mysite.com:8080`
- `pathname`: `/content/index.html`
- `hash`: `#section1`

### 🔄 페이지 이동 방법
```javascript
// 1. 가장 흔히 사용하는 방식
window.location.href = "https://www.google.com";

// 2. assign() 메서드로 이동 (방문 기록 유지)
window.location.assign("https://www.google.com");

// 3. replace() 메서드로 이동 (현재 히스토리 덮어쓰기, 뒤로가기 불가)
window.location.replace("https://www.google.com");
```

---

## 4. navigator 객체 (Navigator Object)

현재 자바스크립트가 실행되는 **웹 브라우저의 정보**를 담고 있는 객체입니다.

| 프로퍼티 | 설명 | Read/Write |
| :--- | :--- | :--- |
| `appName` | 브라우저 이름 문자열 | Read Only |
| `appVersion` | 브라우저의 플랫폼 및 버전 정보 | Read Only |
| `userAgent` | HTTP 요청 헤더의 User-Agent 필드값 (클라이언트 식별용) | Read Only |
| `platform` | 운영체제(OS) 플랫폼 정보 | Read Only |
| `language` | 브라우저 설정 언어 (예: `"ko-KR"`) | Read Only |
| `onLine` | 현재 네트워크 연결 상태 (`true` / `false`) | Read Only |
| `cookieEnabled`| 쿠키 허용 설정 여부 (`true` / `false`) | Read Only |
| `geolocation` | 사용자 위치 정보 접근을 위한 Geolocation 객체 레퍼런스 | Read Only |

---

## 5. screen 객체 (Screen Object)

클라이언트 디바이스의 **모니터 해상도 및 색상 표현 정보**를 제공합니다.

| 프로퍼티 | 설명 | Read/Write |
| :--- | :--- | :--- |
| `width` | 스크린 전체 수평 픽셀 수 (해상도 너비) | Read Only |
| `height` | 스크린 전체 수직 픽셀 수 (해상도 높이) | Read Only |
| `availWidth` | 작업 표시줄 등을 제외한 실제 사용 가능 너비 | Read Only |
| `availHeight` | 작업 표시줄 등을 제외한 실제 사용 가능 높이 | Read Only |
| `colorDepth` | 한 픽셀당 표현 가능한 비트 수 (예: 24비트 = $2^{24}$ 색상) | Read Only |
| `pixelDepth` | 한 픽셀당 비트 수 (`colorDepth`와 동일) | Read Only |

```javascript
console.log(`모니터 전체 해상도: ${screen.width} x ${screen.height}`);
console.log(`작업 표시줄 제외 사용 가능 크기: ${screen.availWidth} x ${screen.availHeight}`);
```

---

## 6. history 객체 (History Object)

사용자가 현재 브라우저 탭에서 방문했던 **웹 페이지 리스트(방문 기록)**를 다룹니다.

| 구분 | 프로퍼티 / 메서드 | 설명 |
| :--- | :--- | :--- |
| **프로퍼티** | `length` | 히스토리 목록에 저장된 방문 페이지 수 |
| **메서드** | `back()` | 이전 페이지로 이동 (브라우저 뒤로가기 버튼과 동일) |
| **메서드** | `forward()` | 다음 페이지로 이동 (브라우저 앞으로가기 버튼과 동일) |
| **메서드** | `go(n)` | 상대적 위치 $n$ 만큼 이동 (`-1`: 이전, `1`: 다음) |

```javascript
// 이전 페이지로 이동
history.back();
// history.go(-1);

// 다음 페이지로 이동
history.forward();
// history.go(1);
```

---

## 📝 핵심 요약 (Takeaways)

1. **BOM 최상위 객체는 `window`**이며, `document`, `location`, `navigator`, `screen`, `history` 객체를 하위로 관리합니다.
2. 비동기 처리 타이머 함수(`setTimeout`, `setInterval`)는 메모리 누수를 방지하기 위해 **반드시 해제 메서드(`clearTimeout`, `clearInterval`)**를 함께 사용하는 습관을 가지는 것이 좋습니다.
3. SPA(Single Page Application) 개발 시 페이지 새로고침 없이 주소 및 방문 기록을 관리하기 위해 **`location` 및 `history` 객체**가 빈번하게 활용됩니다.
``────
