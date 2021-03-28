<template>
    <div 
        class="img-card"
        @click="$emit('on-click', {src, explain})"
    >
        <img :src="src" alt=""> 
    </div>
</template>

<script>
    export default {
        name:"ImgCard",
        props: {
            explain: String,
            src: {
                type: String,
                default: "",
                require: true,
            },
        },
    }
</script>

<style>
.img-card img{
    box-sizing: border-box;
    width: 40px;
    height: 40px;
    vertical-align: middle;
    padding: 3px;
    border: 1px solid #ddd;
    margin-right:5px; 
    border-radius: 5px;
    transition: .3s;
    cursor: pointer;
}
.img-card img:hover{
    border-color: #f40;
}

</style>