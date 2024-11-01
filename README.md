#### 1.新建文件夹，打开cmd

```bash
git clone git@github.com:xiaouzzz/yolov8_tensorrt-main.git
```

### 2.先安装vs2022，再安装cuda11.8（顺序不能反）

### 3.下载tensorrt8.6版本

#### 4.修改cmakelists.txt

把opencv，tensorrt的位置改成自己的

#### 5.使用cmake编译

#### 6.打开编译好的sln文件

#### 7.右键解决方案的yolov8设为启动项

#### 8.先注释main里面这行，运行，再取消注释运行

```c++
   //wts_name = "";
```

