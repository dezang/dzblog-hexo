---
title: 자바스크립트에서 이전 페이지로 이동하는 방법
date: 2014-09-11 10:30:00
categories:
- develop
tags:
- javascript
---

에러 페이지를 개발하면서 이전 페이지로 이동하는 링크를 만들어야 했다. 굳이 링크가 없더라도 웹 브라우저에 뒤로 가기를 누르면 되겠지만, 사용자의 편의성을 고려해야겠지. 이런 작은 부분들을 신경 쓴 것과 안 쓴 것은 개별로 보면 별 것 아닌 것처럼 느껴지지만, 이런 부분들이 모여 나중에 프로젝트의 완성도에 영향을 미친다고 생각한다.

이전 페이지로 이동하는 자바스크립트 코드는 다음 두 가지가 있다.

<!-- more -->

```javascript
history.back();
history.go(-1);
```

두 코드 모두 같은 동작을 하지만 `history.go(-1)`은 이동할 페이지의 갯수를 인자값으로 넘겨주기 때문에 활용도가 더 높다. 추가적으로 대장의 경우 스프링 시큐리티를 적용하는 프로젝트라서 `security-context.xml` 에 아래와 같이 선언해 놓았다.

```xml
<!-- access denied page -->
<access-denied-handler error-page="/403" />
```

만약 관리자 권한이 없는 계정이 관리자 페이지의 접근할 경우 `http://siteAddress/admin` 으로 이동했다가 권한이 없다고 판정되어 /403 으로 이동했으므로, 위 코드를 그대로 적용하면 한 페이지 뒤로 가기 때문에 /admin 경로로 접근하게 되어 다시 403 에러 페이지가 보이게 된다.

따라서 history.go(); 함수를 사용하되 인자값으로 -2를 주면 관리자 페이지에 접근하기 전 페이지로 이동하게 된다. 이를 적용하여 jsp 파일에 추가한 코드는 아래와 같다.

```html
<a href="#" onClick="history.go(-2);">이전페이지로</a>
```