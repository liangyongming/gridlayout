<template>
    <div id="app">
        <div id="content">
            <!-- <button @click="addItem">新增</button>
            <input type="checkbox" v-model="draggable"/> Draggable
            <input type="checkbox" v-model="resizable"/> Resizable -->
            <grid-layout
                :layout="layout"
                :is-draggable="draggable"
                :is-resizable="resizable"
                :vertical-compact="true"
                :use-css-transforms="true"
            >
                <grid-item v-for="item in layout" :key="item.i" :x="item.x" :y="item.y" :w="item.w" 
                    :h="item.h"
                    :i="item.i"
                    @resize="resize"
                    @move="move"
                    @resized="resized"
                    @moved="moved"
                >   
                <span>这是第{{parseInt(item.i)+1}}个</span>
                </grid-item>
            </grid-layout>
        </div>
    </div>
</template>

<script>
    import GridItem from './components/GridItem.vue';
    import GridLayout from './components/GridLayout.vue';
    import {getDocumentDir, setDocumentDir} from "./helpers/DOM";

    let testLayout = [
        {"x":0,"y":0,"w":3,"h":1,"i":"0", resizable: true, draggable: true},
        {"x":3,"y":0,"w":3,"h":2,"i":"1", resizable: null, draggable: null},
        // {"x":6,"y":0,"w":3,"h":2,"i":"2", resizable: false, draggable: false},
        // {"x":9,"y":0,"w":3,"h":4,"i":"3", resizable: false, draggable: false},
    ];

    export default {
        name: 'app',
        components: {
            GridLayout,
            GridItem,
        },
        data () {
            return {
                layout: JSON.parse(JSON.stringify(testLayout)),
                draggable: true,
                resizable: true,
                index: 0
            }
        },
        mounted: function () {
            this.index = this.layout.length;
        },
        methods: {
            move: function(i, newX, newY){
                console.log("MOVE i=" + i + ", X=" + newX + ", Y=" + newY);
            },
            moved: function(i, newX, newY){
                    console.log("### MOVED i=" + i + ", X=" + newX + ", Y=" + newY);
            },
            resize: function(i, newH, newW, newHPx, newWPx){
                console.log("RESIZE i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx);
            },
            resized: function(i, newH, newW, newHPx, newWPx){
                console.log("### RESIZED i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx);
            },
            addItem: function() {
                let item = {"x":0,"y":110,"w":3,"h":3,"i":this.index+"", whatever: "bbb"};
                this.index++;
                this.layout.push(item);
            },

            /**
             * Add change direction button
             */
            changeDirection() {
                let documentDirection = getDocumentDir();
                let toggle = "";
                if (documentDirection === "rtl") {
                    toggle = "ltr"
                } else {
                    toggle = "rtl"
                }
                setDocumentDir(toggle);
                //eventBus.$emit('directionchange');
            }
        },
    }
</script>

<style>
    body{
        margin:0;
    }
    #app {
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
            color: #2c3e50;
        }

        h1, h2 {
            font-weight: normal;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            display: inline-block;
            margin: 0 10px;
        }

        a {
            color: #42b983;
        }
</style>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
