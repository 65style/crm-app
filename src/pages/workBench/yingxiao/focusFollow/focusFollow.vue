<template>
  <!-- 重点关注 -->
  <view class="focus-follow-page">
    <view class="filter-btn-group">
      <view
        class="type"
        :class="{ active: mask === 1 }"
        @tap="handleOperation(1)"
        >颜色 <text class="iconfont icon-arrow-right"></text
      ></view>
      <view
        class="status"
        :class="{ active: mask === 2 }"
        @tap="handleOperation(2)"
        >筛选 <text class="iconfont icon-arrow-right"></text
      ></view>
      <view class="staff" :class="{ active: mask === 3 }" @tap="selectStaff"
        >员工 <text class="iconfont icon-arrow-right"></text
      ></view>
    </view>
    <view class="mask-wrap" v-show="mask" @tap="clickMask">
      <view class="type-container" v-show="mask === 1" @tap.stop="">
        <view>白</view>
        <view>红</view>
        <view>黄</view>
        <view>绿</view>
        <view>蓝</view>
      </view>
      <view class="sort-container" v-show="mask === 2" @tap.stop="">
        <view class="time-select">
          <view class="time-select-label">
            信息来源
          </view>
          <view class="time-select-list">
            <view
              class="time-select-item"
              :class="{ selected: selectedId.includes(item.id) }"
              v-for="(item) in dateRangeList"
              :key="item.id"
              @tap.stop="selectedItem(item)"
              >{{ item.label }}</view
            >
          </view>
        </view>
        <view class="filter-bottom">
          <!-- <view style="color: #007AFF;">自定义筛选</view> -->

          <button
            type="primary"
            size="mini"
            plain="true"
            @tap.stop="filterReset"
          >
            重置
          </button>
          <button type="primary" size="mini" @tap="filterConfirm">确定</button>
        </view>
      </view>
    </view>

    <view class="list-container">
      <scroll-view
        scroll-y="true"
        enableBackToTop="true"
        @scrolltolower="loadMore()"
        style="flex:1"
      >
        <checkbox-group @change="changeCheckbox">
          <view
            class="item"
            :class="{ textColor: item.bgColor !== '#ffffff' }"
            v-for="item in listData"
            :key="item.id"
            :style="'background-color: ' + item.bgColor"
          >
            <label>
              <checkbox :value="item.cusId" :checked="item.checked" />
            </label>
            <view class="item-right">
              <view class="item-right-top">
                <view>
                  <text>{{ item.cusName }} ({{ item.cusId }})</text>
                  <!-- <text class="cus-type">{{ item.type }}</text> -->
                </view>
                <view style="font-size: 24upx;"
                  >营销经理：{{ item.manageName }}</view
                >
              </view>
              <view class="item-right-center">
                <text>信息来源：网络渠道</text>
                <text>联系电话：{{ item.phone }}</text>
              </view>
              <view class="item-right-bottom">
                <text v-if="item.follow">跟进记录：{{ item.follow }}</text>
                <text v-else style="color: red;">请及时跟进</text>
              </view>
            </view>
          </view>
        </checkbox-group>

        <view class="loading-more" v-if="isLoading || listData.length > 10">
          <text class="loading-more-text">{{ loadingText }}</text>
        </view>
      </scroll-view>
    </view>

    <view class="fixed-bottom">
      <checkbox-group @change="changeAll" style="width: auto;">
        <label> <checkbox value="all" :checked="false" /> 全选 </label>
      </checkbox-group>
      <view class="operation" style="flex: 1">
        <button size="mini" @tap="settingColor">
          设置颜色
        </button>
        <button type="warn" size="mini" @tap="handleRemove">取消关注</button>
        <!-- <button type="primary" size="mini" @tap="handleLunHu">轮呼</button>
        <picker
          style="line-height: 0;"
          :range="array"
          :value="index"
          @change="selectLunHu"
        >
          <button type="primary" size="mini">随机轮呼</button>
        </picker> -->
      </view>
    </view>
    <w-picker
      mode="linkage"
      :level="3"
      @confirm="onConfirm"
      ref="linkage"
      :linkList="linkList"
      themeColor="#007aff"
    ></w-picker>
    <w-picker
      mode="selector"
      @confirm="onConfirmColor"
      ref="selector"
      :selectList="bgColor"
      themeColor="#007aff"
    ></w-picker>
  </view>
