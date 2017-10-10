<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    1.请求登录授权认证操作<br/>
    <button @click='authLogin'>Login</button>
    2.请求登录授权认证操作<br/>
    <button @click='getAuthInfo'>getAuthInfo</button>
    3.分享列表<br/>
    <button @click='shareAction'>分享</button>
    4.定位数据<br/>
    <button @click='locationAction'>定位</button>
  </div>
</template>

<script type="text/javascript">
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      auths: []
    }
  },
  created () {
    alert('登录')
    let _this =this
    // 监听plusready事件  
    document.addEventListener( "plusready", function(){
      // 扩展API加载完毕，现在可以正常调用扩展API
      // 获取授权列表
      plus.oauth.getServices( function(services){
          console.log('services = ' + JSON.stringify(services[0]) + JSON.stringify(services[1]) + JSON.stringify(services[2]) + JSON.stringify(services[3]));
        console.log('services = ' + services);
        _this.auths = services;
        console.log('_this.auths = ' + _this.auths);
      }, function(e){
        alert( "获取分享服务列表失败："+e.message+" - "+e.code );
      } );
      //定位
      plus.geolocation.getCurrentPosition(function(p){
        alert('Geolocation\nLatitude:' + p.coords.latitude + '\nLongitude:' + p.coords.longitude + '\nAltitude:' + p.coords.altitude);
      }, function(e){
        alert('Geolocation error: ' + e.message);
      } );
      // 推送
      // 监听plusready事件  
      document.addEventListener( "plusready", function(){
        // 扩展API加载完毕，现在可以正常调用扩展API
        // 获取客户端标识信息
        var info = plus.push.getClientInfo();
        alert( JSON.stringify( info ) );
        // 添加监听从系统消息中心点击消息启动事件
        plus.push.addEventListener( "click", function ( msg ) {
          // 分析msg.payload处理业务逻辑 
          alert( "You clicked: " + msg.content ); 
        }, false ); 
      }, false );
    }, false );
  },
  methods: {
    authLogin () {
        let _this =this
//      var s = auths[3];
        var s;
        console.log('_this.auths = ' + _this.auths);
        for (var i = 0; i < _this.auths.length; i++){
          if (_this.auths[i].id == 'weixin'){
           s = _this.auths[i];
           break; 
          }
        }
      if ( !s.authResult ) {
        s.login( function(e){
          alert( "登录认证成功！" );
        }, function(e){
          alert( "登录认证失败！" +JSON.stringify(e));
        } );
      }else{
        alert( "已经登录认证！" );
      }
    },
    getAuthInfo () {
      // 指向问题，使用this报错，至于为什么，还不知道
      let _this =this
//      var s = auths[3];
        var s;
        for (var i = 0; i < _this.auths.length; i++){
          if (_this.auths[i].id == 'weixin'){
           s = _this.auths[i];
           break; 
          }
        }
      if ( !s.authResult ) {
        alert("未登录授权！");
      } else {
        s.getUserInfo( function(e){
          alert( "获取用户信息成功："+JSON.stringify(s.userInfo) );
          console.log("获取用户信息成功："+JSON.stringify(s.userInfo))
          console.log("获取用户信息成功：e"+ JSON.stringify(e))
        }, function(e){
          alert( "获取用户信息失败："+e.message+" - "+e.code );
        } );
      }
    },
    shareAction () {
      // 监听plusready事件  
      document.addEventListener("plusready", function(){
        // 扩展API加载完毕，现在可以正常调用扩展API
      }, false);
      plus.share.sendWithSystem({content:'分享内容',href:'http://www.dcloud.io/'}, function(){
        console.log('分享成功');
      }, function(e){
        console.log('分享失败：'+JSON.stringify(e));
      });
    },
    locationAction () {
      alert('定位')
      // 扩展API加载完毕后调用onPlusReady回调函数 
      document.addEventListener('plusready', onPlusReady, false);
      // 扩展API加载完毕，现在可以正常调用扩展API
      function onPlusReady(){
        plus.geolocation.getCurrentPosition(function(p){
          alert('Geolocation\nLatitude:' + p.coords.latitude + '\nLongitude:' + p.coords.longitude + '\nAltitude:' + p.coords.altitude);
        }, function(e){
          alert('Geolocation error: ' + e.message);
        } );
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to _this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
