<template>
  <BaseCard>
    <q-form @submit.prevent="handleSubmit">
      <q-card-section class="q-gutter-y-md">
        <div class="text-h6">비밀번호 변경</div>
        <q-input
          v-model="form.oldPassword"
          type="password"
          outlined
          dense
          label="현재 비밀번호"
        />
        <q-input
          v-model="form.newPassword"
          type="password"
          outlined
          dense
          label="새로운 비밀번호"
        />
        <q-input
          v-model="form.newPasswordConfirm"
          type="password"
          outlined
          dense
          label="새로운 비밀번호 확인"
        />
      </q-card-section>
      <q-separator />
      <q-card-actions>
        <q-space />
        <q-btn
          type="submit"
          label="저장하기"
          flat
          color="primary"
          :loading="isLoading"
        />
      </q-card-actions>
    </q-form>
  </BaseCard>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { useAsyncState } from '@vueuse/core';
import BaseCard from 'src/components/base/BaseCard.vue';
import { updateUserPassword } from 'src/services';
import { ref } from 'vue';

const $q = useQuasar();

const { isLoading, execute } = useAsyncState(
  async () => {
    const response = await updateUserPassword(form.value);
    return response;
  },
  null,
  {
    immediate: false,
    throwError: true,
    onSuccess: () => {
      $q.notify('비밀번호가 변경되었습니다.');
      form.value.oldPassword = '';
      form.value.newPassword = '';
      form.value.newPasswordConfirm = '';
    },
    onError: response => {
      $q.notify({
        type: 'negative',
        message: `${response.error}`,
      });
    },
  },
);
const form = ref({
  oldPassword: '',
  newPassword: '',
  newPasswordConfirm: '',
});
const handleSubmit = () => execute(form.value);
// const handleSubmit = async () => {
//   await updateUserPassword(form.value.newPassword);
//   $q.notify('비밀번호가 변경되었습니다.');
//   form.value.newPassword = '';
//   form.value.newPasswordConfirm = '';
// };
</script>

<style lang="scss" scoped></style>
