<template lang="pug">
  .modal(v-if="visible" class="is-active" v-cloak)
    .modal-background(@click="closeModal")
    .modal-card
      header.modal-card-head
        p.modal-card-title 내 메뉴
      section.modal-card-body
        nav.level
          .level-item.has-text-centered
            a.field.has-addons(@click="getUserInfo")
              p 내 정보
        hr                
        nav.level
          .level-item.has-text-centered
            a.field.has-addons
            router-link(to="/MyWriteFeed")
              p 내 글 보기
        hr
        nav.level
          .level-item.has-text-centered
            a.field.has-addons
            router-link(to="/MyGroupFeed")
              p 새 글 보기
        hr
        nav.level
          .level-item.has-text-centered
            a.field.has-addons
            router-link(to="/MyFollowList")
              p 팔로잉 / 팔로워
        hr
        nav.level
          .level-item.has-text-centered
            a.field.has-addons(@click="signOut")
              p 로그아웃
      footer.modal-card-foot
        button.button.is-primary(@click="closeModal") 확인
        button.button(@click="closeModal") 취소
      // level-right 요소로 우측정렬 가능.. 빈 요소라도 level-left는 언제나 필수! 
      my-setting(close_message="close lightbox" ref='my_setting')

</template>

<script>
import MySetting from '../Main/MySetting';

export default {
  components:{
    MySetting,
  },
  props: {
    close_message: {
      type: String,
      default: 'Close Modal'
    },
    is_visible: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      visible: this.is_visible
    }
  },
  computed:{
    
  },
  methods: {
    signOut(){
      this.$http.post('http://api.thekym.com/member/logout/')
      .then(response => {
        let token = response.data.token;
        let pk = response.data.user;
        if ( window.localStorage.getItem('token') ) {
          window.localStorage.removeItem('token', token);
          window.localStorage.removeItem('pk', pk)
          window.localStorage.removeItem('searchKeyword')
          window.localStorage.removeItem('user_email')
          window.localStorage.removeItem('user_img')
          window.localStorage.removeItem('user_nickname')
          window.localStorage.removeItem('user_username')
          window.localStorage.removeItem('this_group')
        }
        this.$router.push( {path: "/"} );
        alert("성공적으로 로그아웃 하셨습니다.")
      })
      .catch(error => {
        console.log(error.response);
      })
    },
    closeModal(){
      this.visible = false;
    },
    getUserInfo(){
      let user_token = window.localStorage.getItem('token');
      this.$http.get('http://api.thekym.com/member/', this.user,
      { headers: {'Authorization' : `Token ${user_token}`}})
                .then(response => {
          
                })
                .catch(error => console.log(error.response));
                this.$refs.my_setting.visible = true;
    }
  }
}
</script>
<style lang="sass" scoped>
  hr
    // margin-top: 5px
    // margin-bottom: 5px
</style>
