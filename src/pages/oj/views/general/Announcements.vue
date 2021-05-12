<template>
  <Row type="flex" :gutter="18">
    <Col :span="22">
      
      
    
    </Col>
    <Col :span="containerSpan" >

      <div>

        <el-carousel :interval="5000"  style="margin: auto; width: 690px;" height="bannerHeight+'px'">
        
        <div v-if="contests.length" class="contest" >
          <el-carousel-item v-for="(contest, index) of contests" :key="index" style="background: url('/public/upload/bg.png')"> 
            <div class="contest-content" @click="goContest(index)">

              <p style="margin-top: 30px; left: 40px; margin-left: 30px;">
                <Button type="text"  class="contest-title" >{{contests[index].title}}</Button>
              </p>
              
                <p style="margin-top: 15px; margin-left: 30px; font-size: 20px">
                  <!-- <Button  size="small" shape="circle" :type="contest.status === '0' ? 'success' : 'warning'" > -->
                  比赛状态：{{$t('m.' + CONTEST_STATUS_REVERSE[contest.status].name.replace(/ /g, "_"))}}
                  <!-- </Button> -->
                </p>
                <p style="margin-top: 7px; margin-left: 30px; font-size: 20px">
                  <!-- <p type="info" shape="circle" size="small" icon="calendar"> -->
                  开始时间：{{contest.start_time | localtime('YYYY-M-D HH:mm') }}
                  <!-- </p> -->
                </p>
                
                <p style="margin-top: 7px; margin-left: 30px; font-size: 20px">
                  <!-- <Button shape="circle" size="small" icon="android-time"> -->
                  比赛时长：{{getDuration(contest.start_time, contest.end_time)}}
                  <!-- </Button> -->
                </p>
                
                <p style="margin-top: 7px; margin-left: 30px; font-size: 20px">
                  <!-- <Button shape="circle" size="small" icon="trophy"> -->
                  赛制：{{contest.rule_type}}
                  <!-- </Button> -->
                </p>
                <p style="margin-top: 7px; margin-left: 30px; font-size: 20px">
                  <!-- <Button shape="circle" size="small" > -->
                  难度：普及组
                  <!-- </Button> -->
                </p>
              
              <!-- <div class="contest-content-description">
                <blockquote v-html="contest.description.slice(0, 400)"></blockquote>...
              </div>
              <ButtonGroup shape="circle" class="btn-group">
                <Button :type="contest.status === '0' ? 'success' : 'warning'" @click="goContest(index)">
                  {{ contest.status === '0' ? $t('m.Enter_Contest') : $t('m.Preview_Contest') }}
                  <Icon type="chevron-right"></Icon>
                </Button>
            </ButtonGroup> -->
            </div>

          </el-carousel-item> 
        </div>

        <el-carousel-item>
          <a href="https://hdnoip.com"><img ref="bannerHeight" src="/public/upload/home1.png" alt="" @load="imgLoad" ></a>
        </el-carousel-item>
        <el-carousel-item v-if="images[0]">
          <a href="https://hdnoip.com/contest"><img ref="bannerHeight" src="/public/upload/contest.png" alt=""></a>
        </el-carousel-item>
        <el-carousel-item v-if="images[1]">
          <a href="https://hdnoip.com/Forum"><img ref="bannerHeight" src="/public/upload/home3.png" alt=""></a>
        </el-carousel-item>
        </el-carousel>
        <!-- <div id="test-div" style="border: 5px solid red;background-color: antiquewhite;">当前高度：{{bannerHeight}}</div> -->
      </div>
      <div style="margin-top: 30px;">
      <Panel shadow :padding="10" >
        <div slot="title" >
          {{title}}
        </div>
        <div slot="extra">
          <Button v-if="listVisible" type="info" @click="init" :loading="btnLoading"><span class="ivu-icon ivu-icon-refresh"></span> {{$t('m.Refresh')}}</Button>
          <Button v-else type="ghost" icon="ios-undo" @click="goBack">{{$t('m.Back')}}</Button>
        </div>
        <transition-group name="announcement-animate" mode="in-out">
          <div class="no-announcement" v-if="!announcements.length" key="no-announcement">
            <p>{{$t('m.No_Announcements')}}</p>
          </div>
          <template v-if="listVisible">
            <ul class="announcements-container" key="list">
              <li v-for="announcement in announcements" :key="announcement.title">
                <div class="flex-container">
                  <div class="title"><a class="entry" @click="goAnnouncement(announcement)">
                    {{announcement.title}}</a></div>
                  <div class="date">{{announcement.create_time | localtime }}</div>
                  <div class="creator"> {{$t('m.By')}} {{announcement.created_by.username}}</div>
                </div>
              </li>
            </ul>
            <Pagination v-if="!isContest"
                        key="page"
                        :total="total"
                        :page-size="limit"
                        @on-change="getAnnouncementList">
            </Pagination>
          </template>

          <template v-else>
          <div v-katex v-html="announcement.content" key="content" class="content-container markdown-body"></div>
          </template>
        </transition-group>
      </Panel>
    </div>
    </Col>
    <Col :span="5" v-if="!isContest" >
      <Panel shadow>
        <div style="font-size:14px; text-align:center; width:100%; line-height:16px; background: transparent; color:#636e72;"><strong>Welcome to ZOJ</strong></div>
        <div class="today">
          <div class="nowWeek">{{nowWeek}}</div>
          <div class="nowDate">
            {{nowMouth}}月{{nowDate}}日
          </div>
        </div>
         <div v-if="days" style="margin:0 auto; margin-top:-30px; margin-bottom:15px; font-size:12px; text-align:center; width:160px; line-height:16px; background: transparent; color:#636e72;"><br>您已连续签到了 <strong>{{days}}</strong> 天</div>
        <div style="margin-top:60px; margin:0 auto; font-size:14px; text-align:center; width:80%; line-height:16px; background: transparent; color:#636e72;">{{word}}</div>
        <Button v-if="!SighinStatus" type="primary" icon="ios-alarm" @click="Sighin" long style="margin-top:30px; margin-bottom:20px; margin-left:10%; width:80%;">签到</Button>
        <Button v-else type="primary" icon="ios-alarm" long disabled style="margin-top:20px; margin-bottom:20px; margin-left:10%; width:80%;">
          签到
        </Button>
      </Panel>

       <Card id="info" style="margin-top: 30px;">
        <div slot="title" class="header" style="margin:0 auto; margin-top: 20px; font-size:25px; text-align:center;">
          <span class="card-title"><strong>大神榜</strong></span>
        </div>
        <ul>
          <ol style="ol">
            <li v-for="u in dataRank" :key="u.id">
              <p>
                <a :style="'font-weight: 600;color: ' + u.color" :href="'/user-home?username=' + u.user.username" :title=" u.title + ' ' + u.user.username">
                {{u.user.username}}
                </a>
              </p>
              <p>
              {{u.experience}} 原力值 
              </p>
            </li>
          </ol>
        </ul>
      </Card>
    </Col>
  </Row>
