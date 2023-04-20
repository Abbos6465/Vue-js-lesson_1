<template>
    <li v-for="pageNumber in totalPages" role="button" @click="updatePageHandler(pageNumber)" class="page-item"
        :class="{ 'active': this.page === pageNumber }" aria-current="page" :key="pageNumber">
        <span class="page-link">{{ pageNumber }}</span>
    </li>
</template>
<script>
export default {
    props: {
        totalPages: {
            type: Number,
            required: true,
        },
        updatePage: {
            type: Function,
            required: true,
        }
    },

    data() {
        return {
            page: 1,
        }
    },

    methods: {
        updatePageHandler(pageNumber) {
            this.page = pageNumber;
            this.updatePage(this.page);
        }
    },
}
</script>
<style scoped></style>