</template>

<script>
import wPicker from "@/components/w-picker/w-picker.vue";
export default {
  components: {
    wPicker
  },
  data() {
    return {
      isLoading: false,
      loadingText: "加载更多...",
      bgColor: [
        { label: "红", value: "#9b2a2a" },
        { label: "绿", value: "#6b8e22" },
        { label: "黄", value: "#ffa500" },
        { label: "蓝", value: "#4169ff" },
        { label: "白", value: "#ffffff" }
      ],
      listData: [
        {
          id: 1,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 2,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 3,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 4,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 5,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 6,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 7,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 8,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 9,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 10,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        }
      ],
      mask: false,
      linkList: [
        {
          label: "中心1",
          value: "10000",
          children: [
            {
              label: "部门1",
              value: "",
              children: [
                {
                  label: "员工1",
                  value: "20"
                }
              ]
            }
          ]
        },
        {
          label: "中心2",
          value: "10000",
          children: [
            {
              label: "部门2",
              value: "",
              children: [
                {
                  label: "员工2",
                  value: "20"
                }
              ]
            }
          ]
        }
      ],
      dateRangeList: [
        {
          id: 1,
          label: "网络渠道"
        },
        {
          id: 2,
          label: "广告渠道"
        },
        {
          id: 3,
          label: "热线直呼"
        },
        {
          id: 4,
          label: "市场推广"
        },
        {
          id: 5,
          label: "其他渠道"
        },
        {
          id: 6,
          label: "客户转介绍"
        }
      ],
      selectedId: [],
      grabArr: []
    };
  },
  onLoad() {
    this.listData.forEach(item => {
      var bgColor = this.bgColor[
        Math.floor(Math.random() * this.bgColor.length)
      ].value;
      this.$set(item, "bgColor", bgColor);
    });
  },
  methods: {
    onConfirmColor(e) {
      console.log(e)
      console.log(this.grabArr)
      this.listData.forEach(item => {
        if (this.grabArr.includes(item.cusId)) {
          item.bgColor = e.checkArr.value
        }
      })
    },
    settingColor() {
      if (this.grabArr.length) {
        console.log(this.grabArr);
        this.$refs.selector.show()
      } else {
        uni.showToast({
          icon: "none",
          title: "请先选择客户"
        });
      }
    },
    // 取消关注
    handleRemove() {
      if (this.grabArr.length) {
        uni.showModal({
          title: "提示",
          content: "确定要把选中的客户取消关注吗？",
          success: res => {
            if (res.confirm) {
            }
          }
        });
      } else {
        uni.showToast({
          icon: "none",
          title: "请选择需要取消关注的客户"
        });
      }
    },
    // 单选
    changeCheckbox(e) {
      this.grabArr = e.detail.value;
      console.log(this.grabArr);
    },
    // 全选
    changeAll(e) {
      let values = e.detail.value;
      let isChecked = values.length ? true : false;
      let arr = [];
      this.listData.forEach(item => {
        this.$set(item, "checked", isChecked);
        if (item.checked) {
          arr.push(item.cusId);
        }
      });
      this.grabArr = arr;
      console.log(this.grabArr);
    },
    loadMore(e) {
      setTimeout(() => {
        this.getList();
      }, 500);
    },
    getList(i) {
      let data = [
        {
          id: 1,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",

          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 2,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 3,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 4,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 5,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 6,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 7,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 8,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 9,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        },
        {
          id: 10,
          cusName: "胡德兵",
          cusId: "3837",
          phone: "1388889999",
          type: "潜在",
          manageName: "张三",
          follow: "最后一次跟进记录"
        }
      ];
      data.forEach(item => {
        var bgColor = this.bgColor[
          Math.floor(Math.random() * this.bgColor.length)
        ].value;
        this.$set(item, "bgColor", bgColor);
      });
      this.listData = this.listData.concat(data);
    },
    filterConfirm() {},
    filterReset() {
      this.selectedId = [];
    },
    selectedItem(item) {
      console.log(this.selectedId.includes(item.id));
      if (this.selectedId.includes(item.id)) {
        console.log("inclues");
        var index = this.selectedId.indexOf(item.id);
        this.selectedId.splice(index, 1);
      } else {
        this.selectedId.push(item.id);
      }
    },
    // 筛选员工
    selectStaff() {
      this.$refs.linkage.show();
    },
    handleOperation(flag) {
      console.log(this.mask);
      if (flag === 0) {
        this.isOpenOperationBtn = !this.isOpenOperationBtn;
        this.mask = 0;
      } else {
        this.mask = flag;
        this.isOpenOperationBtn = false;
      }
    },
    clickMask() {
      this.mask = false;
    }
  }
};
</script>

