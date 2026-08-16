# 개념 03 - input 과 change

## 1. 이번 파일에서 배우는 것

입력칸의 값은 사용자가 직접 바꾸기 전까지는 달라지지 않는다.
그래서 입력값을 다루려면 "값이 바뀌는 순간"을 잡아내는 이벤트가 필요하다.

이번 예제의 핵심 이벤트는 두 가지다.

| 이벤트 | 실행되는 때 | 주로 쓰는 곳 |
| --- | --- | --- |
| `input` | 글자가 바뀔 때마다 바로 실행 | 실시간 검색, 글자 수 세기, 즉시 계산 |
| `change` | 입력을 끝내고 칸을 벗어났을 때 실행 | 최종 값 확인, 체크박스, 선택 상자 |

## 2. input 이벤트

`input` 이벤트는 사용자가 글자를 입력하거나 지울 때마다 실행된다.

```js
nameInput.addEventListener("input", (e) => {
  const value = e.target.value;
  out1.textContent = value;
});
```

예를 들어 `김민준`을 입력하면 콜백 함수가 한 번만 실행되는 것이 아니라,
`김`, `김민`, `김민준`처럼 글자가 바뀔 때마다 계속 실행된다.

그래서 `input`은 화면을 바로바로 바꿔야 할 때 사용한다.

- 입력한 이름을 바로 보여 주기
- 글자 수 실시간 계산하기
- 가격과 개수를 입력하면 합계를 즉시 계산하기
- 검색어를 입력할 때마다 후보 보여 주기

## 3. change 이벤트

`change` 이벤트는 입력 도중에는 실행되지 않고, 입력을 끝낸 뒤 다른 곳으로 이동했을 때 실행된다.

```js
changeInput.addEventListener("change", (e) => {
  out2.textContent = `확정된 값: ${e.target.value}`;
});
```

텍스트 입력칸에서는 타이핑만 했을 때는 아무 반응이 없다.
입력 후 `Tab`을 누르거나 다른 곳을 클릭해서 입력칸을 벗어나면 그때 실행된다.

최종 값만 필요하거나, 실행 비용이 큰 작업에는 `change`가 더 적절할 수 있다.

## 4. e.target.value

이벤트 함수 안에서 현재 입력값을 읽을 때는 보통 `e.target.value`를 쓴다.

```js
input.addEventListener("input", (e) => {
  console.log(e.target.value);
});
```

여기서 `e.target`은 이벤트가 발생한 요소다.
즉, 사용자가 입력한 바로 그 입력칸을 뜻한다.

## 5. 글자 수 세기

문자열의 길이는 `.length`로 구한다.

```js
memoArea.addEventListener("input", (e) => {
  const length = e.target.value.length;
  out3.textContent = `${length} / 20`;
});
```

글자 수 제한을 넘었는지 확인할 때는 조건식을 함께 쓴다.

```js
out3.classList.toggle("over", length > maxLength);
```

`toggle`의 두 번째 값이 `true`면 클래스를 붙이고, `false`면 클래스를 뗀다.

## 6. 숫자 입력값 계산하기

입력칸에서 가져온 값은 겉으로 숫자처럼 보여도 실제로는 문자열이다.
계산할 때는 `Number()`로 숫자로 바꿔 주는 습관이 중요하다.

```js
function updateTotal() {
  const price = Number(priceInput.value);
  const count = Number(countInput.value);
  const total = price * count;

  out4.textContent = `합계: ${total}원`;
}
```

특히 `+` 연산자는 조심해야 한다.

```js
"4500" + "2"; // "45002"
```

문자열끼리 더하면 숫자 덧셈이 아니라 글자 이어 붙이기가 된다.

## 7. 같은 함수를 여러 입력칸에 붙이기

가격이 바뀌어도 합계가 다시 계산되어야 하고, 개수가 바뀌어도 합계가 다시 계산되어야 한다.
이럴 때는 계산 코드를 함수로 만든 뒤 여러 이벤트에 붙인다.

```js
priceInput.addEventListener("input", updateTotal);
countInput.addEventListener("input", updateTotal);
```

주의할 점은 함수 이름만 전달해야 한다는 것이다.

```js
addEventListener("input", updateTotal);   // 맞음
addEventListener("input", updateTotal()); // 틀림
```

`updateTotal()`처럼 괄호를 붙이면 이벤트가 발생했을 때 실행되는 것이 아니라,
코드를 읽는 순간 바로 실행되어 버린다.

## 8. 체크박스와 select

체크박스와 선택 상자는 보통 `change` 이벤트를 사용한다.

```js
agreeCheck.addEventListener("change", updateOptions);
sizeSelect.addEventListener("change", updateOptions);
```

체크박스는 `.value`가 아니라 `.checked`를 확인한다.

```js
const agreeText = agreeCheck.checked ? "함" : "안 함";
```

`.checked`는 체크되어 있으면 `true`, 체크되어 있지 않으면 `false`다.

선택 상자는 `.value`로 현재 선택된 값을 읽는다.

```js
const size = sizeSelect.value;
```

## 9. 처음 화면 맞추기

이벤트는 사용자가 값을 바꿨을 때만 실행된다.
페이지를 처음 열었을 때의 화면까지 맞추려면 함수를 직접 한 번 호출해야 한다.

```js
updateTotal();
updateOptions();
```

이렇게 하면 사용자가 건드리기 전에도 초기값에 맞는 결과가 화면에 표시된다.

## 10. 자주 하는 실수

### 실수 1. 숫자로 바꾸지 않고 계산하기

```js
const total = priceInput.value + countInput.value;
```

입력값은 문자열이므로 `"4500" + "2"`가 되어 `"45002"`가 나올 수 있다.
계산 전에는 `Number()`를 사용한다.

### 실수 2. 이벤트 이름에 on 붙이기

```js
addEventListener("onchange", updateOptions); // 틀림
addEventListener("change", updateOptions);   // 맞음
```

`addEventListener`에 넣는 이벤트 이름에는 `on`을 붙이지 않는다.

### 실수 3. 체크박스에 value 사용하기

```js
agreeCheck.value;   // 보통 "on"
agreeCheck.checked; // true 또는 false
```

체크 여부를 알고 싶으면 `.checked`를 쓴다.

### 실수 4. 처음 화면을 함수로 맞추지 않기

이벤트는 사용자가 바꿀 때만 실행된다.
초기 화면이 필요하면 `updateTotal()`처럼 함수를 한 번 직접 실행한다.

## 11. 한 줄 정리

- `input`은 입력값이 바뀔 때마다 실행된다.
- `change`는 입력을 확정했을 때 실행된다.
- 입력값은 `e.target.value`로 읽는다.
- 숫자 계산 전에는 `Number()`로 변환한다.
- 체크박스는 `.checked`, 선택 상자는 `.value`를 쓴다.
- 같은 동작이 여러 곳에서 필요하면 함수로 만들고 여러 이벤트에 연결한다.
- 페이지를 처음 열었을 때 화면을 맞추려면 함수를 직접 한 번 호출한다.
