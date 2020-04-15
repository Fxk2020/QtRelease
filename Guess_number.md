### 任务描述
**完成一个猜数字的GUI游戏。**
开始游戏后，产生一个**没有重复数字**的4位随机数，用户每猜一个数字，**显示出“完全猜中的数字个数”和“猜中数字但位置错误的数字个数”**，比如nAmB，数字n表示猜中的位置正确的数字个数，数字m表示数字正确而位置不对的数字个数。例如，正确答案为5234，如果用户猜5346，则显示：1A2B，数字1表示数字5及其位置猜对了，数字3和4这两个数字猜对了，但是位置没对，记为2B。然后，用户根据游戏提示的信息继续猜，直到猜中为止。同时设计规则，根据猜中的次数计算积分，**并可以显示不同用户的排行榜**。

### 规则设计
**一次猜中得100分，2 ~3次猜中得90分，4 ~ 5次猜中得80分，6~7得70分，8 ~9得60分，10 ~11得50分，12 ~13得40分,14 ~15得30分，16 ~17得20分，18及以上得10分**    

### 数据库设计
一个表用户-users，主码是user_id并且使默认递增的。
![](https://s1.ax1x.com/2020/04/15/Ji1BwQ.png)
    
### 主要逻辑
* 1)如何产生没有重复数字的四位数字,并且判断不以零开头。
     ```C++
    //获取随机数
    int MainWindow::generateRandomNumber(){
    int tag[]={0,0,0,0,0,0,0,0,0,0};
    int four=0;
    int temp=0;

    while(four<1000){
        temp=qrand()%10;//随机获取0~9的数字
        if(tag[temp]==0){
            four+=temp;
            four*=10;
            tag[temp]=1;
        }
    }
    return four;
   }
    ```   
* 2)如何根据玩家猜测的数字给出几个A和几个B，**返回值是int，一个A是10，一个B是1**
    * A的个数通过逐步求余判断每个位是否相等 
    * B的个数通过求余获得玩家猜测数字的每一位，然后用每一位与产生的随机数的不同位作比较。
```C++
//获得A的个数
int Game_interface::getA(QString quessNumber){
    int quessNum = quessNumber.toInt();
    int resultNum = randomNumber;

    int A = 0;

    //求余运算
    while(quessNum!=0){
        if(quessNum%10 == resultNum%10){
            A+=10;
        }
        quessNum/=10;
        resultNum/=10;
    }

    return A;
}
```
    

```C++    
//获得B的个数
int Game_interface::getB(QString quessNumber){
    int quessNum = quessNumber.toInt();
    int resultNum = randomNumber;

    int B = 0;

    int unit = resultNum%10;
    resultNum/=10;
    int ten = resultNum%10;
    resultNum/=10;
    int hundred = resultNum%10;
    resultNum/=10;
    int thousand = resultNum%10;

    while(quessNum!=0){
        switch (getDigits(quessNum)) {
        case 4:
            if(equalsNumbers(quessNum%10,ten,hundred,thousand)){
                B++;
            }
            quessNum/=10;
            break;
        case 3:
            if(equalsNumbers(quessNum%10,unit,hundred,thousand)){
                B++;
            }
            quessNum/=10;
            break;
        case 2:
            if(equalsNumbers(quessNum%10,unit,ten,thousand)){
                B++;
            }
            quessNum/=10;
            break;
        case 1:
            if(equalsNumbers(quessNum%10,unit,ten,hundred)){
                B++;
            }
            quessNum/=10;
            break;
        }
        }

    return B;

    }

```
* 3）根据int到向用户提示是几个A和几个B（一共有15种情况，switch case语句）
```C++
//记录每次的猜测结果，并插入到表格中
void Game_interface::guessResult(int result_my,QString quessNumber){

    switch(result_my){
    case 0:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("0A0B"));
        break;
    case 1:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("0A1B"));
        break;
    case 2:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("0A2B"));
        break;
    case 3:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("0A3B"));
        break;
    case 4:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("0A4B"));
        break;
    case 10:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("1A0B"));
        break;

    case 11:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("1A1B"));
        break;
    case 12:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("1A2B"));
        break;
    case 13:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("1A3B"));
        break;
    case 20:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("2A0B"));
        break;
    case 21:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("2A1B"));
        break;
    case 22:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("2A2B"));
        break;
    case 30:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("3A0B"));
        break;
    case 31:
        ui->tableWidget->setItem(rowNumber,0,new QTableWidgetItem(quessNumber));//插入猜测的数字
        ui->tableWidget->setItem(rowNumber,1,new QTableWidgetItem("3A1B"));
        break;
    //猜中游戏结束
    case 40:
        qDebug()<<"恭喜你猜对了！/n你一共猜了"+QString::number(rowNumber+1)+"次";
        QMessageBox::StandardButton reply;
        reply = QMessageBox::information(this, tr("闯关成功！"), "恭喜你猜对了！你最终的得分是"+getScore(rowNumber));
        this->close();
        break;
    }
}
```

