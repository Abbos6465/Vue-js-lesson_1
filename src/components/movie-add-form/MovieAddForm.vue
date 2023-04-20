<template>
    <Box class="mb-5">
        <h3>Yangi kino qo'shish</h3>
        <form @submit.prevent>
            <div class="input-group">
                <Input type="text" class="new-movie-label" placeholder="Qanday kino?" v-model="name"/>
                <Input type="number" class="new-movie-label" placeholder="Nechchi marotaba ko'rilgan?" v-model="viewers" />
                <Button class="btn-outline-primary" @click="addMovie">Qo'shish</Button>
            </div>
        </form>
    </Box>
</template>
<script>




export default {
    data() {
        return {
            name: "",
            viewers: "",
        };
    },
    methods: {
        addMovie() {
            if (!this.name || !this.viewers) {
                alert("Ma'lumotlar yetarli emas");
                return;
            }
            const newMovie = {
                name: this.name,
                viewers: this.viewers,
                favourite: false,
                like: false,
            };
            this.$emit("createMovie", newMovie);
            this.name = "",
                this.viewers = "";
        }
    },
}
</script>
<style scoped></style>