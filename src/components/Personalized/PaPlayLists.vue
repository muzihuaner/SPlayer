<template>
  <div class="paplaylists">
    <n-h3 class="title" prefix="bar">
      推荐歌单
      <span class="more" @click="router.push('/discover/playlists')">更多</span>
    </n-h3>
    <CoverLists
      :listData="personalizedData"
      :loadingNum="12"
      :gridCollapsed="true"
    />
  </div>
</template>

<script setup>
import { getPersonalized } from "@/api";
import { useRouter } from "vue-router";
import { formatNumber } from "@/utils/timeTools.js";
import CoverLists from "@/components/DataList/CoverLists.vue";
const router = useRouter();

// 推荐数据
const personalizedData = ref([]);

// 获取推荐数据
const getPersonalizedData = (type = null, limit = 12) => {
  getPersonalized(type, limit).then((res) => {
    personalizedData.value = [];
    if (res.result) {
      res.result.forEach((v) => {
        personalizedData.value.push({
          id: v.id,
          cover: v.picUrl,
          name: v.name,
          playCount: formatNumber(v.playCount),
        });
      });
    } else {
      $message.error("歌单推荐内容为空");
    }
  });
};

onMounted(() => {
  getPersonalizedData();
});
</script>

<style lang="scss" scoped>
.paplaylists {
  margin-top: 40px;
  padding: 0 4px;
  .title {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-left: 16px;
    .more {
      font-size: 14px;
      transition: all 0.3s;
      cursor: pointer;
      &::after {
        content: ">";
        margin-left: 6px;
      }
      &:hover {
        color: $mainColor;
      }
    }
  }
}
</style>