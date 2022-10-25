<template>
    <div class="detailed_list">
        <template v-if="sorted">
            <div class="detailed_list__header">
                <slot></slot>
                <div v-if="items.filter(item => item.checked && item.count > 0).length > 0"
                    class="button_shuffle"
                    @click="sorted = false"
                >Перемешать</div>
            </div>
            <template v-for="(item, index) in items" :key="index">
                <div
                    v-if="item.count > 0 && item.checked"
                    class="item"
                >
                    <div v-for="(elem, i) in item.count"
                         class="item_instance"
                         :key="i"
                         :style="'background-color: ' + item.color"
                         @click="$emit('reduceCount', index)"
                    ></div>
                </div>
            </template>
        </template>
        <template v-else>
            <div class="detailed_list__header">
                <slot></slot>
                <div class="button_sort" @click="sorted = true">Сортировать</div>
            </div>
            <div
                v-if="shuffledItems.length > 0"
                class="item"
            >
                <div v-for="(key, index) in shuffledItems" :key="index"
                     class="item_instance"
                     :style="'background-color: ' + items[key].color"
                     @click="reduceCountShuffle(key, index)"
                ></div>
            </div>
        </template>
    </div>
</template>

<script>
import random from "../modules/random.js";

export default {
    data() {
        return {
            sorted: true
        }
    },
    computed: {
        shuffledItems: function() {
            if (this.sorted) return [];
            const result = [];
            for (let index in this.items) {
                if (!this.items[index].checked)
                    continue;
                for (let i = 0; i < this.items[index].count; i++) {
                    if (index === 0) {
                        result.push(index);
                        continue;
                    }
                    result.splice(
                        random.int(result.length+1), 0, index
                    );
                }
            }
            if (result.length === 0) {
                this.sorted = true;
            }
            return result;
        }
    },
    props: {
        items: {
            type: Array,
            default: []
        }
    },
    methods: {
        reduceCountShuffle(key, index) {
            this.shuffledItems.splice(index, 1);
            this.$emit('reduceCount', key);
        }
    },
    emits: [ "reduceCount" ]
}
</script>

<style scoped>

</style>