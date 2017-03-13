<template>
  <div class="repos container">
    <h1 class="page-header">Manage Repos</h1>
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
      </tr>
      </thead>
      <tbody>
      <tr v-for="record in filteredData">
        <td v-for="key in columns">
          {{record[key]}}
        </td>
      </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
  export default {
    name: 'repos',

    props:{
      columns: {
        type: Array,
        default: () => ['id', 'name', 'created_at']
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
        sortOrders: sortOrders
      }
    },

    created: function(){
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
        xhr.open('GET', 'https://api.github.com/users/laracasts/repos')
        xhr.onload = function () {
          self.reposEx = JSON.parse(xhr.responseText)
          console.log(self.reposEx[0].html_url)
        }
        xhr.send()
      },

      sortBy: function (key) {
        this.sortKey = key
        this.sortOrders[key] = this.sortOrders[key] * -1
      }
    }

  }
</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
