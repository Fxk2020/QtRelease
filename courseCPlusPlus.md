## 课程介绍
**录入、保存一个班级学生多门课程的成绩，并对成绩进行分析。详细分为：**
* （1）通过键盘输入多个学生的多门课程的成绩，建立相应的文件input.dat。
* （2）对文件input.dat中的数据进行处理，要求具有以下功能：
    * 1）按各门课程成绩排序，并生成相应的文件输出。
    * 2）计算每人的平均成绩，按平均成绩排序，并生成文件。
    * 3）求出各门课程的平均成绩、最高分、最低分、不及格人数、60\~69分人数、70\~79分人数、80~89分人数、90分以上人数。
    * 4）根据姓名或学号查询某人的各门课成绩，重名情况也能处理
* （3）界面美观

#### 1.界面设计
**使用ui设计界面单独设计界面，逻辑层和ui层的分离**
![](https://i.imgur.com/JbVPHhr.png)

#### 2.读写文件
**读文件**
```c++
 //打开文件
    QFile file("D:/数据结构课设二/Score.dat");
    file.open(QIODevice::ReadOnly);

    //获得文件大小
    qint64 pos;
    pos = file.size();
    file.open(QIODevice::ReadOnly);

    //读GBK文件
    QTextCodec *codec = QTextCodec::codecForName("GBK");
    QString *content = new QString[360];
    for (int i=0;!file.atEnd();i++) {
        QByteArray line = file.readLine();
        content[i] = codec->toUnicode(line);
        qDebug()<<"ddddddddddddddddddddddddddd"<<content[i];
    }

    file.close();
```    

**写文件**
```c++
//1.打开文件
                QFile file("D:/数据结构课设二/input.dat");

//2.写入数据
                file.open(QIODevice::ReadWrite);
                //获得文件大小
                qint64 pos;
                pos = file.size();
                //重新定位文件输入位置，这里是定位到文件尾端
                file.seek(pos);

                QTextStream out(&file);
                out.setCodec(QTextCodec::codecForName("GBK"));//GBK编码格式

                out<<peopleCount<<endl;
                out<<classCount<<endl;

//3.文件关闭
                file.close();
 ```

 #### 3.使用介绍
 视图分为三个系列界面，信息输入系列界面，主界面和功能查询系列界面。分别如下:
* 1.信息输入系列界面:
    * 1）人数和课程数的输入
 ![](https://i.imgur.com/M59HeiD.png)
    * 2）课程名的输入
 ![](https://i.imgur.com/E4Si2GW.png)
    * 3）学生信息输入（姓名，学号和成绩）
 ![](https://i.imgur.com/vAZHu6q.png)
* 2.主界面
 ![](https://i.imgur.com/9JzXIo2.jpg)
* 3.功能查询系列界面
    * 1）各门成绩排序
 ![](https://i.imgur.com/PD5NKT7.jpg)
    * 2）平均成绩排序
 ![](https://i.imgur.com/fDiypZU.jpg)
    * 3）各门课分数段人数统计
 ![](https://i.imgur.com/DwIbtDX.jpg)
    * 4）查询成绩
 ![](https://i.imgur.com/KUNzjvg.jpg)
 ![](https://i.imgur.com/s4LBqqN.jpg)
 ![](https://i.imgur.com/DchXiMJ.jpg)

 
