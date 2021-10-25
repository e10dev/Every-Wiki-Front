<template lang="pug">
.admin-blockchain
  p 블록체인 API 서버 설정입니다.
  b-field(label="active")
    b-switch(v-model="model.active") {{ model.active }}
  b-field(label="host")
    b-input(v-model="model.host")
  b-field(label="port")
    b-input(v-model="model.port")
  .right-wrapper
    button.button.is-primary(@click="submit") 저장
</template>

<script>
import _ from 'lodash'
import request from '~/utils/request'

export default {
  async asyncData ({ params, req, res, error, store, redirect }) {
    store.commit('meta/clear')
    store.commit('meta/update', {
      title: '관리자 페이지 - 블록체인 설정'
    })
    const resp = await request({
      path: `settings/blockchain`,
      method: 'get',
      req,
      res
    })
    return {
      original: {
        ...resp.data.blockchain
      },
      model: {
        ...resp.data.blockchain
      }
    }
  },
  methods: {
    async submit () {
      if (!this.model.host || !Number(this.model.port)) {
        this.$buefy.toast.open({
          duration: 3000,
          message: '모두 입력해 주세요.',
          type: 'is-danger'
        })
        return
      }
      if (!_.isEqual(this.original, this.model) && this.model.host) {
        await request({
          path: `settings/blockchain`,
          method: 'put',
          body: {
            active: this.model.active || false,
            host: this.model.host,
            port: Number(this.model.port)
          }
        })
        await this.$buefy.toast.open({
          duration: 3000,
          message: '설정 완료!',
          type: 'is-success'
        })
        await history.go(0)
      } else {
        await this.$buefy.toast.open({
          duration: 3000,
          message: '설정 값을 확인해주세요.',
          type: 'is-danger'
        })
      }
    }
  }
}
</script>
