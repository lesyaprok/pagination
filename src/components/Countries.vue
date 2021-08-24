<template>
    <div class="country">
        <span class="id">{{ inputData.id }}</span>
        <span>{{ inputData.name }}</span>
        <span>{{ inputData.population }}</span>
          
        <button @click="onClick">
            {{ inputData.name }}
        </button>
    </div>  
</template>

<script>

export default {
    name: 'Countries',
    props: {
        inputData: {
            type: Object,
            required: false,
            default: () => {
                return {id: 0, name: 'Not Found', population: 0}
            },
        },
    },
    methods: {
        onClick() {
            // console.log(this.inputData.id);
            this.$emit('country', { name : this.inputData.name});
        }
    },
    data() {
        return {}
    },
    mounted() {
        // console.log(this.inputData);
    },
}

</script>

<style scoped>
    .country {
        padding-bottom: 15px;
        padding-left: 15px;
    }
    span {
        margin: 0 5px;
    }
    .id {
        font-weight: 600;
    }

    button {
        margin-left: 15px;
    }
</style>
