<template>
  <div id="header">
    <Menu style="margin: auto; width:75%; margin-left: 10%;" theme="light" mode="horizontal" @on-select="handleRoute" :active-name="activeMenu" :class="oj-menu">
      <div class="logo"><img src="/public/upload/logo.PNG"  alt="Logo" height="55" style="vertical-align:middle" />
      </div>
      <!-- <div class="logo"><span><img data-v-90f51026="" src="/public/upload/logo.PNG" height="55"></span></div> -->
      <!-- <div class="logo">
        <span class="font-color-w">{{website.website_name}}</span>
      </div> -->
      
      <Menu-item name="/">
        <Icon type="home"></Icon>
        {{$t('m.Home')}}
      </Menu-item>
      <Menu-item name="/problem">
        <Icon type="ios-keypad"></Icon>
        {{$t('m.NavProblems')}}
      </Menu-item>
      <Menu-item name="/contest">
        <Icon type="trophy"></Icon>
        {{$t('m.Contests')}}
      </Menu-item>
      <Menu-item name="/status">
        <Icon type="ios-pulse-strong"></Icon>
        {{$t('m.NavStatus')}}
      </Menu-item>
      <Menu-item name="/experience-rank">
        <Icon type="podium"></Icon>
        {{$t('m.Experience_Ranklist')}}
      </Menu-item><!---->
<!--      <Submenu name="rank">
        <template slot="title">
          <Icon type="podium"></Icon>
          {{$t('m.Rank')}}
        </template>
        <Menu-item name="/acm-rank">
          {{$t('m.ACM_Rank')}}
        </Menu-item>
        <Menu-item name="/oi-rank">
          {{$t('m.OI_Rank')}}
        </Menu-item> 
        <Menu-item name="/experience-rank">
          {{$t('m.Experience_Ranklist')}}
        </Menu-item>
      </Submenu>-->
      <Submenu name="onlineapp">
        <template slot="title">
          <Icon type="ios-cloud" />
          {{$t('m.App')}}
        </template>
        <Menu-item name="/IDE">
          {{$t('m.IDE')}}
        </Menu-item>
        <Menu-item v-if="website.allow_forum_post" name="/Forum">
          {{$t('m.Forum')}}
        </Menu-item>
      </Submenu>
      <Submenu name="about">
        <template slot="title">
          <Icon type="information-circled"></Icon>
          {{$t('m.About')}}
        </template>
        <Menu-item name="/about">
          {{$t('m.Judger')}}
        </Menu-item>
        <Menu-item name="/FAQ">
          {{$t('m.FAQ')}}
		</Menu-item>
		<Menu-item name="/AboutUs">
          {{$t('m.AboutUs')}}
        </Menu-item>
      </Submenu>
          
      <Dropdown @on-click="switchChange" class="ivu-menu-submenu">
        <div>
		  <Icon type="ios-browsers"></Icon>
		  &emsp;换肤
          <Icon type="ios-arrow-down"></Icon>
        </div>
        <DropdownMenu slot="list">
          <DropdownItem name="1"><Icon type="ios-browsers" color="#2d8cf0" />&emsp;默认</DropdownItem>
          <DropdownItem name="2"><Icon type="ios-browsers" color="#f58f98" />&emsp;粉色</DropdownItem>
	        <DropdownItem name="4"><Icon type="ios-browsers" color="#d63031" />&emsp;红色</DropdownItem>
		      <DropdownItem name="5"><Icon type="ios-browsers" color="#00b894" />&emsp;绿色</DropdownItem>
          <DropdownItem name="3"><Icon type="ios-browsers" color="#673AB7" />&emsp;紫色</DropdownItem>
        </DropdownMenu>
      </Dropdown>

      <template v-if="!isAuthenticated">
        <div class="btn-menu" style="margin-right: -10%;">
          <Button type="ghost"
                  ref="loginBtn"
                  shape="circle"
                  @click="handleBtnClick('login')">{{$t('m.Login')}}
          </Button>
          <Button v-if="website.allow_register"
                  type="ghost"
                  shape="circle"
                  @click="handleBtnClick('register')"
                  style="margin-left: 5px;">{{$t('m.Register')}}
          </Button>
        </div>
      </template>
      <template v-else>
        <Dropdown style="margin-right: -10%" class="drop-menu" @on-click="handleRoute" placement="bottom" trigger="click">
          <Poptip trigger="hover" :title="`当前 ${ profile.grade } 级`" :content="`当前原力值： ${ profile.experience } 点`" width="200px">
            <Tag v-if="profile.user.title" :color="profile.user.title_color" style="margin-right:-15px;">{{ profile.user.title }}</Tag>
            <Tag v-else :color="color" style="margin-right:-15px;">{{ gradename }}</Tag>
          </Poptip>
          <Button type="text" class="drop-menu-title">{{ user.username }}
            <Icon type="arrow-down-b"></Icon>
          </Button>
          <Dropdown-menu slot="list">
            <Dropdown-item name="/user-home">{{$t('m.MyHome')}}</Dropdown-item>
            <Dropdown-item name="/status?myself=1">{{$t('m.MySubmissions')}}</Dropdown-item>
            <Dropdown-item name="/setting/profile">{{$t('m.Settings')}}</Dropdown-item>
            <Dropdown-item v-if="isAdminRole" name="/admin">{{$t('m.Management')}}</Dropdown-item>
            <Dropdown-item divided name="/logout">{{$t('m.Logout')}}</Dropdown-item>
          </Dropdown-menu>
        </Dropdown>
      </template>
    </Menu>
    <Modal v-model="modalVisible" :width="400">
      <div slot="header" class="modal-title">{{$t('m.Welcome_to')}} {{website.website_name_shortcut}}</div>
      <component :is="modalStatus.mode" v-if="modalVisible"></component>
      <div slot="footer" style="display: none"></div>
    </Modal>
  </div>
