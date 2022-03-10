<template>
  <div class="root">
    <div class="container-shell">
      <div class="container-shell-title">jessibuca demo player <span class="tag-version" v-if="version">({{
          version
        }})</span></div>
      <div class="option">
        <span>缓冲(秒):</span>
        <input
            style="width: 50px"
            type="number"
            ref="buffer"
            value="0.2"
            @change="changeBuffer"
        />
        <input
            type="checkbox"
            v-model="useMSE"
            ref="vod"
            @change="restartPlay('mse')"
        /><span>MediaSource</span>
        <input
            type="checkbox"
            v-model="useWCS"
            ref="vod"
            @change="restartPlay('wcs')"
        /><span>webcodecs</span>

      </div>
      <div id="container" ref="container"></div>
      <div class="input">
        <div>输入URL：</div>
        <input
            type="input"
            autocomplete="on"
            ref="playUrl"
            value=""
        />
        <button v-if="!playing" @click="play">播放</button>
        <button v-else @click="pause">停止</button>
      </div>
      <div class="input" v-if="loaded" style="line-height: 30px">
        <button @click="destroy">销毁</button>
        <button v-if="quieting" @click="cancelMute">取消静音</button>
        <template v-else>
          <button @click="mute">静音</button>
          音量
          <select v-model="volume" @change="volumeChange">
            <option value="1">100</option>
            <option value="0.75">75</option>
            <option value="0.5">50</option>
            <option value="0.25">25</option>
          </select>
        </template>
        <span>旋转</span>
        <select v-model="rotate" @change="rotateChange">
          <option value="0">0</option>
          <option value="90">90</option>
          <option value="270">270</option>
        </select>

        <button @click="fullscreen">全屏</button>
        <button @click="screenShot">截图</button>
        <div style="line-height: 30px">
          <input
              type="checkbox"
              ref="operateBtns"
              v-model="showOperateBtns"
              @change="restartPlay"
          /><span>操作按钮</span>
          <input
              type="checkbox"
              ref="operateBtns"
              v-model="showBandwidth"
              @change="restartPlay"
          /><span>网速</span>
          <span v-if="performance">性能：{{ performance }}</span>
        </div>
      </div>
      <div class="input" v-if="loaded">
        <input
            type="checkbox"
            ref="offscreen"
            v-model="useOffscreen"
            @change="restartPlay('offscreen')"
        /><span>离屏渲染</span>

        <select v-model="scale" @change="scaleChange">
          <option value="0">完全填充(拉伸)</option>
          <option value="1">等比缩放</option>
          <option value="2">完全填充(未拉伸)</option>
        </select>
        <button v-if="!playing" @click="clearView">清屏</button>
        <template v-if="playing">
          <select v-model="recordType">
            <option value="webm">webm</option>
            <option value="mp4">mp4</option>
          </select>
          <button v-if="!recording" @click="startRecord">录制</button>
          <button v-if="!recording" @click="stopAndSaveRecord">暂停录制</button>
        </template>

      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "JessibucaDemo",
  setup() {

  }
}
</script>

<style scoped>

</style>