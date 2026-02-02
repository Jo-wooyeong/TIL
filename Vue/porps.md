## Props와 Emit

### Props (부모 -> 자식)
부모가 자식에게 상태를 전달할때를 의미합니다.
* `:`이 없으면 단순 문자열로 전달되고, `:`이 있으면 자바스크립트 표현식으로 전달
* 주의점: 자식컴포넌트에서 직접 전달받은 `props`를 수정하면 안됩니다.

```vue
<template>
  <ChildComponent 
    title="마스터 강의" 
    :count="userCount" 
    :is-active="true" 
  />
</template>

<script setup>
import { ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

const userCount = ref(100);
</script>
```

자식은 `defineProps`라는 매크로를 사용해 받을 데이터를 선언합니다.
```vue
<script setup>
// 배열 형태로 간단히 선언하거나, 객체 형태로 타입을 명시할 수 있습니다.
const props = defineProps({
  title: String,
  count: Number,
  isActive: Boolean
});

// 스크립트에서 접근할 때는 props.title 식으로 사용
console.log(props.title);
</script>

<template>
  <h2>{{ title }}</h2> <p>현재 인원: {{ count }}</p>
</template>
```

### Emit (자식 -> 부모)
`defineEmits`로 발생시킬 이벤트 이름을 등록하고, 필요할 때 호출합니다.

자식 컴포넌트
```vue
<script setup>
const emit = defineEmits(['update-name', 'delete-user']);

const changeName = () => {
  // '이벤트명', '전달할 데이터'
  emit('update-name', '새로운 마스터');
};
</script>

<template>
  <button @click="changeName">이름 변경 요청</button>
</template>
```

부모 컴포넌트
```vue
<template>
  <ChildComponent @update-name="onUpdateName" />
</template>

<script setup>
const onUpdateName = (newName) => {
  console.log('자식이 보낸 이름:', newName);
};
</script>
```