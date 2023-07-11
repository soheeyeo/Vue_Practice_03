# Vue_Practice_03
## ajax
#### axios 설치

```
npm install axios
```

#### url로 get 요청
```tsx
import axios from 'axios';

axios.get('서버URL').then((result) => {
  console.log(result.data);
})
```

#### 실패시 .catch()
ajax 요청 실패시 .catch 사용. <br>

```tsx
import axios from 'axios';

axios.get('서버URL').then((result) => {
  console.log(result.data);
}).catch(()=>{
  console.log('err');
});
```

#### post 요청
`post('서버URL', '보낼 데이터')` <br>
보낼 데이터는 문자, object 모두 가능. <br>

```tsx
import axios from 'axios';

axios.post('서버URL', {name : 'Kim'}).then((result) => {
  console.log(result.data);
}).catch(()=>{
  console.log('err');
});
```

## slot
slot으로 부모 → 자식데이터 전송 가능. <br>
자식 컴포넌트에서 데이터를 보여줄 곳에 `<slot>` 태그 작성. <br>

```tsx
<button class="buy-btn">
  <slot></slot>
</button>
```

부모 컴포넌트에서는 자식 컴포넌트 태그 사이에 데이터 넣기. <br>
```jsx
<BuyButton>
  Buy Now
</BuyButton>
```

#### Named Slots
`<slot>`을 여러 개 사용하고 싶을 때. <br>
자식 컴포넌트에서 `<slot>`을 name 속성으로 구분. <br> 

```tsx
<div class="item-card">
  <slot name="item"></slot>
  <slot name="price"></slot>
</div>
```

부모 컴포넌트에서 `<template>`을 사용해 `v-slot:` 옆에 `<slot>` 이름 적어 데이터 보내기. <br>

```tsx
<ItemCard>
  <template v-slot:item>
    <h2>텀블러</h2>
  </template>
  <template v-slot:price>
    <p>31,000원<p>
   </template>
</ItemCard>
```

#### Scoped Slots
부모 컴포넌트에서 자식 데이터 필요한 경우. <br>
`<slot>` 요소에 연결된 속성을 <b>slot props</b>라고 함. <br> 
자식 컴포넌트에서 `<slot : 데이터="데이터">` 형식으로 데이터 보냄. <br>

```tsx
<template>
  <div>
    <slot :message="message"></slot>
  </div>
</template>

<script>
export default {
  name : 'MsgBox',
  data(){
    return{
      message : 'hello',
    }
  },
}
</script>
```

부모 컴포넌트에서 자식 컴포넌트의 데이터를 연결. <br>

```tsx
<MsgBox>
  <template v-slot:default="slotProps">
    {{slotProps.message}}
  </template>
</MsgBox>
```