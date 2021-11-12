<template>
  <view> </view>
</template>

<script>
import {
  uniList,
  uniListChat,
  uniIcons,
  uniBadge,
  uniFab,
} from "@dcloudio/uni-ui";
export default {
  components: { uniList, uniListChat, uniIcons, uniBadge, uniFab },
  data() {
    return {
      avatarList: [
        {
          url: "https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png",
        },
        {
          url: "https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png",
        },
        {
          url: "https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png",
        },
      ],
    };
  },
  onLoad() {},
  methods: {
    fabOnClick() {
      uni.navigateTo({
        url: "/pages/solitaire/create",
      });
    },
    onClick(e) {
      console.log(e.url);
    },
  },
};
</script>

<style>
</style>
