<template>
    <div class="lds-ripple">
        <div></div>
        <div></div>
    </div>
</template>

<script>
export default {
    name: 'Loading',
    props: ['loading']
}
</script>

<style>
    .lds-ripple {
        display: block;
        position: relative;
        width: 150px;
        height: 150px;
        margin: auto;
    }
    .lds-ripple div {
    position: absolute;
    border: 4px solid #fff;
    opacity: 1;
    border-radius: 50%;
    animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
    }
    .lds-ripple div:nth-child(2) {
    animation-delay: -0.5s;
    }
    @keyframes lds-ripple {
    0% {
        top: 36px;
        left: 36px;
        width: 0;
        height: 0;
        opacity: 1;
    }
    100% {
        top: 0px;
        left: 0px;
        width: 72px;
        height: 72px;
        opacity: 0;
    }
    }
</style>
