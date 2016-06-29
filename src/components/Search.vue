<template>
    <div class="search">
        <input type="text" class="search-input"/>
    </div>
</template>
<style>
    .search{
        position: absolute;
        z-index: 99;
        width: 375px;
        text-align: center;
        padding: 15px 0;
    }
    .search-input{
        height: 40px;
        opacity: .3;
        width: 315px;
        color:#777;
        font-weight: 500;
        font-size: 1.14em;
        border-radius: 50px;
        border: 0;
        outline: none;
        box-sizing: border-box;
        padding: 5px 20px;
    }
    .search-input:focus{
        opacity: .7;
    }
</style>