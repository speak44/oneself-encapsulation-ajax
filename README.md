# oneself-encapsulation-ajax
自己封装ajax.js

		function ajax(obj){
			let nor={
				//请求路径
				url:'',
				//请求方式
				method:'get',
				//成功返回的数据
				success:function(){},
				//失败返回内容
				fail:function(){},
				//后台数据格式
				dataType:'json',
				//传递后台的数据
				data:{
					//例如：内容
					//user:'user.valuer',
					//age:'age.valuer'
				},
				//ajax的请求是同步还是异步
				aysn:true
			}
			//如果没有信息就走默认，进行赋值
			for (let k in obj) {
				nor[k]=obj[k]
			}
			//传入的数据如果是多个，如：data{user:user.valuer,age:age.valuer}
			let arr=[]
			for (let k in nor.data){
				arr.push(k+'='+nor.data[k])
			}
			//将内容拼接为url头的内容
			nor.data=arr.join('&')
			let ajax=new XMLHttpRequest;
			//判断请求方式是get 或者post
			if (nor.method==='get') {
				ajax.open('get',nor.url+'?'+nor.data+'&'+(new Date().getTime()),nor.aysn);
				ajax.send();
			} else if(nor.method==='post'){
				ajax.open('post',nor.url,nor.aysn)
				ajax.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
				ajax.send(nor.data)
			}
			//由于IE9以下浏览器不会触发onload,但是所有的浏览器都支持ajax.onreadystatechange,来监听ajax进展到哪一步
			ajax.onreadystatechange=function(){
				//ajax.readySate是检测进展到哪一步，进展到第四步说明已经请求数据成功
				if (ajax.readyState==4) {
					//ajax.status是指状态码，当状态码范围在200-207之间或者304都表示成功
					if ((ajax.status>=200&&ajax.status<=207)||ajax.status==304) {
						//nor.dataType指后台给的数据的格式，进行分类之后通过不同方法转成对象格式				
						if (nor.dataType=='json') {
							let str=ajax.responseText;
							nor.success(eval('('+str+')'))
						}else if(nor.dataType=='xml'){
							nor.success(ajax.responseXML)
						}else if(nor.dataType=='str'){
							nor.success(ajax.responseText)
						}
					}
				} else{
					//如果步骤没有进行第四步打印出状态码
					nor.fail(ajax.status)
				}
			}
		}


