
>html代码
```html
 <div class="uploaded">
 <input type="file" multiple @change="previewImg($event)" />
 <input type="button" onclick="submit"> 
 </div>
```
>js代码
```javascript
//上传预览
 function previewImg (e){
      console.log(e.target.files)
      if (window.FileReader) {  //判断浏览器是否支持FileREader
        for (let i = 0; i < e.target.files.length; i++) { //循环批量添加
          var imgSize = file.size / 1024;   
          var fr = new FileReader() //新建FileReader对象
          fr.onloadend = function (e) {  //监听加载完成渲染预览图像
            var ele = `<img src='${e.target.result}' />`
            document.querySelector('.uploaded').insertAdjacentHTML('afterbegin', ele)
            console.log(e.target)
          }
          fr.readAsDataURL(e.target.files[i]) //将对象解析为base64
        }
      }
    }
 //上传提交   
 function submit(e) {
    var formdata= new FormData();
        for (var i =0;i< e.target.files.length;i++) {
            formdata.append("file",e.target.files[i]);
            }
            axios({
                  url:'',
                  method: 'post',
                   data: formdata,
                    processData: false,// 告诉axios不要去处理发送的数据(重要参数)
                    contentType: false,   // 告诉axios不要去设置Content-Type请求头
                    }).then(res=>{
                           if(res.data.code==200){
                               that.form.icon =res.data.data;
                            }else {
                        
                           }
                  })
        
 }   
```