</template>

<script>
  import api from '@oj/api'
  import Pagination from '@oj/components/Pagination'
  import axios from 'axios'
  import { mapState } from 'vuex'
  import time from '@/utils/time'
  // import {  } from '@/utils/constants'
  import { RULE_TYPE, USER_GRADE, CONTEST_STATUS, CONTEST_STATUS_REVERSE } from '@/utils/constants'
  import 'element-ui/lib/theme-chalk/index.css'

  export default {
    name: 'Announcement',
    components: {
      Pagination
    },
    data () {
      return {
        images: [],
        limit: 10,
        total: 10,
        tagList: [],
        dataRank: [],
        rankLimit: 10,
        problemList: [],
        problemLimit: 15,
        query: {
          keyword: '',
          difficulty: '',
          tag: '',
          page: 1,
          orderby: '-create_time'
        },
        btnLoading: false,
        announcements: [],
        announcement: '',
        listVisible: true,
        timer: null,
        containerSpan: 19,
        SighinStatus: false,
        nowWeek: '',
        nowDate: '',
        word: '',
        days: 0,
        bannerHeight: '',
        contests: [],
        index: 0,
        CONTEST_STATUS_REVERSE: CONTEST_STATUS_REVERSE
      }
    },
    mounted () {
      this.init()
      this.timer = setInterval(() => {
        this.setNowTimes()
      }, 1000)
      this.getWord()
      this.isSighin()
      this.getRankData()
      this.getProblemList()
      this.getTagList()
      let params = { status: CONTEST_STATUS.NOT_START }
      api.getContestList(0, 5, params).then((res) => {
        this.contests = res.data.data.results
      })
      this.getExtraImage('/../public/upload/contest.png')
      this.getExtraImage('/../public/upload/home3.png')
      this.imgLoad()
      window.addEventListener('resize', () => {
        this.bannerHeight = this.$refs.bannerHeight.height
        this.imgLoad()
      }, false)
    },
    methods: {
      imgLoad () {
        this.$nextTick(() => {
          this.bannerHeight = this.$refs.bannerHeight.height
          console.log(this.$refs.bannerHeight.height)
          var testH = document.getElementById('test-div')
          testH.style.height = this.bannerHeight + 'px'
        })
      },
      init () {
        if (this.isContest) {
          this.containerSpan = 24
          this.getContestAnnouncementList()
        } else {
          this.getAnnouncementList()
        }
      },
      getTagList () {
        api.getProblemTagList().then(res => {
          this.tagList = res.data.data.sort((a, b) => {
            return a.id - b.id
          })
        })
      },
      getRankData () {
        api.getUserRank(0, this.rankLimit, RULE_TYPE.experience).then(res => {
          this.dataRank = res.data.data.results
          for (let i in this.dataRank) {
            this.dataRank[i]['color'] = USER_GRADE[this.dataRank[i].grade].color
            this.dataRank[i]['title'] = USER_GRADE[this.dataRank[i].grade].name
          }
        }).catch(() => {
        })
      },
      getProblemList () {
        let offset = 0
        api.getProblemList(offset, this.problemLimit, this.query).then(res => {
          this.problemList = res.data.data.results
        })
      },
      getAnnouncementList (page = 1) {
        this.btnLoading = true
        api.getAnnouncementList((page - 1) * this.limit, this.limit).then(res => {
          this.btnLoading = false
          this.announcements = res.data.data.results
          this.total = res.data.data.total
        }, () => {
          this.btnLoading = false
        })
      },
      getContestAnnouncementList () {
        this.btnLoading = true
        api.getContestAnnouncementList(this.$route.params.contestID).then(res => {
          this.btnLoading = false
          this.announcements = res.data.data
        }, () => {
          this.btnLoading = false
        })
      },
      goAnnouncement (announcement) {
        this.announcement = announcement
        this.listVisible = false
      },
      goBack () {
        this.listVisible = true
        this.announcement = ''
      },
      getWord () {
        axios.get('https://v1.hitokoto.cn/?c=d&c=e&c=f&c=h&c=i&c=j&c=k').then(response => {
          this.word = response.data.hitokoto
        })
      },
      setNowTimes () {
        let myDate = new Date()
        let weeks = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
        let mouth = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12']
        this.nowDate = String(myDate.getDate() < 10 ? '0' + myDate.getDate() : myDate.getDate())
        this.nowMouth = mouth[myDate.getMonth()]
        this.nowWeek = weeks[myDate.getDay()]
        this.nowYear = myDate.getFullYear()
      },
      isSighin () {
        api.GetSighinStatus().then(res => {
          if (res.data.data === null || res.data.data.sighinstatus === 'false') {
            this.SighinStatus = false
          } else {
            this.SighinStatus = true
          }
          this.days = res.data.data !== null ? res.data.data.continue_sighin_days : 0
        })
      },
      Sighin () {
        api.UserSighin().then(res => {
          if (res.data.data === 'Singined') {
            this.$Notice.error({
              title: '签到失败',
              desc: '您已经签过到了呀 ~ 明天再来哦'
            })
            this.isSighin()
          } else {
            this.$Notice.success({
              title: '签到成功',
              desc: '恭喜您，获得 ' + res.data.data.experience + ' 原力值，明天记得来签到哦'
            })
            this.days += 1
            this.SighinStatus = true
          }
        })
      },
      getDuration (startTime, endTime) {
        return time.duration(startTime, endTime)
      },
      goContest (index) {
        this.index = index
        this.$router.push({
          name: 'contest-details',
          params: { contestID: this.contests[this.index].id }
        })
      },
      async getExtraImage (url) {
        await axios
          .get(url)
          .then(response => {
            this.images.push(url)
          })
          .catch(error => {
            console.log(error)
          })
      }
    },
    computed: {
      ...mapState(['website']),
      title () {
        if (this.listVisible) {
          return this.isContest ? this.$i18n.t('m.Contest_Announcements') : this.$i18n.t('m.Announcements')
        } else {
          return this.announcement.title
        }
      },
      isContest () {
        return !!this.$route.params.contestID
      }
    }
  }
