<template>
  <Layout>
    <div class="post-title">
      <h1 class="post-title__text">{{ $page.folder.title }}</h1>
    </div>

    <div class="post content-box">
      <div class="post__header">
        <g-image alt="Cover image" v-if="$page.folder.cover_image" :src="$page.folder.cover_image" />
      </div>

      <div class="post__content" v-html="$page.folder.content" />

      <div class="post__footer"></div>
    </div>

    <div v-for="edge in $page.folder.belongsTo.edges" :key="edge.node.id" :post="edge.node">{{edge.node.title}}</div>

    <div class="post-comments">
      <!-- Add comment widgets here -->
    </div>
  </Layout>
</template>

<script>
export default {
  components: {},
  metaInfo() {
    return {
      title: this.$page.folder.title,
      meta: [
        {
          name: "description",
          content: this.$page.folder.description
        }
      ]
    };
  }
};
</script>

<page-query>
query Folder ($id: ID!) {
  folder: folder (id: $id) {
    title
    path
    date (format: "D. MMMM YYYY")
    timeToRead

    description
    content
    cover_image (width: 860, blur: 10)
    belongsTo {
      edges {
        node {
          ...on Post {
            title
            path
            date (format: "D. MMMM YYYY")
            timeToRead
            description
            content
          }
        }
      }
    }
  }
}
</page-query>

<style lang="scss">
.post-title {
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}

.post {
  &__header {
    width: calc(100% + var(--space) * 2);
    margin-left: calc(var(--space) * -1);
    margin-top: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    img {
      width: 100%;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    h2:first-child {
      margin-top: 0;
    }

    p:first-of-type {
      font-size: 1.2em;
      color: var(--title-color);
    }

    img {
      width: calc(100% + var(--space) * 2);
      margin-left: calc(var(--space) * -1);
      display: block;
      max-width: none;
    }
  }
}

.post-comments {
  padding: calc(var(--space) / 2);

  &:empty {
    display: none;
  }
}

.post-author {
  margin-top: calc(var(--space) / 2);
}
</style>
