<template>
  <div class="flex flex-column details-content" :style="{height:state.twoHeight+'px'}">
    <div class="back w-100 position-absolute">
      <img class="w-100" src="@/assets/image/detailsContent/page-bg.png">
    </div>
    <div id="introduction" class="info flex pt-16 menu-js">
      <div class="flex flex1 flex-column flex-center-center flex-align-end">
        <div class="speak flex flex-column flex-center-center">
          <div class="line-1">24/7 <span class="color-yellow">Monitoring</span></div>
          <div class="line-2">Real-time Event Alerts</div>
          <div class="line-3">Optimize <span class="color-red">Progress</span></div>
        </div>
        <div class="devil-speaking color-blue line-4 flex flex-column flex-center-center flex-align-end">
          <div>Efficiency at its peak.</div>
          <div>Stay informed, stay ahead.</div>
        </div>
      </div>
      <div class="flex1">
        <img src="@/assets/image/detailsContent/interface/1.png">
      </div>
    </div>
    <div class="info flex">
      <div class="flex1">
        <img src="@/assets/image/detailsContent/interface/2.png">
      </div>
      <div class="flex1 flex flex-column flex-center-center flex-align-start">
        <div class=" speak flex flex-column flex-center-center">
          <div class="line-1">Essential <span class="color-yellow">Utilities</span></div>
          <div class="line-2">Streamlined tools for peak efficiency</div>
          <div class="line-3"><span class="color-red">Strategy Guides</span> instant push</div>

        </div>
        <div class="devil-speaking color-blue line-4">
          Master every challenge with ease.
        </div>
      </div>

    </div>
    <div id="install" class="install pa-8 pt-16 menu-azzn">
      <box-title icon="icon-xiaoke-a-lianhe1" title="Extension Install Guide"></box-title>
      <div class="install-content pa-8 flex flex-around-center">
        <install-browser v-for="install in state.installInfo"
                         :key="install.name"
                         :browser="install"></install-browser>
      </div>
    </div>
    <div id="install-desktop" class="install pa-8 pt-16 menu-azzn">
      <box-title icon="icon-xiaoke-a-lianhe1" title="Desktop Install Guide"></box-title>
      <div class="install-content pa-8 flex flex-around-center">
        <install-browser v-for="install in state.installDesktopInfo"
                         :key="install.name"
                         :browser="install"
                         @open="openInstallDesktop"></install-browser>
      </div>
    </div>
    <div id="install-app" class="install pa-8 pt-16 menu-azzn">
      <box-title icon="icon-xiaoke-a-lianhe1" title="App Install Guide"></box-title>
      <div class="install-content pa-8 flex flex-around-center">
        <install-browser v-for="install in state.installAppInfo"
                         :key="install.name"
                         :browser="install"
                         @open="openInstallApp"></install-browser>
      </div>
    </div>
    <div id="sponsor" class="donate pa-8 pt-16 menu-zcst">
      <box-title icon="icon-xiaoke-a-lianhe2" title="Support Aether"></box-title>
      <div class="mt-7">
        <h3>Thank you for supporting Aether Canteen</h3>
        Your support helps us keep the servers running and the tools updated. 
        <h4>Please ensure your donation note starts with <span style="color: #D07D00; font-size: 30px;">Aether</span>.</h4>
      </div>
      <div class="flex flex-around-center mt-7">
        <div class="donate-box" v-for="donate in state.donateList">
          <v-card>
            <img class="w-100" :src="require('@/assets/image/detailsContent/donate/'+donate.img)">
            <v-card-actions v-if="donate.link">
              <v-btn style="width:100%"
                     variant="outlined"
                     color="#D07D00"
                     @click="toLink(donate.link)">{{ donate.text }}
              </v-btn>
            </v-card-actions>
            <v-card-text v-else>{{ donate.text }}</v-card-text>
          </v-card>
        </div>
      </div>
      <div class="mt-7">
        <v-btn @click="toLink('https://docs.qq.com/sheet/DZXZObGZuZmtRaXlv?tab=nzfrjv')" class="color-fff" color="#D07D00">Financial Report</v-btn>
      </div>
    </div>
    <div id="about-us" class="us pa-8 pt-16 menu-gywm">
      <box-title icon="icon-xiaoke-a-lianhe3" title="About Us"></box-title>
      <div class="flex flex-row flex-wrap flex-evenly-center">
        <team class="mt-10" :team-info="team" v-for="team in state.teamList"></team>
      </div>
    </div>
    <div id="footers" class="">
      <footers/>
    </div>
  </div>
</template>

<script lang="ts" setup>
import {onMounted, reactive, ref, watch, computed} from "vue";
import InstallBrowser from "@/components/detailsContent/installBrowser.vue";

