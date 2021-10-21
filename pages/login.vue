<template lang="pug">
.page.page-login(@keyup.enter="submit")
  b-field(label="사용자 이름")
    b-input(placeholder="사용자 이름을 입력하세요" v-model="model.username")
  b-field(label="비밀번호")
    b-input(placeholder="비밀번호를 입력하세요" v-model="model.password" type="password")
  button.button.is-primary(@click="submit") 로그인
  p
    | 계정이 없나요?
    = " "
    nuxt-link(to="/signup") 회원가입
</template>

<script>
import { mapState } from 'vuex'
import Cookies from 'js-cookie'
import request from '~/utils/request'

const setToken = (token) => {
  Cookies.set('jwt', token, { path: '/' })
}

export default {
  asyncData ({ store, redirect }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '로그인'
    })
  },
  data () {
    return {
      model: {
        username: '',
        password: ''
      }
    }
  },
  computed: {
    ...mapState(['user'])
  },
  methods: {
    async submit () {
      try {
        const { data: { token } } = await request({
          method: 'post',
          path: 'authentication',
          body: { username: this.model.username, password: this.model.password }
        })
        setToken(token)
        await location.replace('/')
        await this.$buefy.toast.open({
          duration: 3000,
          message: '로그인 성공',
          type: 'is-success'
        })
      } catch (err) {
        const errMsg = await err.response.data.message;
        if (errMsg === 'User does not exist') {
            await this.$buefy.toast.open({
            duration: 3000,
            message: '사용자 이름을 찾을 수 없습니다.',
            type: 'is-danger'
          })
        }
        if (errMsg === 'Password does not match') {
            await this.$buefy.toast.open({
            duration: 3000,
            message: '비밀번호가 맞지 않습니다.',
            type: 'is-danger'
          })
        }
        if (errMsg === 'Email does not confirmed') {
            await this.$buefy.toast.open({
            duration: 3000,
            message: '이메일 인증을 받아주세요.',
            type: 'is-danger'
          })
        }
      }
    }
  }
}
</script>

<style lang="css">
</style>
