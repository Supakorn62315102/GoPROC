<template>
  <div>

    <b-container class="bv-example-row">
    <b-row class="text-left">
         <b-col></b-col>

         <b-col cols="10" class="bg4">

<br><br>
    <center><h1>รีวิวทั้งหมด</h1></center> 
  <!--   <p><button v-on:click="logout">Logout</button></p> -->
    <center><h5>จำนวนรีวิว {{ comments.length }} รีวิว</h5></center> 
<hr>

    <p><b-button pill variant="primary" v-on:click="navigateTo('/comment/create')">เขียนรีวิว</b-button></p>
    <div  class="box3" v-for="comment in comments" v-bind:key="comment.id">
      <p>รีวิวที่: {{ comment.id }}</p>
      <p>หัวข้อ: {{ comment.title }}</p>
      <p>ชื่อภาพ: {{ comment.thumbnail }}</p>
      
      <transition name="fade"> 
        <div class="thumbnail-pic" v-if="comment.thumbnail != 'null'">
          <img :src="BASE_URL+comment.thumbnail" alt="thumbnail">
        </div>
      </transition>
   
      <p>เนื้อหา: {{ comment.content }}</p>
      <hr>
      <p>
        <b-button pill variant="success" v-on:click="navigateTo('comment/'+comment.id)">ดูรีวิว</b-button>
        <b-button pill variant="warning" v-on:click="navigateTo('/comment/edit/' +comment.id)">แก้ไขรีวิว</b-button>
        <b-button pill variant="danger" v-on:click="deleteComment(comment)"> ลบรีวิว</b-button>
      </p>
      
    </div>

    </b-col>
      <b-col> </b-col>
     </b-row>
</b-container>

  </div>
</template>
<script>
import CommentService from "@/services/CommentsService";
export default {
  data() {
    return {
         BASE_URL: "http://localhost:8081/assets/uploads/",
      comments: [],
    };
  },
  async created() {
    this.comments = (await CommentService.index()).data;
  },
  methods: {

  /*   logout() {
      this.$store.dispatch("setToken", null);
      this.$store.dispatch("setUser", null);
      this.$router.push({
        name: "login",
      });
    },
    */
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteComment(comment) {
      let result = confirm("คุณแน่ใจนะ ที่จะลบรีวิวนี้?");
      if (result) {
        try {
          await CommentService.delete(comment);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      this.comments = (await CommentService.index()).data;
    },
  },
};
</script>
<style scoped>
</style>