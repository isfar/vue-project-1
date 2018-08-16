<template>
<table class="table table-sm">
    <thead class="thead-dark">
        <tr>
            <th scope="col">#</th>
            <th scope="col" @click="sort('company_name')">
                Company Name
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.company_name, 
                        'fa-angle-up': sortByAsc.company_name 
                    }"></i>
            </th>
            <th scope="col" @click="sort('product')" style="width: 50%">
                Product
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.product, 
                        'fa-angle-up': sortByAsc.product 
                    }"></i>
            </th>
            <th scope="col" @click="sort('price')">
                Price
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.price, 
                        'fa-angle-up': sortByAsc.price 
                    }"></i>
            </th>
            <th scope="col" @click="sort('fda_date_approved_timestamp')">
                FDA Approve Date
                <i class="fas" 
                    :class="{
                        'fa-angle-down': !sortByAsc.fda_date_approved_timestamp, 
                        'fa-angle-up': sortByAsc.fda_date_approved_timestamp 
                    }"></i>
            </th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(company, index) in data" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td>{{ company.company_name }}</td>
            <td>{{ company.product }}</td>
            <td>{{ company.price ? company.price = parseFloat(company.price) : company.price = 0.00 }}</td>
            <td>{{ company.fda_date_approved ? company.fda_date_approved : company.fda_date_approved = "" }}</td>
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
                company_name: true,
                product: true,
                price: true,
                fda_date_approved_timestamp: true 
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
        },
        getTimestamp(date) {
            if (date === "") return 0;
            return (new Date(date.split("/").reverse().join("-"))).getTime();
        },
    },
    created() {
        this.data = require("./../companies.json");
    },
    mounted() {
        this.data.forEach(row => row.fda_date_approved_timestamp = this.getTimestamp(row.fda_date_approved));
        this.sort('company_name');
        console.log(isNaN(this.data[0].fda_date_approved_timestamp));
    }
}
</script>

<style scoped>
    th {
        cursor: pointer;
    }
</style>
