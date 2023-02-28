<template>
<div>
  <nav class="navbar navbar-dark navbar-expand sticky-top bg-primary shadow">
    <div class="container-fluid"><button data-toggle="collapse" class="navbar-toggler" data-target="#navcol-1"><span class="sr-only">Toggle navigation</span><span class="navbar-toggler-icon"></span></button>
      <div class="collapse navbar-collapse" id="navcol-1">
        <ul class="nav navbar-nav mx-auto">
          <li class="nav-item"><n-link to="/" class="nav-link">Home</n-link></li>
          <li class="nav-item"><n-link to="/product" class="nav-link">Add Product</n-link></li>
          <li class="nav-item"><n-link to="/category" class="nav-link">Add Category</n-link></li>
          <li class="nav-item"><n-link to="/brand" class="nav-link">Add Brand</n-link></li>
          <li class="nav-item"><n-link to="/county" class="nav-link">Add County</n-link></li>
          <li class="nav-item"><n-link to="/orders" class="nav-link">Orders</n-link></li>
        </ul>
      </div>
    </div>
  </nav>
</div>
</template>
