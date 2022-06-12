<template>
  <div class="webview-wrapper"
       :class="{ 'is-focus': isSpaceFocus }">
    <div v-if="titleVisible"
         class="header">
      <div class="title" :title="meta.title">
        <i v-if="backButtonVisible"
           @click="goBack"
           class="finchat-icon icon_global_back"></i>{{ meta.title }}
      </div>

      <input type="text" v-model="curWebviewSrc" />
      <button @click="handleOpenDevTools">devtools</button>

      <div class="store-version" v-if="meta.version">{{ meta.version }}</div>
      <div class="update-btn" v-if="update" @click="clickUpdate">{{ $t('components.webview.upgrade') }}</div>
      <div v-if="enableToolBox" class="tool-box">
        <!-- 机器人 -->
        <!-- <span v-if="meta.fcId"
              @click.stop="sendMessage"
              class="finchat-icon icon_roombar_bot"></span> -->
        <!-- 暂时屏蔽分享、复制链接、浏览器打开 -->
        <!-- 分享 -->
        <!-- <span @click.stop="forwordMessage"
              class="finchat-icon icon_roombar_forward"></span> -->
        <!-- 复制链接 -->
        <!-- <span @click.stop="copyLink"
              class="finchat-icon icon_roombar_link"></span> -->
        <!-- 浏览器打开 -->
        <!-- <span @click.stop="openInBrowser"
              class="finchat-icon icon_roombar_browser"></span> -->
        <!-- 刷新 -->
        <span @click.stop="reloadWebview "
              v-if="this.isPlugin === false"
              :title="$t('global.refresh')"
              class="finchat-icon icon_roombar_refresh"></span>
      </div>
    </div>
    <webview ref="webview"
             :style="{ height: '100%', 'pointer-events': isDragging ? 'none' : 'auto' }"
             webpreferences="allowRunningInsecureContent, contextIsolation=false"
             disablewebsecurity></webview>

    <div v-if="toolbox.visible"
         class="toolbox-mask">
      <!-- <div class="toolbox-content"> -->
      <!-- <p class="logo">
          <img v-img:iconApp="toolbox.logo" />
        </p>
        <p class="title">{{toolbox.name}}</p>
        <p class="desc">{{toolbox.desc}}</p> -->
      <!-- <button class="button button-confirm" @click="toolbox.editorVisible = true">{{ $t('components.webview.addToolsForThisPage') }}</button> -->
      <button class="text button button-confirm"
              @click="toolbox.editorVisible = true">
        <i class="finchat-icon  icon_subscribe_add"></i>
        <span>{{ $t('components.webview.addToolsForThisPage') }}</span>
      </button>
      <!-- </div> -->
    </div>

    <EditModal :visible="toolbox.editorVisible"
               :info="{
                 appId: toolbox.appId,
                 appName: meta.title,
                 logo: toolbox.logo,
                 name: '',
               }"
               :hide-url-input="true"
               @on-confirm="handleToolboxConfirm"
               @on-cancel="handleToolboxCancel" />
  </div>
</template>

<script>
import EditModal from '@/components/func/room-toolbox/edit-modal';
import ToolboxApi from '@/common/api/toolbox';
import eventBridge from '@/utils/event-bridge';
import { openSelectWindow } from '@/common/common';
import { parseURL, createRandomId } from '@/utils/common';
import { recordAppStatisticalData } from '@/model/app-data-statistics';
import { getPluginApps } from '@/common/api/finstore';
import { Modal, Message } from 'view-design';
import PluginUtil from '@/utils/plugin';
import * as languageUtils from '@languages/utils';
import { pluginUsageReport } from '@/common/api/plugin-report';
const i18n = languageUtils.getI18n();

