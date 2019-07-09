<template>
  <!-- Check blog posts exist -->
  <div v-if="posts.length !== 0" class="blog-main">
    <!-- Template for blog posts -->
    <div v-for="post in posts" :key="post.id" v-bind:post="post" class="blog-post">
      <a :href="'/blog/'+post.id">
        <!-- <h2>{{ $prismic.richTextAsPlain(post.data.title) }}</h2> -->
        <h2 class="blog-post-title">{{ post.title }}</h2>
        <!-- <p class="blog-post-meta"><span class="created-at">{{ Intl.DateTimeFormat('en-US', this.dateOptions).format(new Date(post.date)) }}</span></p> -->
        <div>
          <!-- <p>{{getFirstParagraph(post)}}</p> -->
          <p class="blog-post-body">{{ post.body }}</p>
        </div>
      </a>
    </div>
    <h2 id="newTitle" class="blog-post-title-new" contenteditable>{{newPost.title }}</h2>
    <p id="newBody" class="blog-post-body-new" contenteditable>{{newPost.body }}</p>

    <v-container>
      <v-flex>
        <v-btn v-on:click="savePost" class="text-xs-right" depressed>Save post</v-btn>
        <v-btn v-on:click="showMoreOptions" class="text-xs-right" depressed>More options</v-btn>
      </v-flex>
    </v-container>
  </div>
  <!-- If no blog posts return message -->
  <div v-else class="blog-main">
    <v-container>
      <v-flex>
        <p>No Posts published at this time.</p>
        <h2 id="newTitle" class="blog-post-title-new" contenteditable>{{newPost.title }}</h2>
        <p id="newBody" class="blog-post-body-new" contenteditable>{{newPost.body }}</p>
      </v-flex>
      <v-flex>
        <v-btn v-on:click="savePost" class="text-xs-right" depressed>Save post</v-btn>
        <v-btn v-on:click="showMoreOptions" class="text-xs-right" depressed>More options</v-btn>
      </v-flex>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "blog-posts",
  data() {
    return {
      posts: [],
      dateOptions: { year: "numeric", month: "short", day: "2-digit" },
      // linkResolver: this.$prismic.linkResolver,
      newPost: {
        id: null,
        title: "Title",
        body: "Body"
      },
    };
  },
  methods: {
    goToPost(id) {
      console.log(id);
    },
    getPosts() {
      console.log("gettings posts")
      window.gun
        .get("posts")
        .map()
        .on(post => {
          console.log(post)
          window.gun.get(post).on(post => console.log(post, "wooooooooo"))
          if (post != null) {
            console.log(post.id, post.title, post.description);
            this.posts.push(post);
          } else {
            console.log("found an orphant");
          }
        });
    },
    //Function to get the first paragraph of text in a blog post and limit the displayed text at 300 characters
    getFirstParagraph(post) {
      const textLimit = 300;
      const slices = post.data.body;
      let firstParagraph = "";
      let haveFirstParagraph = false;

      slices.map(function(slice) {
        if (!haveFirstParagraph) {
          if (slice.slice_type == "text") {
            slice.primary.text.forEach(function(block) {
              if (block.type == "paragraph") {
                if (!haveFirstParagraph) {
                  firstParagraph += block.text;
                  haveFirstParagraph = true;
                }
              }
            });
          }
        }
      });

      const limitedText = firstParagraph.substr(0, textLimit);

      if (firstParagraph.length > textLimit) {
        return limitedText.substr(0, limitedText.lastIndexOf(" ")) + "...";
      } else {
        return firstParagraph;
      }
    },
    savePost() {
      var title = document.getElementById("newTitle").innerText;
      var body = document.getElementById("newBody").innerText;
      this.newPost.title = title;
      this.newPost.body = body;
      this.newPost.id = this.posts[this.posts.length -1].id +1;
      window.gun.get("posts").set(this.newPost);
    },
    showMoreOptions() {
      // var title = document.getElementById("newTitle").innerText;
      // var body = document.getElementById("newBody").innerText;
      // this.newPost.title = title;
      // this.newPost.body = body;
      // this.newPost.id = this.posts.length;
      // this.gun.get("posts").set(this.newPost);
    }
  },
  created() {
    this.getPosts();
  }
};
</script>

<style scoped>
.blog-main {
  max-width: 700px;
  margin: auto;
}
.blog-post {
  margin-bottom: 3rem;
}
.blog-post h2 {
  margin: 0;
}
.blog-post-meta {
  color: #9a9a9a;
  font-family: "Lato", sans-serif;
  margin-bottom: 8px;
  font-size: 16px;
}

.blog-post-title {
  color: rgb(85, 85, 85);
}

.blog-post-body {
  color: rgb(85, 85, 85);
}

.blog-post-title-new {
  color: rgb(85, 85, 85);
  opacity: 0.5;
}

.blog-post-body-new {
  color: rgb(85, 85, 85);
  opacity: 0.5;
}

.v-btn {
  right: 0px;
  top: 0px;
}

/* Media Queries */
@media (max-width: 767px) {
  .blog-main {
    padding: 0 20px;
    font-size: 18px;
  }
}
</style>