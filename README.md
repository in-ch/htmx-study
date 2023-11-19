[공식 문서](https://htmx.org/docs/)

# 사용법

- hx-post: post 통신을 한다.
- hx-swap: http 통신을 하고 그 결과값을 변환을 시킨다.

```html
<button hx-post="/clicked" hx-swap="outerHTML">
    Click Me
</button>
<script src="https://unpkg.com/htmx.org@1.9.8"></script>
```

# Ajax Method 
| Attribute  | Description                                      |
|------------|--------------------------------------------------|
| `hx-get`   | Issues a GET request to the given URL             |
| `hx-post`  | Issues a POST request to the given URL            |
| `hx-put`   | Issues a PUT request to the given URL              |
| `hx-patch` | Issues a PATCH request to the given URL            |
| `hx-delete`| Issues a DELETE request to the given URL           |

# hx-trigger 

- hx-trigger: 이 속성을 사용하면 AJAX 요청을 트리거하는 항목을 지정할 수 있다.
ex)
```html
<div hx-get="/clicked" hx-trigger="click[ctrlKey]">Control Click Me</div>
<div hx-get="/clicked" hx-trigger="click[checkGlobalState()]">Control Click Me</div>
<div hx-get="/clicked" hx-trigger="click[ctrlKey&&shiftKey]">Control-Shift Click Me</div>
```
다음과 같이 이벤트 이름 뒤에 자바스크립트 표현식을 대괄호로 묶어 이벤트를 필터링할 수 있다. 

### 표준 이벤트 수정자
- <code>once</code>: 이벤트는 한 번만 실행된다. (예. 첫 번째 클릭)
- <code>changed</code>: 요소의 값이 변경된 경우에만 이벤트가 변경된다. 주의할 점은 <code>change</code>라는 이벤트 이름과 <code>changed</code>수식어와 다른 이름이다.
- <code>delay:<timing declaration></code>: 이벤트가 요청을 트리거하기 전에 지연이 발생한다. 이벤트가 다시 표시되면 지연은 재설정된다.
- <code>throttle:<timing declaration></code>: 이벤트가 요청을 트리거한 후에 지연이 발생한다. 지연이 완료되기 전에 이벤트가 다시 표시되면 무시되고 지연이 끝날 때 요소가 트리거 된다. 

### Polling & Load Polling 

- Polling
```html
<div hx-get="/news" hx-trigger="every 2s"></div>
```

- Load Polling
```html
<div hx-get="/messages"
    hx-trigger="load delay:1s"
    hx-swap="outerHTML"
>
</div>
```

# Target & Swap 

### hx-target

<code>hx-target</code> 해당 기능을 통해 요청에 대한 결과값을 수신할 곳을 정할 수 있는데, css 선택자를 통해서 더 디테일한 설정도 가능하다. 
```html
<input type="text" name="q"
    hx-get="/trigger_delay"
    hx-trigger="keyup delay:500ms changed"
    hx-target="#search-results"
    placeholder="Search..."
>
<div id="search-results"></div>
```
- 이렇게 하면 <code>div#search-results</code>에 결과값이 나오게 된다. 

### <code>from:<Extended CSS selector></code>
- 문서의 다른 요소에서 요청을 트리거하는 이벤트가 생긴다. 예를 들어 <code>from:input</code>은 페이지의 모든 입력을 수신하게 된다. 
- 여기에서 확장 CSS 선택기는 다음과 같은 비표준 CSS 값을 허용한다. 
    1. <code>document</code>: 문서의 이벤트를 수신한다.
    2. <code>window</code>: 창에서 이벤트를 수신한다.
    3. <code>closet <CSS selector></code>: 주어진 CSS 선택기와 일치하는 가장 가까운 자식을 찾는다.
    4. <code>find <CSS selector></code>: 주어진 CSS 선택기와 일치하는 첫 번째 요소를 찾기 위해 다음 DOM을 찾는다. 형제 요소를 찾는 것인듯.. ex) <code>next .error</code>
    5. <code>previous</code>: 이건 <code>next</code>랑 반대로 뒤에 있는 형제 요소를 찾는다. 
    6. <code>target:<CSS selector></code>: 이벤트 대상에서 CSS 선택기를 통해 필터링할 수 있다.
    7. <code>consume</code>: 이 옵션을 포함하게 되면 이벤트 부모에 대한 다른 htmx 요청을 트리거 하지 않게 된다.
    8. <code>queue:<queue option></code>: 다른 이벤트에 대한 요청이 진행 중인 동안 이벤트가 발생하면 대기열에 추가하는 데 이 때 추가 방법을 정의할 수 있다. ex) first, last, all, none
    ```html
    <input name="q"
       hx-get="/search" hx-trigger="keyup changed delay:1s"
       hx-target="#search-results"/>
    ```
    여기서 keyup을 할 경우 1초 딜레이가 된다음에 hx-target에 결과값이 반영이 되게 된다. 

### hx-swap 
스왑을 하는 방법을 지정할 수 있다. 

| Name         | Description                                                 |
|--------------|-------------------------------------------------------------|
| `innerHTML`  | 기본값으로 타겟 엘리먼트 내부에 콘텐츠를 삽입합니다.           |
| `outerHTML`  | 반환된 콘텐츠로 전체 타겟 엘리먼트를 대체합니다.               |
| `afterbegin` | 콘텐츠를 타겟 내 첫 번째 자식 앞에 추가합니다.              |
| `beforebegin`| 콘텐츠를 타겟 앞에 타겟의 부모 엘리먼트 내에 추가합니다.    |
| `beforeend`  | 콘텐츠를 타겟 내 마지막 자식 뒤에 추가합니다.              |
| `afterend`   | 콘텐츠를 타겟 뒤에 타겟의 부모 엘리먼트 내에 추가합니다.    |
| `delete`     | 응답과 관계없이 타겟 엘리먼트를 삭제합니다.                  |
| `none`       | 응답에서 콘텐츠를 추가하지 않습니다. (Out of Band Swaps 및 Response Headers는 여전히 처리됩니다.) |
