<template>
  <div class="hello">
    <nav class="navbar navbar-dark bg-info">
      <a href="/" class="navbar-brand">ML Links App</a>
    </nav>
    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-4">
          <div class="card">
            <div class="card-header">
              <h3 class="text-center">Add A new Link</h3>
            </div>
            <div class="card-body text-center">
               <form @submit.prevent="addWebsite">
                 <div class="form-group">
                   <input type="text" class="from-control" placeholder="Name"
                   v-model="newWebsite.name" required>
                 </div>
                 <div class="form-group">
                  <input type="text" class="from-control" placeholder="Description"
                   v-model="newWebsite.description" required>
                 </div>
                 <div class="form-group">
                  <input type="text" class="from-control" placeholder="URL"
                   v-model="newWebsite.url" required>
                 </div>
                 <input type="submit" class="btn bg-info" value="Add Link">
               </form>
            </div>
          </div>
        </div>
        <div class="col sm-8">
          <div class="card">
            <div class="card-header">
              <h3 class="text-center">Websites List</h3>
            </div>
            <div class="card-body">
              <table class="table table-striped table-bordered">
                <thead>
                  <tr>
                    <th class="text-center">Name</th>
                    <th class="text-center">Desc</th>
                    <th class="text-center">Op</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="website in websites" :key="website">
                    <td>
                      <a :href="website.url" target="_blank">{{ website.name }}</a>
                    </td>
                    <td>
                      {{ website.description }}
                    </td>
                    <td>
                      <button class="btn bg-info" @click="deleteWebsite
                      (website)">Del</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase';
import config from '@/firebase/config';
import toastr from 'toastr';

const app = Firebase.initializeApp(config);
const db = app.database();
const websitesRef = db.ref('websites');

export default {
  name: 'HelloWorld',
  firebase: {
    websites: websitesRef,
  },
  data() {
    return {
      newWebsite: {
        name: null,
        description: null,
        url: null,
      },
    };
  },
  methods: {
    addWebsite() {
      websitesRef.push(this.newWebsite);
      toastr.success('Website Added');
      this.newWebsite.name = '';
      this.newWebsite.description = '';
      this.newWebsite.url = '';
    },
    deleteWebsite(website) {
      if (confirm('Are you sure you want to delete it?')) {
        websitesRef.child(website['.key']).remove();
        toastr.success('Website Removed');
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.sm-8 {
  display: flex;
  justify-content: center;
}
</style>
