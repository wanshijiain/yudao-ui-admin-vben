<script lang="ts" setup>
import { List } from 'ant-design-vue'
import { secureSettingList } from './data'
import PasswordModal from './PasswordModal.vue'
import { CollapseContainer } from '@/components/Container/index'
import { useModal } from '@/components/Modal'
import { useMessage } from '@/hooks/web/useMessage'

const ListItem = List.Item
const ListItemMeta = List.Item.Meta

const { createMessage } = useMessage()
const [registerModal, { openModal }] = useModal()

function handleEdit(title: string) {
  if (title === '账户密码')
    openModal(true, {})
}
function handleSuccess() {
  createMessage.success('更新成功！')
}
</script>

<template>
  <CollapseContainer title="安全设置" :can-expan="false">
    <List>
      <template v-for="item in secureSettingList" :key="item.key">
        <ListItem>
          <ListItemMeta>
            <template #title>
              {{ item.title }}
              <div v-if="item.extra" class="float-right mr-7.5 mt-2.5 cursor-pointer font-normal text-blue-500">
                <a-button type="link" @click="handleEdit(item.title)">
                  {{ item.extra }}
                </a-button>
              </div>
            </template>
            <template #description>
              <div>{{ item.description }}</div>
            </template>
          </ListItemMeta>
        </ListItem>
      </template>
    </List>
  </CollapseContainer>
  <PasswordModal @register="registerModal" @success="handleSuccess" />
</template>
