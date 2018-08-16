<template>
<div>
  <div>
    <button @click="current='companies'"> Comapnies </button>
    <button @click="current='users'"> Users </button>
  </div>
  <app-table
    v-show="current == 'companies'" 
    :data="companies.data"
    :columns="companies.columns">
  </app-table>
  <app-table
    v-show="current == 'users'" 
    :data="users.data"
    :columns="users.columns">
  </app-table>
</div>
</template>

<script>
import Table from "./components/Table.vue" 

export default {
  data() {
    return {
      current: 'companies',
      companies: {
        data: require("./companies.json"),
        columns: {
          'company_name': {
            name: 'Company Name',
          },
          'product': {
            name: 'Products',
          },
          'price': {
            name: 'Price',
            filter(data) {
                if (data == undefined) data = "-1.0";
                return parseFloat(data);
            },
            render(price) {
                return price == -1.0 ? "n/s" : price;
            }
          },
          'fda_date_approved': {
            name: "Date of approval",
            filter(date) {
                if (date == undefined) return 0;
                return new Date(date.split("/").reverse().join("-")).getTime();
            },
            render(timestamp) {
                if (timestamp == 0) return "n/s";
                return new Date(timestamp).toLocaleDateString();
            }
          }
        } 
      },
      users: {
        data: require("./users.json"),
        columns: {
          'first_name': {
            name: "First Name"
          },
          'last_name': {
            name: "Last Name"
          },
          'country': {
            name: "Country",
            filter(country) {
              return country == undefined ? "n/s" : country;
            }
          },
          'phone': {
            name: "Phone",
            filter(name) {
              return name == undefined ? "n/s" : name;
            }
          }
        }
      }
    };
  }, 
  components: {
    appTable: Table
  }
}
</script>

<style>
  div { margin: 30px; }
</style>
