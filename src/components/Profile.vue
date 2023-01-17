<template>
  <div class="profile_container">
    <div class="profile_information_subcontainer">
      <span class="profile_shortened_name">Bonjour</span>
      <span class="profile_shortened_name" id="name">{{ shortenedName }}</span>
      <div class="profile_name_selectbox">
        <span class="select_box_sentence">Sélectionner un nouveau profil à l'aide de cette liste :</span>
        <select class="select_box" v-model="selected">
            <option v-for="(data, index) in datalist" :key="index"> 
              {{ data.name.first }} {{ data.name.last }} 
            </option>
        </select>
      </div>
      <button class="profile_picture_button" @click="modifyPictureSrc(selected)">{{ buttonText }}</button>
    </div>
    <div class="profile_image_subcontainer">
      <div class="picture">
        <img class="profile_image" :src= "pictureSrc"/>
      </div>
      <div class="triangle_left"></div>
    </div>
  </div>
</template>
<script>
  import axios from "axios";
  import "../assets/profile.css";
  export default {
    data() {
      return {
        datalist: [],
        selected: "",
        pictureSrc: "",
        buttonText: "AFFICHER SA PHOTO DE PROFIL",
      }
    },
    methods: {
      modifyPictureSrc(selectedName) {
        const found = this.datalist.find(profile => 
          selectedName === profile.name.first + " " + profile.name.last);
        this.pictureSrc = found.picture.large;
      },
    },
    computed : {
      shortenedName() {
        if (this.selected === "" || this.selected === null) {
          return "";
        }
        const slashedName = this.selected.split(" ");
        const slashedLastName = Array.from(slashedName[1]);
        const shortenedName = slashedName[0] + " " + slashedLastName[0] + ".";
        return shortenedName.toUpperCase();
      }
    },
    async mounted() {
      // if you want to get more results, change the number in the url
      let result =  await axios.get("https://randomuser.me/api/?results=10");
      this.datalist = result.data.results;
      this.selected = this.datalist[0].name.first + " " + this.datalist[0].name.last;
      this.pictureSrc = this.datalist[0].picture.large;
    }
  }
</script>
