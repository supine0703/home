<template>
  <footer id="footer" :class="store.footerBlur ? 'blur' : null">
    <Transition name="fade" mode="out-in">
      <div v-if="!store.playerState || !store.playerLrcShow" class="power">
        <span>
          <span :class="startYear < fullYear ? 'c-hidden' : 'hidden'">Copyright</span>
          &copy;
          <span v-if="startYear < fullYear" class="site-start">
            {{ startYear }}
            -
          </span>
          {{ fullYear }}
          <a :href="siteUrl">{{ siteAuthor }}</a>
        </span>
        <!-- made by 信息转移到了`ProjInfo`模块中了 -->
        <!-- 以下信息请不要修改哦 -->
        <!-- <span class="hidden">
          &nbsp;|&nbsp;&nbsp;Made&nbsp;by
          <a :href="config.github" target="_blank">
            {{ config.author }}
          </a>
        </span> -->
        <!-- 站点备案 -->
        <span v-if="siteIcp || siteNet">
          &nbsp;&nbsp;|&nbsp;&nbsp;
          <!-- 公网安备案 -->
          <span v-if="siteNet">
            <img src="/备案图标.png" class="net" />
            <span>&nbsp;</span>
            <a
              href="https://beian.mps.gov.cn/#/query/webSearch?code=34010402704171"
              rel="noreferrer"
              target="_blank"
            >
              {{ siteNet }}
            </a>
            <!-- &amp; -->
            &nbsp;
          </span>
          <!-- ICP备案 -->
          <a href="https://beian.miit.gov.cn" target="_blank">
            {{ siteIcp }}
          </a>
        </span>
      </div>
      <div v-else class="lrc">
        <Transition name="fade" mode="out-in">
          <div class="lrc-all" :key="store.getPlayerLrc">
            <music-one theme="filled" size="18" fill="#efefef" />
            <span class="lrc-text text-hidden" v-html="store.getPlayerLrc" />
            <music-one theme="filled" size="18" fill="#efefef" />
          </div>
        </Transition>
      </div>
    </Transition>
  </footer>
</template>

<script setup>
import { MusicOne } from "@icon-park/vue-next";
import { mainStore } from "@/store";
// import config from "@/../package.json";

const store = mainStore();
const fullYear = new Date().getFullYear();

// 加载配置数据
// const siteStartDate = ref(import.meta.env.VITE_SITE_START);
const startYear = ref(
  import.meta.env.VITE_SITE_START?.length >= 4
    ? import.meta.env.VITE_SITE_START.substring(0, 4)
    : null,
);
const siteIcp = ref(import.meta.env.VITE_SITE_ICP);
const siteNet = ref(import.meta.env.VITE_SITE_NET);
const siteAuthor = ref(import.meta.env.VITE_SITE_AUTHOR);
const siteUrl = computed(() => {
  const url = import.meta.env.VITE_SITE_URL;
  if (!url) return "https://imlzl.cn";
  // 判断协议前缀
  if (!url.startsWith("http://") && !url.startsWith("https://")) {
    return "//" + url;
  }
  return url;
});
</script>

<style lang="scss" scoped>
#footer {
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  height: 46px;
  line-height: 46px;
  text-align: center;
  z-index: 0;
  font-size: 14px;
  // 文字不换行
  word-break: keep-all;
  white-space: nowrap;
  .power {
    animation: fade 0.3s;
    .net {
      padding-bottom: 4px;
      vertical-align: middle;
      width: 1.2rem;
    }
  }
  .lrc {
    padding: 0 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    .lrc-all {
      width: 98%;
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      .lrc-text {
        margin: 0 8px;
      }
      .i-icon {
        width: 18px;
        height: 18px;
        display: inherit;
      }
    }
  }
  &.blur {
    backdrop-filter: blur(10px);
    background: rgb(0 0 0 / 25%);
    font-size: 16px;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.15s ease-in-out;
  }
  @media (max-width: 720px) {
    font-size: 0.9rem;
    &.blur {
      font-size: 0.9rem;
    }
    .power .net {
      width: 1rem;
    }
  }
  @media (max-width: 650px) {
    font-size: 0.75rem;
    &.blur {
      font-size: 0.75rem;
    }
    .power .net {
      width: 0.8rem;
    }
  }
  @media (max-width: 560px) {
    .c-hidden {
      display: none;
    }
    &.blur {
      font-size: 0.7rem;
    }
    .power .net {
      width: 0.75rem;
    }
  }
  @media (max-width: 480px) {
    .hidden {
      display: none;
    }
    font-size: 0.6rem;
    &.blur {
      font-size: 0.6rem;
    }
    .power .net {
      width: 0.7rem;
    }
  }
}
</style>
