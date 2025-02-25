# 🏀 Vue 使用 axios 完成 AJAX 请求

---

**Vue.js 2.0 版本推荐使用 `axios `来完成 ajax 请求**。

Axios 是一个基于 Promise 的 HTTP 库，可以用在浏览器和 node.js 中。

Github 开源地址： https://github.com/axios/axios

## 1. 安装方法

**使用 cdn:**

```js
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

或

```js
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
```

**使用 npm:**

```shell
$ npm install axios
```

## 2. 发送 GET 请求

比如有如下所示的 json 数据

```json
{
    "name":"网站",
    "num":3,
    "sites": [
        { "name":"Google", "info":[ "Android", "Google 搜索", "Google 翻译" ] },
        { "name":"Runoob", "info":[ "菜鸟教程", "菜鸟工具", "菜鸟微信" ] },
        { "name":"Taobao", "info":[ "淘宝", "网购" ] }
    ]
}
```

### Ⅰ response

```html
<meta charset="utf-8">
<title>Vue</title>
<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
</head>
<body>
<div id="app">
  {{ info }}
</div>
<script type = "text/javascript">
new Vue({
  el: '#app',
  data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .get('https://www.runoob.com/try/ajax/json_demo.json')
      .then(response => (this.info = response))
      .catch(function (error) { // 请求失败处理
        console.log(error);
      });
  }
})
</script>
</body>
</html>
```

运行结果

```
{ "data": { "name": "网站", "num": 3, "sites": [ { "name": "Google", "info": [ "Android", "Google 搜索", "Google 翻译" ] }, { "name": "Runoob", "info": [ "菜鸟教程", "菜鸟工具", "菜鸟微信" ] }, { "name": "Taobao", "info": [ "淘宝", "网购" ] } ] }, "status": 200, "statusText": "", "headers": { "accept-ranges": "bytes", "age": "151207", "ali-swift-global-savetime": "1585128585", "cache-control": "s-maxage=165125, max-age=165125", "content-length": "291", "content-type": "application/json", "date": "Mon, 20 Apr 2020 08:17:19 GMT", "eagleid": "b7d6a49515875218465887939e", "etag": "\"5ce7cb1c-123\"", "expires": "Wed, 22 Apr 2020 06:09:24 GMT", "last-modified": "Fri, 24 May 2019 10:44:44 GMT", "server": "Tengine", "status": "304", "timing-allow-origin": "*", "via": "cache1.l2cn2308[0,304-0,H], cache22.l2cn2308[0,0], cache9.cn828[0,304-0,H], cache1.cn828[1,0]", "x-cache": "HIT TCP_IMS_HIT dirn:0:324628806", "x-swift-cachetime": "86400", "x-swift-savetime": "Tue, 21 Apr 2020 06:09:24 GMT" }, "config": { "transformRequest": {}, "transformResponse": {}, "timeout": 0, "xsrfCookieName": "XSRF-TOKEN", "xsrfHeaderName": "X-XSRF-TOKEN", "maxContentLength": -1, "headers": { "Accept": "application/json, text/plain, */*" }, "method": "get", "url": "https://www.runoob.com/try/ajax/json_demo.json" }, "request": {} }
```

### Ⅱ response.data

使用 **`response.data`** 读取 JSON 数据：

```html
<body>
<div id="app">
  <h1>网站列表</h1>
  <div v-for="site in info">
    {{ site.name }}
  </div>
</div>
<script type = "text/javascript">
new Vue({
  el: '#app',
  data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .get('https://www.runoob.com/try/ajax/json_demo.json')
      .then(response => (this.info = response.data.sites))
      .catch(function (error) { // 请求失败处理
        console.log(error);
      });
  }
})
</script>
</body>
```

运行结果：

```
网站列表
Google
Runoob
Taobao
```

⭐ 显然，**`response.data` 才是真正的后台数据**

### Ⅲ GET 方法传递参数

```js
// 直接在 URL 上添加参数 ID=12345
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
 
// 也可以通过 params 设置参数：
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## 3. 发送 POST 请求

### Ⅰ  response

```js
new Vue({
  el: '#app',
  data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .post('https://www.runoob.com/try/ajax/demo_axios_post.php')
      .then(response => (this.info = response))
      .catch(function (error) { // 请求失败处理
        console.log(error);
      });
  }
})
```

### Ⅱ  response.data

```js
<script>
    export default {
      name: 'UserProfile',
      data () {
          return {
            users: [],
            roles: [],
          }
      },
      mounted () {
        this.listUsers()
        this.listRoles()
      },
      methods: {
        listUsers () {
          var _this = this
          this.$axios.get('/admin/user').then(response => {
            if (response && response.status === 200) {
              _this.users = response.data
            }
          })
        },
        listRoles () {
          var _this = this
          this.$axios.get('/admin/role').then(response => {
            if (response && response.status === 200) {
              _this.roles = response.data
            }
          })
        },
      }
    }
</script>
```

### Ⅲ POST 方法传递参数

```js
axios.post('/user', {
    firstName: 'Fred',        // 参数 firstName
    lastName: 'Flintstone'    // 参数 lastName
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

