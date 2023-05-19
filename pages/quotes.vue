<template>
  <div>
    <ckNavBar/>
    <div class="container">
        <h2 class="center" v-if="$fetchState.error">An error occurred</h2>
        <div v-else>
            <h2 class="otro-anime">{{ quote.anime }}</h2>
            <blockquote class="otro-blockquote">"{{ quote.quote }}"
                <span>{{ quote.character }}</span>
            </blockquote>
            <img class="otro-blockquote otro-image" :src="this.character" alt="alternatetext">
            <button class="refresh-button" @click="$fetch">Refresh</button>
        </div>
    </div>

  </div>
</template>

<script>
import ckNavBar from '../components/ckNavBar.vue'

export default {
    data() {
        return{
            quote: {},
            character: {}
        }
    },
  components: { ckNavBar },
  name: 'quotesPage',

  async fetch() {
    this.quote = await fetch(`https://animechan.vercel.app/api/random`)
        .then(res => res.json())

    let character = await fetch(`https://api.jikan.moe/v4/characters?q=${this.quote.character}`)
        .then(res => res.json())

    this.character = character?.data[0]?.images?.jpg?.image_url
  }
}
</script>

<style scoped>
.otro-blockquote{
  width:40%;
  margin:25px auto;
  font-style:italic;
  color: #555555;
  padding:1.2em 30px 1.2em 75px;
  border-left:8px solid #78C0A8 ;
  line-height:1.6;
  position: relative;
  background:#EDEDED;
  text-align: justify;
}

.otro-blockquote::before{
  font-family:Arial;
  content: "\201C";
  color:#78C0A8;
  font-size:4em;
  position: absolute;
  left: 10px;
  top:-10px;
}

.otro-blockquote::after{
  content: '';
}

.otro-blockquote span{
  display:block;
  color:#333333;
  font-style: normal;
  font-weight: bold;
  margin-top:1em;
}

.otro-anime {
  text-align: center;
}

.otro-image {
    margin:25px auto;
}

.container {
  position: relative;
  height: 93vh;
  width: 100vw;
}

.refresh-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
}
</style>