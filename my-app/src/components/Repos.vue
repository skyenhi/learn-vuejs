<template>
  <div class="repos container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">My Repos</h1>
    <input class="form-control" placeholder="Filter by name or created at" v-model="filterInput">
    <br />

    <table class="table table-striped">
      <thead>
      <tr>
        <th v-for="key in columns"
            @click="sortBy(key)"
            :class="{ active: sortKey == key }">
          {{ key | capitalize }}
          <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
          </span>
        </th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="record in filteredData">
        <td v-for="key in columns">
          {{record[key]}}
        </td>
        <td><router-link class="btn btn-default" v-bind:to="'/repos/'+record.name">View</router-link></td>
      </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
  import Alert from './Alert';
  export default {
    name: 'repos',

    props:{
      columns: {
        type: Array,
        default: () => ['id', 'name', 'description','created_at']
      }
  },

  data() {
    var sortOrders = {}
    this.columns.forEach(function (key) {
      sortOrders[key] = 1
    })

    return {
      reposEx: '',
      filterInput: '',
      sortKey: '',
      sortOrders: sortOrders,
      alert: ''
    }
  },

  created: function(){
    if(this.$route.query.alert){
      this.alert = this.$route.query.alert;
    }
    this.fetchData();
  },

  updated: function(){
    this.fetchData();
  },

  computed: {
    filteredData: function () {
      var sortKey = this.sortKey
      var filterKey = this.filterInput && this.filterInput.toLowerCase()
      var order = this.sortOrders[sortKey] || 1
      var data = this.reposEx
      if (filterKey) {
        data = data.filter(function (row) {
          return Object.keys(row).some(function (key) {
            return String(row[key]).toLowerCase().indexOf(filterKey) > -1
          })
        })
      }
      if (sortKey) {
        data = data.slice().sort(function (a, b) {
          a = a[sortKey]
          b = b[sortKey]
          return (a === b ? 0 : a > b ? 1 : -1) * order
        })
      }
      return data
    }
  },

  filters: {
    capitalize: function (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  },

  methods: {
    fetchData: function () {
      var xhr = new XMLHttpRequest()
      var self = this
      xhr.open('GET', 'https://api.github.com/user/repos?access_token=53fbb42711fd37b6812ca58d5a821c261ea66c06')
      xhr.onload = function () {
        self.reposEx = JSON.parse(xhr.responseText)
        console.log(self.reposEx.data)
      }
      xhr.send()
    },

    sortBy: function (key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1
    }
  },
  components: {
    Alert
  }

  }
</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
