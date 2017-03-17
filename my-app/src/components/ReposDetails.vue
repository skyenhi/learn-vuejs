<template>
  <div class="details container">
    <router-link to="/">Back</router-link>
    <h2 class="page-header">{{repos.name}}

    </h2>
    <ul class="list-group">
            <li class="list-group-item"><b> Name:</b> {{repos.name}}</li>
            <li class="list-group-item"><b> Description:</b> {{repos.description}}</li>
            <li class="list-group-item"><b> Created at:</b> {{repos.created_at}}</li>
        </ul>
        <div class="pull-right">
          <router-link class="btn btn-primary" v-bind:to="'/edit/'+repos.name">Edit</router-link>
          <button class="btn btn-danger" v-on:click="deleteRepos(repos.name)">Delete</button>
        </div>
  </div>
</template>

<script>
export default {
  name: 'reposdetails',
  data () {
    return {
      repos: ''
    }
  },
  methods:{
      fetchRepos(name){
          this.$http.get('https://api.github.com/repos/skyenhi/'+name)
          .then(function(response){
            this.repos = response.body;
          });
      },
      deleteRepos(name){
          this.$http.delete('https://api.github.com/repos/skyenhi/'+name+'?access_token=53fbb42711fd37b6812ca58d5a821c261ea66c06')
          .then(function(response){
            this.$router.push({path: '/', query: {alert: 'Repos Deleted'}});
          });
      }
  },
  created: function(){
      this.fetchRepos(this.$route.params.id);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
