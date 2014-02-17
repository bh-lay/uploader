uploader
========

##uploader是什么
一个依靠原生html元素处理上传类的插件。

##uploader有哪儿些特点？

* 支持多文件上传
* 支持可自由定义的事件型API
* 不依赖于swf

##使用
创建一个完整的上传组件，需要指定充当上传按钮的dom、post的目标地址，以及格式化返回数据的方法，可以在参数中传递，也可先创建对象再定义。

```javascript
var uploader = new util.uploader({
    'action' : actionURL,
	'fileinputname' : 'photos[]',
	'dom' : upBtn,
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
			//	'files' : null,
			//	'files' : [],
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
}).on('startUpload',function(ID,files){
	//开始上传时
	alert('正在上传' + files['length'] + '个文件')
}).on('success',function(ID,files){
	//上传结束时
	alert('成功上传' + files['length'] + '个文件')
});

```


-----
暂时就更新到这儿，改日继续完善