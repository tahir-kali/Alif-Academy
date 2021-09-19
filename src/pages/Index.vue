<template>
  <q-page class="flex flex-center">
  <!-- Main Blog Part here -->
  <div class="full-width q-pr-xl q-pl-xl q-pt-sm row">
  <div class="collapse q-pa-sm q-mr-sm bg-black text-white text-h6" style="border-radius:5px">
  Alif Blogging
  </div>
  <q-input dense outlined class="col-10 expand" style="float:right;vertical-align:bottom" v-model="search" @change="SearchBlog()">
  <template v-slot:append>
    <div>
      <q-icon name="search" />
    </div>
  </template>
  </q-input>
  </div>
   <div  v-if="checkEmpty()">
  <h1>You have slow internet connection!</h1>
  </div>
  <div class="column full-width q-pl-xl q-pt-sm q-pr-xl" v-else>
   <q-card class="q-pm-xl q-mr-xl q-mb-md full-width cursor-pointer" v-if="blogs.length" style="height:auto;max-width:1600px" @click="ShowBlog(blogs[current].id)">

<q-parallax :src="photos[blogs[current].id]">
 </q-parallax>
      <q-card-section>
        <div class="text-h6">{{ blogs[current].title }}</div>
        <div class="text-subtitle2">By {{ getUser(blogs[current].userId) }}</div>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn flat round color="red" icon="favorite" />
        <q-btn flat round color="teal" icon="bookmark" />
        <q-btn flat round color="primary" icon="share" />
      </q-card-actions>
    </q-card>
    <q-pagination
      v-model="current"
      :max="blogs.length-1"
      input
    />
  </div>

<q-dialog v-model="dpBlog">
      <q-card>
        <q-card-section>
          <div class="text-h6">{{blog.title}}</div>
        </q-card-section>

        <q-separator />

        <q-card-section style="max-height: 80vh" class="scroll">
        <q-img :src="photos[blogs[current].id]" style="border-radius:10px;max-height:300px" />
         {{blogs[current].body}}
        </q-card-section>
      <q-card-section>
      Author {{getUser(blogs[current].userId)}}
      </q-card-section>
        <q-separator />

        <q-card-actions align="right">
          <q-btn flat label="Close" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    return {
      current: ref(0)
    }
  },
  data () {
    return {
      search: '',
      dpBlog: false,
      blog: {
        title: '',
        description: ''
      },
      user: {},
      photo: '',
      blogs: [],
      users: [],
      photos: []
    }
  },
  async created () {
    await this.ReadUsers()
    await this.ReadBlogs()
    await this.ReadPhotos()
  },
  methods: {
    ReadBlogs: function () {
      fetch('http://jsonplaceholder.typicode.com/posts').then(res => res.json()).then(data => {
        this.blogs = data
      })
    },
    ReadPhotos: function () {
      fetch('https://api.unsplash.com/photos/?client_id=ObzfFTFxLcWCWxUv2zzDS57MaOlpS8HKb4Cwse_Pu8k').then(res => res.json()).then(data => {
        data.forEach(d => {
          if (d.urls) {
            this.photos.push(d.urls.full)
          }
        })
      })
    },
    ReadUsers: function () {
      fetch('https://jsonplaceholder.typicode.com/users').then(res => res.json()).then(data => {
        data.forEach(d => {
          this.users.push(d)
        })
      })
    },
    ShowBlog: function (id) {
      console.log(id)
      this.blog = this.blogs[id - 1]
      this.dpBlog = true
    },
    SearchBlog: function () {
      if (name.length > 6) {
        this.current = this.blogs.find(x => x.name.includes(this.search)).id
      }
    },
    getUser: function (id) {
      return this.users.find(x => x.id === id).name
    },
    checkEmpty: function () {
      let condition = false
      if (this.users.length === 0 && this.photos.length === 0 && this.blogs.length === 0) {
        condition = true
      }
      return condition
    }
  }
})
</script>
<style>
  @media only screen and (max-width:600px){
    .collapse{
    display: none;
    }
    .expand{
      width:100vw
    }
  }
</style>