</template>

<script>
  import { mapGetters, mapActions } from 'vuex'
  import login from '@oj/views/user/Login'
  import register from '@oj/views/user/Register'

  export default {
    components: {
      login,
      register
    },
    mounted () {
      this.getProfile()
    },
    methods: {
      ...mapActions(['getProfile', 'changeModalStatus']),
      handleRoute (route) {
        if (route && route.indexOf('admin') < 0) {
          this.$router.push(route)
        } else {
          window.open('/admin/')
        }
      },
      handleBtnClick (mode) {
        this.changeModalStatus({
          visible: true,
          mode: mode
        })
      },
      // 更换主题
      switchChange (status) {
        let params = document.getElementById('app')
        params.className = 'theme' + status
        window.localStorage.setItem('app', document.getElementById('app').className)
      },
      // 存储主题颜色
      localStorageDate () {
        let memoryColor = window.localStorage.getItem('app')
        let params = document.getElementById('app')
        params.className = memoryColor
      }
    },
    computed: {
      ...mapGetters(['website', 'modalStatus', 'user', 'profile', 'isAuthenticated', 'isAdminRole', 'color', 'gradename']),
      // 跟随路由变化
      activeMenu () {
        return '/' + this.$route.path.split('/')[1]
      },
      modalVisible: {
        get () {
          return this.modalStatus.visible
        },
        set (value) {
          this.changeModalStatus({visible: value})
        }
      }
    },
    created () {
      this.localStorageDate()
    }
  }
</script>

<style lang="less" scoped>
  #header {
    min-width: 300px;
    position: fixed;
    top: 0;
    left: 0;
    height: auto;
    width: 100%;
    z-index: 1000;
    background-color: #fff;
    box-shadow: 0 1px 5px 0 rgba(0, 0, 0, 0.1);
    .oj-menu {
      background: #fdfdfd;
    }

    .logo {
      margin-left: 2%;
      margin-right: 2%;
      font-size: 20px;
      float: left;
      line-height: 60px;
    }

    .drop-menu {
      float: right;
      margin-right: 30px;
      position: absolute;
      right: 10px;
      &-title {
        font-size: 18px;
      }
    }
    .btn-menu {
      font-size: 16px;
      float: right;
      margin-right: 10px;
    }
	.change-menu {
      float: right;
      margin-right: 130px;
      position: absolute;
      right: 10px;
      &-title {
        font-size: 18px;
      }
    }
  }

  .modal {
    &-title {
      font-size: 18px;
      font-weight: 600;
    }
  }
  
</style>
