---
title: Employee record system
key: 20190714
tags: java
---

### 简介

这是一个用java完成的员工考勤系统，主要涉及java基本语法，文件读写，异常处理等
{:.info}

##### 实现的功能：  

> 普通用户：用户登录，签到。签退，查询信息（部分），查询全部职工账户名

> 管理员：登录，增加用户，删除用户，查询信息（全部），查询全部职工账户名、密码及状态


### 设计思路

类图(没写完...)
{:.info}

![类图](http://i2.tiimg.com/691221/accc0387d4a993c5.png)


核心代码
{:.info}

文件创建与写入：循环账户姓名列表中的姓名，并以此为文件名，生成存放用户信息的文件（txt）
{:.warning}

```java

	public void readinfo() throws IOException 
	{
		for(String n:user.names) {
			String name = n;
			File file = new File("f:/dir/"+name+".txt");
			//定义一个fileReader对象，用来初始化BufferedReader
			FileReader reader = new FileReader(file);
			//定义一个BufferedReader对象，将文件内容读取到缓存
			BufferedReader bReader = new BufferedReader(reader);
			//定义一个字符串缓存，将字符串存放缓存中
			StringBuilder sb = new StringBuilder();
	        user.names.clear();
	        user.names.add(bReader.readLine());
	        user.IDs.clear();
			user.IDs.add(Integer.valueOf(bReader.readLine()));
			user.signintimes.clear();
			user.signintimes.add(bReader.readLine());
			user.signouttimes.clear();
			user.signouttimes.add(bReader.readLine());
	        
	        bReader.close();
	        String str = sb.toString();
	        String[] nnnnn  = str.split("\n");
	        for(String info:nnnnn) 
	        {
	        	System.out.println(info);
	        }
		}
	}
  
```
文件与格式转化：读取文件中的信息读取并转化成String类型存入ArrayList列表中
{:.warning}

```java

public void writeinfo() throws IOException 
	{
		for(String n:user.names)
		{
			String name = n;
			File file = new File("f:/dir/"+name+".txt");
			int index = user.names.indexOf(name);
			if(!file.exists()) 
			{
				File dir = new File(file.getParent());
				dir.mkdirs();
				file.createNewFile();
				FileWriter file1 = new FileWriter(file);
				file1.write(name);
				file1.write(user.IDs.get(index));
				file1.write(user.status.get(index));
				file1.write(user.signintimes.get(index));
				file1.write(user.signouttimes.get(index));
				file1.flush();
				file1.close();
			}else 
			{
				FileWriter file2 = new FileWriter("f:/dir"+name+".txt");
				file2.write(name+System.getProperty("line.separator"));
				file2.write(user.IDs.get(index)+System.getProperty("line.separator"));
				file2.write(user.status.get(index)+System.getProperty("line.separator"));
				file2.write(user.signintimes.get(index)+System.getProperty("line.separator"));
				file2.write(user.signouttimes.get(index)+System.getProperty("line.separator"));
				file2.flush();
				file2.close();
			}
		}
	}
  
```

登录功能，检查输入并验证信息，（先执行读取文件操作，获得保存着用户信息的ArrayLists，以实现后续功能）
{:.warning}


```java

public int signIn() throws IOException 
	{
		data.readinfo();
		System.out.println(data.user.names);
		int i = 1;
		System.out.println("\t序号\t\tID");
		for(int element:data.user.IDs) 
		{
			System.out.println("\t"+i+"\t\t\t"+element);
			i++;
		}
		System.out.println();
		try
		{
			System.out.println("请输入你的ID");
			Scanner in = new Scanner(System.in);
			int id = in.nextInt();
			if(data.user.IDs.indexOf(id)!=-1)
			{
				System.out.println("请输入你的姓名");
				String name = in.next();
				if(data.user.IDs.indexOf(id)!=-1) 
				{
					int idindex = data.user.IDs.indexOf(id);
					int nameindex  = data.user.names.indexOf(name);
					if(idindex == nameindex)
					{
						System.out.println("身份验证成功！");
					}else {System.out.println("姓名与ID不符，请尝试重新登陆");return 1;}
				}else {System.out.println("用户名不存在！");return 1;}
				
			}else {System.out.println("用户ID不存在！");return 1;}
			return id;
		}catch(InputMismatchException e ) 
		{
			System.out.println("用户名或ID输入不合法！");
		}
		return 1;
		
	}
  
```

获得签到于签退的时间，返回String类型
{:.warning}

```java

	SimpleDateFormat formatTime = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	public String signInTime() 
	{
		Date datein = new Date();
		return formatTime.format(datein);		
	}
	
	public String signOutTime() 
	{
		Date dateout = new Date();
		return formatTime.format(dateout);
	}

```

遇到的问题与解决方法
{:.error}

> 问题一：使用Scanner进行用户输入时，输入信息后出现程序长时间等待
>> 解决：Scanner输入后，如果关闭输入流`scanner.close()`，则后续的新创建的Scanner对象必然报错，以后Scanner对象再也不敢关闭了😭

> 问题二：引用类对象时，信息不断被刷新，即无法永久存储
>> 解决：每当new一个类对象时，就会新开辟一个内存空间以存储数据，自然不能实现数据的连续操作，因此，指向用户信息的对象必须设为静态对象

> 问题三：数据读取时，第一行不能读取，但对第二行之后的数据能够正常读取并输出
>> 解决：windows系统txt文件的默认为AISN编码，为能够正确显示中文将文件改为UTF-8编码，但是只能保存为带有DOM结构的UTF-8文件，
即会在头文件中插入DOM标识符“EF BB BF”，JDK只能辨别但不能正确处理此类文件（官方声明的JDK存在的BUG，Bug ID:4508058）
用EditorPlus或者Notepad++将文件保存成无DOM结构的UTF-8文件即可。

> 问题四：String类型写入文件后，出现部分乱码，程序方法无法正确读取
>> 解决：尚未解决，将用数据流的形式进行存储，目前使用FileReader方法失败。







