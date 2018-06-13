# A project for code reuse

## Directory

* httpservice

* httpinterceptor


### httpservice

```javascript
    import axios from 'axios'
    import {
      config
    } from '../config.js'
    import {
      Toast
    } from 'mint-ui'
    
    const Get = function (url, params) {
      url = config.baseUrl + url;
      return new Promise((resolve, reject) => {
        axios.get(url, {
          params: params
        }).then(function (response) {
          if (response.data.code == '01') {
            var data = response.data.data;
            resolve(data);
          } else {
            Toast(response.data.msg);
          }
        }).catch(function (err) {
          Toast('网络错误,请检查');
        });
      })
    }
    const Post = function (url, data) {
      url = config.baseUrl + url;
      return new Promise((resolve, reject) => {
        axios.post(url, data)
          .then(function (response) {
            console.log(response);
            if (response.data.code == '01') {
              var data = response.data.data;
              resolve(data);
            } else {
              Toast(response.data.msg);
              resolve(response.data.code);
            }
          }).catch(function (err) {
            Toast('网络错误,请检查');
          });
      })
    }
    
    export const httpRequest = {
      get: Get,
      post: Post
    };
```

### httpinterceptor

```javascript
    import {Indicator,Toast} from 'mint-ui'
    var loadingTimer = null, whiteList=['getPayResult','GetPayConfig','getShopNewOrder'];
    var isInWhiteList=function (url) {
      var flag=false;
      whiteList.forEach(function (e,i) {
        if(url.indexOf(e)>=0)
        {
          flag=true;
        }
      })
      return flag;
    }
    export const request = function (config) {
      if(!isInWhiteList(config.url)){
        Indicator.open({
          text: '拼命加载中...',
          spinnerType: 'fading-circle'
        })
      }
      let token=sessionStorage.token
      if(token){
        config.headers['token']=token;
      }
      return config
    }

    export const response = function (response) {
      clearTimeout(loadingTimer)
      loadingTimer = setTimeout(() => {
        Indicator.close()
        clearTimeout(loadingTimer)
      }, 300)
      return response
    }
    export const responseError = function (error) {
      Toast(error.message)
      clearTimeout(loadingTimer)
      loadingTimer = setTimeout(() => {
        Indicator.close()
        clearTimeout(loadingTimer)
      }, 300)
    }
```