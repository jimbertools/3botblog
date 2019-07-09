<template>
  <div class="main">
    <div class="outer-container">
      <div class="back">
        <router-link to="./">back to list</router-link>
      </div>
      <!-- Button to edit document in dashboard -->
      <!-- <prismic-edit-button :documentId="documentId" /> -->

      <h1 class="blog-title">{{ fields.title }}</h1>
      <!-- <p class="blog-post-meta"><span class="created-at">{{ Intl.DateTimeFormat('en-US', dateOptions).format(new Date(fields.date)) }}</span></p> -->
      <!--
        <p class="blog-post-meta">
        <span class="created-at">{{ Intl.DateTimeFormat('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }).format(new Date(fields.date)) }}</span>
      </p>
      -->
      <!-- <p class="blog-post-meta">{{ fields.description }} - {{fields.date}}</p> -->
      <p class="blog-post">{{ fields.body }}</p>

      <v-btn v-on:click="this.deletePost" depressed>Delete post</v-btn>
    </div>
    <!-- Slice Block Componenet tag -->
    <slices-block :slices="slices" />
  </div>
</template>

<script>
//Importing all the slices components
import SlicesBlock from "../components/SlicesBlock.vue";

export default {
  name: "post",
  components: {
    SlicesBlock
  },
  data() {
    return {
      dateOptions: { year: "numeric", month: "short", day: "2-digit" },
      documentId: "",
      fields: {
        title: null,
        date: null,
        body: null,
        description: null
      },
      gunId: null,
      slices: []
    };
  },
  methods: {
    deletePost() {
      // console.log("deleting", this.$route.params.uid)
      window.gun
        .get("posts")
        .get(this.gunId)
        .put(null);
      // this.$router.push({path: 'blog-home'})
      window.location = "/blog";
      // window.location =
    },
    getContent(id) {
      //Query to get post content
      window.gun
        .get("posts")
        .map()
        .once(post => {
          console.log(id, post);
          if (post != null && id == post.id) {
            this.gunId = post._["#"];
            this.fields.title = post.title;
            this.fields.date = post.date;
            this.fields.description = post.description;
            this.fields.body = post.body;
          }
        });
      // this.$prismic.client.getByUID('post', uid)
      //   .then((document) => {
      //     if (document) {
      //       this.documentId = document.id
      //       this.fields.title = document.data.title
      //       this.fields.date = document.data.date

      //       //Set slices as variable
      //       this.slices = document.data.body
      //     }
      //     else {
      //       //returns error page
      //       this.$router.push({ name: 'not-found' })
      //     }
      //   })
    }
  },
  created() {
    console.log(this.$route.params);
    this.getContent(this.$route.params.uid);
  },
  beforeRouteUpdate(to, from, next) {
    this.getContent(to.params.uid);
    // next()
  }
};
</script>

<style>
.post-part.single a {
  text-decoration: none;
  background: -webkit-linear-gradient(
    top,
    rgba(0, 0, 0, 0) 75%,
    rgba(0, 0, 0, 0.8) 75%
  );
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0) 75%,
    rgba(0, 0, 0, 0.8) 75%
  );
  background-repeat: repeat-x;
  background-size: 2px 2px;
  background-position: 0 23px;
}
.blog-post-meta {
  color: #9a9a9a;
  font-family: "Lato", sans-serif;
  margin-bottom: 8px;
}

/* Media Queries */
@media (max-width: 767px) {
  .post-part pre {
    font-size: 14px;
  }
  .blog-post-meta {
    font-size: 16px;
  }
}

@media screen and (min-width: 768px) {
  .blog-post-meta {
    font-size: 16px;
  }
}
</style>