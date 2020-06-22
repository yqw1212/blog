<template>
    <div id="single-blog">
        <h1>{{ blog.title }}</h1>
        <article>{{ blog.content }}</article>
        <p>作者:{{ blog.author }}</p>
        <p>分类:</p>
        <ul>
            <li v-bind:key="category" v-for="category in blog.categories">
                {{ category }}
            </li>
        </ul>
        <button v-on:click="deleteSingleBlog()">删除</button>
        <router-link v-bind:to="'/edit/'+id"><button>编辑</button></router-link>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:"single-blog",
    data() {
        return{
            id:this.$route.params.id,
            blog: {}
        }
    },
    created() {
        // this.$http.get("https://blog-a5e04.firebaseio.com/posts/" + this.id + ".json")
        axios.get("/posts/" + this.id + ".json")
        .then((data) => {
            // console.log(data);
            // this.blog=data.body;
            // return data.json();
            return data.data;
        })
        .then((data) => {
            this.blog = data;
        })
    },
    methods: {
        deleteSingleBlog:function () {
            this.$http.delete("https://blog-a5e04.firebaseio.com/posts/" + this.id + ".json")
            .then(response=>{
                this.$router.push({path:"/"})
            })
        }
    }
}
</script>

<style scoped>
#single-blog {
    max-width: 960px;
    margin: 0 auto;
    padding: 20px;
    background: #eee;
    border: 1px dotted #aaa;
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
</style>