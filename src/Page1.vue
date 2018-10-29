<template>
  <v-ons-page>
    <custom-toolbar>ログインしてください!</custom-toolbar>
    <ons-fab position="bottom left" @click="canTouchAuth()">
      <ons-icon icon="github-alt"></ons-icon>
    </ons-fab>
    <ons-fab position="bottom right" @click="deleteKeyChain()">
      <ons-icon icon="trash"></ons-icon>
    </ons-fab>
    <br />
    <v-ons-list>
      <v-ons-list-header>ID</v-ons-list-header>
      <v-ons-list-item>
        <v-ons-input placeholder="Input your id" float v-model="userInfo.id"></v-ons-input>
      </v-ons-list-item>
      <v-ons-list-header>Password</v-ons-list-header>
      <v-ons-list-item>
        <v-ons-input placeholder="Input your password" float v-model="userInfo.password"></v-ons-input>
      </v-ons-list-item>
    </v-ons-list>
    <br />
    <div style="text-align: center;">
      <v-ons-button @click="login()" :disabled="!canSave">ログイン</v-ons-button>
    </div>
  </v-ons-page>
</template>

<script>
  import customToolbar from './CustomToolbar';
  import page2 from './Page2';
  import ons from 'onsenui'
  export default {
    data () {
      return {
        userInfo: {
          id: '',
          password: ''
        }
      }
    },
    methods: {
      canTouchAuth: function () {
        // 指紋認証可能か
        window.plugins.touchid.isAvailable(() => {
          ons.notification.alert('この端末は指紋認証が可能です', {title: 'Yeah !'});
        }, () => {
          ons.notification.alert('この端末では指紋認証ができません', {title: 'hmmmm !'});
        })
      },
      verify: function () {
        console.log(window.plugins.touchid.verify)
        window.plugins.touchid.verify('userInfo', 'タッチしてください', (userInfo) => {
          // ログイン処理 
          this.login(true)
        },
        (error) => {
          ons.notification.alert('認証に失敗しました')
        });
      },
      // ログインパスワードをキーチェーンに保存する
      login: function (shimonLogin) {
        ons.notification.alert('ログイン成功しました', {title: 'Yeah !',
          callback: () => {
            if (!shimonLogin) {
              this.confirmShimonLogin();
            }
            this.pageStack.push(page2);
          }
        })
      },
      confirmShimonLogin: function () {
        if (window.plugins) {
          ons.notification.confirm({
            message: '次回から指紋認証ログインが可能です。\n指紋認証ログインしますか？',
            callback: (idx) => {
              if (idx === 1) {

                // 入力されたID,Passwordを登録されている指紋と紐付けます
                window.plugins.touchid.save('userInfo', JSON.stringify(this.userInfo));

                window.plugins.touchid.verify('userInfo', 'タッチしてください', (userInfo) => {
                  ons.notification.alert('設定画面からも指紋認証設定が可能です。', {title: '成功！'});
                },
                // キーが登録されていない場合は-1が帰ってくる
                function (hoshino) {
                  alert(JSON.stringify(hoshino))
                });
              } else {
                ons.notification.alert('設定画面からも指紋認証設定が可能です。', {title: 'おしらせ'});
              }
            }
          });
        }
      },
      deleteKeyChain: function () {
        if (window.plugins) {
          window.plugins.touchid.delete('userInfo', () => {
            ons.notification.alert('指紋認証設定を削除しました', {title: '認証設定削除'});
          });
        }
      }
    },
    props: ['pageStack'],
    components: { customToolbar },
    computed: {
      canSave: function () {
        return this.userInfo.id && this.userInfo.password
      }
    },
    mounted () {
      document.addEventListener('deviceready', () => {
        // キー'userInfo' が存在するかどうか
        window.plugins.touchid.has('userInfo', () => {
          // 存在すれば指紋認証ダイアログ表示
          this.verify();
        }, () => {
          // keyが存在しない
          console.log('指紋に値を紐づけていない');
        });
      }, false);
    }
  }
</script>
