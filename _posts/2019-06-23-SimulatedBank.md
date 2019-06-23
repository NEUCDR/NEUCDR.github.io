## 模拟银行系统

java{:.success}
程设{:.success}

### 说明

这是一个基于java的模拟银行系统，旨在学习使用java编写程序，掌握基本的语法结构，代码规范
{:.info}

目前可以实现的功能有:`注册`,`登录`,`存款`,`取款`,`转账`,`查询余额`,`注销用户`,`查询账户数目`。
{:.info}

### **效果图如下**


初始界面，注册后登录
{:.info}
![1](http://i1.fuimg.com/691221/21763e1ee74ec22b.png)

登陆与存款
{:.info}
![2](http://i1.fuimg.com/691221/13a5cd5b9238905a.png)

查询余额与转账
{:.info}
![3](http://i1.fuimg.com/691221/21763e1ee74ec22b.png)

---

### **源代码如下**

```java

import java.util.*;
import java.lang.Thread;

public class SimulatedBank {
    public static void main(String[] arg)throws InterruptedException
    {
        Appearance appearance=new Appearance();
        appearance.formataccountmoney(appearance.accountmoney);//先初始化金钱arraylist
        appearance.mainmenu();//进入主菜单

    }
}


class Account {

    String getAccountName()//获得账户名称，返回名称String
    {

        System.out.println("请输入您的帐号：");
        Scanner account_name = new Scanner(System.in);
        String hhh=account_name.next();
        return hhh;
    }

    String getAccountPasswd()//获得账户密码，返回密码String
    {
        System.out.println("请输入您的密码：");
        Scanner account_passwd = new Scanner(System.in);
        String hhh=account_passwd.next();
        return hhh;
    }

    int checkInput(String name,String passwd,ArrayList anamelist,ArrayList apasswdlist)throws InterruptedException//传入账号名称及密码数组，循环比对
    {
        Appearance appearance=new Appearance();
        int membernum=-1;

            for(int i=0;i<anamelist.size();i++)
            {
                if(anamelist.get(i).equals(name))//equals用来比较的是两个对象的内容是否相等
                                                 //而"=="是用来比较两个对象的存储地址是否相等，即指针
                {
                        if (apasswdlist.get(i).equals(passwd)) {
                            System.out.println("验证通过！欢迎登陆！");
                            membernum=i;
                        } else {
                            System.out.println("用户名不存在或密码输入错误，请重试！");
                            appearance.mainmenu();
                        }break;
                }
//                else{System.out.println("用户名不存在！");}
            }

        return membernum;
    }


    void saveAccountname(String name,ArrayList accountname)//账户列表中填入新帐户名称
    {
        accountname.add(accountname.size(),name);
    }


    void saveAccountpasswd(String passwd,ArrayList accountpasswd)//密码列表中存入新密码（把密码也当作字符串处理）
    {
        accountpasswd.add(accountpasswd.size(),passwd);
    }

}

//预留的方法位
class Money {


    int getbalance(int number,ArrayList moneylist)//看余额用的，返回余额数值
    {
        int balance=(int)moneylist.get(number);
        return balance;
    }


    void rollin(int number,ArrayList moneylist)//转入大洋方法
    {
        System.out.println("您要转入多少大洋？");
        Scanner money=new Scanner(System.in);
        int hhh=money.nextInt();
        int balance=(int)moneylist.get(number)+hhh;
        moneylist.set(number,balance);//将指定账户金额设置为新金额
        System.out.println("成功存入"+hhh+"大洋！");

//现在要实现的是加一个大循环，让操作完成后返回第二菜单的界面！
    }


    void rollout(int number,ArrayList moneylist)//转出大洋方法
    {

        System.out.println("您要转出多少大洋？");
        Scanner money=new Scanner(System.in);
        int hhh=money.nextInt();
        int balance=(int)moneylist.get(number)-hhh;
        if(balance<0)
        {
            System.out.println("Sorry!余额不足，无法转出！");
        }else
            {
                moneylist.set(number,balance);
                System.out.println("成功转出"+hhh+"大洋！");

//现在要实现的是加一个大循环，让操作完成后返回第二菜单的界面！
            }

    }
    void transferAccounts(int number,ArrayList moneylist,ArrayList names)//转账，即为当前用户转出金额，目标用户转入金额
    {
        Money money=new Money();
        System.out.println("转账给谁？");
        System.out.println(names);
//        for(int i=0;i<appearance.names.size();i++){System.out.println(appearance.names.get(i));}//为什么这块不显示啊？！
        Scanner choice=new Scanner(System.in);
        String touser=choice.next();
        System.out.println("请输入转账金额：");

        Scanner rmb=new Scanner(System.in);
        int hhh=rmb.nextInt();
        int balance2=(int)moneylist.get(number)-hhh;
        if(balance2>0)
        {
            int balance1=(int)moneylist.get(names.indexOf(touser))+hhh;
            moneylist.set(names.indexOf(touser),balance1);
            moneylist.set(number,balance2);//Arraylist.set(int x,***)将arraylist的x位置设值为***
        }else{System.out.println("余额不足，无法完成操作！");}

    }//转账方法尚未完成

}

class Appearance {
    Money money = new Money();
    Account account=new Account();
    ArrayList<String> names=new ArrayList<>();
    ArrayList<String> passwds=new ArrayList<>();
    ArrayList<Integer>accountmoney=new ArrayList<>();
    int usernumber(ArrayList names)
    {
        int number=names.size();
        return number;
    }
    void mainmenu()throws InterruptedException
    {
        System.out.println("———————————————————————");
        System.out.println("|*******************主菜单********************|");
        System.out.println("|                   1.登录                    |");
        System.out.println("|                   2.注册                    |");
        System.out.println("|                   3.退出                    |");
        System.out.println("|                   4.帐户数目                |");
        System.out.println("———————————————————————");
        Scanner choice=new Scanner(System.in);
        int hhh=choice.nextInt();
        switch (hhh){
            case 4:
                {
                    System.out.println("用户数量为："+usernumber(names));
                }
            case 2:
            {
                account.saveAccountname(account.getAccountName(),names);
                account.saveAccountpasswd(account.getAccountPasswd(),passwds);
//                System.out.println(names);//问题，注册账户部分没有把账户成功添加到names和passwds里！
//                System.out.println(passwds);
                System.out.println("您已成功注册！请登录，享受不断的转账与存款吧！");
                int usernumber=account.checkInput(account.getAccountName(),account.getAccountPasswd(),names,passwds);
                singinmeau(usernumber);//成功登录，把账户对应编号传入主程序体系
                break;
            }
            case 1:
            {
             if(names.size()>0)//判断当前账户列表中是否存在账户，如果不存在（names.size()==0），必然不能登录，要求注册
             {
                    int usernumber=account.checkInput(account.getAccountName(),account.getAccountPasswd(),names,passwds);
                    if(usernumber==-1)
                    {
                        mainmenu();
                    }else
                        {
                        System.out.println("你的当前用户名为："+names.get(usernumber));
                        singinmeau(usernumber);
                        }
            }else
                {
                    System.out.println("您未在本银行拥有账户，请先注册！\n");
                    mainmenu();
                }
             break;
            }
            default:{mainmenu();}
        }
    }


    void singinmeau(int usernumber)throws InterruptedException//usernumber为用户编号
    {
        boolean flag=true;
//在这嘎的整一个循环，执行完操作后直接返回到这儿！
        while(flag) {
            System.out.println("———————————————————————");
            System.out.println("|*******************主菜单********************|");
            System.out.println("|                   1.存款                    |");
            System.out.println("|                   2.取款                    |");
            System.out.println("|                   3.转账                    |");
            System.out.println("|                   4.查询余额                |");
            System.out.println("|                   5.注销用户                |");
            System.out.println("|                   6.退出程序                |");
            System.out.println("———————————————————————");
            Scanner choice = new Scanner(System.in);
            int i=choice.nextInt();
            switch (i)
            {
                case 1:
                    money.rollin(usernumber, accountmoney);//转入
                    break;
                case 2:
                    money.rollout(usernumber,accountmoney);//转出
                    break;
                case 3:
                    money.transferAccounts(usernumber,accountmoney,names);//转账
                    break;
                case 4:
                    System.out.println("你的当前用户名为："+names.get(usernumber));//提示用户名
                    System.out.print("您的帐户余额为：");
                    System.out.println(money.getbalance(usernumber,accountmoney));
                    break;
                case 5:
                    mainmenu();//注销用户，及返回主菜单注册或重新登录
                    break;
                case 6:
                    System.out.println("正在退出程序.......");
                    flag=false;
                    Thread.sleep(3000);
                    System.exit(2);
                    break;
            }

        }
    }
    void formataccountmoney(ArrayList accountmoney)
    {
        for(int i=0;i<100;i++){accountmoney.add(0);}
    }

```


### **总结**

上课最好不要跟着老师走，效率低，永远相信：度姐是最好的老师！
{:.warning}

学习Java{:.info}

[W3Lschool](https://www.w3cschool.cn/java/)
[菜鸟教程](https://www.runoob.com/java/java-tutorial.html)


java资料{:.info}

[廖雪峰课程](https://pan.baidu.com/s/1rvFCNcnIFYfdCJMXauM98g)`提取码：3o5o`
[java架构](https://pan.baidu.com/s/1p50XSRryOWxsgcLXdaRB_g)`提取码：1821`
[java基础到就业](https://pan.baidu.com/s/1UlVm_CxQVQG92trJ64aLWQ)`提取码：6yb7`



