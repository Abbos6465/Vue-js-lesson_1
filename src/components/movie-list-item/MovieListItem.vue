<template>
        <li class="movie-list__item d-flex justify-content-between align-items-center" :class="[{like: movie.like }, {favourite: movie.favourite}]">
            <span @click="$emit('onToggle',{id:movie.id,prop:'like'})" class="movie-list__item__label">{{movie.name}}</span>
            <input class="movie-list__item__input" type="number" :value=movie.viewers />
            <div class="d-flex justify-content-between align-items-center">
                <button class="movie-list__item__btn" @click="$emit('onToggle',{id:movie.id,prop:'favourite'})">
                    <i class="fas fa-cookie"></i>
                </button>
                <button class="movie-list__item__btn" @click="$emit('onRemove',this.movie.id)">
                    <i class="fas fa-trash"></i>
                </button>
                <button class="movie-list__item__btn">
                    <i class="fas fa-star"></i>
                </button>
            </div>
        </li>
</template>
<script>
export default {
    props:{
        movie:{
            type:Object,
            required:true,
        }
    },

    methods: {
        
    },
}
</script>
<style scoped>
    .movie-list__item{
        padding: 15px 20px;
        border: none;
        border-bottom: 1px solid #3d5a80;
    }
    .movie-list__item:last-child{
        border-bottom: none;
    }

    .movie-list__item__label{
        line-height: 35px;
        font-size: 20px;
        cursor: pointer;
        width: 550px;
    }

    .movie-list__item__input{
        line-height: 35px;
        font-size: 20px;
        text-align: center;
        border: none;
        outline: none;
        background-color: white;
    }
    .movie-list__item__btn{
        width: 35px;
        height: 35px;
        margin: 3px;
        font-size: 17px;
        border:none;
        background-color: white;
        outline: none;
        cursor: pointer;
    }

    .movie-list__item__btn:first-child{
        color: #e09f3e;
    }

    .movie-list__item__btn:nth-child(2){
        color: #e5383b;
    }

    .movie-list__item__btn:last-child{
        color: #ffd700;
        transition: 0.3s all;
        transform: translateX(30px);
        opacity: 0;
    }
    .movie-list__item.like .movie-list__item__btn:last-child{
        opacity: 1;
        transform: translateX(0);
    }
    .movie-list__item.favourite .movie-list__item__label{
        color: #e09f3e;
    }

    .movie-list__item.favourite .movie-list__item__input{
        color: #e09f3e;
    }
</style>