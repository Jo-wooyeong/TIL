## useForm()
```
라라벨과 함께 쓰이는 Inertia.js를 사용할때, 폼 데이터(입력값) 전송을 쉽게 관리하기 위해 사용합니다.
```

먼저 useForm을 정의해서 원하는 입력값을 받을 변수를 지정합니다.
```js
import { useForm } from '@inertiajs/vue3'

const form = useForm({
    title: '',
    corporationName: '',
    content: '',
    attachments: [],
});

const handleSubmit = async () => {
    try {
        const response = await api.postEvangelistBoard({
            title: form.title,
            corporationName: form.corporationName,
            content: sanitizedContent,
            attachments: form.attachments
        });
    } catch(e) {
		    consol.log(e);
    }
}
```

v-model을 이용해서 입력값을 넣어줍니다.
```js
<TextInput v-model="form.corporationName" />
```