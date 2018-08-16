<template>
<table class="table table-sm">
    <thead class="thead-dark">
        <tr>
            <th scope="col">#</th>
            <th scope="col"
                v-for="(column, key) in columns"
                @click="sort(key)" :key="key">
                {{ column.name }}
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !columns[key].asc, 
                        'fa-angle-up': columns[key].asc 
                    }"></i>
            </th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(elem, index) in data" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td v-for="(value, key) in columns" :key="key"> 
                {{ columns[key].hasOwnProperty('render') ? columns[key].render(elem[key]) : elem[key] }}
            </td>
        </tr>
    </tbody>
</table>
</template>

<script>
import Vue from "vue";

export default {
    props: {
        data: Array,
        columns: Object
    },
    data() {
        return {};
    },
    methods: {
        init() {
            this.data.forEach(obj => {
                for (let key in this.columns) {
                    if (this.columns[key].hasOwnProperty('filter')) {
                        Vue.set(obj, key, this.columns[key]['filter'](obj[key]));
                    }
                }
            });
            for (let key in this.columns) {
                this.columns[key].asc = true;
            }
        },
        sort(property) {
            if (this.columns[property].asc) {
                this.data.sort((a,b) => isNaN(a[property]) ? a[property].localeCompare(b[property]) : a[property] - b[property]);
                this.columns[property].asc = false;
            } else {
                this.data.sort((a,b) => isNaN(a[property]) ? b[property].localeCompare(a[property]) : b[property] - a[property]);
                this.columns[property].asc = true;
            }
        },
    },
    created() {
        this.init();
    },
    mounted() {
    }
}
</script>

<style scoped>
    th { cursor: pointer; }
</style>
