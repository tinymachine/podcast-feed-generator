<template>
  <div
    class="container"
    style="background-color: rgb(238, 235, 235);"
  >
    <header>
      <h2>
        <strong class="numeral">2</strong>
        Preview &amp; grab the feed.
      </h2>
    </header>

    <main>
      <section style="margin-bottom: 1rem;">
        <img
          v-if="podcast.image"
          :src="podcast.fullPath + podcast.image"
          class="cover-art"
          alt=""
        >
        <div v-else class="tc">
          [No image]
        </div>

        <h3>{{ podcast.title || '[Missing Title]' }}</h3>
        <h4>{{ podcast.author || '[Missing Author]' }}</h4>
      </section>

      <section v-if="podcast.requiredFieldsFilled">
        <label>
          Feed (RSS 2.0):

          <p class="note">
            Copy the markup below into a file named
            <code><strong>{{ podcast.rssFilename }}</strong></code>
            and upload it to the following path: 
            <code>{{ podcast.fullPath }}</code>
          </p>

          <textarea
            readonly
            rows="12"
            style="
              color: rgb(110, 100, 100);
              white-space: pre;
              word-wrap: normal;
            "
            @click="copyToClipboard($event)"
          >{{ xmlMarkup }}</textarea>
        </label>

        <label>
          Feed URL:
          <textarea
            readonly
            rows="1"
            style="white-space: nowrap;"
            @click="copyToClipboard($event)"
            v-html="podcast.fullPath + podcast.rssFilename"
          ></textarea>
        </label>

        <transition name="fade">
          <p
            v-if="showClipboardAlert"
            class="clipboard-alert"
          >Copied to clipboard</p>
        </transition>
      </section>

      <section v-if="!podcast.requiredFieldsFilled">
        <p class="error tc">
          <strong>Please fill all required fields.</strong>
        </p>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  props: {
    podcast: { type: Object, required: true }
  },
  data() {
    return {
      showClipboardAlert: false
    }
  },
  computed: {
    categoryMarkup() {
      return this.podcast.subcategory
        ? 
    `<itunes:category text="${this.escapeAmpersands(this.podcast.category)}">
      <itunes:category text="${this.escapeAmpersands(this.podcast.subcategory)}"/>
    </itunes:category>`
        : `<itunes:category text="${this.escapeAmpersands(this.podcast.category)}"/>`
    },
    imageMarkup() {
      return this.podcast.image
        ? `<itunes:image href="${this.podcast.fullPath + this.podcast.image}"/>`
        : ''
    },
    itemsMarkup() {
      return this.podcast.episodes.map((episode, index) => {
        const fullUri = this.podcast.fullPath + encodeURIComponent(episode)
        return `
    <item>
      <title>Part ${index + 1}</title>
      <itunes:author>${this.podcast.author}</itunes:author>
      ${this.imageMarkup}
      <enclosure type="${this.podcast.enclosureType}" url="${fullUri}"/>
      <guid>${fullUri}</guid>
      <pubDate>${this.getPubDate(index)}</pubDate>
    </item>`     
      }).join('\n')
    },
    xmlMarkup() {
      return `<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd">
  <channel>
    <title>${this.podcast.title}</title>
    <language>en-us</language>
    <itunes:author>${this.podcast.author}</itunes:author>
    ${this.imageMarkup}
    ${this.categoryMarkup}
    <itunes:explicit>${this.podcast.isExplicit ? 'yes' : 'no'}</itunes:explicit>
    ${this.itemsMarkup}
  </channel>
</rss>`
    }
  },
  methods: {
    copyToClipboard(e) {
      e.target.select()
      document.execCommand('copy') &&
        this.displayCopyAlert()
    },
    displayCopyAlert() {
      this.showClipboardAlert = true
      setTimeout(() => {
        this.showClipboardAlert = false
      }, 1500)
    },
    escapeAmpersands(str) {
      return str.replace(/&/, '&amp;')
    },
    getPubDate(index) {
      return new Date(
        Date.now() - (index * (24 * 60 * 60 * 1000))
      ).toUTCString()
    }
  }
}
</script>

<style scoped>
.clipboard-alert {
  position: absolute;
  bottom: 1rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  padding: .5rem 1rem;
  text-align: center;
  color: white;
  font-size: .875rem;
  font-weight: bold;
  background: rgba(0, 0, 0, .8);
  border-radius: .5rem;
  box-shadow: 0 .5rem 2rem rgba(0, 0, 0, .5);
}
</style>