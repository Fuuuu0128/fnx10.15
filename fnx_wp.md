###  2017-10-15


# Urldecode
#### 0x00 [原理]
     php基本功，url编码传入，urldecode
#### 0x01 [目的]
     了解服务器url参数的编码解码
#### 0x02 [环境]
     windows
#### 0x03 [工具]
     chrome
#### 0x04 [步骤]

1.首先打开address，提示为?me=

2.在url栏输入//192.168.19.60:13002/?me=1，出现提示 hint

![](/files_for_wp/urldecode_1.png)

3.根据提示，"you must be XMAN",得到参数为"SJU"
![](/files_for_wp/urldecode_2.png)

4.得到第二条hint：urldecode ，  对"SJU"进行二次URL编码传入

  所以用了urldecode函数对参数"SJU"解密

![](/files_for_wp/urldecode_3.png)

##  所以要“二次”urldecode解码

5.urldecode解码：

![](/files_for_wp/urldecode_5.png)

![](/files_for_wp/urldecode_4.png)

得到解码结果：%2553%254a%2555

6.?me=%2553%254a%2555
 在url栏输入//192.168.19.60:13006/?me=%2553%254a%2555

![](/files_for_wp/urldecode_6.png)

7.flag：SJU{UrlDeCode_yOu_u0D3rSta9D!}

#### 0x05 [总结]
    urldecode，二次URL编码传入
</br>
</br>
</br>
</br>
</br>






# PHP

#### 0x00 [原理]
     php弱类型
#### 0x01 [目的]
     了解switch...case弱类型比较绕过，array_search函数绕过，json_encode函数加密，代码审计
#### 0x02 [环境]
     windows
#### 0x03 [工具]
     chrome,御剑后台扫描
#### 0x04 [步骤]

1.首先打开address，什么都没有

![](/files_for_wp/PHP_1.png)

2.用御剑后台扫描扫目录得到目标文件：index.php~

![](/files_for_wp/PHP_2.png)

3.打开目标文件，得到源代码

 ![](/files_for_wp/PHP_3.png)  

  发现是代码审计题

4.观察代码：</br>
 ①.先分析得到flag.php的条件：

 ![](/files_for_wp/PHP_4.png)  </br>
 要三个参数都为1才能得到flag</br>
 ②.分析参数$a:</br>
  是switch..case弱类型比较绕过</br>

 ![](/files_for_wp/PHP_5.png)

 $aaa=1a</br>
 ③.分析参数$b:</br>
   $bbb["ccc"]>2017,所以"ccc":"2018aaa"</br>
   然后是array_search函数绕过,和switch..case弱类型比较绕过</br>
   ![](/files_for_wp/PHP_6.png)

 ④.得到："ddd" => array(array(), 0)</br>
   ![](/files_for_wp/PHP_8.png) 

   类似的使用json_encode函数加密："ddd":[[],0]

5.得到payload：/index.php?aaa=1a&bbb={"ccc":"2018aaa","ddd":[[],0]}</br>
  在url栏输入http://192.168.19.60:13001/index.php?aaa=1a&bbb={%22ccc%22:%222018aaa%22,%22ddd%22:[[],0]}

![](/files_for_wp/PHP_7.png)  

6.flag：SJU{PHP_15_Th3_B35T_L4N6U463}


#### 0x05 [总结]
   array_search函数绕过，json_encode函数加密,switch...case弱类型比较绕过
