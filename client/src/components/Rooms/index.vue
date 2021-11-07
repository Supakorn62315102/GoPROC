<template>
  <div>
    <b-container class="bv-example-row">
    <b-row class="text-left">
         <b-col></b-col>

         <b-col cols="10" class="bg4">
           <br><br>
    <center><h1>กล้องมือ 1 สภาพดี</h1></center>
  <!--   <p><button v-on:click="logout">Logout</button></p> -->
    <center><h5>รายละเอียดการซื้อกล้อง {{ rooms.length }} ตัว</h5> <hr></center>

    <div class="box2" v-for="room in rooms" v-bind:key="room.id">
      <p>ตัวที่: {{ room.id }}</p>
      <p>ชื่อยี่ห้อกล้อง: {{ room.nameroom }}</p>
      <p>ราคากล้อง: {{ room.peopleroom }}</p>
      
      <hr>
    </div>
<center><div class="cam1">
      <p><img src="@/assets/01.jpg"></p>
      </div></center>

      <center><div class="cam1">
      <p><img src="@/assets/02.jpg"></p>
      </div></center>

      <center><div class="cam1">
      <p><img src="@/assets/03.jpg"></p>
      </div></center>

     <center> <div class="cam1">
      <p><img src="@/assets/04.jpg"></p>
      </div></center>

      <center><div class="cam1">
      <p><img src="@/assets/05.jpg"></p>
      </div></center>
      
      <center><div class="cam1">
      <p><img src="@/assets/06.jpg"></p>
      </div></center>
      
      <center><div class="cam1">
      <p><img src="@/assets/07.jpg"></p>
      </div></center>

      <center><div class="cam1">
      <p><img src="@/assets/08.jpg"></p>
      </div></center>

      <center><div class="cam1">
      <p><img src="@/assets/09.jpg"></p>
      </div></center>

    </b-col>
      <b-col> </b-col>
     </b-row>
</b-container>
  </div>
</template>
<script>

import RoomService from "@/services/RoomsService";
export default {
  data() {
    return {
      rooms: [],
    };
  },
  async created() {
    this.rooms = (await RoomService.index()).data;
  }
};
</script>
<style scoped>
</style>