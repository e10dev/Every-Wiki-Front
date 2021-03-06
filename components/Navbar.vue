<template lang="pug">
nav.liberty-navbar.navbar
  .container
    .navbar-menu.is-active
      .navbar-start
        nuxt-link.navbar-item.wikiName(active-class="" to="/")
          
        nuxt-link.navbar-item(active-class="" to="/recent-changes")
          b-icon(icon="refresh")
          span.navbar-text 최근게시물
        nuxt-link.navbar-item(active-class="" to="/random")
          b-icon(icon="random")
          span.navbar-text 랜덤게시물
        b-dropdown
          a.navbar-item(slot="trigger")
            b-icon(icon="book")
            span.navbar-text 게시판
            b-icon.navbar-caret(icon="caret-down")
          b-dropdown-item(has-link)
            nuxt-link(:to="`/article/${encodeURIComponent('자유 게시판')}`") 자유 게시판
          b-dropdown-item(has-link)
            nuxt-link(:to="`/article/${encodeURIComponent('학과별 게시판')}`") 학과별 게시판
        b-dropdown
          a.navbar-item(slot="trigger")
            b-icon(icon="upload")
            span.navbar-text 사진업로드
            b-icon.navbar-caret(icon="caret-down")
          b-dropdown-item(has-link v-if="user.isAdmin")
            nuxt-link(to="/admin") 관리자 도구
          b-dropdown-item(has-link)
            nuxt-link(to="/upload") 사진 업로드
        nuxt-link.navbar-item(active-class="" :to="`/article/${encodeURIComponent('문법 도움말')}`")
          b-icon(icon="graduation-cap")
          span.navbar-text 도움말
          //-   b-icon.navbar-caret(icon="caret-down")
          //- b-dropdown-item(has-link)
          //-   nuxt-link(:to="`/article/${encodeURIComponent('문법 도움말')}`") 문법 도움말
          //- b-dropdown-item(has-link)
          //-   nuxt-link(:to="`/article/${encodeURIComponent('틀사용 도움말')}`") 틀사용 도움말
      .user-items
        nuxt-link.navbar-item(v-if="!user.isLoggedIn" to="/login" active-class="")
          b-icon(icon="sign-in")
        template(v-else)
          b-dropdown
            a.navbar-item.user-gravatar-wrapper(slot="trigger")
              v-gravatar.user-gravatar(:email="user.email")
            b-dropdown-item(has-link)
              nuxt-link(:to="`/article/${encodeURIComponent('사용자:' + user.username)}`") 사용자:{{ user.username }}
            hr.dropdown-divider
            b-dropdown-item(has-link)
              nuxt-link(to="/user-settings") 사용자 설정
            hr.dropdown-divider
            b-dropdown-item(has-link)
              a.navbar-item(@click="logout") 로그아웃
          a.navbar-item(role="button" @click="logout")
            b-icon(icon="sign-out")
      .navbar-end
        .search-box-wrapper
          b-field
            b-input.navbar-search-input(placeholder="검색" type="search" icon="search" v-model="searchInput" @keyup.enter.native="go")
            p.control
              button.button(active-class="" @click="go")
                b-icon(icon="arrow-right")
            p.control
              button.button(active-class="" @click="search")
                b-icon(icon="search")
</template>

<script>
import { mapState } from 'vuex'
import Cookies from 'js-cookie'

export default {
  data () {
    return {
      searchInput: ''
    }
  },
  computed: {
    ...mapState(['settings', 'user'])
  },
  methods: {
    go () {
      if (this.searchInput) {
        this.$router.push(`/go/${encodeURIComponent(this.searchInput)}`)
        this.searchInput = ''
      }
    },
    search () {
      if (this.searchInput) {
        this.$router.push(`/search/${encodeURIComponent(this.searchInput)}`)
        this.searchInput = ''
      }
    },
    logout () {
      Cookies.remove('jwt', { path: '/' })
      location.reload()
    }
  }
}
</script>

<style lang="scss">
@import '~assets/style-variables.scss';

.liberty-navbar {
  top: 0;
  z-index: 18 !important;
  position: fixed;
  width: 100%;
  @include touch {
    &>.container {
      display: flex;
      padding-left: 0.5rem;
      padding-right: 0.5rem;
      padding-bottom: 0.5rem;
    }
    .navbar-text {
      display: none;
    }
    .navbar-item.wikiName {
      height: 2rem !important;
    }
    .navbar-menu {
      flex: 1;
      display: flex !important;
      flex-wrap: wrap;
      padding: 0;
      background-color: inherit;
      justify-content: space-between;
    }
    .navbar-start,
    .navbar-end {
      display: flex;
      flex-direction: row;
    }
    .navbar-item {
      padding: 0.25rem !important;
    }
    .navbar-caret {
      display: none;
    }
    .user-gravatar-wrapper {
      display: flex;
    }
    .navbar-end {
      flex: 0 0 100%;
    }
    .search-box-wrapper {
      flex: 1;
      .input,
      .icon.is-left,
      .button {
        height: 2rem;
      }
      .button {
        padding-top: 0;
        padding-bottom: 0;
      }
      .fa {
        font-size: 1.1rem;
      }
    }
    .navbar-search-input {
      width: 100%;
    }
    .user-items {
      .fa {
        font-size: 1rem;
      }
    }
    .user-gravatar {
      max-height: 1.8rem;
    }
  }
  @include desktop {
    .search-box-wrapper {
      margin-right: 0.3rem;
    }
    .navbar-start .navbar-item .icon:first-child {
      margin-right: 0.5rem;
    }
    .user-items {
      order: 3;
    }
  }

  .navbar-item.wikiName {
    float: left;
    height: 2.6rem;
    width: 4.6rem;
    background: transparent url('~@/static/everywiki-logo.png') no-repeat scroll 50%/contain;
  }

  .navbar-menu {
    box-shadow: none;
  }
  .dropdown + .dropdown {
    margin: 0;
  }
  .navbar-start .navbar-item {
    padding: 0.5rem 0.7rem;
    .icon {
      .fa {
        font-size: 1rem;
      }
    }
  }
  .navbar-item {
    white-space: nowrap;
  }
  .navbar-end {
    align-items: center;
  }
  .search-box-wrapper {
    .button:focus {
      box-shadow: none;
    }
  }
  .navbar-search-input {
    input:focus,
    input:active,
    input:hover {
      border-color: rgb(219, 219, 219);
      box-shadow: inset 0 1px 2px hsla(0,0%,4%,.1);
    }
  }
  .user-items {
    display: flex;
    flex-direction: row;
  }
  .user-gravatar {
    max-height: 2rem;
    border-radius: 0.35rem;
    border: 1px solid #e1e8ed;
  }
}
</style>
