<script setup lang="ts">
import { UiButton, UiFormGroup, UiInput } from '@shgk/vue-course-ui'
import { ref } from 'vue'
import MeetupsAuthForm from '../components/MeetupsAuthForm.vue'
import LayoutAuth from '../components/LayoutAuth.vue'
import { router } from '../router/router.ts'
import { login } from '../api.ts'

const email = ref('demo@email')
const password = ref('password')

async function onSubmit() {
  try {
    await login(email.value, password.value).then(() => {
      const from = router.currentRoute.value.query.from as string | undefined
      router.push(from ? { path: from } : { name: 'index' })
    })
    // Авторизация прошла успешно
  } catch (error) {
    alert((error as Error).message)
  }
}
</script>

<template>
  <LayoutAuth title="Вход">
    <MeetupsAuthForm @submit="onSubmit">
      <UiFormGroup label="Email">
        <UiInput v-model="email" name="email" type="email" placeholder="demo@email" large required />
      </UiFormGroup>

      <UiFormGroup label="Пароль">
        <UiInput v-model="password" name="password" type="password" placeholder="password" large required />
      </UiFormGroup>

      <template #submit>
        <UiButton kind="primary" type="submit" wide size="large">Войти</UiButton>
      </template>

      <template #append>
        Нет аккаунта?
        <router-link to="/register">
          Зарегистрируйтесь
        </router-link>
      </template>
    </MeetupsAuthForm>
  </LayoutAuth>
</template>
