<template>
  <a-form
    class="form"
    :model="formState"
    name="basic"
    autocomplete="off"
    @finish="onFinish"
    @finishFailed="onFinishFailed"
  >
    <a-form-item
      class="item1"
      v-model="username"
      label="Username"
      name="username"
      :rules="[{ required: true, message: 'Please input your username!' }]"
    >
      <a-input v-model:value="formState.username" class="input" />
    </a-form-item>

    <a-form-item
      class="item2"
      v-model="password"
      label="Password"
      name="password"
      :rules="[{ required: true, message: 'Please input your password!' }]"
    >
      <a-input-password v-model:value="formState.password" />
    </a-form-item>

    <a-form-item name="remember" class="item3">
      <a-checkbox v-model:checked="formState.remember">Remember me</a-checkbox>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 16 }" class="item4">
      <a-button type="primary" html-type="submit" class="btn">Submit</a-button>
    </a-form-item>
  </a-form>
</template>

<script setup>
import { reactive } from 'vue'
import _ from 'lodash'
import { getUserData } from './api/api'
const formState = reactive({
  username: '',
  password: '',
  remember: true,
})
const onFinish = () => {
  getUserData().then((res) => {
    const arr = res.data.userList
    const user = _.find(arr, { username: formState.username })
    const pass = _.find(arr, { username: formState.password })
    if (formState.username == user && formState.password == pass) {
      alert('login success!')
    } else {
      alert('login faild!')
    }
  })
}

const onFinishFailed = (errorInfo) => {
  console.log('Failed:', errorInfo)
}
</script>

<style scoped lang="css">
.form {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(10, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  margin-top: 320px;
}
.item1 {
  width: 300px;
  justify-self: center;
  grid-area: 4 / 2 / 5 / 3;
  transform: scale(1.5);
}
.item2 {
  transform: scale(1.5);
  justify-self: center;
  width: 300px;
  grid-area: 5 / 2 / 6 / 3;
}
.item3 {
  transform: scale(1.1);
  margin-left: -250px;
  width: 180px;
  justify-self: center;
  grid-area: 6 / 2 / 7 / 3;
}
.item4 {
  transform: scale(1.33);
  justify-self: center;
  grid-area: 7 / 2 / 8 / 3;
  margin-left: -135px;
  position: absolute;
  top: 730px;
}
.btn {
  width: 150px;
}
</style>
