<template>
  <div class="container">
    <div class="svg-1-right">
      <img src="images/orange-blob-right-1.svg" />
    </div>
    <div class="svg-1-left">
      <img src="images/orange-blob-left-1.svg" />
    </div>
    <div class="svg-2-right">
      <img src="images/orange-blob-right-2.svg" />
    </div>
    <div class="svg-2-left">
      <img src="images/orange-blob-left-2.svg" />
    </div>
    <div class="header">
      <h2>
        <span class="first-name">ALY</span>
        <span class="last-name">YORIO</span>
      </h2>
      <div class="nav">
        <a @click="scrollToFooter">
          <h2>CONTACT</h2>
        </a>
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
          <div v-if="company.mediaType === 'slideshow'" class="slideshow">
            <no-ssr>
              <carousel
                :navigation-enabled="true"
                :pagination-enabled="false"
                navigation-next-label=">"
                navigation-prev-label="<"
                :per-page="1"
                class="carousel"
              >
                <slide v-for="url in company.slides" :key="url">
                  <img :src="`images/${url}`" class="image" />
                </slide>
              </carousel>
            </no-ssr>
          </div>
          <div v-else-if="company.mediaType === 'video'" class="video">
            <iframe
              :src="company.video"
              width="1200"
              height="675"
              frameborder="0"
              allow="autoplay; fullscreen"
              allowfullscreen
            >
            </iframe>
          </div>
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
    <div class="panel footer" :style="footer.style">
      <div class="content">
        <div class="title">
          <h3 class="resume">
            <a :href="footer.resume_url">
              RESUME
              <svg
                xmlns="http://www.w3.org/2000/svg"
                height="24"
                viewBox="0 0 24 24"
                width="24"
              >
                <path d="M0 0h24v24H0z" fill="none" />
                <path
                  d="M19 19H5V5h7V3H5c-1.11 0-2 .9-2 2v14c0 1.1.89 2 2 2h14c1.1 0 2-.9 2-2v-7h-2v7zM14 3v2h3.59l-9.83 9.83 1.41 1.41L19 6.41V10h2V3h-7z"
                />
              </svg>
            </a>
          </h3>
          <h3 class="email">{{ footer.email }}</h3>
          <h3 class="phone">{{ footer.phone }}</h3>
        </div>
        <div class="description">
          <span v-html="footer.description"></span>
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
      const footerFile = await import(`~/content/footer.md`)
      const orderFile = await import(`~/content/order.md`)
      const order = orderFile.attributes.order
      const companyList = []
      for (let i = 0; i < order.length; i++) {
        const company = await import(`~/content/${order[i]}.md`)
        companyList.push({
          title: company.attributes.title,
          subtitle: company.attributes.subtitle,
          mediaType: company.attributes.media_type,
          slides: company.attributes.slides,
          video: company.attributes.video,
          description: company.html,
          style: {
            backgroundColor: `#${company.attributes.background_color}`
          }
        })
      }

      const footer = {
        name: footerFile.attributes.name,
        email: footerFile.attributes.email,
        phone: footerFile.attributes.phone,
        resume_url: footerFile.attributes.resume_url,
        description: footerFile.html,
        style: {
          backgroundColor: `#${footerFile.attributes.background_color}`
        }
      }

      return {
        companyList,
        footer
      }
    } catch (err) {
      return false
    }
  },

  methods: {
    scrollToFooter() {
      document.querySelector('.footer').scrollIntoView({ behavior: 'smooth' })
    }
  }
}
</script>
