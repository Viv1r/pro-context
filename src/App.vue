<template>

<div class="workspace">
    <div class="left_panel">
        <List v-for="(list, index) in mainData"
              :key="index"
              :items="list.items"
              @checkItems="state => checkItems(list, state)"
              @setItemValue="(i, key, value) => list.items[i][key] = value"
        >
            {{ list.title || "List" }}
        </List>
    </div>
    <div class="right_panel">
        <DetailedList v-for="(list, index) in mainData"
                      :key="index"
                      :items="list.items"
                      @reduceCount="i => list.items[i].count--"
        >
            {{ list.title }}
        </DetailedList>
    </div>
</div>

</template>

<script>
import List from "./components/List.vue";
import DetailedList from "./components/DetailedList.vue";
import Main from "./data/Main.json";

export default {
    components: {
        List,
        DetailedList
    },
    data() {
        return {
            mainData: Main
        }
    },
    methods: {
        checkItems(list, state) {
            list.items.forEach(elem => elem.checked = state);
        }
    }
}
</script>

<style scoped>
</style>
