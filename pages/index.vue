<template>
    <main class="m-calculator">
        <div class="m-calculator__box">
            <header class="m-calculator__header c-row">
                <h1>{{ ' ماشین حساب' }}</h1>

                <span>{{ ' | محاسبه خودرو کارکرده' }}</span>
            </header>

            <div class="c-row m-calculator__table">
                <aside class="m-calculator__aside">
                    <p class="m-calculator__aside__text">
                        {{ 'بر اســــاس' }}
                        <span>{{ '875,054' }}</span>
                        {{ 'معامله صورت گرفته کارکرد' }}
                    </p>

                    <div class="m-calculator__filter">
                        <span 
                            class="m-calculator__filter__item" 
                            v-for="filter in filterList" 
                            :key="filter.id"
                        >
                            {{ filter.title }}

                            <span 
                                class="m-calculator__filter__icon" 
                                @click="removeFilter(filter.id, filter.type)"
                            >
                                <font-awesome-icon :icon="['fas', 'xmark']" />
                            </span>
                        </span>
                    </div>
                </aside>

                <section class="m-calculator__products">
                    <header class="m-calculator__products__header">{{ title }}</header>

                    <div 
                        v-if="range" 
                        class="m-calculator__products__range-box"
                    >
                        <range-slider :max="maxUsage" />

                        <div class="m-calculator__submit-button">
                            <button>{{ 'ادامه' }}</button>
                        </div>
                    </div>

                    <article 
                        v-else 
                        class="m-calculator__products__inner"
                    >
                        <template v-if="Array.isArray(sortedArray)">
                            <div 
                                v-for="item in sortedArray" 
                                :key="item.id" class="m-calculator__product" 
                                @click="getModels(item)"
                            >
                                <a>
                                    {{ item.title }}
                                </a>
                            </div>
                        </template>
                        <template v-else>
                            <div
                                v-for="key in Object.keys(sortedArray)"
                                :key="sortedArray[key].id"
                                class="m-calculator__product"
                            >
                                <span class="m-calculator__sort-title"> 
                                    <font-awesome-icon :icon="['fas', 'circle']" />
                                    {{ key }}
                                </span>

                                <div 
                                    v-for="item in sortedArray[key]" 
                                    :key="item.id" 
                                    class="m-calculator__product" 
                                    @click="getModels(item)"
                                >
                                    <a>
                                        {{ item.title }}
                                    </a>
                                </div>
                            </div>
                        </template>
                    </article>
                </section>
            </div>
            <p class="m-calculator__synapsis">
                {{ tableSynapsis }}
            </p>
        </div>
    </main>
</template>

<script lang="ts">
    import Vue from 'vue';
    import json from '../static/data';
    import rangeSlider from '../components/Range/Range.vue';

    export default Vue.extend({
        name: 'IndexPage',

        /**
         * @inheritdoc
         */
        data() {
            return {
                step: 1,
                value: 1,
                range: false,
                maxUsage: '',
                mockData: json as any,
                filterList: [] as any,
                items: json.data.list,
                title: json.heading.brand,
                selectedProduct: [] as any,
                tableSynapsis:
                    '*قیمت خودروها بر اساس پایش، تجمیع و تحلیل قیمت‌های اعلام شده از سوی نمایندگی‌ها، قیمت معاملات انجام شده در بیش از ۱۵۰ نمایشگاه فعال سطج کشور و مراکز خریدوفروش پایتخت و نیز بررسی‌های میدانی در بازار خودرو به صورت روزانه استخراج می‌شود.',
            };
        },

        components: {
            rangeSlider,
        },

        computed: {
            /**
             * Sort Items
             *
             * @returns {Array}
             */
            sortedArray() {
                const sortedList: any = this.items.sort((a: any, b: any) => a.title.localeCompare(b.title));
                const obj: any = {};
                
                sortedList.forEach((item: any) => {
                    const firstLetter = item.title.toString().at(0);
                    if (obj[firstLetter]) {
                        obj[firstLetter] = [...obj[firstLetter], item];
                    } else {
                        obj[firstLetter] = [item];
                    }
                });

                if (this.items.length && this.items[0].type === 'model') {
                    return obj;
                }

                return sortedList;
            },
        },

        methods: {
            /**
             * Get Data By filters
             *
             * @param {object} item
             */
            getModels(item: any): any {
                this.filterList.push(item);

                this.items = item.list;
                this.title = this.mockData.heading[item.type];

                if (item.max_usage) {
                    this.maxUsage = item.max_usage;
                    this.title = this.mockData.heading['usage'];
                    this.range = true;
                }
            },

            /**
             * Remove data by filters
             *
             * @param {string} clickedItemId
             */
            removeFilter(clickedItemId: string, clickedItemType: string): any {
                const findIndex = this.filterList.findIndex((i: any) => i.id === clickedItemId);
                this.filterList = this.filterList.slice(0, findIndex);
                this.title = this.mockData.heading[clickedItemType];
                if (this.filterList.length) {
                    this.items = this.filterList.at(-1).list;
                } else {
                    this.items = json.data.list;
                }
                this.range = false;
            },
        },
    });
</script>
