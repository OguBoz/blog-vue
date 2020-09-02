<template>
  <div id="show-blogs">
      <h1>All Blog Articles</h1>
      <input type="text" v-model="search" placeholder="Search for an article...">
      <div v-for="blog in filteredBlogs" class="single-blog" :key="blog.title">
          <router-link v-bind:to="'/blog/' + blog.id"><h2>{{blog.title}}</h2></router-link>
          <article>{{blog.content}}</article>
      </div>
  </div>
</template>

<script>

export default {

  data () {
    return {
        search: "",
        blogs: []
    }
  },
  methods: {
    
  },
  computed: {
      filteredBlogs: function() {
          return this.blogs.filter((blog) => {
              return blog.title.match(this.search)
          });
      }
  },
  created() {
      this.$http.get("https://vue-blog-ae927.firebaseio.com/posts.json")
        .then(data => {
            return data.json();
        }).then((data)=>{
            let blogsArray = [];
            for(let key in data) {
                data[key].id = key;
                blogsArray.push(data[key]);
            }
            this.blogs = blogsArray;
        })
  }
}
</script>

<style>
    #show-blogs {
        max-width: 800px;
        margin: 0 auto;
    }

#show-blogs a {
    text-decoration: none;
    color: #444;
} 

    .single-blog {
        padding: 20px;
        margin: 20px 0;
        box-sizing: border-box;
        background: #eee;
    }

    input {
        display: block;
        padding: 7px;
        width: 100%;
        box-sizing: border-box;
    }
</style>
