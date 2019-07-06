<template>
  <div id="wrapper">
    <form @submit.prevent="open">
      <label>
        Query:
        <input type="text" v-model="query"/>
      </label>
      <button type="submit" :disabled="loading">{{loading ? 'Loading...' : 'Submit'}}</button>
    </form>

    <ul>
      <li v-for="result in results">{{result.q}} - {{result.totalResults}}</li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios'
  import SystemInformation from './LandingPage/SystemInformation'

  export default {
    name: 'landing-page',
    components: { SystemInformation },
    data () {
      return {
        query: '',
        results: [],
        loading: false
      }
    },
    methods: {
      async open () {
        this.loading = true

        let self = this
        const key = 'AIzaSyDgj773L2BAQC19M-NBxasNPxcw3EKq99s'
        const cx = '016028619539290569725:x1wbb_awkik'

        await axios.get(`https://www.googleapis.com/customsearch/v1?key=${key}&cx=${cx}&q=${this.query}&num=1`)
          .then(function (response) {
            self.results.push({totalResults: response.data.searchInformation.totalResults, q: self.query})
            self.loading = false
          })
          .catch(function (error) {
            console.log(error)
            self.loading = false
          })
      }
    }
  }
</script>
