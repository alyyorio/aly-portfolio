<template>
  <div class="container">
    <div v-for="company in companyList" :key="company.title" class="company">
      <div class="media">
        <no-ssr>
          <carousel :per-page="1" class="carousel">
            <slide v-for="url in company.slides" :key="url">
              <img :src="url" class="image" />
            </slide>
          </carousel>
        </no-ssr>
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

.image {
  max-height: 600px;
  max-width: 1200px;
}

.content {
  text-align: left;
  word-wrap: none;
  display: flex;
  flex-direction: row;
}

.title {
  width: 33%;
  display: flex;
  flex-direction: column;
}

.description {
  width: 66%;
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

.description {
  column-count: 2;
}
</style>