<style>
.focus-follow-page {
  display: flex;
  flex-direction: column;
  height: 100%;
  position: relative;
}
.list-container {
  flex: 1;
  display: flex;
  width: 100%;
  overflow: hidden;
}
.item {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 20upx 30upx;
  background-color: #ffffff;
  position: relative;
}

.item::after {
  position: absolute;
  z-index: 3;
  right: 0;
  bottom: 0;
  left: 0;
  height: 1px;
  content: "";
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
  background-color: #c8c7cc;
}
.item-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  /* margin-left: 20upx; */
}
.item-right-top,
.item-right-center {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.item-right-center,
.item-right-bottom {
  font-size: 24upx;
}

.loading-more {
  align-items: center;
  justify-content: center;
  padding-top: 10px;
  padding-bottom: 10px;
  text-align: center;
}

.loading-more-text {
  font-size: 28upx;
  color: #999;
}
.filter-btn-group {
  width: 100%;
  height: 80upx;
  background-color: #ffffff;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  position: relative;
}
.filter-btn-group .iconfont {
  font-size: 22upx;
  vertical-align: middle;
  margin: 0 8upx;
  color: #cccccc;
}
.filter-btn-group::after {
  position: absolute;
  z-index: 3;
  right: 0;
  bottom: 0;
  left: 0;
  height: 1px;
  content: "";
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
  background-color: #c8c7cc;
}
.filter-btn-group > button {
  margin: 0 10upx;
}
.status.active,
.type.active,
.staff.active {
  color: #007aff;
}
.mask-wrap {
  position: absolute;
  top: 40px;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 7;
}
.sort-container,
.filter-container,
.type-container {
  background-color: #ffffff;
}
.sort-container > view,
.type-container > view {
  line-height: 80upx;
  padding: 0 36upx;
  border-bottom: 1px solid #f2f2f2;
}
.sort-container > view:last-child,
.type-container > view:last-child {
  border: none;
}
.time-select {
  display: flex;
  flex-direction: column;
  padding: 20upx 30upx;
  background-color: #ffffff;
  border-bottom: 1upx solid #f1f1f1;
}

.time-select-label {
  line-height: 80upx;
  flex-shrink: 0;
  font-size: 28upx;
  color: #999999;
}

.time-select-list {
  flex: 1;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.time-select-item {
  width: 150upx;
  height: 64upx;
  line-height: 64upx;
  margin: 15upx 20upx;
  background-color: #f4f4f4;
  border-radius: 10upx;
  color: #666666;
  text-align: center;
  font-size: 24upx;
}

.time-select-item.selected {
  background-color: #007aff;
  color: #ffffff;
}

.filter-bottom {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  height: 100upx;
  padding: 0 36upx;
  border-top: 1px solid #f2f2f2;
}
.fixed-bottom {
  z-index: 10;
  height: 100upx;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 0 30upx;
  background-color: #ffffff;
  border-top: 1upx solid #cccccc;
}
.fixed-bottom .operation {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
}
.fixed-bottom .operation button {
  padding: 0 1em;
  margin: 0;
  /* margin-left: 20upx; */
  font-size: 24upx;
}
.item.textColor {
  color: #ffffff;
}

</style>
