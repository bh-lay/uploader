uploader
========

##uploader是什么
uploader是一个依靠html元素处理上传的javascript类（目前依赖于jquery）。

##uploader有哪儿些特点？

* 支持可自由定义的事件型API
* 支持多文件上传
* 不依赖于swf
* 解析定义方式配合后端结果输出

#使用
创建一个完整的上传组件，需要指定充当上传按钮的dom、post的目标地址，以及格式化返回数据的方法，可以在参数中传递，也可先创建对象再定义。

##参数(属性)解释
* @{string} action 接受上传的地址
* @{object} dom 上传控件的触发按钮
* @{object} data 上传时附加字段
* @{function} responseParser 用于过滤上传结果，对事件的正确响应非常重要
* {string} fileinputname 承载上传文件的字段名

##事件监听
事件的监听是通过on来注册，每个事件传入的第一个参数为单次上传的ID，用于标识同一次上传过程。

## beforeUpload
介于用户选择文件与上传开始之前，主要用来阻止上传
接受参数:ID，files
可通过定义this.can_upload为true或false来允许或阻止(默认为允许)上传。

##startUpload
开始向服务端上传文件
接受参数:ID，files

##success
上传成功
接受参数:ID，files

##error
上传失败
接受参数:ID



###demo1先创建后完善
```javascript
var uploader = new util.uploader();

uploader.action = '/album/upload';
uploader.dom =  $('a.upbtn');
uploader.fileinputname   =  'photos[]';
uploader.data =  {
    'action' : 'addcover'
};
uploader.responseParser =  function(data){
  //console.log(1,data);
  if(data && data.ret == 200){
      var files = [];
      for(var i in data.data){
      files.push({
        'url' : data.data[i]['src'],
        'name' : data.data[i]['name']
      });
    }
    //触发上传完成事件
    return {
      'files' : files,
      'extra' : '676767'
    };
  }else{
    //触发上传失败事件
    return {
      //'files' : null,
      //'files' : [],
        'extra' : '服务器异常！'
    };
  }
}
```

###demo2 初始化指定参数
```javascript
var uploader = new util.uploader({
  'action' : actionURL,
  'fileinputname' : 'photos[]',
  'dom' : upBtn,
  'data' : {
    'action' : 'addcover'
  },
  'responseParser' : function(data){
    //console.log(1,data);
    if(data && data.ret == 200){
      var files = [];
      for(var i in data.data){
        files.push({
          'url' : data.data[i]['src'],
          'name' : data.data[i]['name']
        });
      }
      //触发上传完成事件
      return {
        'files' : files,
        'extra' : '676767'
      };
    }else{
      //触发上传失败事件
      return {
        //  'files' : null,
        //  'files' : [],
        'extra' : '服务器异常！'
      };
    }
  }
});

//开始上传之前
uploader.on('beforeUpload',function(ID,files){
  this.can_upload = true;
  for(var i in files){
    if(!files[i]['name'].match(/\.(jpg|jpeg|gif|png|bmp)$/i)){
      this.can_upload = false;
      alert('只能上传图片(支持.jpg .jpeg .gif .png .bmp格式)');
      break
    }
  }
});

uploader.on('startUpload',function(ID,files){
  //开始上传时
  alert('正在上传' + files['length'] + '个文件')
});

uploader.on('success',function(ID,files){
  //上传结束时
  alert('成功上传' + files['length'] + '个文件')
});
```



-----
暂时就更新到这儿，改日继续完善
