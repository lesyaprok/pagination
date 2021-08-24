<template>
    <div class="paginationButton" @click="onClick">{{ value }}</div>
</template>

<script>
export default {
    name: 'PaginationButton',
    props: {
        value: Number,
    },
    methods: {
        onClick() {
            this.$emit('change')
        },
    },
}
</script>

<style scoped>
    
</style>
