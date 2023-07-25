<template>
  <a-list item-layout="horizontal" :data-source="userList">
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card hoverable style="width: 240px">
          <template #cover>
            <img alt="example" :src="profile" />
          </template>
          <a-card-meta :title="item.userName">
            <template #description>{{ item.userProfile }}</template>
          </a-card-meta>
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>

<script lang="ts" setup>
import profile from '../assets/profile.jpeg'
interface Props {
  userList: any[]
}
const props = withDefaults(defineProps<Props>(), {
  userList: () => [],
})
</script>

<style lang="sass" scoped></style>
