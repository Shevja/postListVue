<script setup>
import { ref, reactive } from 'vue'
import PostCard from './PostCard.vue'

const emit = defineEmits(['addNewPost', 'removePost', 'editPost'])

const props = defineProps({
    posts: {
        type: Array,
        default: []
    },
})

const newPost = reactive({
    id: 0,
    title: '',
    error: ''
})

const inputMode = ref('addPost')

function validateInput() {
    if (newPost.title === '') {
        newPost.error = 'Заголовок не может быть пустым'
    }
}

function editPost(id, title) {
    inputMode.value = 'editPost'
    newPost.title = title
    newPost.id = id
}

function emitAddPost() {
    if (newPost.error === '') {
        emit('addNewPost', newPost.title)
        newPost.title = ''
    }
}

function emitEditPost() {
    if (newPost.error === '') {
        emit('editPost', newPost.id, newPost.title)
        newPost.id = 0
        newPost.title = ''
        inputMode.value = 'addPost'
    }
}

function emitRemovePost(id) {
    emit('removePost', id)
}

</script>

<template>
    {{ props.posts }}
    <div class="posts">
        <div class="posts-interface">
            <div class="posts-interface__input">
                <input @focus="newPost.error = ''" @input="validateInput" v-model.trim="newPost.title" type="text">
                <span v-if="newPost.error" class="">
                    {{ newPost.error }}
                </span>
            </div>
            <button v-if="inputMode === 'addPost'" @click="emitAddPost" class="posts-interface__button">+</button>
            <button v-else-if="inputMode === 'editPost'" @click="emitEditPost" class="posts-interface__button">
                \/
            </button>
        </div>

        <ul class="posts-list">
            <li v-for="post in props.posts" :key="post.id">
                <PostCard :id="post.id" :title="post.title" @removePost="emitRemovePost" @editPost="editPost" />
            </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>
.posts {
    padding: 40px 0;
    max-width: 500px;
    width: 100%;

    &-interface {
        display: flex;
        gap: 10px;
        width: 100%;
        margin-bottom: 10px;

        &__input {
            display: flex;
            flex: 1;
            width: 100%;
            position: relative;

            & input {
                width: 100%;
                border: 1px solid #bbb;
                background-color: transparent;
                font-size: 16px;
                padding: 10px;
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