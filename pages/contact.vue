<template>
  <div class="container">
    <div class="header">
      <a href="/">
        <h2>
          <span class="first-name">ALY</span>
          <span class="last-name">YORIO</span>
        </h2>
      </a>
      <div class="nav">
        <a class="work" :class="{ active: $nuxt.$route.path === '/' }" href="/">
          <h2>WORK</h2>
        </a>
        <a
          class="contact"
          :class="{ active: $nuxt.$route.path === '/contact' }"
          href="/contact"
        >
          <h2>CONTACT</h2>
        </a>
      </div>
    </div>
    <div class="main">
      Contact
    </div>
  </div>
</template>

<script>
export default {
  components: {},

  async asyncData({ params }) {
    try {
      const order = ['contact']

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

.nav a:not(:hover) {
  text-decoration: none;
}

.nav a.active,
.nav a:visited.active {
  color: #ea3d25;
}

.nav a,
.nav a:visited {
  color: black;
}

.header .last-name {
  color: #ea3d25;
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
