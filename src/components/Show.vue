<template>
  <div>
    <input type="button" @click="allRecords()" value="Select All users">
    <br>
    <br>

    <!-- Select record by ID -->
    <input type="number" v-model="userid" placeholder="Enter Userid between 1 - 24">
    <input type="button" @click="recordByID()" value="Select user by ID">
    <br>
    <br>

    <!-- List records -->
    <table id = 'customers'>
      <tr>
        <th>ID</th>
        <th>Username</th>
        <th>Name</th>
        <th>Email</th>
      </tr>

      <tr v-for="(user,index) in users" :key = "index">
        <td>{{ user.id}}</td>
        <td>{{ user.username }}</td>
        <td>{{ user.name }}</td>
        <td>{{ user.email }}</td>
      </tr>
    </table>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  data() {
    return {
      users:[],
      userid:0
    };
  },
  methods: {
    allRecords: function() {
      axios
        .get("http://localhost/select.php")
        .then(response => {
        const data = response.data;
        for (let key in data) {
          const user = data[key];
          this.users.push(user);
        }

      })
        .catch(function(error) {
          console.log(error);
        });
    },
    recordByID: function() {
      if (this.userid > 0) {
        axios
          .get("http://localhost/select.php", {
            params: {
              userid: this.userid
            }
          })
          .then(response => {
            this.users = response.data;
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    }
  }
};
</script>

<style>
#customers {
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#customers td, #customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even){background-color: #f2f2f2;}

#customers tr:hover {background-color: #ddd;}

#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #4CAF50;
  color: white;
}
</style>
