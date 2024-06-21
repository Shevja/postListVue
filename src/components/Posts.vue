<script setup>
import PostsList from './PostsList.vue'
import { reactive, ref, watch, computed } from 'vue'

const postsList = reactive([
    {
        id: 0,
        title: 'hello world'
    },
    {
        id: 1,
        title: 'hello world2'
    },
    {
        id: 2,
        title: 'hello world3'
    },
])

function addItem(itemTitle) {
    postsList.push({
        title: itemTitle
    })
}

function removeItem(itemId) {
    const postIndex = postsList.findIndex(post => post.id === itemId)
    postsList.splice(postIndex, 1)
}

function editItem(itemId, itemTitle) {
    const post = postsList.find(post => post.id === itemId)
    post.title = itemTitle
    console.log('edit', itemId, itemTitle)
}

watch(
    () => postsList.length,
    () => {
        console.log('watch', postsList.length)
        postsList.forEach((post, ind) => {
            post.id = ind
        })
    }
)

</script>

<template>
    <PostsList :posts="postsList" @addNewPost="addItem" @removePost="removeItem" @editPost="editItem" />
</template>

<style lang="scss"></style>