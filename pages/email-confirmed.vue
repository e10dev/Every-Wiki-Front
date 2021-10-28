<template lang="pug">
.page.page-email-confirmed
  p 이메일 인증이 완료되었습니다.
  p {{ username }} 님!
  p 저희 Every Wiki 에서 즐거운 시간 보내세요!
</template>

<script>
import request from '~/utils/request'

export default {
  asyncData ({ query, store }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '이메일 인증'
    })
    return {
      username: decodeURIComponent(query.username)
    }
  },
  async mounted() {
    try {
      const username = this.$route.query.username
      const code = this.$route.query.code
      await request({
        method: 'get',
        path: 'email-confirm',
        query: {
          username: username,
          code: code
        }
      })
    } catch (err) {
      const errMsg = await err.response.data.message
      if (errMsg === 'User does not exist') {
        await this.$buefy.toast.open({
        duration: 3000,
        message: '사용자 이름을 찾을 수 없습니다.',
        type: 'is-danger'
        })
      }
    }
  }
}
</script>
