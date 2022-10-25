<template>
    <div class="list">
        <div class="list__header">
            <input
                type="checkbox"
                :checked="allChecked"
                :indeterminate="!allChecked && someChecked"
                @input="$emit('checkItems', $event.target.checked)"
            >
            <div class="list__header__content" @click="expanded = !expanded">
                <div class="expand_indicator" :class="{ active: expanded }"></div>
                <div class="list__title">
                    <slot></slot>
                </div>
            </div>
        </div>
        <div class="list__items" :style="expanded ? '' : 'display: none'">
            <div v-for="(item, index) in items" class="item" :key="index">
                <input type="checkbox" v-model="item.checked">
                <div class="item__title">{{ item.title }}</div>
                <template v-if="item.editCount">
                    <input
                        :ref="'count_input_' + index"
                        type="text"
                        class="item__count_input"
                        v-model="inputTemp[index]"
                        @keyup.enter="submitCount(index)"
                        @focusout="submitCount(index)"
                    >
                </template>
                <template v-else>
                    <div
                        class="item__count"
                        @click="() => {
                            inputTemp[index] = item.count;
                            $emit('setItemValue', index, 'editCount', true);
                            focusOn = 'count_input_' + index;
                        }"
                    >
                        {{ item.count }}
                    </div>
                </template>
                <template v-if="item.editColor">
                    <input
                        :ref="'color_input_' + index"
                        type="color"
                        v-model="inputTemp[index]"
                        @change="submitColor(index)"
                    >
                </template>
                <template v-else>
                    <div
                        class="item__icon"
                        :style="'background-color: ' + item.color"
                        @click="() => {
                            inputTemp[index] = item.color;
                            $emit('setItemValue', index, 'editColor', true)
                            focusOn = 'color_input_' + index;
                        }"
                    ></div>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            expanded: true,
            inputTemp: {},
            focusOn: null
        }
    },
    computed: {
        allChecked: function() {
            return this.items.every(item => item.checked);
        },
        someChecked: function() {
            return this.items.some(item => item.checked);
        }
    },
    props: {
        items: {
            type: Array,
            default: []
        }
    },
    methods: {
        submitCount(index) {
            this.$emit('setItemValue', index, 'count', parseInt(this.inputTemp[index]));
            this.$emit('setItemValue', index, 'editCount', false);
        },
        submitColor(index) {
            this.$emit('setItemValue', index, 'color', this.inputTemp[index]);
            this.$emit('setItemValue', index, 'editColor', false);
        },
    },
    updated() {
        if (this.$refs[this.focusOn] && this.$refs[this.focusOn][0]) {
            const input = this.$refs[this.focusOn][0];
            input.select();
            this.focusOn = null;
        }
    },
    emits: [ "checkItems", "setItemValue" ]
}
</script>

<style scoped>

</style>