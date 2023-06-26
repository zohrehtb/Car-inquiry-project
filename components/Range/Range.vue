<template>
    <div class="c-range-slider">
        <div class="c-range-slider__number-input">
            <input 
                v-model="sliderValue" 
                type="number" 
            />
            <span>{{ 'km' }}</span>
        </div>

        <input 
            v-model="sliderValue" 
            type="range" min="0" 
            :max="max" 
            class="slider" 
        />

        <div class="c-range-slider__ranges-number">
            <span>{{ '0' }}{{ 'km' }}</span>
            <span>{{ max }}{{ 'km' }}</span>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';

    export default Vue.extend({
        name: 'RangeSlider',

        data() {
            return {
                sliderValue: 0,
            };
        },

        props: {
            max: String,
        },
    });
</script>

<style lang="scss" src="./Style.scss" />
