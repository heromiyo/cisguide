<script>
  import Bmob from '../static/bmob/Bmob-1.4.4.min'

  export default {
    onLaunch: function () {
      console.log('onluanch')
      Bmob.User.auth().then(res => {
        console.log(res)
        wx.setStorageSync('userData', res)
        wx.login({
          success: () => {
            let current = Bmob.User.current()
          if (current.nickName !== undefined) {

            } else {
              wx.getUserInfo({
                success: result => {
                  console.log('user Info API: ', result)
                  var objectId = current.objectId
                 // var userInfo = result.userInfo
                  var nickName = result.nickName
                  var avatarUrl = result.avatarUrl

                  var query = Bmob.Query('_User')

                  query.get(objectId).then(res => {
                        res.set('nickName', nickName)
                        res.set('userPic', avatarUrl)
                        res.save()

                        var userData = wx.getStorageSync('userData')
                        userData['nickName'] = nickName
                        userData['userPic'] = avatarUrl
                      wx.setStorageSync('userData', userData)
                  }).catch(err => {
                    console.log('error querying user objectId ', err)
                  })
                }
              })
            }
          }
        })
      })
    },

    created () {
      let userData = wx.getStorageSync('userData')
      console.log(userData)
    }
  }
</script>

<style>
    @import "style/common.css";
    /*.container {*/
        /*height: 100%;*/
        /*display: flex;*/
        /*flex-direction: column;*/
        /*align-items: center;*/
        /*justify-content: space-between;*/
        /*padding: 200rpx 0;*/
        /*box-sizing: border-box;*/
    /*}*/
    /* this rule will be remove */
    * {
        transition: width 2s;
        -moz-transition: width 2s;
        -webkit-transition: width 2s;
        -o-transition: width 2s;
    }
    page {
        height: 100%;
        line-height: 1;
        font-family: 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', 'arial', 'sans-serif', 'Droid Sans Fallback', 'Helvetica neue', 'Microsoft Yahei', 'Helvetica', 'Tahoma', 'lantinghei sc';
        -webkit-tap-highlight-color: transparent;
        background: #fff;
    }
    navigator {
        color: #7e8c8d;
    }

    .navigator-hover{
        background-color: #fff;
        opacity: 1;
    }

    .zan-form {
        background-color: #fff;
        border-top: 0.5px solid #e5e5e5;
        border-bottom: 0.5px solid #e5e5e5;
    }

    .zan-form__item {
        line-height: 26px;
    }

    .zan-form__title {
        float: left;
        width: 80px;
        font-size: 14px;
    }

    .zan-form__title--top {
        align-self: flex-start;
    }

    .zan-form__input, .zan-form__textarea {
        min-height: 26px;
    }

    .zan-form__input input {
        min-height: 26px;
    }

    .zan-form__textarea {
        display: -webkit-flex;
        display: flex;
        align-items: center;
    }

    .zan-form__textarea textarea {
        width: auto;
        flex: 1;
    }

</style>
