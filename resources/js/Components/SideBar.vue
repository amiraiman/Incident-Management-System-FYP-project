<template>
    <aside class="main-sidebar sidebar-dark-primary elevation-4">
    <!-- Brand Logo -->
    <inertia-link :href="route('admin.dashboard.index')" class="brand-link">
      <img src="/assets/img/IMS%20Logo.png" alt="gear Logo" class="brand-image img-circle elevation-3" >
      <span class="brand-text font-weight-light">CIMS</span>
    </inertia-link>

    <!-- Sidebar -->
    <div class="sidebar" >
      <!-- Sidebar user panel (optional) -->
      <div class="user-panel mt-3 pb-3 mb-3 d-flex">
        <div class="image">
          <img src="/assets/img/User_1.jpg" class="img-circle elevation-2" alt="User">
        </div>
        <div class="info">
          <a :href="route('profile.show')" class="d-block">{{$page.props.user.name}}</a>
        </div>
      </div>



      <!-- Sidebar Menu -->
      <nav class="mt-2">
        <ul class="nav nav-pills nav-sidebar flex-column" data-widget="treeview" role="menu" data-accordion="false">
          <!-- Add icons to the links using the .nav-icon class
               with font-awesome or any other icon font library -->
          <li class="nav-item menu-open">
            <inertia-link :href="route('admin.dashboard.index')" class="nav-link" :class="route().current('admin.dashboard.*') ? 'active' :''">
              <i class="nav-icon fas fa-tachometer-alt"></i>
              <p>
                Dashboard
              </p>
            </inertia-link>
          </li>

          <li class="nav-item menu-open">
            <inertia-link :href="route('tickets.index')" class="nav-link" :class="route().current('tickets.*') ? 'active' :''">
              <i class="nav-icon fas fa-ticket-alt"></i>
              <p>
                Ticket
              </p>
            </inertia-link>
          </li>

          <li class="nav-item menu-open">
            <inertia-link :href="route('news.news.index')" class="nav-link" :class="route().current('news.news.*') ? 'active' :''">
              <i class="nav-icon fas fa-newspaper"></i>
              <p>
                News
              </p>
            </inertia-link>
          </li>

          <li class="nav-item">
            <a href="#" class="nav-link">
              <i class="fas fa-viruses"></i>
              <p>
                Virus Scanner
                <i class="fas fa-angle-left right"></i>
              </p>
            </a>
            <ul class="nav nav-treeview">
              <li class="nav-item">
                <inertia-link :href="route('virus.virus.virus')" class="nav-link"
                :class="route().current('Virus.virus.*' ? 'active' : '')" >
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                     Website
                   </p>
                </inertia-link>
              </li>
               <li class="nav-item">
                <inertia-link :href="route('File.File.File')" class="nav-link" :class="route().current('File.File.*') ? 'active' : ''">
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                     File
                   </p>
                </inertia-link>
              </li>
            </ul>
          </li>

          <li class="nav-item" v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin || $page.props.auth.hasRole.technician || $page.props.auth.hasRole.developer">
            <a href="#" class="nav-link">
              <i class="nav-icon fas fa-table"></i>
              <p >
               Administration
              <i class="fas fa-angle-left right"></i>
              </p>
            </a>
            <ul class="nav nav-treeview">
              <li class="nav-item" v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin || $page.props.auth.hasRole.technician || $page.props.auth.hasRole.developer">
                <inertia-link :href="route('admin.admins.index')" class="nav-link"
                :class="route().current('admin.admins.*' ? 'active' : '')" >
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                     Admins
                   </p>
                </inertia-link>
              </li>
               <li class="nav-item" v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin || $page.props.auth.hasRole.technician">
                <inertia-link :href="route('admin.users.index')" class="nav-link" :class="route().current('admin.users.*') ? 'active' : ''">
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                     Users
                   </p>
                </inertia-link>
              </li>
               <li class="nav-item" v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin || $page.props.auth.hasRole.technician">
                <inertia-link :href="route('admin.roles.index')" class="nav-link" :class="route().current('admin.roles.*') ? 'active' : ''">
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                     Roles
                   </p>
                </inertia-link>
              </li>
               <li class="nav-item" v-if="$page.props.auth.hasRole.superAdmin || $page.props.auth.hasRole.admin || $page.props.auth.hasRole.technician">
                <inertia-link :href="route('admin.permissions.index')" class="nav-link" :class="route().current('admin.permissions.*') ? 'active' : ''">
                   <i class="far fa-circle nav-icon "></i>
                   <p>
                    Permission
                   </p>
                </inertia-link>
              </li>
            </ul>
          </li>



        </ul>
      </nav>
      <!-- /.sidebar-menu -->
    </div>
    <!-- /.sidebar -->
  </aside>
</template>

<script>
import { InertiaLink } from '@inertiajs/inertia-vue3'
export default {


}


</script>
