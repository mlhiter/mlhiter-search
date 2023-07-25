<template>
  <a-list item-layout="horizontal" :data-source="postList">
    <template #renderItem="{ item }">
      <a-list-item>
        <a-list-item-meta :description="item.content">
          <template #title>
            {{ item.title }}
          </template>
          <template #avatar>
            <a-avatar :src="profile" />
          </template>
        </a-list-item-meta>
      </a-list-item>
    </template>
  </a-list>
</template>

<script lang="ts" setup>
import profile from '../assets/profile.jpeg'
interface Props {
  postList: any[]
}
const props = withDefaults(defineProps<Props>(), {
  postList: () => [],
})
</script>

<style lang="sass" scoped></style>
