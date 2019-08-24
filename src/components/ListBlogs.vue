<template>
  <div v-theme:column="'narrow'" id="show-blogs">
    <!-- <ShowBlogs /> -->
    <h1>测试Mixin</h1>
    <!-- serch -->
    <input type="text" v-model="search" placeholder="search something...." />
    <div v-for="(blog,index) in filterdBlogs" :key="index" class="single-blog">
      <h2 v-rainbow>{{blog.title | to-uppercase }}</h2>
      <article>{{blog.body|snippet}}</article>
    </div>
  </div>
</template>

<script>
import searchMixin from "../mixins/searchMixins";

export default {
  name: "ShowBlogs",
  data() {
    return {
      blogs: [],
      search: ""
    };
  },
  created() {
    this.$axios.get("/posts").then(res => {
      console.log(res.data);
      this.blogs = res.data.slice(0, 10);
    });
  },

  mixins: [searchMixin],

  //可注册多个局部指令
  directives: {
    rainbow: {
      bind(el, bingding, vnode) {
        el.style.color =
          "#" +
          Math.random()
            .toString(16)
            .slice(2, 8);
      }
    },
    theme: {
      bind(el, binding, vnode) {
        if (binding.value == "wide") {
          el.style.maxWidth = "1260px";
        } else if (binding.value == "narrow") {
          el.style.maxWidth = "560px";
        }
        if (binding.arg == "column") {
          el.style.background = "blue";
          el.style.padding = "20px";
        }
      }
    }
  },
  filters: {
    // "to-uppercase": function(value) {
    //   return value.toUpperCase();
    // }
    toUppercase(value) {
      return value.toUpperCase();
    },
    snippet(value) {
      return value.slice(0, 100) + "...";
    }
  }
};
</script>


<style scoped>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
}
.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background-color: #eee;
}
input[type="text"] {
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}
</style>
