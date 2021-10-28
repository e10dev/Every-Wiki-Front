<template lang="pug">
.page.page-find-id(@keyup.enter="submit")
  p 회원가입 시, 기입했던 이메일 주소를 입력해주세요.
  b-field(label="이메일 주소")
    b-input(placeholder="이메일 주소를 입력하세요" v-model="model.email" type="email")
  button.button.is-primary(@click="submit") 아이디 찾기
</template>

<script>
import request from '~/utils/request'

export default {
  asyncData ({ store }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '아이디 찾기'
    })
  },
  data () {
    return {
      model: {
        email: ''
      }
    }
  },
  methods: {
    async submit () {
      try {
        if (!this.model.email) {
          this.$buefy.toast.open({
            duration: 3000,
            message: '이메일을 입력해 주세요.',
            type: 'is-warning'
          })
          return
        }
        const resp = await request({
          method: 'post',
          path: 'users/find/id',
          body: {
            email: this.model.email
          }
        })
        const username = resp.data.username
        this.$router.push({ path: '/users/id/find-success', query: { username: username }})
      } catch(err) {
        const errMsg = await err.response.data.message;
        if (errMsg === 'Cannot find the resource.') {
            await this.$buefy.toast.open({
            duration: 3000,
            message: '등록된 아이디를 찾을 수 없습니다.',
            type: 'is-danger'
          })
        }
      }
    }
  }
}
</script>

<style lang="scss">
.page.page-find-id {
  p {
    padding-bottom: 10px;
  }
  button {
    margin-top: 10px;
  }
}
</style>
