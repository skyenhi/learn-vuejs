<template>
  <div class="add container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Add Repos</h1>
    <form v-on:submit="addRepos">
        <div class="well">
            <h4>Repos Info</h4>
            <div class="form-group">
                <label>Name</label>
                <input type="text" class="form-control" placeholder="Name" v-model="repos.name">
            </div>
            <div class="form-group">
                <label>Description</label>
                <input type="text" class="form-control" placeholder="Description" v-model="repos.description">
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
        addRepos(e){
            if(!this.repos.name || !this.repos.description){
                this.alert = 'Please fill in all required fields';
            } else {
                let newRepos = {
                    name: this.repos.name,
                    description: this.repos.description

             }

                this.$http.post('https://api.github.com/user/repos?access_token=0a0ae264ca6f434ea87e808a957b45cd3449fd67', newRepos)
                    .then(function(response){
                        this.$router.push({path: '/', query: {alert: 'Repos Added'}});
                    });

                e.preventDefault();
            }
            e.preventDefault();
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
