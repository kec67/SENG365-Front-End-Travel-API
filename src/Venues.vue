
<template>

  <div>
    <navbar></navbar>

    <b-container class="mt-4" align="center">

    <h1>
      <img src="https://img.icons8.com/wired/60/000000/3-star-hotel.png">
      Venues
      <img src="https://img.icons8.com/wired/60/000000/3-star-hotel.png">
    </h1>

<div>

    <b-form inline @submit.prevent="updateSearch" id="searchform" class="taskbar">

      <b-input-group class="lg">
        <b-form-input v-model="searchTerm" placeholder="Enter venue name"></b-form-input>

          <b-btn class="mb-2 mr-sm-2 mb-sm-0" variant="success" type="submit">

            <div style="transform: rotate(-45deg);">&#9906;</div>
          </b-btn>

      </b-input-group>





      <div>
      Filter:


      <b-form-select class="ml-5 mt-4 mb-4"v-model="selectedCity" :options="cities" @input="updateSearch()" ><</b-form-select>


      <b-form-select v-model="selectedCategory" :options="optionsCategory" @input="updateSearch()" ><</b-form-select>

      <b-form-select v-model="sortByRating" :options="optionsSortBy" @input="updateSearch()" ><</b-form-select>

      <b-form-select v-model="starRating" :options="optionsStarRating" @input="updateSearch()" ><</b-form-select>

      <b-form-select v-model="costRating" :options="optionsCostRating" @input="updateSearch()" ><</b-form-select>
</div>

    </b-form>

</div>



    <div  id = "venues">
      <table class="table table-bordered">
        <!--<thead class="thead-dark">-->

        <td>
          <b>Photo</b>
        </td>
        <td>

          <b>Venue Name</b>
        </td>
        <td>
          <b>City</b>
          </td>

        <td>
          <b>Mean Star Rating</b>
        </td>
        <td>
          <b>Mode Cost Rating</b>
        </td>
        <td>
        <b>Category</b>
      </td>
        <!--</thead>-->

        <!--<div v-if="$route.params.venueId"></div>-->

        <tr  v-for="venue in venues">
          <td>
            <div v-if="venue.primaryPhoto === null">
            <img src="/src/assets/default.png" width="150" height="150" >
            </div>
            <div v-else="venue.primaryPhoto">
              <img :src="getPhoto(venue)" width="150" height="150" >
            </div>
          </td>




          <td> {{ venue.venueName }} <router-link :to="{ name: 'Details', params: { venueId: venue.venueId }}" >View</router-link></td>
          <td> {{venue.city}}</td>
          <td v-if="venue.meanStarRating === null">3</td>
          <td v-if="venue.meanStarRating !== null"> {{venue.meanStarRating}}</td>
          <td v-if="venue.modeCostRating === null"> 0</td>
          <td v-if="venue.modeCostRating !== null"> {{venue.modeCostRating}}</td>
          <td> {{venue.categoryName}}</td>
        </tr>

      </table>


      <!--<div class="overflow-auto">-->
        <!--&lt;!&ndash; Use text in props &ndash;&gt;-->
        <!--<b-pagination-->
          <!--v-model="currentPage"-->
          <!--:total-rows="rows"-->
          <!--:per-page="perPage"-->
          <!--class="mt-4"-->
          <!--align="center"-->
        <!--&gt;-->
          <!--<span class="text-success" slot="first-text">First</span>-->
          <!--<span class="text-danger" slot="prev-text">Prev</span>-->
          <!--<span class="text-warning" slot="next-text">Next</span>-->
          <!--<span class="text-info" slot="last-text">Last</span>-->
          <!--<div slot="ellipsis-text">-->
            <!--<b-spinner small type="grow"></b-spinner>-->
            <!--<b-spinner small type="grow"></b-spinner>-->
            <!--<b-spinner small type="grow"></b-spinner>-->
          <!--</div>-->
          <!--<span slot="page" slot-scope="{ page, active }">-->
        <!--<b v-if="active">{{ page }}</b>-->
        <!--<i v-else>{{ page }}</i>-->
      <!--</span>-->
        <!--</b-pagination>-->
      <!--</div>-->

      <div class="overflow-auto">

      <b-pagination @input="updateSearch()"
                    align="center"
                    class="mt-2"
                    v-model="currentPage"
                    :total-rows="numberOfVenues"
                    :per-page="perPage"
                    :limit="10">
        <span class="text-success" slot="first-text">First</span>
        <span class="text-danger" slot="prev-text">Prev</span>
        <span class="text-warning" slot="next-text">Next</span>
        <span class="text-info" slot="last-text">Last</span>
        <div slot="ellipsis-text">
          <b-spinner small type="grow"></b-spinner>
          <b-spinner small type="grow"></b-spinner>
          <b-spinner small type="grow"></b-spinner>
        </div>
        <span slot="page" slot-scope="{ page, active }">
        <b v-if="active">{{ page }}</b>
        <i v-else>{{ page }}</i>
      </span>

      </b-pagination>
      </div>






    </div>
    </b-container>
  </div>


</template>

