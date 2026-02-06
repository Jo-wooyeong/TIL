## v-on:change
```
`@change`로 축약해서 사용합니다.
`<input type="file">`을 통해 파일 업로드 시 주로 사용되고, 셀렉트 박스 등에도 사용이 됩니다.
(파일 입력창은 `v-model`이 작동하지 않음. 반드시 e를 통해 파일객체를 가져와야한다.)
```

다음 예시는 클릭할때마다 `@change`를 통해 업로드한 파일을 배열에 추가하는 로직
```vue
<template>
	<input 
	    id="file-upload"
	    type="file" 
	    multiple
	    @change="handleFileChange"
	    class="hidden"
	/>
</template>

<script setup>
// ...

const handleFileChange = (e) => {
    const newFiles = Array.from(e.target.files);

    form.attachments = [...form.attachments, ...newFiles];

    event.target.value = ''; 
};
</script>
```