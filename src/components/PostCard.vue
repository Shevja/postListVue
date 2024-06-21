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
    emit('editPost', props.id, props.title, props.body)
}

function remove() {
    emit('removePost', props.id)
}
</script>

<template>
    <div class="post-card">
        <p class="post-card__title">
            {{ title }}
        </p>
        <div class="post-card__bottom">
            <span class="post-card__id">
                id: {{ id }}
            </span>
            <div class="post-card-buttons">
                <button @click="edit" class="post-card-buttons__button post-card-buttons__button_edit">
                    Редактировать
                </button>
                <button @click="remove" class="post-card-buttons__button post-card-buttons__button_remove">
                    Удалить
                </button>
            </div>
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
    transition: border-color .1s;

    &:hover {
        border-color: #62606a;
    }

    &__id {
        font-size: 12px;
    }

    &__title {
        width: 100%;
        font-size: 20px;
    }

    &__bottom {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
        align-items: center;
        justify-content: space-between;
        width: 100%;
    }

    &-buttons {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;

        &__button {
            padding: 3px 10px;

            transition:
                color .1s,
                border-color .1s,
                background-color .1s;

            &_edit {
                // background-color: #3a578d33;
                // border-color: #b5b5b5;
                // color: #fff;

                &:hover {
                    background-color: #3a578d;
                    border-color: #3a578d;
                    color: #fff;
                }
            }

            &_remove {
                // background-color: #a74e4e33;
                // border-color: #b5b5b5;
                // color: #fff;

                &:hover {
                    background-color: #a74e4e;
                    border-color: #a74e4e;
                    color: #fff;
                }
            }
        }
    }
}
</style>