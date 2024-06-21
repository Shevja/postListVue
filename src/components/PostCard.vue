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

            &_edit {
                // background-color: #3e5e99;
                // border-color: #3e5e99;
                // color: #fff;

                &:hover {
                    background-color: #1e54b8;
                    border-color: #1e54b8;
                    color: #fff;
                }
            }

            &_remove {
                // background-color: #b96767;
                // border-color: #b96767;
                // color: #fff;

                &:hover {
                    background-color: #df4444;
                    border-color: #df4444;
                    color: #fff;
                }
            }
        }
    }
}
</style>