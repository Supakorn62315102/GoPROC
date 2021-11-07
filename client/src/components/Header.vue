<template>
    <div>
      
        <div class="sidebar"> 
          <div class="title"><center>
            <div class="img">
            </div>
            <img src="@/assets/Lo1.png">
            
        </center></div>

            <a><router-link :to="{name: 'login'}"><i class="" aria-hidden="true"></i> เข้าสู่ระบบ</router-link></a>
            
            <a><router-link :to="{name: 'blogs'}"><i class="" aria-hidden="true"></i> เลือกซื้อกล้อง</router-link></a>
            <a><router-link :to="{name: 'comments'}"><i class="" aria-hidden="true"></i> รีวิวกล้อง</router-link></a>
            <a><router-link :to="{name: 'users'}"><i class="" aria-hidden="true"></i> USER</router-link></a>
            <a><a v-on:cick.prevent="logout" href="login"><i class="" aria-hidden="true"></i> ออกจากระบบ</a></a>
        
        </div>
    </div>

</template>

<script>
export default {
  methods: {
    logout () {
      this.$store.dispatch('setToken', null)
      this.$store.dispatch('setUser', null)
      this.$router.push({
        name: 'login'
      })
    }
  }
};
</script>

<style scoped>
    /* The side navigation menu */
.sidebar {
  margin-top: 12%;
  padding: 0;
  width: 200px;
  background-color: rgba(255, 70, 156);
  position: fixed;
  height: 50%;
  overflow: auto;
  font-size: 18px;
  border-radius: 0px 30px 30px 0px;

}

/* Sidebar links */
.sidebar a {
  display: block;
  color: rgb(14, 13, 13);
  padding: 9px; /* ห่างจากตัวออกไป */
  text-decoration: none;
}

/* Active/current link */
.sidebar a.active {
  background-color: #fffb00;
  color: rgb(0, 0, 0);
}

/* Links on mouse-over */
.sidebar a:hover:not(.active) {
  background-color: #fffb0d;
  border-radius: 50px 50px 50px 50px;
  color: rgb(0, 0, 0);
}

/* Page content. The value of the margin-left property should match the value of the sidebar's width property */
div.content {
  margin-left: 200px;
  padding: 1px 16px;
  height: 1000px;
}

/* On screens that are less than 700px wide, make the sidebar into a topbar */
@media screen and (max-width: 700px) {
  .sidebar {
    width: 100%;
    height: auto;
    position: relative;
  }
  .sidebar a {float: left;}
  div.content {margin-left: 0;}
}

/* On screens that are less than 400px, display the bar vertically, instead of horizontally */
@media screen and (max-width: 400px) {
  .sidebar a {
    text-align: center;
    float: none;
  }
}

.sidebar a.router-link-active{
    color: rgb(0, 0, 0);
    background-color: #fba13b;
     border-radius: 50px 50px 50px 50px;

}
img {
  width: 100px;
  height: 125px;
}
</style>