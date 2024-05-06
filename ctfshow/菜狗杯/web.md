# web签到
```php
eval($_REQUEST[$_GET[$_POST[$_COOKIE['CTFshow-QQ群:']]]][6][0][7][5][8][0][9][4][4]);
/*
    # $_REQUEST 能够获取get，post，cookie中的消息
    # $_COOKIE 获取cookie中的值，此题中获取CTFshow-QQ群的值
    # $_POST 获取cookie的名的值
    # $_GET 获取post对应的值
*/
```
构造如下请求（不知道request c[][]...[]=system('ls /');命令放在get处无法执行。。。，改到post处就可以）
![pic1](/ctfshow/image/image-1.png)

# web2 come_to_sign
浏览器f12，按照提示控制台输入g1ve_flag()

# 我的眼里只有$
```php
$var = str
$str = hello
$$var = $str = hello
```
![image2](/ctfshow/image/image-2.png)

# 抽老婆
打开网站，发现可以下载图片，修改download的请求，报错
![error](/ctfshow/image/image-3.png)
发现报错处为/app/app.py,尝试download该文件，打开发现secret_key
并且发现如下代码
```python
@app.route('/secret_path_U_never_know',methods=['GET'])
def getflag():
    if session['isadmin']:
        return jsonify({"msg":flag})
    else:
        return jsonify({"msg":"你怎么知道这个路径的？不过还好我有身份验证"})
```
使用脚本解密session，修改isadmin的值为True，重新加密生成session
![alt text](/ctfshow/image/image-5.png)
![alt text](/ctfshow/image/image-4.png)

# 一言既出&驷马难追
xor

114514^1897488

# TapTapTap
修改游戏代码，habibiscript.js,跳关，小球点击次数改为一，depends on you

# webshell
php的序列化和反序列化，<mark>由于有shell_exec就不用system了</mark>
```php
$cmd=O:8:"Webshell":1:{s:3:"cmd";s:4:"nl *";}
```
