<template>
  <view class="department-page">
    <scroll-view scroll-y="true" style="height: 100%;">
      <!-- <text class="example-info">列表组件可以在其中使用图标、略缩图或放置任何你想放的元素，使用场景如：导航菜单、列表、设置中心排版等</text>
		<uni-section title="基础用法" type="line"></uni-section> -->
      <uni-list>
        <uni-list-item
          thumb="/static/file.png"
          :show-arrow="true"
          v-for="item in depList"
          :key="item.id"
          :title="item.title"
          @click="handleClick"
        />
      </uni-list>
      <!-- <uni-section title="菜单列表" type="line"></uni-section> -->
      <!-- <uni-list>
			<uni-list-item title="列表右侧带箭头" rightText="右侧文字" />
			<uni-list-item :show-badge="true" title="列表右侧带箭头 + 角标" badge-text="12" />
			<uni-list-item title="列表左侧带略缩图" thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png" />
			<uni-list-item :show-extra-icon="true" :extra-icon="extraIcon1" title="列表左侧带扩展图标" />
			<uni-list-item :show-extra-icon="true" :extra-icon="extraIcon1" :show-switch="true" title="列表右侧带 switch" @switchChange="switchChange" />
			<uni-list-item :disabled="true" :show-extra-icon="true" :extra-icon="extraIcon2" :show-switch="true" :switch-checked="true" title="禁用状态" @switchChange="switchChange" />
		</uni-list> -->
    </scroll-view>
  </view>
</template>

<script>
import uniSection from "@/components/uni-section/uni-section.vue";
import uniList from "@/components/uni-list/uni-list.vue";
import uniListItem from "@/components/uni-list-item/uni-list-item.vue";
export default {
  components: {
    uniSection,
    uniList,
    uniListItem
  },
  data() {
    return {
      depList: [
        {
          id: 1,
          title: "营销①中心"
        },
        {
          id: 2,
          title: "营销②中心"
        },
        {
          id: 3,
          title: "营销③中心"
        },
        {
          id: 4,
          title: "营销④中心"
        },
        {
          id: 5,
          title: "营销四中心"
        }
      ],
    };
  },
  onNavigationBarButtonTap(e) {
    console.log(e);
    uni.showLoading({
    });
  },
  methods: {
    handleClick() {
      uni.navigateTo({
        url: ""
      });
    },
    switchChange(e) {
      uni.showToast({
        title: "change:" + e.value,
        icon: "none"
      });
    }
  }
};
</script>

<style>
.department-page {
  display: flex;
  height: 100%;
  width: 100%;
  flex: 1;
  flex-direction: column;
  overflow: hidden;
  background-color: #fff;
}
/* 头条小程序组件内不能引入字体 */
/* #ifdef MP-TOUTIAO */
@font-face {
  font-family: uniicons;
  font-weight: normal;
  font-style: normal;
  src: url("~@/static/uni.ttf") format("truetype");
}

/* #endif */

/* #ifndef APP-NVUE */

view {
  font-size: 14px;
  line-height: inherit;
}

.example {
  padding: 0 15px 15px;
}

.example-info {
  padding: 15px;
  color: #3b4144;
  background: #ffffff;
}

.example-body {
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  padding: 0;
  font-size: 14px;
  background-color: #ffffff;
}

/* #endif */
.example {
  padding: 0 15px;
}

.example-info {
  /* #ifndef APP-NVUE */
  display: block;
  /* #endif */
  padding: 15px;
  color: #3b4144;
  background-color: #ffffff;
  font-size: 14px;
  line-height: 20px;
}

.example-info-text {
  font-size: 14px;
  line-height: 20px;
  color: #3b4144;
}

.example-body {
  flex-direction: column;
  padding: 15px;
  background-color: #ffffff;
}

.word-btn-white {
  font-size: 18px;
  color: #ffffff;
}

.word-btn {
  /* #ifndef APP-NVUE */
  display: flex;
  /* #endif */
  flex-direction: row;
  align-items: center;
  justify-content: center;
  border-radius: 6px;
  height: 48px;
  margin: 15px;
  background-color: #007aff;
}

.word-btn--hover {
  background-color: #4ca2ff;
}
</style>
