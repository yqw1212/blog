<template>
    <div id="edit-blog">
        <h2>编辑博客</h2>
        <form v-if="!submitted">
            <label>博客标题</label>
            <input type="text" v-model="blog.title" required>
            <label>博客内容</label>
            <textarea v-model="blog.content" required></textarea>
            <div id="checkboxes">
                <label>学习</label>
                <input type="checkbox" value="study" v-model="blog.categories">
                <label>生活</label>
                <input type="checkbox" value="life" v-model="blog.categories">
                <label>情感</label>
                <input type="checkbox" value="emotion" v-model="blog.categories">
            </div>
            <label>作者:</label>
            <select v-model="blog.author">
                <option v-bind:key="author" v-for="author in authors">
                    {{ author }}
                </option>
            </select>
            <button v-on:click.prevent="post()">编辑博客</button>
        </form>
        <div v-if="submitted">
            <h3>您的博客发布成功</h3>
        </div>
        <hr>
        <div id="preview">
            <h3>博客总览</h3>
            <p>博客标题:{{ blog.title }}</p>
            <p>博客内容:</p>
            <p>{{ blog.content }}</p>
            <p>博客分类:</p>
            <ul>
                <li v-bind:key="category" v-for="category in blog.categories">
                    {{ category }}
                </li>
            </ul>
            <p>作者:{{ blog.author }}</p>
        </div>
    </div>
  
</template>

<script>
export default {
  name: 'edit-blog',
  data () {
    return {
        id:this.$route.params.id,
        blog: {},
        authors: ["I","Anonymous"],
        submitted: false
    }
  },
  created() {
      this.fetchData();
  },
  methods: {
      fetchData:function() {
          this.$http.get("https://blog-a5e04.firebaseio.com/posts/" + this.id + ".json")
          .then(response=>{
              this.blog = response.body;
          })
      },
      post:function(){
            this.$http.put("https://blog-a5e04.firebaseio.com/posts/" + this.id + ".json",this.blog)
            .then(function(data){
                console.log(data);
                this.submitted = true;
            });
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#edit-blog *{
    box-sizing: border-box;
}
#edit-blog {
    margin: 20px auto;
    max-width: 600px;
    padding: 20px;
}
label {
    display: block;
    margin: 20px 0 10px;
}
input[type="text"],textarea,select {
    display: block;
    width: 100%;
    padding: 8px;
}
textarea {
    height: 200px;
}
#checkboxes label{
    display: inline-block;
    margin-top: 0;
}
#checkboxes input{
    display: inline-block;
    margin-right: 15px;
}
button {
    display: block;
    margin: 20px 0;
    background: crimson;
    color: #fff;
    border: 0;
    padding: 14px;
    border-radius: 4px;
    font-size: 18px;
    cursor: pointer;
}
#preview {
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
}
h3 {
    margin-top: 10px;
}
</style>
