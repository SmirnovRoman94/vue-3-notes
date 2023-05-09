<template>
    <div class="tags-list">
        <div class="tag-item" v-for="tag in listTag.items" :key="tag" :id="tag" @click="tagClick(tag)" :class="computedClass">
            <span>{{ tag }}</span>
        </div>
    </div>
</template>

<script setup>
import { computed, watch } from "vue"

    const listTag = defineProps({ // получаем пропы
        items: {
            type: Array,
            required: true
        },
        isPreview: {
            type: Boolean,
            default: false
        },
        isActive: {
            type: Boolean,
            default: false
        },
        reload: {
            type: Boolean,
            default: false
        }
    })

    const computedClass = computed(() => {
        return listTag.isPreview == true ? 'isPreview' : 'isActive'
    })
    const emits = defineEmits(['onItemClick']);

    const tagClick = (tag) =>{
        console.log(tag);
        let el = document.getElementById(`${tag}`);
        el.classList.toggle('active')

        emits('onItemClick', tag)
    }

    watch(() => listTag.reload, (n, o) => {
        if(n == true){
            let items = document.querySelectorAll('.active')
            items.forEach(el=>{
                el.classList.toggle('active')
            })
        }
    })


</script>

<style lang="scss">
.tags-list {
  padding: 10px 0;
  display: flex;
  justify-content: center;
}
.tag-item {
  padding: 8px 22px;
  margin-right: 10px;
  background-color: #fff;
  border-radius: 22px;
  user-select: none;
  cursor: pointer;
  &.isActive:hover{
    transform: scale(1.1);
    transition: all .3s ease-out;
    background: #fc5c65;
    color: #fff;
  }
  &.isActive.active {
    background: #26de81;
    transition: all .3s ease-out;
  }
  &.isPreview {
    padding: 0;
    color: #444ce0;
    cursor: default;
    &:before {
      content: '#';
    }
  }
  &:last-child {
    margin-right: 0;
  }
}
</style>
