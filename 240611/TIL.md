# React 19 

React 19은 개발 경험을 향상시키고, 성능을 최적화하며, 새로운 기능을 도입했습니다. 이번 README에서는 주요 신기술들과 예시 코드를 소개합니다.

## 목차

1. [서버 컴포넌트 (Server Components)](#서버-컴포넌트-server-components)
2. [use() 훅](#use-훅)
3. [문서 메타데이터 관리](#문서-메타데이터-관리)
4. [개선된 자산 로딩](#개선된-자산-로딩)
5. [향상된 훅 및 컨텍스트 API](#향상된-훅-및-컨텍스트-api)
6. [웹 컴포넌트 통합](#웹-컴포넌트-통합)
7. [Ref를 프로퍼티로 전달](#ref를-프로퍼티로-전달)

## 서버 컴포넌트 (Server Components)

서버 컴포넌트는 서버에서 렌더링되어 클라이언트로 전송됩니다. 이를 통해 초기 로드 시간을 줄이고, 클라이언트 측의 부담을 줄일 수 있습니다.

### 특징

- 서버에서 렌더링하여 클라이언트로 전송
- 초기 로드 시간 단축
- 클라이언트 측의 렌더링 부담 감소

### 예시 코드

```jsx
// ServerComponent.server.js
export default function ServerComponent() {
  return (
    <div>
      <h1>서버에서 렌더링된 컴포넌트</h1>
    </div>
  );
}

// App.js
import ServerComponent from './ServerComponent.server';

export default function App() {
  return (
    <div>
      <ServerComponent />
    </div>
  );
}

```

## use() 훅

use() 훅은 비동기 작업을 처리하고, 그 결과를 React 컴포넌트 내에서 사용할 수 있게 합니다.

### 특징

- 비동기 작업을 간단하게 처리
- 컴포넌트 내에서 비동기 상태 관리

### 예시 코드

```jsx
import { use } from "react";

function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("데이터 로드 완료"), 1000);
  });
}

export default function App() {
  const data = use(fetchData());

  return (
    <div>
      <h1>{data}</h1>
    </div>
  );
}
```

## 문서 메타데이터 관리

React 19에서는 메타데이터를 컴포넌트 내에서 직접 관리할 수 있습니다.

### 특징

- 메타데이터를 컴포넌트 내에서 정의
- 외부 라이브러리 불필요

### 예시 코드

```jsx

import { Helmet } from 'react-helmet';

export default function App() {
  return (
    <div>
      <Helmet>
        <title>React 19 App</title>
        <meta name="description" content="React 19의 새로운 기능을 소개합니다." />
      </Helmet>
      <h1>안녕하세요, React 19!</h1>
    </div>
  );
}


export default function App() {
    return (
        <div>
            <title>React 19 App</title>
            <meta name="description" content="React 19의 새로운 기능을 소개합니다." />
            <h1>안녕하세요, React 19!</h1>
        </div>
    )
}


```

## 개선된 자산 로딩

백그라운드에서 자산을 로딩하여 초기 로드 시간을 단축합니다.

### 특징

- 초기 로드 시간 단축
- 자산을 비동기적으로 로딩

### 예시 코드

```jsx

import React, { useEffect } from "react";

export default function App() {
  useEffect(() => {
    const img = new Image();
    img.src = "path/to/image.jpg";
  }, []);

  return (
    <div>
      <h1>이미지가 백그라운드에서 로드됩니다.</h1>
    </div>
  );
}

```

## 향상된 훅 및 컨텍스트 API

React 19에서는 새로운 훅과 컨텍스트 API의 개선된 사용법이 도입되었습니다.


### 특징

- 새로운 훅 도입
- 컨텍스트 API 사용 간편화

### 예시 코드

```jsx

import React, { createContext, useContext } from 'react';

const MyContext = createContext();

function MyProvider({ children }) {
  const value = "컨텍스트 값";
  return (
    <MyContext.Provider value={value}>
      {children}
    </MyContext.Provider>
  );
}

function MyComponent() {
  const value = useContext(MyContext);
  return <div>{value}</div>;
}

export default function App() {
  return (
    <MyProvider>
      <MyComponent />
    </MyProvider>
  );
}

```

## 웹 컴포넌트 통합

React 19에서는 웹 컴포넌트와의 통합이 더욱 쉬워졌습니다.


### 특징

- 웹 컴포넌트와의 원활한 통합
- 기존 웹 컴포넌트를 React 프로젝트에 쉽게 통합

### 예시 코드

```jsx

import React, { useEffect } from 'react';

class MyWebComponent extends HTMLElement {
  connectedCallback() {
    this.innerHTML = "<p>웹 컴포넌트 내용</p>";
  }
}

customElements.define('my-web-component', MyWebComponent);

export default function App() {
  useEffect(() => {
    customElements.define('my-web-component', MyWebComponent);
  }, []);

  return (
    <div>
      <my-web-component></my-web-component>
    </div>
  );
}

```

## Ref를 프로퍼티로 전달

이제 refs를 프로퍼티로 전달할 수 있어, forwardRef를 사용하지 않고도 refs를 관리할 수 있습니다.

### 특징

- refs를 프로퍼티로 전달
- forwardRef 사용 필요성 감소

### 예시 코드

```jsx

import React, { useRef } from 'react';

function CustomInput({ inputRef }) {
  return <input ref={inputRef} />;
}

export default function App() {
  const inputRef = useRef();

  return (
    <div>
      <CustomInput inputRef={inputRef} />
      <button onClick={() => inputRef.current.focus()}>포커스</button>
    </div>
  );
}

```