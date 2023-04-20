<template>
    <nav aria-label="...">
        <ul class="pagination pagination-sm d-flex justify-content-center mt-3">
            <PaginationListItem :totalPages="totalPages" :updatePage="updatePage" />
        </ul>
    </nav>
</template>

<script>
import PaginationListItem from '../pagination-list-item/PaginationListItem.vue';

export default {
    components: { PaginationListItem },

    props:{
        totalPages:{
            type:Number,
            required:true,
        },

        updatePage:{
            type:Function,
            required:true,
        }
    }
}
</script>

<style scoped></style>