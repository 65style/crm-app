<template>
  <!-- 发布公告 -->
  <view class="annouce-page">
    <scroll-view scroll-y="true" style="height: 100%">
      <form @submit="submit">
        <view class="list-item">
          <view class="item-label required">公告标题</view>
          <view class="item-value">
            <input type="text" name="title" placeholder="请输入公告标题" />
          </view>
        </view>
        <view class="list-item">
          <view class="item-label required">公告类型</view>
          <view class="item-value">
            <picker @change="changeType" :value="typeIndex" :range="typeArr">
              <text :class="{ selected: typeIndex === null }">{{
                typeIndex === null ? "请选择公告类型" : typeArr[typeIndex]
              }}</text>
            </picker>
          </view>
        </view>

        <view class="list-textarea">
          <view class="item-label">公告内容</view>
          <view class="item-value">
            <!-- <textarea name="checkOpinion" placeholder="请输入审核意见" /> -->
            <editor
              id="editor"
              class="ql-container"
              :placeholder="placeholder"
              @ready="onEditorReady"
              @blur="onEditorBlur"
            ></editor>
            <button type="primary" @tap="onPush">发布</button>
          </view>
        </view>
        <button ref="submitBtn" form-type="submit"></button>
      </form>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      typeArr: ["公告通告", "行政制度", "意见建议", "系统优化", "员工手册"],
      typeIndex: null,
      placeholder: "开始输入..."
    };
  },
  methods: {
    onEditorReady() {
      uni
        .createSelectorQuery()
        .select("#editor")
        .context(res => {
          this.editorCtx = res.context;
          console.log(this.editorCtx)
        })
        .exec();
    },
    onEditorBlur(e) {
      console.log(e.detail)
    }
  }
};
</script>

<style>
.annouce-page {
  height: 100%;
  background-color: #ffffff;
  overflow: hidden;
}
.list-item {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  height: 94upx;
  padding: 0 30upx;
  border-bottom: 1upx solid #f2f2f2;
}
.item-value {
  flex: 1;
  text-align: right;
  font-size: 32upx;
}
.item-value input {
  /* padding-right: 20upx; */
  text-align: right;
}

.list-textarea {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 0 30upx;
  border-bottom: 1upx solid #f2f2f2;
}
.list-textarea .item-label {
  height: 94upx;
  line-height: 94upx;
}
.list-textarea > .item-value {
  width: 100%;
}
.list-textarea textarea {
  flex: 1;
  font-size: 32upx;
  width: 100%;
  height: 160upx;
  text-align: left;
}
.item-label.required::after {
  content: "*";
  color: red;
}
.selected {
  color: #848484;
}
.text-gray {
  color: gray;
}
</style>
