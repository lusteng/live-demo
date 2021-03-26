<template>
    <div class="img-card">
        <img src="./../static/source/1.png" alt=""> 
    </div>
</template>

<script>
    export default {
        name:"ImgCard",
        props: {
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
    width: 40px;
    height: 40px;
}
</style>