</script>

<style scoped lang="less">
  .announcements-container {
    margin-top: -10px;
    margin-bottom: 10px;
    li {
      padding-top: 15px;
      list-style: none;
      padding-bottom: 15px;
      margin-left: 20px;
      font-size: 16px;
      border-bottom: 1px solid rgba(187, 187, 187, 0.5);
      &:last-child {
        border-bottom: none;
      }
      .flex-container {
        .title {
          flex: 1 1;
          text-align: left;
          padding-left: 10px;
          a.entry {
            color: #495060;
            &:hover {
              color: #2d8cf0;
              border-bottom: 1px solid #2d8cf0;
            }
          }
        }
        .creator {
          flex: none;
          width: 200px;
          text-align: center;
        }
        .date {
          flex: none;
          width: 200px;
          text-align: center;
        }
      }
    }
  }
  .contest {

    &-title {
      font-style: normal;
      font-size: 30px;
      color: #00A2E8;
    }
    &-content {
      font-size: 15px;
      padding: 0 50px 40px 50px;
      &-description {
        height: 250px;
      }
    }
  }
  .contest-content-description{
    margin-top: 120px;
    height: 200px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .content-container {
    padding: 0 20px 20px 20px;
  }

  .no-announcement {
    text-align: center;
    font-size: 16px;
  }changeLocale

  .announcement-animate-enter-active {
    color: rgba(255, 255, 255, 0.5);
    animation: fadeIn 1s;
  }
  .nowWeek {
    text-align: center;
    padding-top: 20px;
    font-size: 25px;
    font-weight: 600;
  }
  .nowDate {
    margin-top: 20px;
    text-align: center;
    padding-bottom: 10px;
    font-size: 20px;
    font-weight: 600;
  }
  .tag-btn {
    margin-right: 10px;
    margin-bottom: 20px;
  }
  .tag-btn:hover {
    a {
       color: #2d8cf0;
    }
  }
  .link-style {
    color:#264c67
  }
  .link-style:hover {
    color: #2d8cf0;
  }
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 100px;
    margin: 20px;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: #d3dce6; 
    border-radius: 25px;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #ffffff;
    border-radius: 25px;
  }
  #info {
    margin-bottom: 20px;
    margin-top: 20px;
    ul {
      list-style-type: none;
      li {
        border-bottom: 1px dotted #e9eaec;
        margin-bottom: 10px;
        p {
          display: inline-block;
        }
        p:first-child {
          width: 60px;
        }
        p:last-child {
          float: right;
        }
      }
    }
  }
  ol {
    padding: 0;
    margin: 0;
    list-style: none;
    counter-reset: sectioncounter
  }
  ol > li{
    width: 100%;
    line-height: 30px;
    color: #130b18;
    font-size: 0.9rem;
  }
  ol > li:before {
    content: '' counter(sectioncounter) '';
    counter-increment: sectioncounter;
    width: 20px;
    height: 20px;
    line-height: 20px;
    text-align: center;
    display: inline-block;
    background-color: #e3c690;
    color: #bc8011;
    font-size: 0.9rem;
    border-radius: 20%;
    margin-right: 3%;
  }
</style>
