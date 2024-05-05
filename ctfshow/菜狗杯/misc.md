# 杂项签到
想得太复杂了，window图片正常显示，linux报错，说明png crc错误，大概率改了图片的长和宽，试了下长度改成宽度宽度改成长度，然后图片正常了，stegsolve打开图片，没发现什么，binwalk发现图片里包含了别的文件，导出是29和29.zlip，29为空，然后查各种zlip文件的解压发现解压不了十分绝望。
```python
import zlib
import binascii

deflate_compress = zlib.compressobj(9,zlib.DEFLATED,-zlib.MAX_WBITS)
zlib_compress = zlib.compressobj(9, zlib.DEFLATED, zlib.MAX_WBITS)
gzip_compress = zlib.compressobj(9, zlib.DEFLATED, zlib.MAX_WBITS | 16)


with open('test.txt','r',encoding='utf-8') as f:
    s = f.read()
    print(s)
    byte_s = bytes(s,encoding='utf-8')
    print(byte_s)
    b_s = binascii.hexlify(byte_s)
    print(b_s)
    
    with open('delfate.zlib','wb') as w:
        data = deflate_compress.compress(b_s) + deflate_compress.flush()
        w.write(data)
    with open('zlib.zlib','wb') as w:
        data = zlib_compress.compress(b_s)+ zlib_compress.flush()
        w.write(data)
    with open('gzip.zlib','wb') as w:
        data = gzip_compress.compress(b_s)+ gzip_compress.flush()
        w.write(data)
        
```
看了wp发现过于简单，十分怀疑自己

# 损坏的压缩包
winhex打开发现是png图片

# 谜之栅栏
winhex 文件对比

# 你会数数吗
字符统计

# 你会异或吗
winhex，编辑->修改数据->xor 0x50

# flag一分为二
crc损坏，用工具回复图片的大小，发现了flag的一部分，另一部分为盲水印，用watermark工具得到

# 我是谁
用官方脚本，识别图片
```python
import requests
from lxml import html
import cv2
import numpy as np
import json
 
 
url="http://7d8e32d7-0988-4cc9-ae71-e1a30652b02d.challenge.ctf.show"
 
sess=requests.session()
 
all_girl=sess.get(url+'/static/all_girl.png').content
 
with open('all_girl.png','wb')as f:
        f.write(all_girl)
 
big_pic=cv2.imdecode(np.fromfile('all_girl.png', dtype=np.uint8), cv2.IMREAD_UNCHANGED)
big_pic=big_pic[50:,50:,:]
image_alpha = big_pic[:, :, 3]
mask_img=np.zeros((big_pic.shape[0],big_pic.shape[1]), np.uint8)
mask_img[np.where(image_alpha == 0)] = 255
 
cv2.imwrite('big.png',mask_img)
 
 
 
def answer_one(sess):
        #获取视频文件
        response=sess.get(url+'/check')
        if 'ctfshow{' in response.text:
                print(response.text)
                exit(0)
        tree=html.fromstring(response.text)
        element=tree.xpath('//source[@id="vsource"]')
        video_path=element[0].get('src')
        video_bin=sess.get(url+video_path).content
        with open('Question.mp4','wb')as f:
                f.write(video_bin)
        #获取有效帧
        video = cv2.VideoCapture('Question.mp4')
        frame=0
        while frame<=55:
                res, image = video.read()
                frame+=1
        #cv2.imwrite('temp.png',image)
        video.release()
        #获取剪影
        image=image[100:400,250:500]
        gray_image=cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)
        #cv2.imwrite('gray_image.png',gray_image)
        temp = np.zeros((300, 250), np.uint8)
        temp[np.where(gray_image>=128)]=255
        #去白边
        temp = temp[[not np.all(temp[i] == 255) for i in range(temp.shape[0])], :]
        temp = temp[:, [not np.all(temp[:, i] == 255) for i in range(temp.shape[1])]]
        #缩放至合适大小，肉眼大致判断是1.2倍，不一定准
        temp = cv2.resize(temp,None,fx=1.2,fy=1.2)
        #查找位置
        res =cv2.matchTemplate( mask_img,temp,cv2.TM_CCOEFF_NORMED)
        min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
        x,y=int(max_loc[0]/192),int(max_loc[1]/288)#为什么是192和288，因为大图去掉标题栏就是1920*2880
        guess='ABCDEFGHIJ'[y]+'0123456789'[x]
        print(f'guess:{guess}')
        #传答案
        response=sess.get(url+'/submit?guess='+guess)
        r=json.loads(response.text)
        if r['result']:
                print('guess right!')
                return True
        else:
                print('guess wrong!')
                return False
 
i=1
 
while i<=31:
        print(f'Round:{i}')
        if answer_one(sess):
                i+=1
        else:
                i=1

```

# you and me
BlindWaterMark-master

# 我吐了你随意
[0宽隐写](https://330k.github.io/misc_tools/unicode_steganography.html)
![pic](/ctfshow/image/image.png)

# 迅疾响应
[qrazybox](https://merri.cx/qrazybox/)

没怎么懂，找时间了解一下二维码原理

# 打不开的图片
每个字节为0x100-d，0的话不变
'''python
with open('misc5.5.png','rb') as f:
    raw = f.read()
    data = []
    for d in raw:
        # print(d)
        if d !=0:
            dd = 0x100-d
            data.append(dd)
        else:
            data.append(d)
        
        # print(hex(dd))
    with open('output.png','wb') as w:
        w.write(bytes(data))

```