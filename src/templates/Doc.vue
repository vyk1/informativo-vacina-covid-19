<template>
  <Layout>
    <h1>
      {{ $page.doc.title }}
    </h1>
     <div class="markdown" v-html="$page.doc.content" />
     <small>Última atualização: <i v-html="$page.doc.date" /></small>
  </Layout>
</template>

<static-query>
query {
  metadata {
    siteName,
    siteUrl
  }
}
</static-query>

<page-query>
query Doc ($path: String!) {
  doc: doc (path: $path) {
    title
    path
    image,
    date (format: "D  MMMM YYYY", locale: "pt-BR")
    timeToRead
    content
  }
}
</page-query>

<script>
export default {
  metaInfo() {
    const imagePath = `${this.$static.metadata.siteUrl}`
    return {
      title: this.$page.doc.title,
      meta: [
        { key: 'description', name: 'description', content: this.$page.doc.title },
        {
          key: 'og:image',
          property: 'og:image',
          content: `${imagePath}${this.$page.doc.image}`
        }, 
      ]
    }
  }
}
</script>


<style lang="scss" scoped>
/deep/ > p {
  opacity: .8;
}

/deep/ > h2 {
  padding-top: 100px;
  margin-top: -80px;

  @include respond-above(md) {
    font-size: 2rem;
  }
}

/deep/ > p > img {
    max-width: 100%;
  }

.markdown {
  padding-bottom: 50vh;
}
</style>
