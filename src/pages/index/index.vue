<template>
  <view>
    <view class="content">
      <view>
        <uni-calendar
          ref="calendar"
          :insert="true"
          :lunar="true"
          @change="change"
        />
        <uni-list>
          <uni-list-item
            v-for="(event, i) in evnetList"
            :key="i"
            clickable
            @click="onClick()"
            :title="event.name"
          ></uni-list-item>
        </uni-list>
        <uni-fab horizontal="right" @fabClick="fabOnClick"></uni-fab>
      </view>
    </view>
    <!-- <uni-popup animation="false" ref="popup" type="bottom">底部弹出 Popup</uni-popup> -->
  </view>
</template>

<script>
import {
  uniCalendar,
  uniList,
  uniListItem,
  uniFab,
  //   uniTransition,
  //   uniPopup,
} from "@dcloudio/uni-ui";
import moment from "moment";
export default {
  components: {
    uniCalendar,
    uniList,
    uniListItem,
    uniFab,
    // uniTransition,
    // uniPopup,
  },
  data() {
    return {
      searchDate: "",
      evnetList: [],
    };
  },
  onLoad() {},
  onShow() {
    var param = null;
    if (this.searchDate) {
      param = { fulldate: this.searchDate };
    }
    this.change(param);
  },
  methods: {
    change(e) {
      console.log(e);
      this.searchDate = e
        ? e.fulldate
        : moment(new Date()).format("YYYY-MM-DD");
      uni.request({
        method: "GET",
        url: "http://localhost:3000/events", //仅为示例，并非真实接口地址。
        data: { date: this.searchDate },
        // header: {
        //   "custom-header": "hello", //自定义请求头信息
        // },
        success: (res) => {
          console.log(res.data);
          this.evnetList = res.data.data || [];
        },
      });
    },
    fabOnClick() {
      //   this.$refs.popup.open("bottom");
      var param = { date: this.searchDate };
      uni.navigateTo({
        url:
          "/pages/index/create?param=" +
          encodeURIComponent(JSON.stringify(param)),
      });
    },
    onClick(e) {
      uni.navigateTo({
        url: "/pages/index/details",
      });
    },
  },
};
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
