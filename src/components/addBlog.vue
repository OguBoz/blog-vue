<template>
  <div id="add-blog">
      <h2>Add a New Post</h2>
      <form v-if="!submitted">
          <label>Blog Title:</label>
          <input type="text" required v-model.lazy.trim="blog.title">
          <label>Blog Content:</label>
            <textarea v-model.lazy.trim="blog.content"></textarea>
            <div id="checkboxes">
                <label>Sports</label>
                <input type="checkbox" value="Sports" v-model="blog.categories">
                <label>Politics</label>
                <input type="checkbox" value="Politics" v-model="blog.categories">
                <label>Music</label>
                <input type="checkbox" value="Music" v-model="blog.categories">
                <label>Culture</label>
                <input type="checkbox" value="Culture" v-model="blog.categories">
            </div>
            <select v-model="blog.author">
                <option v-for="author in authors" :key="author">{{author}}</option>
            </select>
            <button v-on:click.prevent="post">Add Blog</button>
        </form>
        <div v-if="submitted">
            <h3>Your post has been added!</h3>
        </div>
        <div id="preview">
            <h3>Preview blog</h3>
            <p>Blog title: {{ blog.title }}</p>
            <p>Blog content:</p>
            <p style="white-space: pre">{{ blog.content }}</p>
            <p>Blog Categories:</p>
            <template v-if="blog.categories.length"><span v-for="category in blog.categories" :key="category">{{ category }}</span></template>
            <p>Author: {{ blog.author }}</p>
        </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
        blog: {
            title: '',
            content: '',
            author: '',
            categories: []
        },
        authors: ["Oguz", "Jack", "Robert"],
        submitted: false
    }
  },
  methods: {
      post: function() {
          this.$http.post("https://vue-blog-ae927.firebaseio.com/posts.json", this.blog)
            .then(data => {
                console.log(data)
                this.submitted = true
            })
      }
  }
}
</script>

<style>
#add-blog *{
    box-sizing: border-box;
}
#add-blog{
    margin: 20px auto;
    max-width: 500px;
}

#checkboxes {
    margin: 20px 0;
}

label{
    margin: 20px 0 10px;
}
input[type="text"], textarea{
    display: block;
    width: 100%;
    padding: 8px;
}
#preview{
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
}

#preview span {
    margin-left:7px;
}

h3{
    margin-top: 10px;
}
</style>
