<template>
  <div 
    class="container"
    style="background-color: rgb(248, 245, 245);"
  >
    <header>
      <h2>
        <strong class="numeral">1</strong>
        Enter podcast details.
      </h2>
    </header>

    <form>
      <InputText
        label="Root Path to Media Files"
        required
        v-model="path"
        @input="update()"
      />

      <InputText
        label="Directory for Media Files"
        required
        v-model="directory"
        @input="update()"
      >
        <p
          slot="afterField"
          class="note"
        >
          Full path to media files: 
          <code>{{ fullPath }}</code>
        </p>
      </InputText>

      <InputText
        label="Feed Filename"
        required
        v-model="rssFilename"
        @input="update()"
      />

      <InputText
        label="Podcast Title"
        required
        v-model="title"
        @input="update()"
      />

      <InputText
        label="Podcast Author"
        required
        v-model="author"
        @input="update()"
      />

      <InputText
        label="Podcast Image Filename"
        required
        v-model="image"
        @input="update()"
      />

      <InputCategory
        v-model="rawCategory"
        @input="update()"
      />

      <InputText
        label="Media Filenames (one per line)"
        required
        rows="12"
        type="textarea"
        v-model="rawEpisodes"
        @input="update()"
      >
        <template slot="afterField">
          <code>
            {{ episodes.length }}
            episode{{ episodes.length === 1 ? '' : 's' }}
          </code>
          <p class="note">
            List files in listening order.
            Most podcast apps play newest episodes first,
            so episode dates will be applied with the first 
            listed episode marked as published today, the 
            second yesterday, etc.
          </p>
        </template>
      </InputText>

      <InputText
        label="Enclosure Type"
        required
        v-model="enclosureType"
        @input="update()"
      />

      <label>
        <input
          type="checkbox"
          style="
            display: inline;
            margin-right: .25rem;
            width: auto;
          "
          v-model="isExplicit"
          @change="update()"
        />
        Explicit Content
      </label>
    </form>
  </div>
</template>

<script>
import InputCategory from './InputCategory.vue'
import InputText from './InputText.vue'

export default {
  components: {
    InputCategory,
    InputText
  },
  mounted() {
    this.update()
  },
  data() {
    return {
      author: 'Jared Diamond',
      directory: 'Guns, Germs, and Steel',
      enclosureType: 'audio/mpeg',
      image: 'guns-germs-and-steel-2.jpg',
      isExplicit: false,
      path: 'https://tinymachine.s3.amazonaws.com/media/',
      rawCategory: 'Society & Culture|History',
      rawEpisodes: 
`01 Guns, Germs and Steel - Part 01.mp3
02 Guns, Germs and Steel - Part 02.mp3
03 Guns, Germs and Steel - Part 03.mp3
04 Guns, Germs and Steel - Part 04.mp3
05 Guns, Germs and Steel - Part 05.mp3
06 Guns, Germs and Steel - Part 06.mp3
07 Guns, Germs and Steel - Part 07.mp3
08 Guns, Germs and Steel - Part 08.mp3
09 Guns, Germs and Steel - Part 09.mp3
10 Guns, Germs and Steel - Part 10.mp3
11 Guns, Germs and Steel - Part 11.mp3
12 Guns, Germs and Steel - Part 12.mp3
13 Guns, Germs and Steel - Part 13.mp3`,
      rssFilename: 'rss.xml',
      title: 'Guns, Germs, and Steel'
    }
  },
  computed: {
    category() {
      return this.rawCategory.split('|')[0]
    },
    episodes() {
      return this.rawEpisodes
        .split(/\n/)
        .filter(line => (/\S/).test(line))
    },
    fullPath() {
      return encodeURI(this.path.replace(/\/+$/, '')) + '/' +
        encodeURIComponent(this.directory) + '/'
    },
    requiredFieldsFilled() {
      return (
        this.path &&
        this.rssFilename &&
        this.title &&
        this.author &&
        this.enclosureType &&
        this.episodes.length
      )
    },
    podcast() {
      // This object gets emitted back to parent
      // when data in the form changes.
      return {
        author: this.author,
        category: this.category,
        enclosureType: this.enclosureType,
        episodes: this.episodes,
        fullPath: this.fullPath,
        image: this.image,
        isExplicit: this.isExplicit,
        requiredFieldsFilled: this.requiredFieldsFilled,
        rssFilename: this.rssFilename,
        subcategory: this.subcategory,
        title: this.title,
      }
    },
    subcategory() {
      return this.rawCategory.split('|')[1]
    }
  },
  methods: {
    update() {
      this.$nextTick(function () {
        this.$emit('update', this.podcast)
      })
    }
  }
}
</script>