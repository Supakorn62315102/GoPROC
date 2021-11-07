<template>
  <div>
    <br><br>
    <b-container class="bv-example-row">
      <b-row class="text-left">
        <b-col></b-col>
        
        <b-col cols="10" class="bg4">
          <br>
          <center><h1>เลือกซื้อกล้อง</h1></center>
          <hr />

          <form class="box1" v-on:submit.prevent="createBlog">
         
            <p> ชื่อ : <input type="text" class="form-control" v-model="blog.firstname" /> </p>
            
            <p> นามสกุล : <input type="text" class="form-control" v-model="blog.lastname" /> </p>
            
            <p> ชื่อยี่ห้อของกล้อง :
              <select required class="form-control"  v-model="blog.nroom">
                      <option>Canon Camera EOS 250D</option>
                      <option>Olympus OM-D E-M10 Mark III</option>
                      <option>Sony Camera Alpha A6000</option>
                      <option>Canon EOS 90D</option>
                      <option>NIKON D850 Kit</option>
                      <option>Nikon CoolPix W300</option>
                      <option>Fujifilm X-T3</option>
                      <option>Nikon D3500 DSLR Camera</option>
                      <option>Olympus PEN E-PL10</option>
                      <option>Fujifilm</option>
              </select>
            </p>

            <p> ราคากล้อง : <input type="number" class="form-control" required v-model="blog.pnum" /></p>

            <p> เบอร์โทรศัพท์ : <input type="tel" class="form-control" required v-model="blog.tel" /></p>
            
            <p> วันที่ซื้อ : <input type="date" required class="form-control" v-model="blog.dates" /></p>

            <p> ที่อยู่ : <input type="text" required class="form-control" v-model="blog.other" /></p>


            
          
<hr>
            <p>
              <b-button pill variant="success" type="submit"
                >ยืนยันการซื้้อ</b-button
              >
              <b-button pill variant="danger" type="reset"
                >ล้างประวัติการซื้อ</b-button
              >
              <b-button
                pill
                variant="secondary"
                v-on:click="navigateTo('/blogs')"
                >ย้อนกลับ</b-button
              >
            </p>
          </form>
<br><br><br><br><br><br><br><br><br><br>
        </b-col>
        <b-col> </b-col>
      </b-row>
    </b-container>
  </div>
  
</template>



<script>
import BlogService from "@/services/BlogsService";
import VueCkeditor from "vue-ckeditor2";
import UploadService from "@/services/UploadService";
import App from "../../App.vue";

const STATUS_INITIAL = 0,
  STATUS_SAVING = 1,
  STATUS_SUCCESS = 2,
  STATUS_FAILED = 3;

export default {
  data() {
    return {
      BASE_URL: "http://localhost:8081/assets/uploads/",
      error: null,
      //uploadedFiles: [],
      uploadError: null,
      currentStatus: null,
      uploadFieldName: "userPhoto",
      uploadedFileNames: [],

      pictures: [],
      pictureIndex: 0,

      blog: {
        title: "",
        thumbnail: "null",
        pictues: "null",
        content: "",
        category: "",
        status: ""
      },
      config: {
        /* toolbar: [
          ["Bold", "Italic", "Underline", "Strike", "Subscript", "SuperScript"],
        ], */
        height: 300
      }
    };
  },
  methods: {
    async createBlog() {
      this.blog.pictures = JSON.stringify(this.pictures); //update ค่าของ blogs.pictrues
      try {
        await BlogService.post(this.blog);
        this.$router.push({
          name: "blogs"
        });
      } catch (err) {
        console.log(err);
      }
    },
    onBlur(editor) {
      console.log(editor);
    },
    onfocus(editor) {
      console.log(editor);
    },
    navigateTo(route) {
      console.log(route);
      this.$router.push(route);
    },
    wait(ms) {
      return x => {
        return new Promise(resolve => setTimeout(() => resolve(x), ms));
      };
    },
    reset() {
      //reset form to initial state
      this.currentStatus = STATUS_INITIAL;
      //this.uploadedFiles = []
      this.uploadError = null;
      this.uploadedFileNames = [];
    },
    async save(formData) {
      //upload data to the server
      try {
        this.currentStatus = STATUS_SAVING;
        await UploadService.upload(formData);
        this.currentStatus = STATUS_SUCCESS;

        // update image uploaded display
        let pictureJSON = [];
        this.uploadedFileNames.forEach(uploadFilename => {
          let found = false;
          for (let i = 0; i < this.pictures.length; i++) {
            if (this.pictures[i].name == uploadFilename) {
              found = true;
              break;
            }
          }

          if (!found) {
            this.pictureIndex++;
            let pictureJSON = {
              id: this.pictureIndex,
              name: uploadFilename
            };
            this.pictures.push(pictureJSON);
          }
        });

        this.clearUploadResult();
      } catch (error) {
        console.log(error);
        this.currentStatus = STATUS_FAILED;
      }
    },
    filesChange(filedName, fileList) {
      //handle file changes
      const formData = new FormData();

      if (!fileList.length) return;

      //append the files to FormData
      Array.from(Array(fileList.length).keys()).map(x => {
        formData.append(filedName, fileList[x], fileList[x].name);
        this.uploadedFileNames.push(fileList[x].name);
      });

      //save it
      this.save(formData);
    },
    clearUploadResult: function() {
      setTimeout(() => this.reset(), 5000);
    },

    //เรียกใช้ v-0n:cick.prevent ส่ง picture ไปลบ
    async delFile(material) {
      let result = confirm("Want to delete?");
      if (result) {
        let dataJSON = {
          filename: material.name
        };

        await UploadService.delete(dataJSON);

        for (var i = 0; i < this.pictures.length; i++) {
          if (this.pictures[i].id === material.id) {
            this.pictures.splice(i, 1);
            this.materialIndex--;
            break;
          }
        }
      }
    },

    /* thumbnail */
    useThumbnail(filename) {
      console.log(filename);
      this.blog.thumbnail = filename;
    }
  },

  created() {
    this.reset();
  },
  computed: {
    isInitial() {
      return this.currentStatus === STATUS_INITIAL;
    },
    issaving() {
      return this.currentStatus === STATUS_SAVING;
    },
    isSuccess() {
      return this.currentStatus === STATUS_SUCCESS;
    },
    isFailed() {
      return this.currentStatus === STATUS_FAILED;
    }
  },
  components: {
    VueCkeditor
  }
};
</script>
<style scoped>
.dropbox {
  outline: 2px dashed gray; /* the dash box */
  outline-offset: -10px;
  background: #fec3df;
  color: dimgray;
  padding: 10px 10px;
  min-height: 200px; /* minimum height */
  position: relative;
  cursor: pointer;
}

.input-file {
  opacity: 0; /* invisible but it's there! */
  width: 100%;
  height: 200px;
  position: absolute;
  cursor: pointer;
}

.dropbox:hover {
  background: #fceebe;
}

.dropbox p {
  font-size: 1.2em;
  text-align: center;
  padding: 50px 0;
}

ul.pictures {
  list-style: none;
  padding: 0;
  margin: 0;
  float: left;
  padding-top: 10px;
  padding-bottom: 10px;
}
ul.pictures li {
  float: left;
}
ul.pictures li img {
  max-width: 180px;
  margin-right: 20px;
}
.clearfix {
  clear: both;
}
/* thumbnail */
.thumbnail-pic img {
  width: 200px;
}

input,select {
      width: 450px;
    }
</style>
