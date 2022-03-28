# travel


# 黑马旅游网案例

## 注册

![image-20211013110927089](https://i.loli.net/2021/10/13/F2NjVgKMeyUBEhz.png)

```
<script>
			/*
			* 表单校验：
			* 	1.用户名：单词字符，长度8到20位
			* 	2.密码：单词字符，长度8到20位
			*	3.email：邮件格式
			* 	4.姓名：非空
			* 	5.手机号：手机号格式
			*	6.出生日期：非空
			*	7.验证码：非空
			* */
			function checkUsername(){
				var username = $("#username").val();
				var reg_username = /^\w{8,20}$/;
				var flag = reg_username.test(username);
				if(flag){
					$("#username").css("border","");
				}else{
					$("#username").css("border","1px solid red");
				}
				return flag;
			}			
			$(function (){
				//表单提交时调用所有校验方法
				$("#registerForm").submit(function (){
					return checkUsername() && checkPassword() && checkEmail() && checkName()
							&& checkTelephone() && checkBirthday() && checkCheck();
				});
				//组件失去焦点时，调用对应的校验方法
				$("#username").blur(checkUsername);
				$("#password").blur(checkPassword);//注意这里的checkPassword不要加括号
				$("#email").blur(checkEmail);
				$("#name").blur(checkName);
				$("#telephone").blur(checkTelephone);
				$("#birthday").blur(checkBirthday);
				$("#check").blur(checkCheck);
			});
		</script>
	

```

**异步提交表单：在此使用异步提交表单是为了获取服务器响应的数据。因为我们前台使用的是html作为视图层，不能够直接从servlet相关的域对象获取值，只能通过ajax获取响应数据**

**用了一个filter解决全站的编码问题**

### 邮件激活

**为什么要进行邮件激活？为了保证用户填写的邮箱的正确的。将来可以推广一些宣传信息，到用户邮箱中。**

**发送邮件**

qq邮箱发送需要授权码登录才能成功发送

``` 
 private static final String USER = "1972666358@qq.com"; // 发件人称号，同邮箱地址
 private static final String PASSWORD = "xoleghbelfloefcd"; // 如果是qq邮箱可以使户端授权码，或者登录密码
```

**用户点击激活**

​	![image-20211013161745191](https://i.loli.net/2021/10/13/jXOIzDbEKQHBJcR.png)

## 登录

![image-20211013173730172](https://i.loli.net/2021/10/13/Twykt2blq3snEUa.png)



## 退出

![image-20211013204214215](https://i.loli.net/2021/10/13/ZQSfDyMjRuVveKC.png)



## BaseServlet抽取

![image-20211014094939241](https://i.loli.net/2021/10/14/V5Du8erwQ2fLaJK.png)

要坚守Servlet的数量，现在是一个功能一个servlet，将其优化为一个模块一个servlet，相当于在数据库中一张表对应一个servlet，在servlet中提供不同的方法，完成用户的请求。

![image-20211014095611518](https://i.loli.net/2021/10/14/J5gmDUHtcLEAlVF.png)

完成方法分发

![image-20211014100445213](https://i.loli.net/2021/10/14/jbPMxonrv8YSDck.png)

```
public class BaseServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //完成方法分发
        //1.获取请求路径
        String uri = req.getRequestURI();
        System.out.println("请求uri:"+uri);
        //2.获取方法名称
        String methodName = uri.substring(uri.lastIndexOf('/') + 1);
        System.out.println("方法名称："+methodName);
        //3.获取方法对象method
        //this 代表调用的对象
        try {
            Method method = this.getClass().getMethod(methodName,HttpServletRequest.class, HttpServletResponse.class);
            method.invoke(this,req,resp);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        //4.执行方法
    }
}
```



## 分类条目

![image-20211014114407216](https://i.loli.net/2021/10/14/cK3ls4oON5gPAJE.png)

![image-20211014120828859](https://i.loli.net/2021/10/14/jJdhk3IQNEgPBsS.png)

### 分类数据缓存优化

使用redis缓存

![image-20211014130855827](https://i.loli.net/2021/10/14/ivsprEYImJyT6GA.png)

## 旅游线路分类

点击了不同的分类之后,将来看到的旅游路线不一样。通过分析数据库表结构，发现旅游线路表和分类表是多对一的关系。

![image-20211016162556093](https://i.loli.net/2021/10/16/9ZBRFk8w6xGztad.png)

redis中查询score(cid)   页面得到cid 通过location.search  获取cid: split切割获得cid

![image-20211016164429237](https://i.loli.net/2021/10/16/otlUTwSVDYuvEhF.png)

创建PageBean, RouteServlet RouteService RouteDao

## 旅游线路名称查询

给搜索按钮绑定单击事件,获取输入框内容传递参数.

