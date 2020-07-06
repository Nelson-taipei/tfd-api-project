<template>
  <div>
    <div class="topZone">
      <div class="leftZone"><input class="myInput" placeholder="Input ID" v-model="searchValue" @keydown.enter="mySearch(searchValue)"/><div class="myBtn" @click="mySearch(searchValue)">Search</div></div>
      <div class="rightZone">
        By Date Sort: <div class="myBtn" @click="dateSort">{{btnContent}}</div>
      </div>
    </div>
    <div class="myWrap">
      <div class="titleRow">
        <div class="myNo">No.</div>
        <div>ID</div>
        <div>GPS</div>
        <div>BATTERY</div>
        <div>MOTION</div>
        <div>DATE</div>
        <div>DATE UPLOADED</div>
      </div>
      <div v-show="displayFlag" class="contentRow" v-for="(item, index) in myResult" :key="index" :class="[index % 2 == 0 ? 'grayBg': 'yellowBg']">
        <div class="myNo">{{index * 1 + 1}}</div>
        <div>{{item.id}}</div>
        <div>
          <div>lat: {{item.gps.lat}}</div>
          <div>long: {{item.gps.long}}</div>
        </div>
        <div>
          <div>battery1: {{item.battery.battery1}}</div>
          <div>battery2: {{item.battery.battery2}}</div>
        </div>
        <div>
          <div>x: {{item.motion.x}}</div>
          <div>y: {{item.motion.y}}</div>
          <div>z: {{item.motion.z}}</div>
        </div>
        <div>{{item.date}}</div>
        <div>{{item.date_uploaded}}</div>
      </div>
      <div class="noData" v-show="!displayFlag">No information found</div>
    </div>
    <div class="myWrap_mobile">
      <div v-show="displayFlag" class="contentRow_mobile" v-for="(item, index) in myResult" :key="index" :class="[index % 2 == 0 ? 'grayBg': 'yellowBg']" >
        <div class="col_10">{{index * 1 + 1}}</div>
        <div class="col_90">
          <div>
            <div>
              <div>ID</div>
              <div>{{item.id}}</div>
            </div>
            <div>
              <div>GPS</div>
              <div>lat: {{item.gps.lat}}</div>
              <div>long: {{item.gps.long}}</div>
            </div>
          </div>
          <div>
            <div>
              <div>BATTERY</div>
              <div>battery1: {{item.battery.battery1}}</div>
              <div>battery2: {{item.battery.battery2}}</div>
            </div>
            <div>
              <div>MOTION</div>
              <div>x: {{item.motion.x}}</div>
              <div>y: {{item.motion.y}}</div>
              <div>z: {{item.motion.z}}</div>
            </div>
          </div>
          <div>
            <div>
              <div>DATE</div>
              <div>{{item.date}}</div>
            </div>
            <div>
              <div>DATE UPLOADED</div>
              <div>{{item.date_uploaded}}</div>
            </div>
          </div>
        </div>
      </div>
      <div class="noData" v-show="!displayFlag">No information found</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
// import Koa from 'koa'
// import cors from 'koa-cors'

export default {
  data () {
    return {
      myResult: [],
      myResultCopy: [],
      btnFlag: false,
      btnContent: 'Diminishing',
      searchValue: '',
      displayFlag: true
    }
  },
  mounted () {
     // const url = 'http://crossorigin.me/http://167.99.171.123:8080/api/report?dateFrom=2020-05-01&dateTo=2020-05-30&battery1From=5&battery1To=100'
    // const url = 'https://yacdn.org/proxy/http://167.99.171.123:8080/api/report?dateFrom=2020-05-01&dateTo=2020-05-10&battery1From=5&battery1To=100'
    const url = 'https://api.allorigins.win/raw?url=http://167.99.171.123:8080/api/report'
    axios.get(url).then(response => {
      const result = response.data
      const myResult = result.data
      // this.result = result
      this.myResult = myResult
      this.myResultCopy = myResult
      console.log(myResult)
    })
  },
  methods: {
    dateSort () {
      this.btnFlag = !this.btnFlag
      if (this.btnFlag) {
        this.btnContent = 'Increment'
      } else {
        this.btnContent = 'Diminishing'
      }
      this.myResult.reverse()
    },
    mySearch (searchValue) {
      if (searchValue === '') {
        alert('Please Input Keyword')
        return
      }
      searchValue.trim()
      this.myResult = this.myResultCopy
      this.myResult = this.myResult.filter(item => item.id === searchValue)
      const arrLength = this.myResult.length
      this.displayFlag = true
      if (arrLength === 0) {
        this.displayFlag = false
        console.log(this.displayFlag)
      }
      console.log(this.myResult)
    }
  }
}
</script>

