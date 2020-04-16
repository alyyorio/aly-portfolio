<template>
  <div class="container">
    <div v-for="company in companyList" :key="company.title" class="company">
      <div class="media">
        <h1>media</h1>
      </div>
      <div class="content">
        <div class="title">
          <h2>{{ company.title }}</h2>
          <h3>{{ company.subtitle }}</h3>
        </div>
        <div class="description">
          <span v-html="company.description"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},

  async asyncData({ params }) {
    try {
      const order = ['stubhub', 'thumbtack']
      const companyList = []
      for (let i = 0; i < order.length; i++) {
        const company = await import(`~/content/${order[i]}.md`)
        companyList.push({
          title: company.attributes.title,
          subtitle: company.attributes.subtitle,
          media_type: company.attributes.media_type,
          slides: company.attributes.slides,
          description: company.html
        })
      }

      return {
        companyList
      }
    } catch (err) {
      return false
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  max-width: 1200px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.content {
  display: flex;
  flex-direction: row;
}

.title {
  display: flex;
  flex-direction: column;
}

.company {
  display: flex;
  flex-direction: column;
  margin: 64px 0 64px;
  width: 100%;
}

.media {
  background: pink;
  height: 600px;
  width: 100%;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
