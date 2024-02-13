<template>
  

  <div class="container mx-auto mt-8">
    <table class="min-w-full bg-white border border-gray-300">
      <thead>
        <tr>
          <th @click="sortBy('serialNo')" class="py-2 px-4 border-b cursor-pointer">S.No</th>
          <th @click="sortBy('name.first')" class="py-2 px-4 border-b cursor-pointer">First Name
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
          <th @click="sortBy('dob.date')" class="py-2 px-4 border-b cursor-pointer">DOB
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
          <th @click="sortBy('email')" class="py-2 px-4 border-b cursor-pointer">Email
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
          <th @click="sortBy('location.city')" class="py-2 px-4 border-b cursor-pointer">Location
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
          <th @click="sortBy('phone')" class="py-2 px-4 border-b cursor-pointer">Phone
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
          <th @click="sortBy('picture.thumbnail')" class="py-2 px-4 border-b cursor-pointer">Picture
            <span class="arrow-up"></span>
            <span class="arrow-down"></span>
          </th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(item) in list" :key="item.login.uuid">
          <td class="py-2 px-4 border-b">{{ item.id.value }}</td>
          <td class="py-2 px-4 border-b">{{ item.name.title }} {{ item.name.first }} {{ item.name.last }}</td>
          <td class="py-2 px-4 border-b">{{ item.dob.date }}</td>
          <td class="py-2 px-4 border-b">{{ item.email }}</td>
          <td class="py-2 px-4 border-b">{{ item.location.city }}, {{ item.location.country }}</td>
          <td class="py-2 px-4 border-b">{{ item.phone }}</td>
          <td class="py-2 px-4 border-b">
            <img :src="item.picture.thumbnail" alt="Profile Picture" />
          </td>
        </tr>
      </tbody>
    </table>
    
  </div>
  
</template>
<script>

import axios from 'axios'

export default {

  name: 'App',
 
  data(){
  return {
  
    list:[],
    sortKey: '',
    sortOrder: 'desc',
    
  };
  },
  /*
  async mounted() {
 
 try {
   const response = await axios.get('https://randomuser.me/api/?results=50');
   this.list = response.data.results;
 }
 catch (error) {
   console.error('Error fetching data:', error.message);

 }
  },
 */


  computed:{
    sortedList() {
      const sortedList = [...this.list];

      if (this.sortKey) {
        sortedList.sort((a, b) => 
          this.dynamicSort(a, b, this.sortKey));
        if (this.sortOrder === 'desc') {
          sortedList.reverse();
          
          
        }

  }

  return sortedList;
},},

methods: {
    async fetchData() {
      try {
        const response = await axios.get('https://randomuser.me/api/?results=50');
        this.list = response.data.results.map((item, index) => ({ ...item, serialNo: index + 1 }));
      } catch (error) {
        console.error('Error fetching data:', error.message);
      }
    },
    sortBy(key) {
      if (this.sortKey === key) {
        this.sortOrder = this.sortOrder === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortKey = key;
        this.sortOrder = 'asc';
      }
      this.sortList();
    },
    sortList() {
      const sortedList = [...this.list];

      if (this.sortKey) {
        sortedList.sort((a, b) => this.dynamicSort(a, b, this.sortKey));
        if (this.sortOrder === 'desc') {
          sortedList.reverse();
        }
      }

      this.list = sortedList;
    },

    dynamicSort(a, b, key) {
      const valueA = this.getValue(a, key);
      const valueB = this.getValue(b, key);

      if (valueA < valueB) return -1;
      if (valueA > valueB) return 1;
      return 0;},
    getValue(obj, key) {
      return key === 'serialNo' ? obj : key.split('.').reduce((o, i) =>o && o[i], obj);

    },
  },
  async mounted() {
    await this.fetchData();
  },

}

</script>
<style scoped>
.arrow-up::before,
.arrow-down::before {
  content: '\25BC';
  display: inline-block;
  margin-left: 4px;
}

.arrow-up::before {
  transform: rotate(180deg);
}

/* Default arrow styling */
.arrow-up,
.arrow-down {
  opacity: 1; /* Make both arrows visible by default */
}

</style>