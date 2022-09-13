<template>
   <div class="main">
      <div class="main__table">
         <div class="table__header">
            <div @click="processSort(item)" v-for="item in ['Id', 'Email', 'First Name', 'Pay Status', 'Last Name', 'Username', 'Profile Link' ]" class="table__cell table__headercell">
               <div class="table__title">{{ item }}</div>
               <div v-if="columnInFocus === item" :class="{ 'rotated': direction }" class="table__icon"><font-awesome-icon :icon="'arrow-up'"></font-awesome-icon></div>
            </div>
         </div>
         <div v-for="item in filteredTable" class="table__row">
            <div v-if="value !== 'profile_link'" v-for="cell, value in item" class="table__cell table__bodycell">{{ cell }}</div>
            <a v-else :href="cell" class="table__cell table__link table__bodycell">{{ cell }}</a>
         </div>
         <div class="table__line"></div>
      </div>
      <div class="main__inputbox">
         <input v-model="filter" type="text" class="inputbox__input">
         <div class="inputbox__title">Search</div>
      </div>

      <div class="main__form">
         <div class="main__row">
            <input v-model="form.id" type="text" class="main__input">
            <div class="main__title">Id</div>
         </div>
         <div class="main__row">
            <input v-model="form.email" type="text" class="main__input">
            <div class="main__title">Email</div>
         </div>
         <div class="main__row">
            <input v-model="form.first_name" type="text" class="main__input">
            <div class="main__title">First name</div>
         </div>
         <div class="main__row">
            <input v-model="form.pay_status" type="checkbox" class="main__input">
            <div class="main__title">Pay status</div>
         </div>
         <div class="main__row">
            <input v-model="form.last_name" type="text" class="main__input">
            <div class="main__title">Last name</div>
         </div>
         <div class="main__row">
            <input v-model="form.username" type="text" class="main__input">
            <div class="main__title">Username</div>
         </div>
         <div class="main__row">
            <input v-model="form.profile_link" type="text" class="main__input">
            <div class="main__title">Profile link</div>
         </div>
         <button @click="submit" class="main__button">Submit</button>
      </div>
   </div>
</template>

<script>

import table from '../data/table.json'

export default {
   data() {
      return {
         table,
         filteredTable: table,
         direction: false,
         columnInFocus: '',
         filter: '',
         form: {
            id: '',
            email: '',
            first_name: '',
            pay_status: false,
            last_name: '',
            username: '',
            profile_link: ''
         }
      }
   },
   watch: {
      filter: {
         immediate: false,
         handler: function() {
            this.filteredTable = [];
            this.table.filter(row => {
               for (var cell in row) {
                  console.log(row[cell])
                  if (row[cell].toString().toLowerCase().indexOf(this.filter.toLowerCase()) == -1) {
                     continue
                  } else {
                     this.filteredTable.push(row)
                     return
                  }
               }
            })
         }
      }
   },
   methods: {
      submit() {
         this.table.push({
            id: this.form.id,
            email: this.form.email,
            first_name: this.form.first_name,
            pay_status: this.form.pay_status,
            last_name: this.form.last_name,
            username: this.form.username,
            profile_link: this.form.profile_link
         })
      },
      processSort(item) {
         if (this.columnInFocus !== item) {
            this.direction = false;
         } else {
            this.direction = !this.direction
         }
         this.columnInFocus = item;
         const varItem = item.replace(' ', '_').toLowerCase();
         this.sortTable(varItem, this.direction)
      },
      sortTable(item, direction) {
         console.log(item, direction)
         if (this.direction) {
            this.filteredTable.sort(function(a, b) {
               if (a[item] > b[item]) {return -1;}
               if (a[item] < b[item]) {return 1;}
               return 0
            })
         } else {
            this.filteredTable.sort(function(a, b) {
               if (a[item] < b[item]) {return -1;}
               if (a[item] > b[item]) {return 1;}
               return 0
            })
         }
      }
   }
}
</script>
