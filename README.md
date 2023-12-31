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

## mitt 라이브러리
#### mitt 설치
멀리 떨어진 컴포넌트에 데이터 전송할 때 편리. <bt>

```
npm install mitt
```
main.js 수정. <br>

```tsx
import mitt from 'mitt'
let emitter = mitt();
let app = createApp(App)
app.config.globalProperties.emitter = emitter;

app.mount('#app')
```

#### mitt 데이터 사용법
데이터 보내고 싶은 곳에서 `this.emitter.emit()`으로 전송. <br>

```tsx
this.emitter.emit('이벤트명', '데이터')
```

데이터 필요한 곳에서 `this.emitter.on()`으로 수신. <br>
보통 `mounted()`안에 작성. <br>

```tsx
mounted(){
  this.emitter.on('이벤트명', ()=>{
    실행할 코드
  });
},
```

## Vuex
#### Vuex 사용 이유
모튼 컴포넌트들이 데이터를 공유할 수 있는 하나의 js 파일. <br>
Vue 파일, 데이터 너무 많을 때 사용. <br>

#### Vuex 설치
```
npm install vuex@next
```

store.js 생성. <br>
```jsx
import { createStore } from 'vuex'

const store = createStore({
  state(){
    return {
     // 보관할 state 
    }
  },
})

export default store
```

main.js에 추가. <br>
```jsx
import store from './store.js'
app.use(store).mount('#app')
```

#### state 접근
state 출력할 컴포넌트에서 `{{ $store.state.데이터명 }}`로 불러옴. <br>

```jsx
// (store.js)
const store = createStore({
  state(){
    return {
     name : 'Kim',
    }
  },
})
```

```tsx
// (App.vue)
<h4>hello, {{ $store.state.name }}</h4>
```

#### Mutations
vue 파일에서 직접 state 수정 불가. <br>
state 변경하는 코드 짜려면 `mutations`에 state 수정 방법 정의(함수). <br>

```tsx
// (store.js)
const store = createStore({
  state () {
    return {
      age : 20,
    }
  },
  mutations :{
    addAge(state){
      state.age++
    }
  },
}
```

vue 파일에서 `$store.commit(함수명)`으로 store.js에 state 변경 요청. <br>

```tsx
// (App.vue)
<button @click="$store.commit('addAge')">버튼</button>
```

#### Actions
비 순차적 또는 비동기 처리 로직 선언할 때 actions에 등록. <br>
actions 항목 만든 후 함수 형태로 만들기. <br>

```tsx
// (store.js)
const store = createStore({
  actions : {
    getData(){
      axios.get('서버URL').then((results)=>{ 
        console.log(results.data);
      })
    }
  }
}
```

vue 파일에서 `dispatch()`를 사용하여 호출. <br>

```tsx
// (App.vue)
<button @click="$store.dispatch('getData')">더보기버튼</button>
```

`actions` 후에 state 변경하려면 `mutations` 함수 호출. <br>

```tsx
// (store.js)
const store = createStore({
  mutations: {
    setMore(state, data) {
      state.more = data;
    },
  },
  actions : {
    getData(){
      axios.get('서버URL').then((results)=>{ 
        console.log(results.data);
        context.commit('setMore');
      })
    },
  },
}
```

#### computed
데이터 사용할 때마다 `$store.state.name` 작성하면 코드 길어짐. <br>
state를 `computed`에 넣어 놓으면 축약 가능. <br>
`{{}}`안에서 호출하여 사용 가능. <br>

```tsx
<template>
  <p>{{name}}</p>
</template>

<script>
export default {
  computed : {
    name(){
      return this.$store.state.name
    },
  },
}
</script>
```


#### MapState
컴포넌트가 여러 state를 사용해야 할 때 여러 computed를 반복해야 하면 복잡해짐. <br>
`mapState` 사용하면 computed 코드 짧아짐. <br>

```tsx
<template>
  <p>{{name}} {{age}}</p>
</template>

<script>
import {mapState} from 'vuex';

export default {
  computed : {
    ...mapState(['name', 'age'])
  },
}
</script>
```