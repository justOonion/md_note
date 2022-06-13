```shell
tar -xvf file.tar
jar -xvf file.jar
```

```zip``` 
文件夹压缩 : 
```zip -r test.zip test/```
```unzip test.zip -d ./test/```

**压缩**

//将目录里所有jpg文件打包成tar.jpg 
```tar -cvf jpg.tar *.jpg```

//将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
```tar -czf jpg.tar.gz *.jpg```

//将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
```tar -cjf jpg.tar.bz2 *.jpg```

//将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
```tar -cZf jpg.tar.Z *.jpg```   

//rar格式的压缩，需要先下载rar for linux
```rar a jpg.rar *.jpg ```

//zip格式的压缩，需要先下载zip for linux   [-r递归]
```zip [-r] jpg.zip *.jpg```

**解压**

```tar -xvf file.tar``` //解压 tar包

```tar -xzvf file.tar.gz``` //解压tar.gz

```tar -xjvf file.tar.bz2```   //解压 tar.bz2

```tar -xZvf file.tar.Z```    //解压tar.Z   **-C 指定目录解压**

```unrar e file.rar``` //解压rar

```unzip file.zip``` //解压zip


**总结**

1、*.tar 用 **tar -xvf **解压

2、*.gz 用 **gzip -d**或者**gunzip** 解压

3、*.tar.gz和*.tgz 用 **tar -xzf** 解压

4、*.bz2 用 **bzip2 -d**或者用**bunzip2 **解压

5、*.tar.bz2用**tar -xjf** 解压

6、*.Z 用 **uncompress** 解压

7、*.tar.Z 用**tar -xZf** 解压

8、*.rar 用 **unrar e**解压

9、*.zip 用 **unzip** 解压

