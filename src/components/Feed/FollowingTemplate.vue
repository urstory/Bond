<template lang="pug">
  article.media.for-hr
    figure.media-left
      p.image.is-48x48.img-user-48
        img.img-user-profile(:src='following.profile_img', alt='Image')
    .media-content
      .content
        p.namelist.name {{following.nickname}}
    
    .media-right
      button(type="submit" ).card-footer-item.btn-show-like
        span.icon-like
          button.tag.is-rounded.is-follow(v-if="!following.is_follow" type="submit" @click="addFollow(following.pk)") 팔로우
          button.tag.is-rounded.is-primary(v-else type="submit" @click="deleteFollow(following.pk)") 팔로잉
</template>

<script>
export default {
  props: [
    'following'
  ],
  methods: {
    addFollow(pk) {
      let user_token = window.localStorage.getItem('token');
      this.$http.post('http://api.thekym.com/member/relation/',
       {to_user: pk},
       {headers: {'Authorization' : `Token ${user_token}`}}
      )
      .then(response=> {
        let user_token = window.localStorage.getItem('token');
        let pk = window.localStorage.getItem('pk');
        // let path = null;
        // let page_num = this.$parent.page_num
        this.$http.get( 'http://api.thekym.com/member/' + `${pk}` + '/following/',
        { headers: {'Authorization' : `Token ${user_token}`}})
                  .then(response => {
                    let followings = response.data;
                    this.$parent.following_list = followings;
                    
                    })
                  .catch(error => console.log(error.message))
      }
      )
      // .catch(error => console.log(error.response))
      .catch(error => alert(error.response.data.to_user))
    },
    deleteFollow(pk) {
      let user_token = window.localStorage.getItem('token');
      this.$http.delete('http://api.thekym.com/member/relation/',{
        headers: {'Authorization' : `Token ${user_token}`},
        data:{"to_user":pk}
       }
      )
      .then(response=> {
        let user_token = window.localStorage.getItem('token');
        let pk = window.localStorage.getItem('pk');
        // let path = null;
        // let page_num = this.$parent.page_num
        this.$http.get('http://api.thekym.com/member/' + `${pk}` + '/following/',
        { headers: {'Authorization' : `Token ${user_token}`}})
                  .then(response => {
                    let followings = response.data;
                    this.$parent.following_list = followings;
                    
                    })
                  .catch(error => console.log(error.message))
      }
      )
      .catch(error => alert(error.response.data.to_user))
    },
  }
}
</script>

<style lang="sass" scoped>
@import "~bulma"
@import "~style"

body
  background: #eee
.page-wrapper
  min-height: 115vh

.img-user-48
  background: #eee
  width: 48px
  height: 48px
  overflow: hidden
  border-radius: 50%

.img-user-profile
  min-height: 100%
  width: 100%
.group-img-small-wrapper
  width: 32px
  height: 32px
  overflow: hidden
  border-radius: 10%
.group-img-small
  width: 100%
  min-height: 100%
.namelist,
  padding-top: 13px
// .tag.is-rounded
//   margin-top: 13px
.name
  margin-left: 20%
.card-header-title
  font-size: 25px
  padding-left: 0
.pagination.is-centered
  padding-top: 15px
.pagination-btn
  color: $bond
.card-wrapper
  min-height: 80vh
.is-follow
  background: none
  border: 1px solid $bond
  color: $bond
.li-hr
  // padding: 0
  margin: 10px 0
.for-hr
  // background: yellow
  position: relative
  border-top: 1px solid hsl(0, 0%, 86%)
  padding: 10px 0


</style>