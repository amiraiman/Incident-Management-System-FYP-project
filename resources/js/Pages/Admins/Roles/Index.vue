
<template>
    <div>
        <admin-layout>
        <section class="content">
            <div class ="container-fluid">
                <div  class="row">
                    <div class="col-12">
                        <div class="card">
                            <div class="card-header">
                                <h3 class="card-title">Roles & Permission</h3>
                                <div class="card-tools"  v-if="$page.props.auth.hasRole.superadmin || $page.props.auth.hasRole.admin">
                                    <button type="button" class="btn btn-info text-uppercase" style="letter-spacing: 0.1em;" @click="openModal">
                                        Create
                                    </button>
                                </div>
                            </div>
                            <div class="card-body table-responsive p-0">
                            <table class="table table-hover table-dark text-nowrap">
                                <thead>
                                    <tr>
                                        <th class="text-capitalize">Role Name</th>
                                        <th class="text-capitalize">Permission</th>
                                        <th class="text-capitalize">Created</th>
                                        <th class="text-capitalize text-right"  v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin ">Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(role, index) in roles.data" :key="index">
                                        <td>{{ role.name }}</td>
                                        <td>
                                            <div class="d-flex flex-column">
                                                <span v-for="(permission,index) in role.permissions" :key="index">
                                                {{ permission.name }}
                                                </span>
                                            </div>
                                        </td>
                                        <td>{{ role.created_at }}</td>
                                        <td class="text-right"  v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin ">
                                            <button class="btn btn-success text-uppercase" style="letter-spacing 0.1em;" @click="editModal(role)">Edit</button>
                                            <button class="btn btn-danger text-uppercase" style="letter-spacing 0.1em;" @click="deleteRole(role)">Delete</button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                            </div>
                            <div class=".card-footer clearfix">
                                <pagination :links="roles.links"></pagination>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <div class="modal fade" id="modal-lg">
            <div class="modal-dialog modal-lg modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">{{ formTitle }}</h4>
                        <button type="button" class="close" @click="closeModal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="h4">
                            <span>Preview: <span class="text-capitalize">{{ form.name }}</span>
                            </span>
                        </div>
                        <div class="card card-primary">
                            <form @submit.prevent="checkMode">
                                <div class="card-body">
                                    <div class="form-group">
                                        <label for="role" class="h4">Role Name</label>
                                        <input type="text" class="form-control" id="role" placeholder="Role Name" v-model="form.name" :class="{'is-invalid' : form.errors.name}" autofocus="autofocus" autocomplete="off" >
                                    </div>
                                    <div class="invalid-feedback mb-3" :class="{ 'd-block' : form.errors.name }">
                                        {{form.errors.name}}
                                    </div>

                                    <div class="form-group">
                                        <label for="permission" class="h4">Permissions</label>
                                        <multiselect
                                            v-model="form.permissions"
                                            :options="permissionOptions"
                                            :multiple="true"
                                            :taggable="true"
                                            Placeholder="Choose Permission"
                                            @addPermissions="addPermissions"
                                            label="name"
                                            track-by="id"
                                        ></multiselect>
                                    </div>
                                    <div class="invalid-feedback" :class="{'d-block' : form.errors.permissions}">
                                        {{form.errors.permissions}}
                                    </div>
                                </div>
                                <div class="modal-footer justify-content-between">
                                    <button type="button" class="btn btn-danger text-uppercase" style="letter-spacing: 0.1em;" @click="closeModal">Cancel</button>
                                    <button type="submit" class="btn btn-info text-uppercase" style="letter-spacing: 0.1em;" :disabled="!form.name || form.processing">{{ buttonTxt }}</button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        </admin-layout>
    </div>

</template>

<script>
import AdminLayout from '@/Layouts/AdminLayout'
import Pagination from '@/Components/Pagination'
export default {
    props: ['roles','permissions'],
    components: {
        AdminLayout,
        Pagination,
        },

  data() {
      return {
          editedIndex: -1,
          editMode: false,
          form: this.$inertia.form({
              id:'',
              name:'',
              permissions: []
          }),
          permissionOptions: this.permissions,
      }
  },
  computed: {
      formTitle(){
          return this.editedIndex === -1 ? 'create new role!' : 'Edit current role';
      },
      buttonTxt(){
          return this.editedIndex === -1 ? 'Create' : 'Edit' ;
      },
      checkMode(){
          return this.editMode === false ? this.createRole : this.editRole;
      },
  },
  methods: {
      addPermissions(newPermission) {
          let permission = {
              name: newPermission,
          }
          this.permissionOptions.push(permission)
          this.form.permissions.push(permission)
      },
      editModal(role){
          this.editMode = true
          $('#modal-lg').modal('show')
          this.editedIndex = this.roles.data.indexOf(role)
          this.form.name = role.name
          this.form.id = role.id
          this.form.permissions = role.permissions
      },
      openModal(){
          this.editedIndex = -1
          $('#modal-lg').modal('show')
      },
      closeModal(){
          this.form.clearErrors()
          $('#modal-lg').modal('hide')
      },
      createRole(){
        this.form.post(this.route('admin.roles.store'),{
            preserveScroll: true,
            onSuccess:()=> {
                this.closeModal()
                Toast.fire({
                    icon: 'success',
                    title: 'New Role Created!'
                })
            }
        })
       },
       editRole(){
            this.form.patch(this.route('admin.roles.update' , this.form.id, this.form),{
            preserveScroll: true,
            onSuccess:()=> {
                this.closeModal()
                 Toast.fire({
                    icon: 'success',
                    title: 'Role has been succesfully edited!'
                })
                }
            })
        },

       deleteRole(role){
           Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!'
        }).then((result) => {
                if (result.isConfirmed) {
                    this.form.delete(this.route('admin.roles.destroy',role),{
                    preserveScroll: true,
                    onSuccess:()=> {
                    Swal.fire(
                    'Deleted!',
                    'The Role has been deleted',
                    'success'
                            )
                        }
                    })
                }
            })
        }
    }
}
</script>
