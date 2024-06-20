<script setup>
const emit = defineEmits(['editPost', 'removePost'])

const props = defineProps({
    id: {
        type: Number,
        default: 0,
    },
    title: {
        type: String,
        default: '',
    },
})

function edit() {
    emit('editPost', props.id, props.title)
}

function remove() {
    emit('removePost', props.id)
}
</script>

<template>
    <div class="post-card">
        <span class="post-card__id">
            {{ id }}
        </span>
        <p class="post-card__title">
            {{ title }}
        </p>
        <div class="post-card-buttons">
            <button @click="edit" class="post-card-buttons__button post-card-buttons__button_edit">
                Редактировать
            </button>
            <button @click="remove" class="post-card-buttons__button post-card-buttons__button_remove">
                Удалить
            </button>
        </div>
    </div>
</template>

<style lang="scss" scoped>
p {
    margin: 0;
}

.post-card {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    width: 100%;
    border: 1px solid #bbb;
    padding: 10px;
    gap: 10px;

    &__id {
        font-weight: bold;
    }

    &__title {
        font-size: 20px;
    }

    &-buttons {
        display: flex;
        margin-left: auto;
        gap: 10px;
        width: 100%;
        flex: none;

        &__button {
            padding: 3px 10px;
            transition:
                color .1s,
                border-color .1s,
                background-color .1s;

            &_edit:hover {
                background-color: #224a92;
                border-color: #224a92;
                color: #fff;
            }

            &_remove:hover {
                background-color: #d35757;
                border-color: #d35757;
                color: #fff;
            }
        }
    }
}
</style>