export default {
  name: 'webview-component',
  components: {
    EditModal,
  },
  props: {
    spaceId: {
      type: String,
      default: '',
    },
    processId: {
      type: String,
      default: '',
    },
    tab: {
      type: Object,
      required: false,
      default() {
        return {};
      },
    },
    isSpaceFocus: {
      type: Boolean,
      default: false,
    },
    titleVisible: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    const enableToolBox = ENABLE_WEBVIEW_TOOL_BOX;
    return {
      webviewId: createRandomId('webview'),
      isDragging: false,
      enableToolBox,
      resetCallback: null,
      backButtonVisible: false,
      updateTimer: null,
      update: null,
      createdReport: false,

      // 工具箱交互相关数据
      toolbox: {
        visible: false,
        editorVisible: false,
        appId: '',
        name: '',
        logo: '',
        desc: '',
        roomId: '',
        userId: '',
      },

      curWebviewSrc: '',
    };
  },
  mounted() {
    eventBridge.on('APP_POINTER_EVENTS', this.setIsDragging);
    // TODO 测试专用，后续去掉
    eventBridge.on('TEST_CHECK_UPDATE_WEBVIEW', this.checkAppUpdate);
    this.initWebview();
    recordAppStatisticalData('open_web_page', this.meta.id);
    if (this.meta.appId) {
      this.updateTimer = setInterval(() => {
        this.checkAppUpdate();
      }, 3600000);
    }
  },
  beforeDestory() {
    eventBridge.remove('APP_POINTER_EVENTS', this.setIsDragging);
    this.resetWebview();
    clearInterval(this.updateTimer);
  },
  computed: {
    isPlugin() {
      if (Array.isArray(this.meta?.appType) && this.meta?.appType.includes('Plugin')) {
        return true
      }
      return false
    },
    meta() {
      return this.tab.meta || {};
    },
    url() {
      const url = this.meta.url;
      // 临时代码，兼容知识库必须带 jwt
      if (url.indexOf('5d0face0-a71d-11ea-ad74-ddc776d517dd') > -1 || /\/webapps\/pages\/knowledge/.test(url)) {
        return `${url}?jwt=${eventBridge.store.loginData.apiToken}`;
      }
      return url;
    },
    focusPluginId() {
      return eventBridge.store.workspace.focusPluginId;
    },
    myId() {
      return eventBridge.store.myUserInfo.userId;
    },
  },
  methods: {
    handleOpenDevTools() {
      // curWebviewSrc
      const webView = this.$refs.webview;
      if (this.curWebviewSrc) {
        webView.src = this.curWebviewSrc;
      }
      webView.openDevTools();
    },
    // 检查应用更新机制
    async checkAppUpdate() {
      const pluginApp = await getPluginApps(eventBridge.store.loginData.userId);
      const findThis = pluginApp.find(val => val.appId === this.meta.appId);
      // 版本存在更新
      if (this.meta.version && this.meta.version !== findThis.version) {
        this.update = findThis;
      } else {
        this.update = false;
      }
    },
    // 升级应用
    async clickUpdate() {
      const { id } = this.tab;
      Modal.confirm({
        title: i18n.t('components.webview.sureToUpdate'),
        content: i18n.t('components.webview.pluginBeforeUpdateingMsg'),
        onOk: async () => {
          let isUpdate = true;
          try {
            // await eventBridge.emit(`${id}.BEFORE_PLUGIN_DESTROY`);
            isUpdate = await PluginUtil.beforeDestroyPlugin(id, 100);
          } catch (error) {
            isUpdate = true;
            console.log('该应用无__BEFORE_PLUGIN_DESTROY__相关方法，忽略');
          }
          if (isUpdate) {
            await eventBridge.proxy.spaceManager.batchCloseRooms([id]);
            // await eventBridge.proxy.removePlugin({ id });
            if (this.update.pluginInfo && this.update.pluginInfo.packageName && this.update.pluginInfo.packageURL) {
              await eventBridge.proxy.downloadAndUnzipPlugin(this.update.pluginInfo.packageURL, this.update, true);
            }
            // 此处通知侧边栏更新，不然侧边栏打开会形成版本差
            eventBridge.emit('UPDATE_STORE_APP_LIST');
            Message.success(i18n.t('components.webview.updateSuccess'));
          }
        },
      });
    },
    setIsDragging(tag) {
      this.isDragging = tag;
    },
    focusWebview() {
      if (this.processId) { eventBridge.proxy.spaceManager.setFocusSpace(this.processId, this.spaceId); }
      recordAppStatisticalData('open_web_page', this.meta.id);
    },
    initWebview() {
      if (typeof window.finstation !== 'object') {
        return;
      }
      if (this.meta.actions) {
        const { actions = [] } = this.meta;
        actions.forEach((item = {}) => {
          const { type } = item;
          if (type === 'TOOLBOX') {
            this.toolbox = {
              visible: true,
              editorVisible: false,
              ...item,
            };
          }
        });
      }
      this.resetWebview();
      const webview = this.$refs.webview;

      if (webview) {
        // 兼容知识库临时代码
        webview.addEventListener('dom-ready', this.specialHandler);
        // 临时代码结束
        this.resetCallback = window.finstation.loadWebviewContent(webview, {
          spaceId: this.spaceId,
          processId: this.processId,
          webviewId: this.webviewId,
          ...this.meta,
          url: this.url,
        });

        webview.addEventListener('focus', this.focusWebview);
        webview.addEventListener('did-navigate', this.updateWebviewStatus);
        webview.addEventListener('did-navigate-in-page', this.updateWebviewStatus);
      
        eventBridge.on('WEBVIEW_TAB_ACTIVE', this.handleWebviewTabActive);
      }
    },
    specialHandler() {
      // 临时兼容知识库代码
      const webview = this.$refs.webview;
      // webview.openDevTools();
      if (/\/webapps\/pages\/knowledge/.test(this.url)) {
        const userAgent = webview.getUserAgent();
        const replaceStr = userAgent.replace('FinChat', 'FC');
        webview.setUserAgent(replaceStr);
      }
      webview.removeEventListener('dom-ready', this.specialHandler);
    },
    reloadWebview() {
      const webview = this.$refs.webview;
      const url = webview ? webview.getURL() : this.url;
      if (url === 'about:blank') {
        this.initWebview();
        return;
      }
      if (webview) webview.reload();
    },
    refreshWebview(payload = {}) {
      payload.webviewId = this.webviewId;
      window.finstation.loadWebviewContent(this.$refs.webview, {
        ...payload,
      });
    },
    getWebviewUrl() {
      const webview = this.$refs.webview;
      const url = webview ? webview.getURL() : '';
      return url === 'about:blank' ? '' : url;
    },
    goBack() {
      this.$refs.webview.goBack();
    },
    updateWebviewStatus() {
      const webview = this.$refs.webview;
      this.backButtonVisible = (this.url.startsWith('https://') || this.url.startsWith('http://')) && webview.canGoToOffset(-1);
    },
    resetWebview() {
      if (this.resetCallback) this.resetCallback();
      const webview = this.$refs.webview;
      webview.removeEventListener('focus', this.focusWebview);
      webview.removeEventListener('did-navigate', this.updateWebviewStatus);
      webview.removeEventListener('did-navigate-in-page', this.updateWebviewStatus);
    
      eventBridge.remove('WEBVIEW_TAB_ACTIVE', this.handleWebviewTabActive);
    },
    copyLink() {
      if (window.finstation) {
        try {
          window.finstation.copy(this.getWebviewUrl() || this.url);
          this.$Message.success(i18n.t('components.webview.linkCopied'));
        } catch (err) {
          console.log('[Copy Url] fail');
        }
      }
    },
    openInBrowser() {
      if (this.url && window.finstation) {
        window.finstation.openExternal(this.getWebviewUrl() || this.url);
      }
    },
    // 打开机器人房间
    // 暂时隐藏, 需要完善
    async sendMessage() {
      const roomId = await eventBridge.proxy.inviteFriend({
        userId: this.meta.fcId,
        name: this.meta.title,
      });
      if (roomId) {
        eventBridge.proxy.spaceManager.setSpace({
          type: 'chat-view',
          name: this.meta.title,
          id: roomId,
        });
        // 发起聊天时置顶房间
        eventBridge.emit('UPDATE_CHAT_LIST', {
          [roomId]: { toTop: true },
        });
      }
    },
    async forwordMessage() {
      // const roomList = await eventBridge.proxy.getAllSortedRooms();
      const roomList = await eventBridge.proxy.getSortedRoomIdList('lastUpdateTime', 'roomId');
      openSelectWindow({
        list: roomList,
        disableList: [],
        key: 'roomId',
      }).then((rooms) => {
        const { meta } = this;
        let { title } = meta;
        try {
          if (!title) {
            title = this.$refs.webview.getTitle();
          }
        } catch (err) {
          console.log('[Webview getTitle err]', err);
        }

        const url = parseURL(meta.url);
        const currentUrl = this.getWebviewUrl();
        rooms.data.forEach((room) => {
          eventBridge.proxy.sendMessage(room.roomId, {
            body: currentUrl || meta.url,
            extra: {},
            info: {
              description: meta.desc || '',
              domain: url.host,
              image: meta.image || '',
              proto: url.protocol,
              title,
              url: currentUrl || meta.url,
            },
            msgtype: 'm.url',
          });
        });
      });
    },

    async handleToolboxConfirm(info) {
      const {
        appId, name, url, logo,
      } = info;
      await ToolboxApi.addTool({
        roomId: this.toolbox.roomId,
        userId: this.toolbox.userId,
        appId,
        toolName: name,
        toolURL: this.getWebviewUrl() || url,
        toolLogo: logo || '',
      }).then(() => {
        eventBridge.emit(`TOOLBOX_UPDATE_${this.toolbox.roomId}`);
        eventBridge.emit('TOOLBOX_EDITOR_HIDE_SUB');
        this.handleToolboxCancel();
        window.finstation.closeWindow();
      }).catch((err) => {
        const msg = err.response.data;
        if (msg) {
          this.$Message.error(msg.error || i18n.t('components.webview.addFailed'));
        }
      });
    },
    handleToolboxCancel() {
      this.toolbox.editorVisible = false;
    },
    handleWebviewTabActive(meta) {
      if (this.meta.id === meta.id) {
        this.focusWebview();
      }
    },
  },
  watch: {
    meta(newVal, oldVal) {
      let currentUrl = this.getWebviewUrl();
      currentUrl = currentUrl.replace(/file:\/\/\/|#.*/g, ''); // 抹去url前面的 file:// , 如遇到 hash 路由, 截取前面部分
      currentUrl = currentUrl.replace(/\//g, '\\');
      if (
        newVal.url !== oldVal.url
        || newVal.url !== currentUrl
        || currentUrl === ''
      ) {
        this.$refs.webview.loadURL(newVal.url);
        if (newVal.appId) {
          this.checkAppUpdate();
        }
      }
    },
    focusPluginId(newVal, oldVal) {
      if (newVal === this.meta.id) {
        const op = this.createdReport ? 'focus' : 'open';
        this.createdReport = true;
        pluginUsageReport({
          userId: this.myId,
          appId: this.meta.id,
          type: 'Plugin',
          platform: 'desktop',
          op,
        });
      }
      if (oldVal === this.meta.id) {
        pluginUsageReport({
          userId: this.myId,
          appId: this.meta.id,
          type: 'Plugin',
          platform: 'desktop',
          op: 'unfocus',
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.webview-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  .header {
    min-height: 40px;
    @extend %flex-horizontal;
    // justify-content: space-between;
    align-items: center;
    width: 100%;
    background: $roombar-unfoc-bg;
    border-bottom: 1px solid $roombar-unfoc-border;
    padding: 0 10px;
    box-sizing: border-box;
    .title {
      padding-right: 8px;
      // width: 70%;
      font-size: 18px;
      font-weight: bold;
      color: $roombar-unfoc-room;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      .finchat-icon {
        margin-right: 5px;
        font-size: 16px;
        cursor: pointer;
      }
    }
    .store-version{
      margin-right: 8px;
      font-size: 12px;
      color: $text-n2;
    }
    .update-btn{
      margin-right: auto;
      cursor: pointer;
      font-size: 12px;
      color: $text-h1;
    }
    .tool-box {
      margin-left: auto;
      white-space: nowrap;
      .finchat-icon {
        font-size: 20px;
        color: $roombar-unfoc-more;
        cursor: pointer;
      }
    }
  }
  &.is-focus {
    .header {
      background: $roombar-foc-bg;
      border-bottom: 1px solid $roombar-foc-border;
      .title {
        color: $roombar-foc-room;
      }
      .panel {
        .finchat-icon {
          color: $roombar-foc-more;
        }
      }
    }
  }
}

.toolbox-mask {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 36px;
  background: $item-normal;
  border-top: 1px solid $border-dark;
  .text {
    display: flex;
    align-items: center;
    cursor: pointer;
    width: 180px;
    height: 28px;
    i {
      font-size: 14px;
    }
    span {
      font-size: 14px;
      line-height: 1;
    }
  }
  // .toolbox-content {
  //   text-align: center;
  //   .logo {
  //     width: 60px;
  //     height: 60px;
  //     margin: 0 auto 16px;
  //     border-radius: 6px;
  //     overflow: hidden;
  //     img {
  //       width: 100%;
  //       height: 100%;
  //     }
  //   }
  //   .title {
  //     margin-bottom: 12px;
  //     color: $text-n1;
  //     font-size: 16px;
  //     white-space: nowrap;
  //   }
  //   .desc {
  //     margin-bottom: 48px;
  //     color: $text-n2;
  //     font-size: 16px;
  //   }
  //   .button {
  //     width: 400px;
  //     height: 40px;
  //     font-size: 16px;
  //     border-radius: 4px;
  //   }
  // }
}
</style>
