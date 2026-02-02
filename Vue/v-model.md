## v-model
리액트에서 `value`, `onChange` 두개로 변하는 값을 감지해야하는것을 `v-model`하나로 사용

```vue
<script setup>
import { ref } from 'vue';
const text = ref("");
</script>

<template>
  <input v-model="text" />
  <p>입력한 내용: {{ text }}</p>
</template>
```

### 수식어
- `.lazy`: 입력할 때마다가 아니라, change 이벤트(포커스를 잃었을 때 등) 발생 시에만 동기화합니다.
- `.number`: 사용자 입력을 자동으로 숫자 타입(Number)으로 형변환해줍니다.
- `.trim`: 앞뒤 공백을 자동으로 제거합니다.