<script>
  import Navbar from './Navbar.vue'

    export default {
       name: "Venues",
      components:{Navbar},
      data() {
        return {
          error: "",
          errorFlag: false,
          searchTerm: null,
          venues: [],
          allvenues: [],
          categories: [],
          starRating: null,
          costRating: null,
          selectedCity: null,
          selectedCategory: null,
          goodVenues: [],
          sortByRating: 'Star Rating (Highest to Lowest)',
          optionsCategory: [
            {value: null, text: 'Select a category'},
            {value: 'Cafés & Restaurants', text: 'Cafés & Restaurants'},
            {value: 'Accommodation', text: 'Accommodation'},
            {value: 'Events', text: 'Events'},
            {value: 'Nature Spots', text: 'Nature Spots'}


          ],

          cities: [
            {value: null, text: 'Select city'}

            ],

          optionsSortBy:[
            {value: 'Star Rating (Highest to Lowest)', text: 'Star Rating (Highest to Lowest)'},
            {value: 'Star Rating (Lowest to Highest)', text: 'Star Rating (Lowest to Highest)'},
            {value: 'Cost Rating (Highest to Lowest)', text: 'Cost Rating (Highest to Lowest)'},
            {value: 'Cost Rating (Lowest to Highest)', text: 'Cost Rating (Lowest to Highest)'}
          ],

          optionsStarRating: [
            {value: null, text: 'Select star rating'},
            {value: '1', text: '1'},
            {value: '2', text: '2'},
            {value: '3', text: '3'},
            {value: '4', text: '4'},
            {value: '5', text: '5'}
          ],
          optionsCostRating: [
            {value: null, text: 'Select cost rating'},
            {value: '0', text: 'Free (0)'},
            {value: '1', text: '$ (1)'},
            {value: '2', text: '$$ (2)'},
            {value: '3', text: '$$$ (3)'},
            {value: '4', text: '$$$$ (4)'}
          ],

          perPage: 10,
          currentPage: 1,
          rows: 100,
          numberOfVenues: 0

        }
      },
      mounted: function() {
        this.getVenues();
        this.updateSearch();
        // this.getCategories();
      },
      methods: {
        getVenues: function() {
          this.$http.get('http://127.0.0.1:4941/api/v1/venues')
            .then(function (response) {
              this.allvenues = response.data;

              this.$http.get('http://127.0.0.1:4941/api/v1/categories')
                .then(function (response) {
                  this.categories = response.data;


                  console.log(this.categories);

                  for (let venue of this.allvenues) {
                    for (let category of this.categories) {
                      if (venue.categoryId === category.categoryId) {
                        venue.categoryName = category.categoryName;
                      }
                    }
                  }
                 this.venues= this.allvenues;

                  var checkcity = [];
                  var City = [];
                  City.push({value: null, text: "Select city"});
                  for(let venue of this.allvenues) {
                    let city = {};
                    city.value = venue.city;
                    city.text = venue.city;

                    if(checkcity.includes(city.text) === false) {
                      checkcity.push(city.text);
                      City.push(city);
                    } this.cities = City
                  }




                  this.numberOfVenues = this.venues.length;
                  console.log("number", this.numberOfVenues);


                  this.updateSearch();
                  //this.categories = response.data;

                }, function (error) {
                  this.error = error;
                  this.errorFlag = true;
                });
            })
        },

        updateSearch : function() {
          // console.log(this.selectedCity);
          let paramsJSON = {};
          if(this.searchTerm != null && this.searchTerm.length > 1 ) {
            paramsJSON.q = this.searchTerm;
          }
          if(this.selectedCity != null) {
            paramsJSON.city = this.selectedCity;
          }

          if(this.sortByRating != null) {
            if(this.sortByRating === 'Star Rating (Highest to Lowest)'){
              paramsJSON.sortBy = "STAR_RATING";
              paramsJSON.reverseSort = false;
            }
            if(this.sortByRating === 'Star Rating (Lowest to Highest)'){
              paramsJSON.sortBy = "STAR_RATING";
              paramsJSON.reverseSort = true;
            }
            if(this.sortByRating === 'Cost Rating (Highest to Lowest)'){
              paramsJSON.sortBy = "COST_RATING";
              paramsJSON.reverseSort = true;
            }
            if(this.sortByRating === 'Cost Rating (Lowest to Highest)'){
              paramsJSON.sortBy = "COST_RATING";
              paramsJSON.reverseSort = false;
            }


          }

        if(this.starRating != null) {
            paramsJSON.minStarRating = this.starRating;
          }
        if(this.costRating != null) {
          paramsJSON.maxCostRating = this.costRating;
        }



          this.$http.get('http://127.0.0.1:4941/api/v1/venues', {
              params: paramsJSON
            }
          ).then(function (response) {
            this.venues = response.data;

            this.numberOfVenues = this.venues.length;

            this.venues = this.venues.slice(this.perPage * (this.currentPage-1), (this.perPage * this.currentPage-1));


            for (let venue of this.venues) {
              for (let category of this.categories) {
                if (venue.categoryId === category.categoryId) {
                  venue.categoryName = category.categoryName;
                }
              }
            }

            console.log("this.venues1",this.venues);
            for (let venue of this.venues){
              console.log(venue.categoryName);
              console.log(this.selectedCategory);
              console.log(venue.categoryName == this.selectedCategory);
              if(this.selectedCategory != venue.categoryName && this.selectedCategory != null) {
                this.venues.pop(venue);


              }
              // if(this.selectedCategory == venue.categoryName) {
              //   this.goodVenues.push(venue);
              //   this.venues = this.goodVenues;
              // }

            }
            console.log("this.venues2",this.venues);






          }, function (error) {
            console.log(error);
          });


        },
        getPhoto (venue) {
          return "http://127.0.0.1:4941/api/v1/venues/" + venue.venueId + "/photos/" + venue.primaryPhoto

        },

        moveToVenues: function () {
          this.$router.push({name: "Venues"});

        },

        moveToHome: function () {
          this.$router.push({name: "Home"});
        },
        moveToRegister: function () {
          this.$router.push({name: "Register"})
        }




      }
    }

</script>

<style scoped>
</style>
