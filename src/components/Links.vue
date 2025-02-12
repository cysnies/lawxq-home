<template>
  <div v-if="groupedSiteLinks.length > 0" class="links">
    <div class="line">
      <Icon size="20">
        <Link />
      </Icon>
      <span class="title">{{ currentGroupTitle }}</span>
    </div>
    <!-- 网站列表 -->
    <Swiper
      v-if="groupedSiteLinks.length > 0"
      :modules="[Pagination, Mousewheel]"
      :slides-per-view="1"
      :space-between="40"
      :pagination="{
        el: '.swiper-pagination',
        clickable: true,
        bulletElement: 'div',
      }"
      :mousewheel="true"
      @swiper="onSwiper"
      @slideChange="onSlideChange"
    >
      <SwiperSlide v-for="(group, index) in groupedSiteLinks" :key="index">
        <el-row class="link-all" :gutter="20">
          <el-col v-for="(item, itemIndex) in group.links" :span="8" :key="itemIndex">
            <div
              class="item cards"
              :style="itemIndex < 3 ? 'margin-bottom: 20px' : null"
              @click="jumpLink(item)"
            >
              <Icon size="26">
                <component :is="siteIcon[item.icon]" />
              </Icon>
              <span class="name text-hidden">{{ item.name }}</span>
            </div>
          </el-col>
        </el-row>
      </SwiperSlide>
      <div class="swiper-pagination" />
    </Swiper>
  </div>
</template>

<script setup>
import { Icon } from "@vicons/utils";
// 可前往 https://www.xicons.org 自行挑选并在此处引入
import { Link, Blog, CompactDisc, Cloud, Compass, Book, Fire, Windows, Android } from "@vicons/fa"; // 注意使用正确的类别
import { mainStore } from "@/store";
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination, Mousewheel } from "swiper/modules";
import siteLinks from "@/assets/siteLinks.json";

const store = mainStore();

// 网站链接图标
const siteIcon = {
  Blog,
  Cloud,
  CompactDisc,
  Compass,
  Book,
  Fire,
  Windows,
  Android,
};

// 计算分组后的网站链接
const groupedSiteLinks = computed(() => {
  return siteLinks.map((group) => {
    return {
      groupTitle: group.group,
      links: group.links
    };
  });
});

// 当前分组标题
const currentGroupTitle = ref('');

// Swiper 实例
let swiperInstance = null;

// 初始化 Swiper 实例
const onSwiper = (swiper) => {
  swiperInstance = swiper;
  updateCurrentGroupTitle(swiper.activeIndex);
};

// 切换分组时更新当前分组标题
const onSlideChange = () => {
  if (swiperInstance) {
    updateCurrentGroupTitle(swiperInstance.activeIndex);
  }
};

// 更新当前分组标题
const updateCurrentGroupTitle = (index) => {
  currentGroupTitle.value = groupedSiteLinks.value[index].groupTitle;
};

// 链接跳转
const jumpLink = (data) => {
  if (data.name === "音乐" && store.musicClick) {
    if (typeof $openList === "function") $openList();
  } else {
    window.open(data.link, "_blank");
  }
};

onMounted(() => {
  console.log(siteLinks);
});
</script>

<style lang="scss" scoped>
.links {
  .line {
    margin: 2rem 0.25rem 1rem;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    animation: fade 0.5s;
    .title {
      margin-left: 8px;
      font-size: 1.15rem;
      text-shadow: 0 0 5px #00000050;
    }
  }
  .swiper {
    left: -10px;
    width: calc(100% + 20px);
    padding: 5px 10px 0;
    z-index: 0;
    .swiper-slide {
      height: 100%;
    }
    .swiper-pagination {
      margin-top: 12px;
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: center;
      :deep(.swiper-pagination-bullet) {
        background-color: #fff;
        width: 30px;
        height: 8px;
        margin: 0 4px;
        border-radius: 10px;
        opacity: 0.2;
        transition: opacity 0.3s;
        &.swiper-pagination-bullet-active {
          opacity: 1;
        }
        &:hover {
          opacity: 1;
        }
      }
    }
  }
  .link-all {
    height: 220px;
    .item {
      height: 100px;
      width: 100%;
      display: flex;
      align-items: center;
      flex-direction: row;
      justify-content: center;
      padding: 0 10px;
      animation: fade 0.5s;

      &:hover {
        transform: scale(1.02);
        background: rgb(0 0 0 / 40%);
        transition: 0.3s;
      }

      &:active {
        transform: scale(1);
      }

      .name {
        font-size: 1.1rem;
        margin-left: 8px;
      }
      @media (min-width: 720px) and (max-width: 820px) {
        .name {
          display: none;
        }
      }
      @media (max-width: 720px) {
        height: 80px;
      }
      @media (max-width: 460px) {
        flex-direction: column;
        .name {
          font-size: 1rem;
          margin-left: 0;
          margin-top: 8px;
        }
      }
    }
    @media (max-width: 720px) {
      height: 180px;
    }
  }
}
</style>