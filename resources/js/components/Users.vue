<template>
    <div class="container">
        <div class="row  mt-3">
            <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">User Table</h3>

                <div class="card-tools">
                  <button class="btn btn-success" data-toggle="modal" data-target="#myModal">Add New
                    <i class="fas fa-user-plus fa-fw"></i>
                  </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <tbody><tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Type</th>
                    <th>Registered At</th>
                    <th>modify</th>
                  </tr>
                  <tr v-for="user in users" :key="user.id">
                    <td>{{user.id}}</td>
                    <td>{{user.name}}</td>
                    <td>{{user.email}}</td>
                    <td>{{user.type | uptext }}</td>
                    <td>{{user.created_at | myDate}}</td>
                    <td>
                      <a href="#">
                        <i class="fa fa-edit"></i>
                      </a>
                      /
                      <a href="#">
                        <i class="fa fa-trash red"></i>
                      </a>
                    </td>
                  </tr>
                  
                </tbody></table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>
    <!-- Button trigger modal -->

<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <form @submit.prevent="CreateUser">
      <div class="modal-body">
         <div class="form-group">
      <input v-model="form.name" type="text" name="name" placeholder="Name"
        class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
      <has-error :form="form" field="name"></has-error>
    </div>
     <div class="form-group">
      <input v-model="form.email" type="email" name="email" placeholder="email@email.com"
        class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
      <has-error :form="form" field="email"></has-error>
    </div>
     <div class="form-group">
      <textarea v-model="form.bio" name="bio" placeholder="Short bio for user (Optional)"
        class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
      <has-error :form="form" field="bio"></has-error>
    </div>
    <div class="form-group">
      <select name="type" v-model="form.type"
        class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
        <option value="">Select User Role</option>
        <option value="admin">Admin</option>
        <option value="user">Standard User</option>
        <option value="author">Author</option>
        </select>
    </div>
     <div class="form-group">
      <input v-model="form.password" type="password" name="password" placeholder="********"
        class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
      <has-error :form="form" field="password"></has-error>
    </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="submit" class="btn btn-primary">Create</button>
      </div>
      </form>
    </div>
  </div>
</div>

    </div>

    
</template>

<script>
    export default {
        data(){
          return {
            users:{},
            form: new Form({
              id: '',
              name: '',
              email: '',
              password: '',
              type: '',
              bio: '',
              photo: ''
            })
          }
        },
        methods: {
          loadUsers(){
            axios.get("api/user").then(({data})=> (this.users = data.data))
          },
          CreateUser(){
            this.$Progress.start();
            this.form.post('api/user');

            Fire.$emit('AfterCreate');
            $('#myModal').modal('hide');
            
            toast({
              type:'success',
              title: 'User Created Successfully'
            })

            this.$Progress.finish();
          }
        },
        created() {
            this.loadUsers();
            Fire.$on('AfterCreate', () =>{
              this.loadUsers();
            })
        }
    }
</script>
