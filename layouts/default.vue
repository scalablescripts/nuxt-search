<template>
  <div>
    <div class="container">
      <header class="d-flex justify-content-center py-3">
        <ul class="nav nav-pills">
          <li class="nav-item">
            <NuxtLink to="/frontend" active-class="active" class="nav-link">Frontend</NuxtLink>
          </li>
          <li class="nav-item">
            <NuxtLink to="/backend" active-class="active" class="nav-link">Backend</NuxtLink>
          </li>
        </ul>
      </header>
    </div>

    <Nuxt/>
  </div>
</template>
