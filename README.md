#   License Plate Labeler

这个repo是一个用于视频中车辆和车牌标注的，可以快速部署的标注工具。支持*框标注*和车牌*顶点标注*，同时该工具也可以用于其他视频中的目标标注项目中。

![show](images/readme.jpg)
----

### 环境要求
* Python 3.+
* Django(pip install django)
* nginx(高并发时建议安装)


------

### 工具特点
1. 部署方便，可以进行工作量分配来达到多人标注
2. 工具中含有线性插值功能，可以跳帧标注，相对减轻了工作量

----



### 如何运行

1. 将该repository 克隆到本地工作区:

*   git clone https://github.com/Elin24/license_labeler.git

*   cd lplabeler

2. 在data文件夹下新建user, video, type和json四个文件夹。其中json文件夹下的文件存放的是标注结果，type文件夹是辅助标注用的文件，不需要对其进行操作。video文件夹用于存放需要标注的视频帧，具体格式如下：
```
video
│  
├─1
│      frame1.jpg
│      frame2.jpg
│      frame3.jpg
│      ...
├─2
│      frame1.jpg
│      frame2.jpg
│      frame3.jpg
│      ...
└─3
│      frame1.jpg
│      frame2.jpg
│      frame3.jpg
│      ...
└─ ...
```
在相应的用户下写明其标注的文件夹标号（1，2，3，...）即可



3. 标注账号设置
```json
{
    "name": "root",
    "password": "root",
    "data": [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    "done": [],
    "half": []
}
```
`name`表示登录名，`password`表示登录密码，`data`文件夹表示该用户需要标注的视频帧的文件夹标号。`done`和`half`分别表示已完成标注和正在标注中的视频，初始置空即可。


4. 运行django服务

*   python manage.py runserver 0.0.0.0:8000

5. 用Chrome浏览器打开localhost:8000，登陆账号并开始标注


----


### 用户使用方法

1. 点击 `new` 拖动鼠标可标注一辆车
2. 内部的4个点用来标注车牌的四角
3. 使用`Ctrl+滚轮`可以对图片进行放大和缩小，并使用`WASD`调节放大位置
4. 完成坐标点的标注后再右上角的`输入框`中输入车牌号
5. 新建的标注框会自动复制到下一帧，为了减少工作量只需要在后面进行微调，需要停止该车的标注时，点击`delete`会自动删除该帧之后的所有帧中的该车辆框 
5. 可间隔几帧进行修改，中间部分的会自动线性插值处理
6. 在某一帧修改完后，一定要**保存**，方法为点击`save`键或在键盘上`ctrl + S`
7. 左边为相应帧，可以使用`滚轮`上下调帧；
8. 在移动到最后一帧时，底部进度条回变化成一个按钮，询问是否已经标注完毕。若已标注完毕，可以点击跳转到主界面；否则可忽略。

---



### 标注进度查看
登录 http://localhost:8000/summary 可以获得目前所有标注人员的情况和进度
