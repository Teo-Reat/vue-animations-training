<template>
    <div id="app">
        <h1>Animations</h1>
        <button @click="block = !block">Change between one / group animations</button>
        <hr>
        <div v-show="block">
            <h2>One element animation</h2>
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
            <button @click="load = !load">Load / Remove element</button>
            <transition :css="false"
                        @before-enter="beforeEnter"
                        @enter="enter"
                        @after-enter="afterEnter"
                        @enter-cancelled="enterCancelled"

                        @before-leave="beforeLeave"
                        @leave="leave"
                        @after-leave="afterLeave"
                        @leave-cancelled="leaveCancelled">
                <div class="some-div" v-if="load"></div>
            </transition>
            <hr>
            <button @click="changeComponent">Change component</button>
            <transition name="slide" mode="out-in">
                <component :is="activeComponent"></component>
            </transition>
        </div>
        <div v-show="!block">
            <h2>Group of elements animation</h2>
            <hr>
            <button @click="addItem">Add Item</button>
            <ul>
                <transition-group name="slide">
                    <li v-for="(item, key) in items"
                        :key="item"
                        @click="removeItem(key)">
                        <div class="some-div" :style="{background: item}"></div>
                    </li>
                </transition-group>
            </ul>
        </div>
    </div>
</template>

<script>
    import AlertSuccess from './components/AlertSuccess'
    import AlertDanger from './components/AlertDanger'

    export default {
        data() {
            return {
                block: false,
                show: false,
                load: false,
                alertAnimation: 'fade',
                elementWidth: null,
                activeComponent: 'app-alert-danger',
                items: []
            }
        },
        components: {
            appAlertSuccess: AlertSuccess,
            appAlertDanger: AlertDanger,
        },
        methods: {
            generateColor() {
                return '#' + Math.floor(Math.random() * 16777215).toString(16);
            },
            addItem() {
                this.items.unshift(this.generateColor())
            },
            removeItem(index) {
                this.items.splice(index, 1);
            },
            changeComponent() {
                if (this.activeComponent === 'app-alert-danger') {
                    this.activeComponent = 'app-alert-success';
                } else {
                    this.activeComponent = 'app-alert-danger';
                }
            },
            beforeEnter(el) {
                console.log('beforeEnter');
                this.elementWidth = 400;
                el.style.width = this.elementWidth + 'px';

            },
            enter(el, done) {
                console.log('enter');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                });
            },
            afterEnter() {
                console.log('afterEnter');
            },
            enterCancelled() {
                console.log('enterCancelled');
            },
            beforeLeave(el) {
                console.log('beforeLeave');
                this.elementWidth = 400;
                el.style.width = this.elementWidth + 'px';
            },
            leave(el, done) {
                console.log('leave');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                });
            },
            afterLeave() {
                console.log('afterLeave');
            },
            leaveCancelled() {
                console.log('leaveCancelled');
            }
        }
    }
</script>

<style>
    #app {
        text-align: center;
        color: #2c3e50;
        margin: 60px auto;
        width: 960px;
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
        position: absolute;
        left: 0;
        top: 0;
    }
    .slide-move {
        transition: transform 1s;
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

    ul {
        padding: 0;
        margin: 0 auto;
        width: 500px;
    }

    li {
        list-style-type: none;
        width: 100%;
    }

    li .some-div {
        background: purple;
        color: white;
        font-weight: bold;
        letter-spacing: 2px;
        margin: 5px auto;
    }
</style>
