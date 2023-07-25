<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch" />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="post" tab="文章">
        <PostList :postList="postList" />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片">
        <PictureList :pictureList="pictureList" />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UserList :userList="userList" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import PostList from '@/components/PostList.vue'
import PictureList from '@/components/PictureList.vue'
import UserList from '@/components/UserList.vue'
import MyDivider from '@/components/MyDivider.vue'
import { ref, watchEffect } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import myAxios from '@/plugins/myAxios'
const postList = ref([])
const userList = ref([])
const pictureList = ref([])
const loadData = (params: any) => {
  const query = {
    ...params,
    searchText: params.text,
  }
  myAxios.post('/search/all', params).then((res: any) => {
    postList.value = res.postList
    userList.value = res.userList
    pictureList.value = res.pictureList
  })
}

const router = useRouter()
const route = useRoute()
const activeKey = route.params.category
const initSearchParams = {
  text: '',
  pageSize: 10,
  pageNum: 1,
}
const searchParams = ref(initSearchParams)
// 搜索参数同步url
const onSearch = (value: string) => {
  router.push({
    query: searchParams.value,
  })
  loadData(searchParams.value)
}
onSearch('')
// url改变页面状态
watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text as string,
  }
})
// 切换tab同步url
const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  })
}
</script>
