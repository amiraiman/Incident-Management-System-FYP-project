 <!-- Main Footer -->
<template>


  <footer class="main-footer">
    <strong>Copyright &copy; 2021-{{currentDate.getFullYear()}}<inertia-link href="#">Incident</inertia-link>.</strong>
    All rights reserved.
    <div class="float-right d-none d-sm-inline-block">

      <b>Version</b> 0.9.0
    </div>
  </footer>

</template>

   <script>
  export default {
      data(){
          return{
              currentDate: new Date
          }
      }
    }

    </script>
