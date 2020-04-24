<template>
  <div class="container">
    <div class="header">
      <h2>Aly Yorio</h2>
      <div class="nav">
        <h2 style="color: #ea3d25;">Work</h2>
        <h2>Contact</h2>
      </div>
    </div>
    <div class="main">
      <div
        v-for="company in companyList"
        :key="company.title"
        class="panel"
        :style="company.style"
      >
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
  </div>
</template>

<script>
export default {
  components: {},

  async asyncData({ params }) {
    try {
      const order = [
        'stubhub',
        'stubhub_2',
        'motion_reel',
        'thumbtack',
        'bluewolf',
        'grcd'
      ]

      const companyList = []
      for (let i = 0; i < order.length; i++) {
        const company = await import(`~/content/${order[i]}.md`)
        companyList.push({
          title: company.attributes.title,
          subtitle: company.attributes.subtitle,
          mediaType: company.attributes.media_type,
          slides: company.attributes.slides,
          description: company.html,
          style: {
            backgroundColor: `#${company.attributes.background_color}`
          }
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
body {
  font-family: 'AntarcticanMono-Bold';
}

.header {
  width: 100%;
  max-width: 1200px;
  padding: 48px 0 24px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.panel {
  width: 100%;
  padding: 64px 0;
}

.container {
  background-color: #f4f4f4;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.main {
  width: 100%;
}

.main .panel:first-child {
  padding-top: 0px;
}

.media {
  width: 100%;
  margin: 0 auto;
  max-width: 1200px;
}

.content {
  text-align: left;
  word-wrap: none;
  display: flex;
  flex-direction: row;
  max-width: 1200px;
  margin: 0 auto;
}

.company {
  display: flex;
  flex-direction: column;
  padding: 0 0 64px;
  width: 100%;
}

.panel .content .title h2 {
  color: #ea3d25;
  font-size: 30px;
}

.panel .content .title h3 {
  font-size: 30px;
  line-height: 37px;
}

.VueCarousel-dot-container {
  margin-top: 0 !important;
}

.VueCarousel-dot-container button {
  margin-top: 0 !important;
}

.VueCarousel-slide {
  flex-shrink: 0 !important;
}

.nav {
  display: flex;
  flex-direction: row;
}

.header .nav h2 {
  margin-left: 12px;
}

.description p {
  margin-bottom: 16px;
}

.image {
  max-width: 1200px;
}

.title {
  padding-right: 12px;
  width: 33%;
  display: flex;
  flex-direction: column;
}

.description {
  width: 66%;
}

.window {
  margin: 0 140px;
  max-width: 1380px;
  min-height: 100vh;
}

.media {
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
