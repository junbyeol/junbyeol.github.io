---
title: 'Javascript 표준'
date: 2020-12-17 12:00:00
category: 'web'
draft: false
---

> 이 글은 윤준성의 개발꼬맹이 시절, 혼자 노션에 공부하며 정리해둔 것 중 괜찮은 것을 추려올린 글입니다.
> 기술블로그 글 기고 목적으로 작성되지 않아, 가독성이 좋지 않거나 알 수 없는 워딩이 있을 수 있습니다.

[https://wormwlrm.github.io/2020/08/12/History-of-JavaScript-Modules-and-Bundlers.html](https://wormwlrm.github.io/2020/08/12/History-of-JavaScript-Modules-and-Bundlers.html)

[https://ahnheejong.name/articles/ecmascript-tc39/](https://ahnheejong.name/articles/ecmascript-tc39/)

## ECMAScript

- 1990년대 중반, Netscape Navigator가 브라우저에 동적인 부분을 더하기 위해 개발
- 1996년, Microsoft가 상표권 이슈를 피하기 위해 JScript라는 자바스크립트의 방언을 개발
- 이들이 찾아간 곳은 ECMA 인터내셔널(Ecma International)
    - 정보와 통신 시스템을 위한 국제적 표준화 기구
    - ASCII 코드, CD-ROM 볼륨, JSON 등 온갖 표준을 책임지고 있다
- 이들간의 합의 끝에 ECMA-262(ECMAScript 언어 표준)이라는 표준이 탄생하고, 이를 ECMA 인터네셔널의 산하 위원회, TC39(Technical Committee 39)가 관리하게 된다
- 2020년 6월까지 ECMA-262의 개정판이 11판 까지 나왔다
    - 개정 연도에 따라 ECMAScript 2015 혹은 ES2015로 불리는 것.
    - 또한, 몇번째 개정판인지에 따라 ES6라고 불리기도 한다.
    - 즉, 2015년에 나온 6번째 개정판이 ES6 == ES2015

> 즉, ECMAScript가 사양이라면, Javascript는 그 구현체.
> 

## Javascript 모듈화

- 원래 자바스크립트는 모듈화가 없었다

```jsx
<html>
  <script src="/src/foo.js"></script>
  <script src="/src/bar.js"></script>
  <script src="/src/baz.js"></script>
  <script src="/src/qux.js"></script>
  <script src="/src/quux.js"></script>
</html>
```

- 그래서 여러 js 파일들을 일일이 <script>로 가져와야 하는데, 이는 큰 문제가 있다.
    - 모든 변수들이 전역 컨텍스트에 담긴다
        - 이름이 같은 두 변수가 있다면.. js 파일 호출 순서에 따라 덮어쓰기가 돼버린다!
- 이를 어떻게 해결해야 하는가? 해결방법은 크게 ES6 전과 후로 나뉜다

## ES6 이전의 js 모듈화

- CommonJS, AMD 등 확장된 Syntax를 지원하는 일종의 애드온을 활용했다

<aside>
💡 CommonJS, AMD는 사실 명세일 뿐. 구현체는 따로 있어야 한다. 이 구현체를 우리는 모듈 로더(Module Loader)라고 부른다. AMD의 대표적인 모듈 로더는 RequireJS.

</aside>

- 물론, CommonJS나 AMD를 지원하지 않는 브라우저/엔진 이라면 활용 불가능

### CommonJS

```jsx
// CommonJS

// 모듈 정의
module.exports = foo;

// 모듈 사용
const foo = require("./foo");
```

- 동기적인 모듈 호출이 특징
- 장점
    - 동기 코드라 직관적
    - 지금까지도 NodeJS가 이를 채택하고 사용중
- 단점
    - 비동기보다 느리다
    - 트리쉐이킹(tree shaking, 임포트 되었지만, 실제로 사용되지 않는 코드를 찾는 최적화 기술)
    - 동기적으로 작동하여, 브라우저에서는 사용할 수 없다
        - CommonJS 방식을 브라우저에서 사용하기위한 빌드도구 Browserify도 등장

### AMD(Asynchronous Module Definition)

```jsx
//AMD

//모듈 정의
defineModule('util', {  
    trim: function () { 
        //
    },
    extend: function () {
        //
    }
});

//모듈 사용
var util = loadModule('util');  
util.trim();
```

[//todo](//todo) AMD 신택스

## ES6의 모듈화

- ES6에서 드디어 자바스크립트가 모듈화를 지원!

```jsx
// ES6
import foo from "bar";

export default qux;
```

- 장점
    - 동기/비동기 로드를 모두 지원
    - 간단한 문법
    - 실제 객체/함수를 바인딩하기에 순환 참조 관리가 편리
    - 코드 정적 분석(코드를 실행하지 않기 전에 하는 분석)이 가능하여, 트리 쉐이킹이 가능
- 단점
    - 비교적 최근에 정의된 문법이라 구형 브라우저가 구현하지 않을 수 있음
        - 그래서 CommonJS, AMD, ES6를 모두 지원하는 SystemJS가 나오기도..

## 트랜스파일러

- ES6 문법으로 일단 js 코드를 작성
- 트랜스파일러가 이 코드를 옛날 js코드로 바꿔주면 구형 브라우저에서도 쓸 수 있다!
    - 가장 유명한 트랜스파일러가 바벨(babel)
- 트랜스파일러가 있으면 꼭 js로 짤 필요도 없지!
    - TypeScript, CoffeeScript 같은 자바스크립트의 슈퍼셋 언어로 코드를 작성하자!