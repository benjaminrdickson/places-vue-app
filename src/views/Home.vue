<template>
  <div class="home">
    <h2>New Place </h2>
    Name: <input type="text" v-model="newPlaceName"> <br>
    Address <input type="text" v-model="newPlaceAddress"> <br>
    <button v-on:click="createPlace"> Add New Place </button>




    <div v-for="place in places" v-bind:key="place.id">
      <h1> {{place.name}} </h1> <br>
      <p> {{place.address}}</p>
      <button v-on:click="showPlace(place)"> Info </button>

      <dialog id="place-details">
      <form method="dialog">
        <h3>Place Info</h3>
        <p>Name: <input type="text" v-model="currentPlace.name" /></p>
        <p>Address: <input type="text" v-model="currentPlace.address" /></p>
        <button v-on:click="updatePlace">Update</button>
        <button v-on:click="destroyPlace">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
      
    



    </div>
    
   
  </div>
</template>

<style></style>

<script>
  import axios from "axios";
  export default {
    data: function () {
      return {
        message: "Favorite Places",
        places: [],
        currentPlace: {}
      };
    },
    created: function () {
      this.indexPlaces();
    },
    methods: {
      indexPlaces: function () {
        axios.get("http://localhost:3000/places").then((response) => {
          console.log("Places Array", response.data);
          this.places = response.data;
        });
      },
      createPlace: function () {
        var params = {
          name: this.newPlaceName,
          address: this.newPlaceAddress
        };
        axios.post("http://localhost:3000/places", params).then((response) => {
          console.log("Places array", response.data);
          this.places.push(response.data);
          })
          .catch((error) => {
            console.log(error.response.data.errors);
          });
      },
      showPlace: function (place) {
        console.log(place);
        this.currentPlace = place;
        document.querySelector("#place-details").showModal();
      },
      updatePlace: function () {
        console.log(this.currentPlace);
        axios.patch(`http://localhost:3000/places/${this.currentPlace.id}`,
        this.currentPlace)
        .then((response) => {
          console.log("Updated", response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        })
      },
      destroyPlace: function () {
        axios.delete(`http://localhost:3000/places/${this.currentPlace.id}`,
        this.currentPlace)
        .then((response) => {
          console.log("Deleted", response.data);
          var index = this.places.indexOf(this.currentPlace);
          console.log(index);
          this.places.splice(index, )
        }

        )
      }
    }
  };
</script>
