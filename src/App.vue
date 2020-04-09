<template>
    <div id="app">
        <h1>Animations</h1>
        <hr>
        <select v-model="alertAnimation">
            <option value="fade">Fade</option>
            <option value="slide">Slide</option>
        </select>
        <button @click="show = !show">Some btn</button>
        <transition :name="alertAnimation">
            <div class="some-div" v-if="show"></div>
        </transition>
        <transition name="slide" type="animation" appear>
            <div class="some-div" v-if="show"></div>
        </transition>
        <transition
                enter-active-class="animated bounce"
                leave-active-class="animated shake">
            <div class="some-div" v-if="show"></div>
        </transition>
        <transition :name="alertAnimation" mode="out-in">
            <div class="some-div" v-if="show" key="info"></div>
            <div class="some-div alert" v-else key="warning"></div>
        </transition>
        <hr>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: false,
                alertAnimation: 'fade'
            }
        }
    }
</script>

<style>
    #app {
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    .some-div,
    .alert {
        width: 400px;
        background: cornflowerblue;
        height: 40px;
        margin: 10px auto;
    }
    .alert {
        background: coral;
    }
    /*animations*/
    /*fade*/
    .fade-enter {
        opacity: 0;
    }
    .fade-enter-active {
        transition: opacity 1s;
    }
    .fade-leave {

    }
    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

    /*slide*/
    .slide-enter {
        opacity: 0;
    }
    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }
    .slide-leave {

    }
    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
    }
    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }
    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
</style>