import {PC_INSTALL_HELP_LIST, DESKTOP_INSTALL_HELP_LIST, APP_INSTALL_HELP_LIST, DESKTOP_STRUCTURE, APP_STRUCTURE} from "@/assets/constant/install"
import {DONATE_LIST} from "@/assets/constant/donate"
import {TEAM_LIST} from "@/assets/constant/team"
import { version_desktop, version_app } from '@/request/api'
import BoxTitle from "@/components/detailsContent/boxTitle.vue";
import Team from "@/components/detailsContent/team.vue";

import Footers from "@/views/home/footers.vue";

interface state {
  installInfo: Array<any>,
  installDesktopInfo: Array<any>,
  installAppInfo: Array<any>,
  twoHeight: number,
  donateList: Array<any>,
  teamList: Array<any>,
  desktopStructure: Array<any>,
  appStructure: Array<any>
}

let state = reactive<state>({
  installInfo: PC_INSTALL_HELP_LIST,
  installDesktopInfo: JSON.parse(JSON.stringify(DESKTOP_INSTALL_HELP_LIST)),
  installAppInfo: APP_INSTALL_HELP_LIST,
  twoHeight: 0,
  donateList: DONATE_LIST,
  teamList: TEAM_LIST,
  desktopStructure: DESKTOP_STRUCTURE,
  appStructure: APP_STRUCTURE
})

const emit = defineEmits(['heightToZero'])

let contentDom: any = ref(null);
let pageHeight = ref(0)


onMounted(() => {
  state.twoHeight = window.innerHeight;
  // contentDom.value.addEventListener('scroll', () => {
  //   pageHeight.value = contentDom.value.scrollTop
  // })
})

watch(pageHeight, (data: number, oldData: number) => {
  if (data == 0) {
    emit('heightToZero');
  }
});

let toLink = (url: string) => {
  window.open(url, "_black")
}
let toContent = (className: string) => {
  let dom = contentDom.value.querySelector('.' + className);
  dom.scrollIntoView({
    behavior: "smooth",  // 平滑过渡
    block: "start"  // 上边框与视窗顶部平齐。默认值
  });

}

const getVersionDesktop = (params?: any) => {
  version_desktop(params).then((res: any) => {
    const data = res.data
    const baiduCryp = data.baidu_text.match(/ (.*?)）/)[1]
    const downloadLinks = state.desktopStructure.map(p => {
      if (p.key === 'baidu')
        return { text: p.text, link: `${data[p.key]}?pwd=${baiduCryp}` }
      return { text: p.text, link: data[p.key] }
    })
    state.installDesktopInfo.forEach(p => {
      p.help.unshift({image:'',btn:downloadLinks})
    })
  }).catch((err) => {
    console.log(err)
  })
}

const getVersionApp = (params?: any) => {
  version_app(params).then((res: any) => {
    const data = res.data
    const baiduCryp = data.baidu_text.match(/ (.*?)）/)[1]
    const downloadLinks = state.appStructure.map(p => {
      if (p.key === 'baidu')
        return { text: p.text, link: `${data[p.key]}?pwd=${baiduCryp}` }
      return { text: p.text, link: data[p.key] }
    })
    
    state.installAppInfo.forEach((p, index) => {
      if (index > 0) return false;
      p.help.unshift({image:'',btn:downloadLinks})
    })
  }).catch((err) => {
    console.log(err)
  })
}

const openInstallDesktop = () => {
  state.installDesktopInfo.forEach(p => {
    if (p.help.length>1)
      p.help.shift()
  })
  getVersionDesktop()
}

const openInstallApp = () => {
  state.installAppInfo.forEach((p, index) => {
    if (index > 0) return false;
    if (p.help.length>1)
      p.help.shift()
  })
  getVersionApp()
}
</script>

<style lang="scss" scoped>
.details-content {
  min-width: 1200px;
  min-height: 700px;

  .back {
    z-index: -1;
    opacity: 0.3;
    //background: no-repeat top /cover scroll;
    //background-image: -webkit-cross-fade(
    //        url("@/assets/image/transparent.png"),
    //        url("@/assets/image/detailsContent/page-bg.png"),
    //        30%);
    //background-image: cross-fade(
    //        url("@/assets/image/transparent.png"),
    //        url("@/assets/image/detailsContent/page-bg.png"),
    //        30%);
  }

  .info {
    font-family: 'sucai';
    margin-top: 50px;


    .speak {
      font-size: 50px;
      font-weight: bold;
      letter-spacing: 5px;

      .line-2 {
        font-size: 30px;
        margin: 5px 0;
      }

      .line-4 {
        font-size: 16px;
        letter-spacing: normal;
        margin: 5px 0;
      }
    }

    img {
      width: 80%;
    }
  }

  .donate {
    .donate-box {
      width: 200px;
    }
  }

}
</style>