<style scoped>
  @import url(https://fonts.googleapis.com/earlyaccess/notosanstc.css);
  * {
    box-sizing: border-box;
  }

  body {
    font-family: Arial, 'Noto Sans TC', "微軟正黑體", "繁黑體", 'Microsoft JhengHei','Microsoft YaHei','Heiti TC','LiHei Pro',sans-serif, "新細明體",PMingLiU;
    color: #555;
    line-height: 26px;
    /*font-size: 15px;*/
    text-align: justity;
    text-justify: inter-word;
    line-height: 25px;
    padding: 10px 0;
    margin: 0;
    background-color: #FFF;
    font-size: 14px;
  }

  .grayBg {
    background-color: #f6f6f6;
  }

  .yellowBg {
    background-color: #ffffcc;
  }

  div.myWrap {
    max-width: 1200px;
    width: 95%;
    margin: 10px auto 0;
    border: 1px solid #555;
    border-radius: 8px 8px 0 0;
  }

  div.titleRow {
    display: flex;
    flex-flow: row nowrap;

  }

  div.titleRow div {
    flex: 0 0 15.5%;
    border-right: 1px solid #555;
    border-bottom: 1px solid #555;
    text-align: center;
    padding-top: 16px;
    padding-bottom: 16px;
    font-weight: bold;
  }

  div.titleRow div.myNo {
    flex: 0 0 7%;
  }

  div.titleRow div:nth-last-child(1) {
    border-right: none;
  }

  div.contentRow {
    display: flex;
    flex-flow: row nowrap;
  }

  div.contentRow > div {
    flex: 0 0 15.5%;
    border-right: 1px solid #555;
    border-bottom: 1px solid #555;
    text-align: center;
    padding-top: 5px;
    padding-bottom: 5px;
    /*font-weight: bold;*/
    font-size: 14px;
    display: flex;
    justify-content: center;
    align-content: center;
    flex-flow: column;
  }

  div.contentRow > div > div {
    padding: 3px 0;
  }

  /*
  div.myWrap > div.contentRow:nth-of-type(2) > div {
    border-bottom: none;
  }
  */

  /*div.myWrap > div.contentRow:nth-last-child(1) > div {
    border-bottom: none;
  }*/

  div.myWrap > div.contentRow:nth-last-of-type(2) > div {
    border-bottom: none
  }

  div.contentRow div.myNo {
    flex: 0 0 7%;
  }

  div.contentRow div:nth-last-child(1) {
    border-right: none;
  }

  div.noData {
    font-size: 14px;
    padding: 20px;
    color: #555;
    text-align: center;
  }

/* 手機版表格開始---------------------------------- */

  div.myWrap_mobile {
    width: 95%;
    margin: 10px auto 0;
    border: 1px solid #555;
    /* border-radius: 8px;*/
    display: none;
    font-size: 14px;
  }

  div.myWrap_mobile > div.contentRow_mobile {
    display: flex;
    flex-flow: row nowrap;
    border-bottom: 1px solid #555;
  }

  div.myWrap_mobile > div.contentRow_mobile:nth-last-of-type(2) {
    border-bottom: none;
  }

  div.contentRow_mobile > div.col_10 {
    flex: 0 0 10%;
    border-right: 1px solid #555;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  div.contentRow_mobile > div.col_90 {
    flex: 0 0 90%;
    display: flex;
    flex-flow: column;
  }

  div.contentRow_mobile > div.col_90 > div {
    border-bottom: 1px solid #555;
    display: flex;
  }

  div.contentRow_mobile > div.col_90 > div > div {
    flex: 0 0 50%;
    border-right: 1px solid #555;
    display: flex;
    flex-flow: column;
    justify-content: center;
    align-items: center;
    padding: 3px 0;
  }

  div.contentRow_mobile > div.col_90 > div > div > div {
    padding: 5px 0;
  }

  div.contentRow_mobile > div.col_90 > div > div:nth-last-of-type(1) {
    border-right: none;
  }

  div.contentRow_mobile > div.col_90 > div:nth-last-of-type(1) {
    border-bottom: none;
  }

  /* top zone 開始 --------------------------------*/
  div.topZone {
    display: flex;
    flex-flow: row nowrap;
    width: 95%;
    max-width: 1200px;
    margin: 5px auto;
    font-size: 12px;
  }

  div.topZone > div {
    display: flex;
    flex: 0 0 50%;
  //padding: 10px;
  }

  div.topZone > div.leftZone {
    justify-content: flex-start;
    align-items: center;
  }

  div.topZone > div.rightZone {
    justify-content: flex-end;
    align-items: center;
  }

  div.myBtn {
    padding: 5px 8px;
    border: 1px solid #555;
    background-color: #FFFFCC;
    color: #555;
    cursor: pointer;
    font-size: 12px;
    border-radius: 3px;
    margin-left: 5px;
  }

  .myInput {
    width: 70%;
    padding: 6px;
    border: 1px solid #555;
    border-radius: 3px;
  }

  @media (max-width: 576px) {
  div.myWrap {
    display:none;
  }
    div.myWrap_mobile {
      display:block;
    }

  }
</style>
