### Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。很多情况我们要对请求和其响应进行特定的处理；如果请求数非常多，单独对每一个请求进行处理会变得非常麻烦。好在强大的axios为开发者提供了这样一个API：拦截器。拦截器分为 请求（request）拦截器和 响应（response）拦截器。

#### 安装 axios,qs

```bash
npm install axios –save-dev
```
#### 安装 qs
```bash
npm install qs –save-dev
```
### 请求拦截器

```javascript
//引入axios和 qs
import axios from "axios";
import qs from "qs";
/****** 创建axios实例 ******/
const serve = axios.create({
    baseURL: process.env.BASE_URL,  // api的基本url，也可以直接写定好的url
    timeout: 5000  // 请求超时时间
});
serve.interceptors.request.use(config => {    
    config.method === 'post'        
        ? config.data = qs.stringify({...config.data})        
        : config.params = {...config.params};    
		  config.headers['Content-Type'] = 'application/x-www-form-urlencoded'; 
    return config;
    }, error => {  //请求错误处理   
           
        Promise.reject(error)
    }
);
```
### 响应拦截器

```javascript
serve.interceptors.response.use(    
    response => {  //成功请求到数据        
           
        //这里根据后端提供的数据进行对应的处理        
        if (response.data.result === 'TRUE') {            
            return response.data;        
        } else { 
			//打印错误信息
			console.error(response.data.data.msg)
             
        }    
    },    
    error => {  //响应错误处理console.log('error');        
        console.error(error);        
        console.error(JSON.stringify(error));         
        let text = JSON.parse(JSON.stringify(error)).response.status === 404            
            ? '404'            
            : '网络异常，请重试';        
        alert(text)        
        return Promise.reject(error)   
    }
)
```