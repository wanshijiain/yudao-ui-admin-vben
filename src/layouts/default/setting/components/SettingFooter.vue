<script lang="ts" setup>
import { unref } from 'vue'

import { CopyOutlined, RedoOutlined } from '@ant-design/icons-vue'

import { useAppStore } from '@/store/modules/app'
import { usePermissionStore } from '@/store/modules/permission'
import { useMultipleTabStore } from '@/store/modules/multipleTab'
import { useUserStore } from '@/store/modules/user'

import { useI18n } from '@/hooks/web/useI18n'
import { useMessage } from '@/hooks/web/useMessage'
import { copyText } from '@/utils/copyTextToClipboard'
import { updateColorWeak } from '@/logics/theme/updateColorWeak'
import { updateGrayMode } from '@/logics/theme/updateGrayMode'
import defaultSetting from '@/settings/projectSetting'
import { changeTheme } from '@/logics/theme'
import { updateSidebarBgColor } from '@/logics/theme/updateBackground'

defineOptions({ name: 'SettingFooter' })

const permissionStore = usePermissionStore()
const { t } = useI18n()
const { createSuccessModal, createMessage } = useMessage()
const tabStore = useMultipleTabStore()
const userStore = useUserStore()
const appStore = useAppStore()

function handleCopy() {
  copyText(JSON.stringify(unref(appStore.getProjectConfig), null, 2), null)
  createSuccessModal({
    title: t('layout.setting.operatingTitle'),
    content: t('layout.setting.operatingContent'),
  })
}
function handleResetSetting() {
  try {
    appStore.setProjectConfig(defaultSetting)
    const { colorWeak, grayMode, themeColor } = defaultSetting
    changeTheme(themeColor)
    updateSidebarBgColor()
    updateColorWeak(colorWeak)
    updateGrayMode(grayMode)
    createMessage.success(t('layout.setting.resetSuccess'))
  }
  catch (error: any) {
    createMessage.error(error)
  }
}

function handleClearAndRedo() {
  localStorage.clear()
  appStore.resetAllState()
  permissionStore.resetState()
  tabStore.resetState()
  userStore.resetState()
  location.reload()
}
</script>

<template>
  <div class="flex flex-col items-center">
    <a-button type="primary" block @click="handleCopy">
      <CopyOutlined class="mr-2" />
      {{ t('layout.setting.copyBtn') }}
    </a-button>

    <a-button color="warning" block class="my-3" @click="handleResetSetting">
      <RedoOutlined class="mr-2" />
      {{ t('common.resetText') }}
    </a-button>

    <a-button color="error" block @click="handleClearAndRedo">
      <RedoOutlined class="mr-2" />
      {{ t('layout.setting.clearBtn') }}
    </a-button>
  </div>
</template>
