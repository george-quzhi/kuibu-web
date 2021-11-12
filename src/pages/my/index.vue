<template>
  <view class="content">
    <view>
      <uni-list>
        <uni-list-item title="列表文字"></uni-list-item>
        <uni-list-item :disabled="true" title="列表禁用状态"></uni-list-item>
      </uni-list>
    </view>
  </view>
</template>

<script>
import { uniCalendar, uniList, uniListItem } from "@dcloudio/uni-ui";
export default {
  components: { uniCalendar, uniList, uniListItem },
  data() {
    return {
      title: "Hello",
    };
  },
  onLoad() {},
  methods: {
    change(e) {
      console.log(e);
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
