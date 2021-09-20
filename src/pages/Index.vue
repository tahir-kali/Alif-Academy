<template>
  <q-page class="flex flex-center">
  <!-- Main Blog Part here -->
  <div class="full-width q-pr-xl q-pl-xl q-pt-sm">

   <q-fab
        style="vertical-align:middle"
        dense
        v-model="fab2"
        vertical-actions-align="left"
        label="Options"
        padding="none md"
        icon="keyboard_arrow_down"
        class="text-black q-mt-sm"
        direction="down"
      >
        <q-fab-action padding="3px" external-label color="primary" @click="dpFilters=true" icon="filter_alt" label="Filters" />
        <q-fab-action padding="3px" external-label color="primary" @click="RouteTo('https://facebook.com/lifexrahimi')" icon="fab fa-facebook" label="Facebook" />
         <q-fab-action padding="3px" external-label color="primary" @click="RouteTo('https://www.linkedin.com/in/mohammad-tahir-rahimi-6a14531a2/')" icon="fab fa-linkedin" label="LinkedIn" />
        <q-fab-action padding="3px" external-label color="grey" @click="RouteTo('https://https://github.com/tahir-kali')" icon="fab fa-github" label="Github" />
        <q-fab-action padding="3px" external-label color="red" @click="RouteTo('https://callme-rahimi.web.app/')" icon="code_off" label="Developer" />
      </q-fab>
  <q-input dense outlined style="float:right;vertical-align:bottom" v-model="search" @change="SearchBlog" label="Search blog title">
  <template v-slot:after @click="SearchBlog">
    <div>
      <q-btn icon="search" round dense />
    </div>
  </template>
  </q-input>
  </div>
   <div  v-if="checkEmpty()">
  <h1>You have slow internet connection!</h1>
  </div>
  <div class="column full-width q-pl-xl q-pt-sm q-pr-xl" v-else>
   <q-card class="q-pm-xl q-mr-xl q-mb-md full-width" v-if="blogs.length" style="height:auto;max-width:1600px">

<q-parallax :src="photos[Math.floor(Math.random() * 10)]">
 </q-parallax>
      <q-card-section>
        <div class="text-h6">{{ blogs[current].title }}</div>
        <div class="text-subtitle2">By {{ getUser(blogs[current].userId) }}</div>
      </q-card-section>
      <q-card-actions align="center">
        <q-btn flat round color="grey" icon="visibility"  @click="ShowBlog(blogs[current].id)"/>
        <q-btn flat round color="grey" icon="comment" @click="FetchComments(blogs[current].id)" />
        <q-btn flat round color="grey" icon="share" />
      </q-card-actions>
    </q-card>
    <q-pagination
      v-model="current"
      :max="blogs.length-1"
      input
    />
  </div>

<q-dialog v-model="dpComments">
      <q-card>
        <q-card-section>
          <div class="text-h6">{{blog.title}}</div>
        </q-card-section>

        <q-separator />
      <q-card-section v-if="comments.length" style="max-height:70vh;overflow-y:auto">
      <div v-for="c in comments" :key="c.id" class="q-pa-md">
      <q-item>
         <q-item-section top avatar>
          <q-avatar>
            <img src="https://cdn.quasar.dev/img/boy-avatar.png">
          </q-avatar>
        </q-item-section>
         <q-item-section>
          <q-item-label>{{c.email}}</q-item-label>
          <q-item-label caption lines="2">{{c.body}}</q-item-label>
        </q-item-section>
      </q-item>
      </div>
      </q-card-section>
        <q-separator />

        <q-card-actions align="right">
          <q-btn flat label="Close" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

