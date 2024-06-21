<script setup>
import { ref, reactive } from 'vue'
import PostCard from './PostCard.vue'

const emit = defineEmits(['addPost', 'removePost', 'editPost'])

const props = defineProps({
    posts: {
        type: Array,
        default: []
    },
})

const newPost = reactive({
    id: 0,
    title: '',
    error: '',
})

// Переключатель для поля ввода
// addPost - режим добавления нового поста
// editPost - режим редактирования поста
const inputMode = ref('addPost')

// Проверяет ошибки поля ввода
function validateInput() {
    if (newPost.title === '') {
        newPost.error = 'Заголовок поста не может быть пустым'
    }
}

// Выполняет проверки на ошибку через validateInput
// Проверяет есть ли ошибки и отправляет соответсвующее inputMode событие
// Затем обнуляет поле инпута и индентификатор поста
function submitHandler() {
    validateInput()

    if (newPost.error !== '') {
        return
    }

    if (inputMode.value === 'addPost') {
        emit('addPost', newPost.title)
    }
    else if (inputMode.value === 'editPost') {
        emit('editPost', newPost.id, newPost.title)
        newPost.id = 0
        inputMode.value = 'addPost'
    }

    newPost.title = ''
}

// Подставляет в поле ввода заголовок поста 
// и передает индентификатор поста в обьект newPost
function editPostHandler(id, title) {
    inputMode.value = 'editPost'
    newPost.title = title
    newPost.id = id
}

// emits
function emitRemovePost(id) {
    emit('removePost', id)
}

</script>

<template>
    <div class="posts">
        <form class="posts-interface" @submit.prevent="submitHandler">
            <div class="posts-interface__input">
                <input @focus="newPost.error = ''" @input="validateInput" v-model.trim="newPost.title" type="text">

                <span v-if="newPost.error" class="">
                    {{ newPost.error }}
                </span>
            </div>
            <button class="posts-interface__button">
                <span v-if="inputMode === 'addPost'">+</span>
                <span v-else-if="inputMode === 'editPost'">
                    &#10003
                </span>
            </button>
        </form>

        <ul class="posts-list">
            <li v-for="post in props.posts" :key="post.id">
                <PostCard :id="post.id" :title="post.title" @removePost="emitRemovePost" @editPost="editPostHandler" />
            </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>
.posts {
    padding: 40px 0;
    max-width: 600px;
    width: 100%;

    &-interface {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
        width: 100%;
        margin-bottom: 10px;

        width: 100%;
        gap: 10px;
        display: flex;

        &__input {
            display: flex;
            flex: 1;
            width: 100%;
            position: relative;

            &_textarea {
                order: 3;
                flex: none;

                textarea {
                    resize: vertical;
                    height: 120px;
                    max-height: 180px;
                    min-height: 43px;
                }
            }

            input,
            textarea {
                width: 100%;
                transition: border-color .1s;

                &:hover,
                &:focus {
                    border-color: #62606a;
                }
            }

            span {
                position: absolute;
                bottom: 0;
                left: 0;
                width: 100%;
                background-color: #d35757;
                font-size: 12px;
                color: #fff;
                padding: 0 10px;
            }
        }

        &__button {
            order: 1;
            font-size: 30px;
            width: 20%;
            transition: background-color .1s, color .1s, border-color .1s;

            &:hover {
                cursor: pointer;
                background-color: #608860;
                border-color: #608860;
                color: #fff;
            }
        }
    }

    &-list {
        display: flex;
        gap: 10px;
        flex-direction: column;
        list-style: none;

        &>* {
            width: 100%;
        }
    }
}
</style>