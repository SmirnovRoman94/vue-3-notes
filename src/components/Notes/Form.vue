<template>
    <div class="note-form-wrapper">
        <form @submit.prevent="onSubmit" class="form">
            <textarea required v-model="note.title" placeholder="Type ur note"/>
            <TagsList :items="tags" @onItemClick="handleTagClick" :isActive="true" :reload="reloadTasgs"/>
            <button class="btn btnPrimary" type="submit">Add new note</button>
            <!-- <button class="btn btnPrimary" type="submit" @click="$emit('emitTwo', localComputed )">Add</button>
            <button class="btn btnPrimary" type="submit" @click="onSub">Add 2</button> -->
        </form>
    </div>
</template>

<script setup>
    import { reactive, ref } from 'vue'
    import TagsList from '@/components/UI/TagsList'

    const emit = defineEmits(['onSubmit']) // отпеределяем emit-ы
    // const itemInput  = ref('');
    // const onSubmit = () => {
    //     emit('onSubmit', itemInput.value) // передаем параметр в родительский компонент
    //     console.log(itemInput.value); // выводим что ввел пользователь
    //     itemInput.value = ''; // очищаем форму
    // }

    const note = reactive({
        title: '',
        tags: []
    });

    const onSubmit = () => {
        emit('onSubmit', note)
        note.title = '';
        reloadTasgs.value = true;
        setTimeout(()=>{
            reloadTasgs.value = false;
        },1000)
    }

    // const localComputed = computed(() => {
    //     return itemInput.value[0].toUpperCase() + itemInput.value.slice(1) // так сделал первую букву заглавной и отправил обратно
       
    // })
    // const localComputedTwo = computed(()=>{
    //     return itemInput.value.toUpperCase()
    // })
    // const onSub = () => {
    //     emit('emitThree', localComputedTwo.value)
    // }

    //TAGS
    const tags = ['home', 'work', 'travel'];
    const handleTagClick = (item) =>{
        let find = note.tags.findIndex(el=> el === item);
        if(find != -1){
            note.tags.splice(find, 1)
        }else{
            note.tags.push(item)
        }
    }
    const reloadTasgs = ref(false)
</script>

<style>
.note-form-wrapper{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
}
.form{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 600px;
}
.form button {
    width: 100%
}
textarea {
    margin-bottom: 0 !important;
}
</style>