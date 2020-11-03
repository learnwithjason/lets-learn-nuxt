<template>
  <div class="container">
    <div>
      <h1 class="title">
        Hi Chat!
      </h1>

      <!-- TODO: loop through images -->
      <div class="image-container">
        <img
          v-for="image in cloudinaryImages"
          :key="image"
          :src="image"
          alt="sticker image"
        />
      </div>

      <form @submit.prevent="saveSticker">
        <input
          class="input"
          type="text"
          id="stickerId"
          name="stickerId"
          v-model="input"
        />
        <button class="button" type="submit">Add</button>
      </form>
    </div>


    <div class="rick-container">
      <img :src="rick" alt="Rick Sanchez" />
      <img :src="marisa" alt="Marisa Morby" />
    </div>

  </div>
</template>

<script>
export default {
  data: () => {
    return {
      input: '',
    }
  },
  computed: {
    cloudinaryImages() {
      return this.images.map((image) => {
        return this.$cloudinary.image.url(image)
      })
    }
  },
  methods: {
    saveSticker() {
      if (!!this.input) {
        this.images.push(this.input)
        this.input = ''
      }
    }
  },
  async asyncData(context) {
    const rick = await fetch('https://rickandmortyapi.com/api/character/1')
      .then((res) => res.json());
    return {
      rick: context.$cloudinary.image.fetchRemote(rick.image, {
        effect: 'grayscale',
        gravity: 'face',
        crop: 'thumb',
        width: 200,
        height: 200,
        radius: 'max'
      }),
      marisa: context.$cloudinary.image.url('jason.af/marisa-flowers-fb600be34f1e139a9feab1a85149d04d', {
        effect: 'grayscale',
        gravity: 'face',
        crop: 'thumb',
        width: 200,
        height: 200,
        radius: 'max'
      }),
      images: [
        'lwj/store/rubber-corgi',
        'lwj/store/corgi-pal',
        'lwj/store/boop',
      ]
    }
  }
}
</script>

<style scoped>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.image-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.image-container img {
  max-width: 100%;
}

.input {
  border: 1px solid black;
  padding: 0.25rem 1rem;
}

.button {
  background: black;
  color: white;
  padding: 0.25rem 1rem;
}

.rick-container {
  display: block;
  padding: 4rem;
  text-align: center;
  width: 100%;
}
</style>
