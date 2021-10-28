<template lang="pug">
.page.page-pwd-sendmail(@keyup.enter="submit")
  b-field(label="사용자 이름")
    b-input(v-model="model.username")
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
      }
    }
  },
  methods: {
    async submit () {
      if (!this.model.username) {
        this.$buefy.toast.open({
          duration: 3000,
          message: '사용자 이름을 입력해 주세요.',
          type: 'is-warning'
        })
        return
      }
      const resp = await request({
        method: 'post',
        path: 'users/find/password',
        body: {
          username: this.model.username
        }
      })
      this.$router.push({
        path: '/users/password/sendmail-success',
        query: { username: this.model.username, email: resp.data.email }
      })
    }
  }
}
</script>