### 界面设计
* 设计登录界面(**知识点：文本框，按钮的美化**)
![](https://upload.cc/i1/2020/04/15/FYS7nH.png)
* 设计注册界面
![](https://s1.ax1x.com/2020/04/15/JCqM5T.png)
* 设计主界面
![](https://s1.ax1x.com/2020/04/15/JCOStf.png)
* 排行榜界面(**表格**)
![](https://s1.ax1x.com/2020/04/15/JCjZWT.png)
* 游戏主界面
![](https://s1.ax1x.com/2020/04/15/Jiul6g.png)

### 体现面向对象的特点
#### 1.封装
* 1)将与数据库的操作封装到qtODBC中。
```C++
class ODBC

    QSqlDatabase db ;
    QSqlQuery result;

    //构造方法
public :ODBC(){
        db = QSqlDatabase::addDatabase("QODBC");
        //输出可用数据库
        qDebug()<<"available drivers:";
        QStringList drivers = QSqlDatabase::drivers();
        foreach(QString driver, drivers)
            qDebug()<<driver;


        db.setHostName("127.0.0.1");
        db.setPort(3306);
        db.setDatabaseName("qt_contection_mysql");
        db.setUserName("root");
        db.setPassword("Fxk199959");
        bool ok = db.open();
        if (ok){
            //            QMessageBox::information(this, "infor", "success");
            qDebug()<<"数据库连接成功！"<<endl;
        }
        else {
            //        QMessageBox::information(this, "infor", "open failed");
            qDebug()<<"error open database because"<<db.lastError().text();
        }

    }

    ~ODBC(){
        db.close();
    }

    //显示数据库中表的名称
    void showTables(){
        //查询数据库中所有表的名称
        QStringList tables = db.tables();
        foreach(QString table, tables)
            qDebug()<<table;
    }

    //通过sql查询表
public : QList<Users *> selectData(QString sql){

        QList<Users *> message_user;//储存所有老用户的信息
        Users *user;//单个老用户

        //ODBC查询数据
        result = db.exec(sql);
        while(result.next()){
            int user_id = result.value("user_id").toInt();
            QString name = result.value("name").toString();
            QString password = result.value("password").toString();
            int score = result.value("score").toInt();

            qDebug()<<"user_id:"<<user_id;
            qDebug()<<"name:"<<name;
            qDebug()<<"password:"<<password;
            qDebug()<<"score:"<<score<<endl;

            user = new Users(user_id,name,password,score);

            message_user.insert(0,user);
        }

        return  message_user;

    }

    //插入表
public :void insertData(QString sql){

        //ODBC插入数据
        QSqlQuery insertResult(db);
        bool result_successed = insertResult.exec(sql);

        if(result_successed){
            qDebug()<<"插入成功！";
        }else {
            qDebug()<<"插入失败！";
        }
    }

    //更新表
public :bool updateData(QString sql){
        //ODBC更新数据
        QSqlQuery updateResul(db);
        bool result_successed2 = updateResul.exec(sql);

        if(result_successed2){
            qDebug()<<"更新成功！"<<endl;
        }else{
            qDebug()<<"更新失败！"<<endl;
        }

        return result_successed2;
    }
```
* 2)封装了用户类
```c++
class Users{
    int user_id;
    QString name;
    QString  password;
    int score;

public:
    Users(int user_id,QString name,QString password,int score){
        this->name = name;
        this->password = password;
        this->user_id = user_id;
        this->score = score;
    }

    Users(int user_id,QString name,QString password){
        this->name = name;
        this->password = password;
        this->user_id = user_id;
    }

    // 重载运算符（ == ）
    bool equalUser(const Users* user2)
    {


       if(user_id==user2->user_id&&QString::compare(name,user2->name)==0
       &&QString::compare(password,user2->password)==0){
           return true;
       }
       return false;
    }

    QString getName(){
        return  name;
    }
    int getID(){
        return  user_id;
    }
    int getScore(){
        return  score;
    }

};
```
#### 2.继承
在该项目中，所有可视化的界面类都继承自QWidget类。在qt中QWidget是所有用户界面对象的基类，被称为基础窗口部件。
#### 3.多态