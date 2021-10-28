<template lang="pug">
.page.page-pwd-reset(@keyup.enter="submit")
  b-field(label="사용자 이름")
    b-input(v-model="model.username" disabled=true)
  b-field(label="비밀번호")
    b-input(placeholder="비밀번호를 입력하세요" v-model="model.password" type="password")
  b-field(label="비밀번호 확인")
    b-input(placeholder="비밀번호를 한번 더 입력하세요" v-model="model.passwordcheck" type="password")
  button.button.is-primary(@click="submit") 확인
</template>

<script>
import request from '~/utils/request'

export default {
  asyncData ({ store }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '비밀번호 초기화'
    })
  },
  data () {
    return {
      model: {
        username: '',
        password: '',
        passwordcheck: ''
      }
    }
  },
  async mounted() {
    try {
      const username = this.$route.query.username
      const code = this.$route.query.code

      this.model.username = username

      await request({
        method: 'get',
        path: 'email-confirm',
        query: {
          username: username,
          code: code,
          pwdreset: true
        }
      })
    } catch (err) {
      console.log(err)
    }
  },
  methods: {
    async submit () {
      try {
        if (this.model.password !== this.model.passwordcheck) {
          this.$buefy.toast.open({
            duration: 3000,
          message: '비밀번호를 확인해주세요.',
          type: 'is-warning'
        })
        return
      }
      await request({
        method: 'post',
        path: 'users/find/password/reset',
        body: {
          username: this.model.username,
          password: this.model.password
        }
      })
      await this.$buefy.toast.open({
        duration: 3000,
        message: '비밀번호 초기화 성공',
        type: 'is-success'
      })
      await location.replace('/login')
      } catch(err) {
          await this.$buefy.toast.open({
            duration: 3000,
            message: '이전 비밀번호와 같습니다.',
            type: 'is-danger'
          })
      }
    }
  }
}
</script>
