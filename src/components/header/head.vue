<template>
    <header class="head" id="head">
        <section class="head-top">
            <section class="logo">
                <img src="@/image/logo.png" alt="">
            </section>
        <section class="nav">
            <ul>
                <li>
                    首页
                </li>
                <li class="active">
                    <router-link to="/systemManage">系统管理</router-link>
                </li>
                <li>
                    <router-link to="/customerManagement">客户管理</router-link>
                </li>
            </ul>
        </section>
    </header>
</template>

<script>
import HeaderAjax from '@/api/Header/Header'
import { message } from '@/components/Message/Message'
export default {
  data () {
    return {
      msg: '这是头部',
      userName: '',
      companyName: '',
      showSetting: false,
      odlPasswordError: ''
    }
  },
  mounted () {
    this.userMessage()
  },
  methods: {
    logOut () {
      sessionStorage.removeItem('token')
      localStorage.removeItem('tabsArr')
      localStorage.removeItem('tabsFlagArr')
      this.$router.push({path: '/'})
    },
    userMessage () {
      let userInfo = JSON.parse(sessionStorage.getItem('userInfo'))
      this.userName = userInfo.name
      this.companyName = userInfo.enterpriseName
    },
    clickCancelForm (cancel) {
      this.showSetting = false
    },
    clickSureForm (formData) {
      HeaderAjax.ModifyPassword(formData).then(response => {
        console.log(response)
        if (response.code === 200) {
          this.showSetting = false
          message({
            message: '修改密码成功！',
            type: 'success'
          })
        } else if (response.code === 400) {
          this.odlPasswordError = response.message
        }
      })
    }
  },
  components: {
  }
}
</script>

<style lang="less" scoped>
@import '../../style/base.less';
.head {
    height: 104px;
    .head-top {
        padding: 0  0 0 20px;
        width: 100%;
        height: 64px;
        background: #fff;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .logo {
            img {
                width: 128px;
                height: 40px;
            }
        }
        .info {
            height: 100%;
            display: flex;
            ul {
                display: flex;
                flex-direction: row;
                align-items: center;
                li {
                    height: 64px;
                    line-height: 64px;
                    margin-left: 15px;
                    padding: 0 15px;
                    .mixin-sc(@text-title,@color-title);
                    &.company-li {
                        cursor: pointer;
                        &:hover {
                            .company-icon {
                                .mixin-bis(18px, 18px, -11px, -51px);
                            }
                        }
                    }
                    &.info-li {
                        position: relative;
                        top: 1px;
                        cursor: pointer;
                        &:hover {
                            .info-icon {
                                .mixin-bis(20px, 18px, -50px, -51px);
                            }
                        }
                    }
                    .icon {
                        margin-right: 10px;
                    }
                    .company-icon {
                        display: inline-block;
                        position: relative;
                        top: 3px;
                        .mixin-bis(18px, 18px, -11px, -11px);
                    }
                    .info-icon {
                        display: inline-block;
                        position: relative;
                        top: 3px;
                        .mixin-bis(20px, 18px, -50px, -11px);
                    }
                    .personal-icon {
                        display: inline-block;
                        position: relative;
                        top: -1px;
                        .mixin-bis(16px, 20px, -90px, -10px);
                    }
                }
                li.username-li {
                    padding-right: 30px;
                    display: flex;
                    flex-direction: row;
                    align-items: center;
                    position: relative;
                    top: 1px;
                    cursor: pointer;
                    i::after {
                        margin-left: 10px;
                        display: block;
                        content: "";
                        .minxin-triangle;
                        cursor: pointer;
                    }
                    .dropdown {
                        display: flex;
                        flex-direction: column;
                        display: none;
                        width: 126px;
                        height: 174px;
                        position: absolute;
                        top: 61px;
                        right: 5px;
                        padding: 12px;
                        background: #fff;
                        z-index: 9999;
                        border: 1px solid #eee;
                        li {
                            margin-top: 8px;
                            margin-left: 0;
                            width: 100%;
                            height: 32px;
                            line-height: 32px;
                            text-align: left;
                            font-size: 14px;
                            border: none;
                            border-radius: 4px;
                            cursor: pointer;
                            &:first-child {
                                margin-top: 0;
                            }
                            &:hover {
                                background: #f3f3f3;
                                color: #3399cc;
                            }
                        }
                    }
                    &:hover {
                        .personal-icon {
                            .mixin-bis(16px, 20px, -90px, -50px);
                        }
                        .dropdown {
                            display: block;
                        }
                    }
                }
            }
        }
    }
    .nav {
        ul {
            width: 100%;
            height: 40px;
            display: flex;
            flex-direction: row;
            align-items: center;
            background: #35485e;
            li {
                height: 40px;
                line-height: 40px;
                margin: 0 10px;
                color: #fff;
                a {
                    display: block;
                    width: 100%;
                    padding: 0 20px;
                    text-decoration: none;
                    color: #fff;
                }
                .router-link-exact-active {
                    background: @color-extrude;
                }
            }
        }
    }
}
</style>
