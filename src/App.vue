<template>
<div>
  <div>
    <button @click="currentComponent='companyTable'"> Comapnies </button>
    <button @click="currentComponent='userTable'"> Users </button>
  </div>
  <component :is="currentComponent"></component>
</div>
</template>

<script>
import CompanyTable from "./components/CompanyTable.vue" 
import UserTable from "./components/UserTable.vue" 

export default {
  data() {
    return {
      currentComponent: 'companyTable'
    };
  }, 
  components: {
    companyTable: CompanyTable,
    userTable: UserTable
  }
}
</script>

<style>
  div { margin: 30px; }
</style>
