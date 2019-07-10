<template>
  <!-- Vue conditional to check if there is any content in document -->
  <div v-if="hasContent" class="page">
    <div class="home">
      <!-- Button to edit document in dashboard -->
      <!-- <prismic-edit-button :documentId="documentId" /> -->
      <div class="blog-avatar" :style="{ backgroundImage: 'url(/' + headLineImage + ')' }"></div>
      <input class="blog-title" @keyup.enter="editBlog" v-model="headLineTitle">
      <br>
      <textarea class="blog-description" @keyup.enter="editBlog" v-model="headLineDescription"></textarea>

      <!-- <div class="blog-avatar" :style="{ backgroundImage: 'url(' + fields.image + ')' }"></div> -->
      <!-- Template for page title -->
      <!-- <h1 class="blog-title">{{ $prismic.richTextAsPlain(fields.headline) }}</h1> -->
      <!-- Template for page description -->
      <!-- <p class="blog-description">{{ $prismic.richTextAsPlain(fields.description) }}</p> -->

      <!-- <div></div> -->
    </div>
    <!-- Vue reference for blog posts component -->
    <blog-posts />
  </div>
  <!-- If no content return message -->
  <div v-else class="home">
    <p>Please add some content to your blog home document.</p>
  </div>
</template>
<script>
import BlogPosts from "../components/BlogPosts.vue";


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
      hasContent: false,
    };
  },
  methods: {
    getContent() {
      this.hasContent = true;
      //Query to get home content
      window.gun.get("headline").on(headline => {
        // this.headLineImage = headline.image;
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
      if(e.shiftKey) return;
      console.log(e)
      e.target.blur();
      var data = e.target.value.replace(/^\s+|\s+$/g, '');
      e.target.innerText = data;
      var toChange = e.target.className.split('-')[1]
      console.log(toChange)

      window.gun.get("headline").get(toChange).put(data)
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
.home, .blog-title, .blog-description {
  max-width: 700px;
  margin: auto;
  text-align: center;
  width: 100%;
}
.blog-title {

  font-size: 36px;
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
  width: 100%;
  height: 100%;
  overflow: hidden;
}

/* Media Queries */
@media (max-width: 767px) {
  .home {
    padding: 0 20px;
  }
}
</style>
