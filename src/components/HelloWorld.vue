<template>
  <div class="hello">
    <nav class="navbar navbar-dark bg-info">
      <a href="/" class="navbar-brand">ML Links App</a>
    </nav>
    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-6">
          <div class="card">
            <div class="card-header">
              <h3 class="text-center">Website Links</h3>
            </div>
            <div class="card-body text-center">
               <form @submit.prevent="addWebsite">
                 <div class="form-group">
                   <input type="text" class="from-control" placeholder="Name"
                   v-model="newWebsite.name" required>
                 </div>
                 <div class="form-group">
                  <input type="text" class="from-control" placeholder="URL"
                   v-model="newWebsite.url" required>
                 </div>
                 <input type="submit" class="btn bg-info" id="Btn-Del" value="Add Link">
               </form>
            </div>
          </div>
        </div>
        <div class="col-sm-6">
          <div class="card">
            <div class="card-header">
              <h3 class="text-center">Projects List</h3>
            </div>
            <div class="card-body">
              <table class="table table-striped table-bordered">
                <thead>
                  <tr>
                    <th class="text-center">Name</th>
                    <th class="text-center">Operation</th>
                  </tr>
                </thead>
                <tbody class="tbody">
                  <tr v-for="website in websites" :key="website">
                    <td>
                      <a :href="website.url" target="_blank">{{ website.name }}</a>
                    </td>
                    <td>
                      <button class="btn bg-info" id="btn-del" @click="deleteWebsite
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
        url: null,
      },
    };
  },
  methods: {
    addWebsite() {
      websitesRef.push(this.newWebsite);
      toastr.success('Website Added');
      this.newWebsite.name = '';
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
.navbar-brand {
  margin-top: -9px;
  font-size: 1.6em;
  font-weight: bold;
}
.form-control {
  text-align: center;
}
#Btn-Del {
  width: 50%;
}
.btn {
  width: 100%;
}
</style>
