<template lang="pug">
.page.page-email-confirmed
  p {{ username }} 님!
  p
    span 저희 Every Wiki 에 회원가입시 사용한 이메일&nbsp;
    span.email {{ email }}
    span  으로 메일을 보냈습니다.
  p 메일을 확인해주세요.
</template>

<script>

export default {
  asyncData ({ query, store }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '비밀번호 초기화'
    })
    return {
      email: query.email,
      username: query.username
    }
  },
}
</script>

<style lang="scss">
  .page.page-email-confirmed {
    p {
      span.email {
        font-size: 1.3rem;
      }
    }
  }
</style>
