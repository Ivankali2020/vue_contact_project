<template>
  <nav class="navbar navbar-light bg-light fixed-top ">
    <div class="container ">
      <a class="navbar-brand" href="#">
        <span class="fw-bolder text-secondary ">NINJA</span>
        <span class="fw-light text-secondary mx-2 small ">MMS</span>
      </a>
      <button style="outline: none" class="navbar-toggler" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasNavbar" aria-controls="offcanvasNavbar">
        <i class="bi-list-columns "></i>
      </button>
      <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasNavbar" aria-labelledby="offcanvasNavbarLabel">
        <div class="offcanvas-header">
          <div class="">
            <span class="fw-bolder text-secondary ">NINJA</span>
            <span class="fw-light text-secondary mx-2 small ">MMS</span>
          </div>
<!--          <button type="button" class="btn-close text-reset navbar-toggler small  " data-bs-dismiss="offcanvas" aria-label="Close"></button>-->
        </div>
        <div class="offcanvas-body">
          <ul class="navbar-nav d-flex ">
            <li class="nav-item">
              <router-link to="/" class="btn  text-start  btn-outline-secondary mb-3 nav-link " aria-current="page" href="#">
                <i class="bi-house mx-3 "></i>
                <span class="me-3 " >Home</span>
              </router-link>
            </li>
            <li class="nav-item">
              <router-link to="/about" class="btn  text-start  btn-outline-secondary mb-3 nav-link" href="#">
                <i class="bi-info  mx-3 "></i>
                <span class="me-3 ">About</span>
              </router-link>
            </li>

          </ul>

        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavBar"
}
</script>

<style scoped>

</style>