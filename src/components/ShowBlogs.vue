<template>
    <div v-theme="'wide'" id="show-blogs">
        <h1>博客总览</h1>
        <input type="text" v-model="search" placeholder="搜索">
        <div v-bind:key="blog.id" v-for="blog in filteredBlogs" class="single-blog">
            <router-link v-bind:to="'/blog/'+blog.id "><h2 v-rainbow>{{ blog.title | to-uppercase }}</h2></router-link>
            <article>{{ blog.content | snippet }}</article>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'show-blogs',
    data() {
        return {
            blogs: [],
            search:""
        }
    },
    created() {
        // this.$http.get("https://blog-a5e04.firebaseio.com/posts.json")
        axios.get("/posts.json")
        .then(function(data){
            // console.log(data);
            // this.blogs = data.body.slice(0,10)
            // console.log(this.blogs)
            // return data.json();
            return data.data;
        })
        .then((data) => {
            var blogsArray = [];
            for(let key in data){
                data[key].id = key;
                blogsArray.push(data[key]);
            }
            this.blogs = blogsArray;
        })
    },
    computed: {
        filteredBlogs:function(){
            return this.blogs.filter((blog)=>{
                return blog.title.match(this.search);
            })
        }
    },
    filters: {
        "to-uppercase":function(value){
            return value.toUpperCase();
        },
        "snippet":function (value) {
            return value.slice(0,100) + "......";
        }
    }
}
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
    background: #eee;
    border: 1px dotted #aaa;
}
#show-blogs a{
    color: #444;
    text-decoration: none;
}
input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
}
</style>
