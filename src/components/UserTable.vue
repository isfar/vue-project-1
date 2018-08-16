<template>
<table class="table table-sm">
    <thead class="thead-dark">
        <tr>
            <th scope="col">#</th>
            <th scope="col" @click="sort('first_name')">
                First name
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.first_name, 
                        'fa-angle-up': sortByAsc.first_name 
                    }"></i>
            </th>
            <th scope="col" @click="sort('last_name')">
                Last name
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.last_name, 
                        'fa-angle-up': sortByAsc.last_name 
                    }"></i>
            </th>
            <th scope="col" @click="sort('country')">
                Country
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.country, 
                        'fa-angle-up': sortByAsc.country 
                    }"></i>
            </th>
            <th scope="col" @click="sort('phone')">
                Phone
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.phone, 
                        'fa-angle-up': sortByAsc.phone 
                    }"></i>
            </th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(user, index) in data" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td>{{ user.first_name }}</td>
            <td>{{ user.last_name }}</td>
            <td>{{ user.country ? user.country : user.country = "" }}</td>
            <td>{{ user.phone ? user.phone : user.phone = "" }}</td>
        </tr>
    </tbody>
</table>
</template>

<script>
export default {
    data() {
        return {
            data: [], 
            sortByAsc: {
                first_name: true,
                last_name: true,
                phone: true,
                country: true 
            }
        };
    },
    methods: {
        sort(property) {
            if (this.sortByAsc[property]) {
                this.data.sort((a,b) => isNaN(a[property]) ? a[property].localeCompare(b[property]) : a[property] - b[property]);
                this.sortByAsc[property] = false;
            } else {
                this.data.sort((a,b) => isNaN(a[property]) ? b[property].localeCompare(a[property]) : b[property] - a[property]);
                this.sortByAsc[property] = true;
            }
        }
    },
    created() {
        this.data = require("./../users.json");
    },
    mounted() {
        this.sort('first_name');
    }
}
</script>

<style>
    th {
        cursor: pointer;
    }
</style>
