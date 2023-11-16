# 사용법

- hx-post: post 통신을 한다.
- hx-swap: http 통신을 하고 그 결과값을 변환을 시킨다.

```html
<button hx-post="/clicked" hx-swap="outerHTML">
    Click Me
</button>
<script src="https://unpkg.com/htmx.org@1.9.8"></script>
```