<template>
    <div class="row flex relative z-0 hover:z-10">
        <div class="w-full relative">
            <input v-model="task.title" @keydown.enter="updateTask($event, 'title', task.title)" class="input px-2 md:px-4 py-2 md:py-4 text-md w-full rounded-sm md:rounded-lg bg-transparent" :class="task.state ? 'line-through text-gray-400' : ''" type="text" tabindex="1" />   

            <div class="buttons hidden absolute top-0 right-0 flex bg-gray-100">
                <button v-if="task.state" @click="updateTask($event, 'state', false)" class="btn" target="_blank">
                    <font-awesome-icon icon="redo-alt" class="text-md" />
                </button>
                <button v-else @click="updateTask($event, 'state', true)" class="btn" target="_blank">
                    <font-awesome-icon icon="check" class="text-md" />
                </button>

                <button @click="removeTask" class="btn" target="_blank">
                    <font-awesome-icon icon="trash-alt" class="text-md" />
                </button>
            </div>
        </div>
        <a v-for="link in extractLinks(task.title)" :key="link" :href="link" class="btn ml-2 md:ml-4" target="_blank">
            <font-awesome-icon icon="link" class="text-md" />
        </a>
    </div>
</template>

<script>
    export default {
        name: "TaskRow",
        props: {
            task: {
                type: Object,
                default: () => {
                    return {}
                }
            }
        },
        methods: {
            extractLinks (string) {
                let reg = /(https?:\/\/[^\s]+)/g;
                return string.match(reg);
            },
            updateTask (event, field, value) {
                this.$set(this.task, field, value)
                this.$emit('updateTask')
                event.currentTarget.blur()
            },
            removeTask () {
                this.$emit('removeTask', this.task.id)
            },
        }
    }
</script>

<style lang="scss">
    .input {
        &:focus {
            @apply outline-none;
        }
    }

    .row:hover {
        .input {
            @apply bg-gray-100 shadow-md;
        }

        .buttons {
            @apply flex;
        }
    }

    @screen md {
        .input:focus + .buttons {
            @apply hidden;
        }
    }

    .btn {
        @apply py-2 px-3 rounded-lg;

        &:hover {
            @apply shadow-lg bg-gray-200;
        }
    }

    @screen md {
        .btn {
            @apply py-4 px-5;
        }
    }
</style>