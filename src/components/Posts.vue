<script setup>
import PostsList from './PostsList.vue'
import { onBeforeMount, reactive, watch } from 'vue'

const postsList = reactive([])

// Перед монтированием компонента проверяет есть ли список постов в localStorage
// если нет то вызывает функцию getPostList для загрузки постов
onBeforeMount(() => {
    if (!localStorage.getItem('postsList')) {
        getPostList()
    } else {
        try {
            let stringifiedPostsList = localStorage.getItem('postsList')
            postsList.push(...JSON.parse(stringifiedPostsList))
        } catch {
            getPostList()
        }
    }
})

// Получает список постов и сохраняет в postsList
async function getPostList() {
    await fetch('https://jsonplaceholder.typicode.com/posts/1')
        .then((response) => response.json())
        .then((json) => postsList.push(json));
}

// Добавляет пост в postsList
function addItem(itemTitle) {
    postsList.push({
        title: itemTitle,
    })
}

// Удаляет пост из postsList
function removeItem(itemId) {
    const postIndex = postsList.findIndex(post => post.id === itemId)
    postsList.splice(postIndex, 1)
}

// Обновляет заголовок поста в postsList
// и сохраняет в localStorage
function editItem(itemId, itemTitle) {
    const post = postsList.find(post => post.id === itemId)
    post.title = itemTitle
    localStorage.setItem('postsList', JSON.stringify(postsList))
}

// Следит за изменением длины postsList
// eсли произошло изменение (удаление) то обновляет id постов
// или если произошла загрузка постов через fetch (при первом запуске)
// и сохраняет в localStorage
watch(
    () => postsList.length,
    () => {
        postsList.forEach((post, ind) => {
            post.id = ind
        })
        localStorage.setItem('postsList', JSON.stringify(postsList))
    }
)

</script>

<template>
    <section class="section">
        <div class="container">
            <PostsList :posts="postsList" @addPost="addItem" @removePost="removeItem" @editPost="editItem" />
        </div>
    </section>
</template>

<style lang="scss"></style>