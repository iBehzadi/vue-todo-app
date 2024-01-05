<template>
  <nav class="navbar fixed w-full top-0 z-10 bg-[--clr-white] shadow-lg" >
      <div class="navbar-center w-full max-w-[1170px] mx-auto">
          
          <div class="nav-header flex justify-between items-center py-4 px-6">
              <div class="company">
                  <img class="brand" src="../assets/img/logo.png" alt="">
              </div>
              <span @click="toggleMenu" class="nav-menu-icon">///
                  <i class="bi bi-list"></i>
              </span>
          </div>

          <ul ref="sideMenu" @click="toggleMenu" class="nav-links me-10">
              <li>
                  <router-link :to="{name: 'index'}" class="nav-link scroll-link cursor-pointer" >Home</router-link>
              </li>
              <li>
                  <!-- <router-link :to="{name: 'Jobs'}" class="nav-link scroll-link cursor-pointer" >Jobs</router-link> -->
              </li>
              <li>
                  <!-- <router-link :to="{name: 'signup'}" class="nav-link scroll-link cursor-pointer" >Sign-Up</router-link> -->
              </li>
              <li>
                  <!-- <router-link :to="{name: 'AboutUs'}" class="nav-link scroll-link cursor-pointer" >About-Us</router-link> -->
              </li>
          </ul>

      </div>
  </nav>
</template>
<script setup lang="ts">



const sideMenu = ref<HTMLElement>();
function toggleMenu() {
    sideMenu.value?.classList.toggle('show-links')
}
</script>



<style >


.nav-links {
    height: 0;
    overflow: hidden;
    transition: var(--transition);
}

.nav-link {
    display: block;
    color: var(--clr-grey-1);
    transition: var(--transition);
    padding: 1rem 2rem;
    font-size: 1rem;
}

.nav-link:hover {
    background: var(--clr-primary-3);
}

.show-links {
    height: 230px;
}

.nav-menu-icon {
    font-size: 1.5rem;
    cursor: pointer;
}

.company {
    object-fit: cover;
    width: 7rem;
}

.nav-social-links {
    margin: 0 1rem;
    display: none;
}

@media screen and (min-width: 992px) {
    .nav-menu-icon {
        display: none;
    }

    .nav-social-links {
        display: flex;
    }

    .nav-links {
        height: auto;
        display: flex;
    }

    .nav-link {
        padding: 0 0;
        margin-right: 0.75rem;
    }

    .nav-link:hover {
        background: transparent;
        color: var(--clr-primary-2);
    }

    .nav-social-icon {
        color: var(--clr-primary-2);
        font-size: 1.2rem;
        margin-right: 0.75rem;
    }

    .nav-social-icon:hover {
        color: var(--clr-primary-3);
    }

    .navbar-center {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
}</style>