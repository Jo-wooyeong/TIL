## computed_vs_methods

### computed
리액트의 **useMemo**와 아주 비슷하면서도 훨씬 직관적입니다. 쉽게 말해 **"기존 데이터를 가공해서 새로운 데이터를 만들 때, 그 값을 기억(캐싱)해두는 똑똑한 변수입니다.
- computed안의 변수에 따라 값이 바뀌는 캐싱기능이 있음

```vue
<script setup>
import { ref, computed } from 'vue';

const firstName = ref('홍');
const lastName = ref('길동');

// ✅ computed 정의
// firstName이나 lastName이 바뀔 때만 fullName이 다시 계산됩니다.
const fullName = computed(() => {
  console.log('계산 중...'); // 값이 바뀔 때만 찍힙니다.
  return `${firstName.value} ${lastName.value}`;
});
</script>

<template>
  <div>
    <input v-model="firstName" />
    <input v-model="lastName" />
    
    <p>전체 이름: {{ fullName }}</p>
    <p>전체 이름: {{ fullName }}</p> </div>
</template>
```

### methods
캐싱은 없고 일반 핸들러로 사용할 때 사용

```vue
<script setup>
import { ref } from 'vue';

const count = ref(0);

function increment() {
  count.value++;
}

const greet = (name) => {
  alert(`안녕하세요, ${name}님!`);
};
</script>

<template>
  <div>
    <p>현재 카운트: {{ count }}</p>
    <button @click="increment">1 증가</button>
    
    <hr />
    
    <button @click="greet('Vue 선생님')">인사하기</button>
  </div>
</template>
```