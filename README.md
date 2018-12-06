# vue-monaca-touch　Monaca ✖︎ Vue.jsで指紋認証機能を実装してみた 

# Tech
- monaca
- cordova
- vue.js
- OnsenUI

# Description
### 目的
- cordova-plugin-keychain-touch-idプラグインの検証  
自分がやっているプロジェクトに指紋認証が追加できるか、工数はどれくらいかを見るのが目的です。
- 新しくなった [Monaca IDE](https://ja.monaca.io/)でトランスパイルが必要なプロジェクトが構築できるようになったので試したかった  

手元にiOSしかなかったのでiOSのみでの検証です。

### プラグイン
ロジック的には、localstrageのgetトリガーを指紋情報にした感じでしょうか。この手のプラグインは何種類かありますが、両OSに対応しているので選定しました。

Touch authentication login test with hybrid application using cordova plugin (https://github.com/sjhoeksma/cordova-plugin-keychain-touch-id)
This plugin enables touch authentication on Android and iOS.
But I tried only iOS Device...

# Building
<b>iOS</b>
```
git clone 
cd vue-monaca-touch
npm install
cordova plugin add ios
npm run build
cordova prepare
```
cordova prepareが済むと、xcodeprojが作成されます。これをxcodeで開き、実機にビルドします。
monacaで作成した場合は、ビルド画面からビルドします。  
After, please building at xcode
