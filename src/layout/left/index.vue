<template>
  <div style="background: var(--left-bg-color)">
    <div style="background: var(--left-bg-color)" class="h-30px"></div>
    <main class="left min-w-64px h-full p-[0_6px_40px] box-border flex-col-center select-none" data-tauri-drag-region>
      <p class="text-(16px [--left-text-color]) cursor-default select-none m-[4px_0_16px_0]">HuLa</p>
      <!-- 头像模块 -->
      <LeftAvatar />
      <!-- 导航选项按钮模块 -->
      <ActionList />
      <!-- 编辑资料弹窗 -->
      <InfoEdit />

      <!-- 弹出框 -->
      <component :is="componentMap" v-bind="componentProps" />
    </main>
  </div>
</template>
<script lang="ts" setup>
import LeftAvatar from './components/LeftAvatar.vue'
import ActionList from './components/ActionList.vue'
import InfoEdit from './components/InfoEdit.vue'
import { useMitt } from '@/hooks/useMitt.ts'
import { modalShow, LockScreen, CheckUpdate, RemoteLogin } from './model.tsx'
import type { Component } from 'vue'
import { MittEnum, ModalEnum } from '@/enums'

const componentMap = shallowRef<Component>()
// 存储要传递给组件的props
const componentProps = shallowRef<Record<string, any>>({})
/** 弹窗组件内容映射 */
const componentMapping: Record<number, Component> = {
  [ModalEnum.LOCK_SCREEN]: LockScreen,
  [ModalEnum.CHECK_UPDATE]: CheckUpdate,
  [ModalEnum.REMOTE_LOGIN]: RemoteLogin
}

onMounted(() => {
  useMitt.on(MittEnum.LEFT_MODAL_SHOW, (event: { type: ModalEnum; props?: Record<string, any> }) => {
    componentMap.value = componentMapping[event.type]
    // 保存传入的props
    componentProps.value = event.props || {}
    nextTick(() => {
      modalShow.value = true
    })
  })
})
</script>

<style lang="scss" scoped>
@use 'style';
</style>
