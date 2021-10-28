<template lang="pug">
.page.page-find-id-success
  p
    span 사용자님께서 회원가입 시 기입하셨던 아이디는&nbsp;
    span.username [ {{ username }} ]
    span &nbsp;입니다.
  <br><br>
  p.login 
    nuxt-link(to="/login") 로그인
    span
    nuxt-link(to="/users/password/sendmail") 비밀번호 초기화
</template>

<script>

export default {
  asyncData ({ query, store }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '아이디 찾기'
    })
    return {
      username: query.username
    }
  }
}
</script>

<style lang="scss">
  .page.page-find-id-success {
    p {
      font-size: 1.1rem;
    }
    p span.username {
      font-size: 1.7em;
    }
    p.login {
      font-size: 1.3rem;
      span {
        padding-left: 20px;
      }
    }
  }
</style>