<q-dialog v-model="dpBlog">
      <q-card>
        <q-card-section>
          <div class="text-h6">{{blog.title}}</div>
        </q-card-section>

        <q-separator />

        <q-card-section style="max-height: 600px;overflow-y:auto" class="scroll">
        <q-img :src="photos[Math.floor(Math.random() * 10)]" style="border-radius:10px;max-height:300px" />
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
  <q-dialog v-model="dpFilters">
      <q-card style="min-width:400px;max-width:400px">
        <q-card-section>
          <div class="text-h6">
          <q-input label="Title of the post" outlined dense>
          <template v-slot:after>
            <div>
              <q-btn color="primary" icon="search" no-caps />
            </div>
          </template>
          </q-input>
          </div>
        </q-card-section>

        <q-separator />

        <q-card-section style="max-height: 60vh;overflow-y:auto">
        <q-select
          outlined
          use-input
          map-options
          emit-value
          dense
          v-model="category"
          multiple
          :options="categories"
          use-chips
          class="q-mb-sm"
          stack-label
          label="Categories">
          <template v-slot:before>
            <div>
              <q-toggle v-model="ctg" />
            </div>
          </template>
        </q-select>
        <q-select
          outlined
          use-input
          map-options
          emit-value
          dense
          v-model="author"
          multiple
          :options="authors"
          use-chips
          class="q-mb-sm"
          stack-label
          label="Authors">
          <template v-slot:before>
            <div>
              <q-toggle v-model="ctg" />
            </div>
          </template>
        </q-select>
        <q-select
          outlined
          use-input
          map-options
          emit-value
          dense
          v-model="tag"
          multiple
          :options="tags"
          use-chips
          class="q-mb-sm"
          stack-label
          label="Tags">
          <template v-slot:before>
            <div>
              <q-toggle v-model="ctg" />
            </div>
          </template>
        </q-select>
           <q-date v-model="range" range multiple style="float:right" />
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
      current: ref(0),
      fab2: ref(false)
    }
  },
  data () {
    return {
      ctg: true,
      ath: true,
      tg: false,
      category: ref(null),
      author: ref(null),
      tag: ref(null),
      categories: ['Politics', 'Law', 'Literature'],
      authors: [],
      tags: ['Politics', 'Law', 'Literature'],
      range: [{ from: '2020/07/01', to: '2020/07/10' }, { from: '2020/07/21', to: '2020/07/25' }],
      dpFilters: false,
      search: '',
      dpComments: false,
      dpBlog: false,
      blog: {
        title: '',
        description: ''
      },
      user: {},
      photo: '',
      blogs: [],
      comments: [],
      users: [],
      photos: []
    }
  },
  async created () {
    await this.FetchUsers()
    await this.FetchBlogs()
    await this.FetchPhotos()
  },
  methods: {
    FetchBlogs: function () {
      fetch('http://jsonplaceholder.typicode.com/posts').then(res => res.json()).then(data => {
        this.blogs = data
      })
    },
    FetchComments: function (id) {
      this.blog = this.blogs[id - 1]
      fetch('http://jsonplaceholder.typicode.com/posts/' + id + '/comments').then(res => res.json()).then(data => {
        data.forEach(d => {
          this.comments.push(d)
        })
      })
      this.dpComments = true
    },
    FetchPhotos: function () {
      fetch('https://api.unsplash.com/photos/?client_id=ObzfFTFxLcWCWxUv2zzDS57MaOlpS8HKb4Cwse_Pu8k').then(res => res.json()).then(data => {
        data.forEach(d => {
          if (d.urls) {
            this.photos.push(d.urls.full)
          }
        })
      })
    },
    FetchUsers: function () {
      fetch('https://jsonplaceholder.typicode.com/users').then(res => res.json()).then(data => {
        data.forEach(d => {
          this.users.push(d)
          this.authors.push(d.name)
        })
      })
    },
    ShowBlog: function (id) {
      console.log(id)
      this.blog = this.blogs[id - 1]
      this.dpBlog = true
    },
    SearchBlog: function () {
      if (this.search.length > 6) {
        this.blogs.forEach(b => {
          if (b.title.includes(this.search)) {
            console.log(b.id)
            this.current = b.id - 1
            return 0
          }
        })
      }
    },
    RouteTo: function (url) {
      window.location.assign(url)
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
