<template>
  <div>
    <navbar></navbar>

    <div id ="Home">
      <b-container class="mt-4" align="center">

        <h1>
          Welcome!
        </h1>
        <img src="https://img.icons8.com/dusk/100/000000/3-star-hotel.png">

        <div>
          <img src="http://www.animatedimages.org/data/media/111/animated-arrow-image-0472.gif" border="0" alt="animated-arrow-image-0472" /></a>

        <router-link :to = "{name: 'Venues'}"> Click here to see the list of venues</router-link>
          <img src="http://www.animatedimages.org/data/media/111/animated-arrow-image-0473.gif" border="0" alt="animated-arrow-image-0473" /></a>

      </div>
        <div>
          <img src="https://www.fueltravel.com/wp-content/uploads/2016/04/travel-trends-disruption-in-ota-space.png">
        </div>





      </b-container>
  </div>


  </div>
</template>

<script>
  import Navbar from './Navbar.vue'
    export default {
        name: "Home",
       components: {Navbar},

    }
</script>

<style scoped>

</style>
