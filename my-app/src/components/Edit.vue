<template>
  <div class="edit container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Edit Repos</h1>
    <form v-on:submit="updateRepos">
        <div class="well">
            <h4>Repos Info</h4>
            <div class="form-group">
                <label>Name</label>
                <input type="text" class="form-control" placeholder="name" v-model="repos.name">
            </div>
            <div class="form-group">
                <label>Description</label>
                <input type="text" class="form-control" placeholder="description" v-model="repos.description">
            </div>
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
    import Alert from './Alert'
    export default {
    name: 'add',
    data () {
        return {
        repos: {},
        alert:''
        }
    },
    methods: {
        fetchRepos(name){
            this.$http.get('https://api.github.com/repos/skyenhi/'+name+'?access_token=0a0ae264ca6f434ea87e808a957b45cd3449fd67')
            .then(function(response){
                this.repos = response.body;
            });
        },
        updateRepos(e){
            if(!this.repos.name){
                this.alert = 'Please fill in all required fields';
            } else {
                let updRepos = {
                    name: this.repos.name,
                    description: this.repos.description
                }

                this.$http.patch('https://api.github.com/repos/skyenhi/'+this.$route.params.id+'?access_token=0a0ae264ca6f434ea87e808a957b45cd3449fd67', updRepos)
                    .then(function(response){
                        this.$router.push({path: '/', query: {alert: 'Repos Updated'}});
                    });

                e.preventDefault();
            }
            e.preventDefault();
        }
    },
    created: function(){
        this.fetchRepos(this.$route.params.id);
    },
    components:{
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
