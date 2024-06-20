<script setup>
import PostsList from './PostsList.vue'
import { reactive, ref } from 'vue'

const postsList = reactive([
    {
        id: 0,
        title: 'hello world'
    },
    {
        id: 1,
        title: 'hello world'
    },
    {
        id: 2,
        title: 'hello world'
    },
])

function addItem(itemTitle) {
    postsList.push({
        id: postsList.length + 1,
        title: itemTitle
    })
}

function removeItem(itemId) {
    const postIndex = postsList.findIndex(post => post.id === itemId)
    postsList.splice(postIndex, 1)
}

function editItem(itemId, itemTitle) {
    console.log('edit', itemId, itemTitle)
}
</script>

<template>
    <PostsList :posts="postsList" @addNewPost="addItem" @removePost="removeItem" @editPost="editItem" />
</template>

<style lang="scss"></style>