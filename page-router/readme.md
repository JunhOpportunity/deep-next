### 기본 세팅

`npx prisma studio` : DB 깔끔한 UI로 확인 가능

`npx create-next-app@14 폴더이름` : 최신 버전 node package 다운

`import alias` : 절대 경로를 사용하는 것

# 기본 지식

### 사전 렌더링

브라우저의 요청에 사전에 렌더링이 완료된 HTML을 응답하는 렌더링 방식
CSR의 단점을 효율적으로 해결하는 기술

# Page Router

파일 또는 폴더 이름을 기준으로 페이지 라우팅을 제공

동적 경로의 경우에는 대괄호를 사용한다. /item/[id].js

## pages 폴더

### _app.tsx

모든 페이지에 공통적으로 적용될 레이아웃이나 데이터 적용을 위한 페이지이다.

_app.tsx 파일의 App 컴포넌트는, 모든 페이지의 부모가 되는 컴포넌트라고 보면 된다.

App 컴포넌트에 Nav bar 또는 header 를 추가하면 모든 페이지에 적용된다.

### _document.tsx

기존 React의 index.html 이라고 보면 된다.

Meta, font 등등의 html 태그 관리

### next.config.mjs

nextjs의 기본 설정들을 관리하는 곳.

reactStrictMode 설정도 가능하다. (잠재적 에러를 확인하기 위해 두 번 실행되는 것)