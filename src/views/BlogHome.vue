<template>
  <!-- Vue conditional to check if there is any content in document -->
  <div v-if="hasContent" class="page">
    <div class="home">
      <!-- Button to edit document in dashboard -->
      <prismic-edit-button :documentId="documentId" />
      <div class="blog-avatar" :style="{ backgroundImage: 'url(/' + headLineImage + ')' }"></div>
      <h1 class="blog-title" @keyup.enter="editBlog" contenteditable>{{ headLineTitle }}</h1>
      <p class="blog-description" @keyup.enter="editBlog" contenteditable>{{ headLineDescription }}</p>

      <!-- <div class="blog-avatar" :style="{ backgroundImage: 'url(' + fields.image + ')' }"></div> -->
      <!-- Template for page title -->
      <!-- <h1 class="blog-title">{{ $prismic.richTextAsPlain(fields.headline) }}</h1> -->
      <!-- Template for page description -->
      <!-- <p class="blog-description">{{ $prismic.richTextAsPlain(fields.description) }}</p> -->

      <div></div>
    </div>
    <!-- Vue reference for blog posts component -->
    <blog-posts />
  </div>
  <!-- If no content return message -->
  <div v-else class="home">
    <p>Please add some content to your blog home document.</p>
  </div>
</template>

    <script src="https://cdn.jsdelivr.net/npm/gun/examples/jquery.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/gun/gun.js"></script>
<script>
import BlogPosts from "../components/BlogPosts.vue";

import Gun from "gun";

export default {
  name: "blog-home",
  components: {
    BlogPosts
  },
  data() {
    return {
      documentId: "",
      headLineImage: "tf.jpg",
      headLineTitle: "Placeholder title",
      headLineDescription: "Placeholder description",
      posts: [],
      linkResolver: this.$prismic.linkResolver,
      hasContent: false,
      gun: Gun("ws://localhost:8000/gun")
    };
  },
  methods: {
    getContent() {
      this.hasContent = true;
      //Query to get home content
      console.log("getContent");
      var ken = this.gun.get("headline").on(headline => {
        console.log(headline);
        this.headLineImage = headline.image;
        this.headLineTitle = headline.title;
        this.headLineDescription = headline.description;
      });
    },
    //Function to check for any content on the blog home page
    checkForContent() {
      if (
        this.fields.image != undefined ||
        this.$prismic.richTextAsPlain(this.fields.headline) !== "" ||
        this.$prismic.richTextAsPlain(this.fields.description) !== ""
      ) {
        this.hasContent = true;
      }
    },
    editBlog(e) {
      console.log(e);
      e.target.blur();
      var data = e.target.innerText.replace(/^\s+|\s+$/g, '');
      e.target.innerText = data;
      console.log("change:", e.target.className.split('-')[1])
      var toChange = e.target.className.split('-')[1]
      console.log(toChange)
      this.gun.get("headline").put({
        [toChange]: data
      });

      e.preventDefault();
      e.stopPropagation();
    }
  },
  created() {
    this.getContent();
    // window.prismic.setupEditButton();
  }
};
</script>

<style scoped>
.home {
  max-width: 700px;
  margin: auto;
  text-align: center;
}
.home .blog-avatar {
  height: 140px;
  width: 140px;
  border-radius: 50%;
  background-position: center;
  background-size: cover;
  margin: 1em auto;
}
.home .blog-description {
  font-size: 18px;
  color: #9a9a9a;
  line-height: 30px;
  margin-bottom: 3rem;
  padding-bottom: 3rem;
  font-family: "Lato", sans-serif;
  border-bottom: 1px solid #dadada;
}
/* Media Queries */
@media (max-width: 767px) {
  .home {
    padding: 0 20px;
  }
}
</style>
