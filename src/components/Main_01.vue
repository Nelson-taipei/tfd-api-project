<template>
  <div>
    <ul>
      <li v-for="(item, index) in result.data[3]" :key="index">{{index}} {{item}} </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
// import Koa from 'koa'
// import cors from 'koa-cors'

export default {
  data () {
    return {
      result: []
    }
  },
  mounted () {
     // const url = 'http://crossorigin.me/http://167.99.171.123:8080/api/report?dateFrom=2020-05-01&dateTo=2020-05-30&battery1From=5&battery1To=100'
    // const url = 'https://yacdn.org/proxy/http://167.99.171.123:8080/api/report?dateFrom=2020-05-01&dateTo=2020-05-10&battery1From=5&battery1To=100'
    const url = 'https://api.allorigins.win/raw?url=http://167.99.171.123:8080/api/report'
    axios.get(url).then(response => {
      const result = response.data
      this.result = result
      // console.log(result)
    })
  }
}
</script>

<style scoped>

</style>
