<template>
<div class="mainpage" ref="mainpage" id="mainpage">

<div>
        <grid-layout :layout.sync="layout" :col-num="12" :row-height="rowheight" :is-draggable="true" :is-resizable="true" :vertical-compact="true" :use-css-transforms="true" >
            <grid-item v-for="item in layout" v-bind:key="item.i" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i" drag-allow-from=".vue-draggable-handle" drag-ignore-from=".no-drag" style="overflow:hidden">
                <div class="text">
                    <div class="vue-draggable-handle" style="z-index:11"></div>
                    <div class="no-drag">
                        
                        <div v-if="item.i==0">
                            left
                        </div>
                        <div v-if="item.i==1" ref="chartt2" width=100% height=100%>
                        <div style="overflow:hidden;" ref="chartt" >
                            <trading-vue :data="dc" :width="width" :height="height" title-txt="BTCUSD" ref="ogchartt"
                                :extensions="ext" 
                                :overlays="overlays" 
                                :toolbar="true"                          
                                :color-back="colors.colorBack"
                                :color-grid="colors.colorGrid"
                                :color-text="colors.colorText"
                                :chart-config="{DEFAULT_LEN:70}">
                        </div>
                        </div>
                    </div>
                </div>
            </grid-item>
        </grid-layout>
    </div>

<div>
</template>

<style>
* {
    margin: 0;
}
</style>
<style>
@import './css/globaltheme.css';
</style>

<script>
import TradingVue from 'trading-vue-js'
import { GridLayout, GridItem } from 'vue-grid-layout';
import Data from '../data/data.json'

export default {
    name: 'app',
    components: { TradingVue },
    methods: {
        onResize(event) {
            this.height = ((this.layout[1].h)*(this.rowheight+9.5))-20
            this.width = this.$refs.chartt[0].clientWidth; 
        }
    },
    mounted() {
        window.addEventListener('resize', this.onResize)
        this.ro = new ResizeObserver(this.onResize).observe(this.$refs.chartt[0])
        this.onResize()
        
    },
    beforeDestroy() {
        window.removeEventListener('resize', this.onResize)
    },
    destroyed () {
        delete this.ro
    },
    data() {
        return {
            chart: Data,
            width: window.innerWidth,
            height: window.innerHeight,
            colors: {
                colorBack: '#fff',
                colorGrid: '#eee',
                colorText: '#333',
            }
            layout: [
                {"x":0,"y":0,"w":2,"h":10,"i":"0"},
                {"x":2,"y":0,"w":7,"h":21,"i":"1"},
                {"x":4,"y":0,"w":2,"h":5,"i":"2"},
                {"x":6,"y":0,"w":2,"h":3,"i":"3"},
                {"x":8,"y":0,"w":2,"h":3,"i":"4"},
                {"x":10,"y":0,"w":2,"h":3,"i":"5"},
            ],
            draggable: true,
            resizable: true,
            index: 0,
            eventLog: [],
            rowheight: 30        
        }
    }
}
</script>

<style scoped>
@import './css/vue-grid.css';
</style>
