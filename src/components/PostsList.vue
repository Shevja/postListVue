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
    title: '',
    error: ''
})

function addPost() {
    if (newPost.title !== '') {
        emit('addNewPost', newPost.title)
        newPost.title = ''
    } else {
        newPost.error = 'Заголовок не может быть пустым'
    }
}

function editPost(id, title) {
    emit('editPost', id, title)
}

function removePost(id) {
    emit('removePost', id)
}

</script>

<template>
    <div class="posts">
        <div class="posts-interface">
            <div class="posts-interface__input">
                <input @focus="newPost.error = ''" v-model.trim="newPost.title" type="text">
                <span v-if="newPost.error" class="">
                    {{ newPost.error }}
                </span>
            </div>
            <button @click="addPost" class="posts-interface__button">+</button>
        </div>

        <ul class="posts-list">
            <li v-for="post in props.posts" :key="post.id">
                <PostCard :id="post.id" :title="post.title" @removePost="removePost" @editPost="editPost" />
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

            &:hover {
                cursor: pointer;
                background-color: rgb(96, 136, 96);
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