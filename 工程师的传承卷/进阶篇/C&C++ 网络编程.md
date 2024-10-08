# C&C++ 网络编程（基于Socket与Boost Asio）
## ZERO. 先期准备
### ZERO.1 网络编程环境的配置
#### ZERO.1.1 C++前置环境
- 参见`入门篇-软件编程-C++`
#### ZERO.1.2 Boost库
- [下载 "Boost C++ Libraries"](https://www.boost.org/)
- 解压压缩包
- 运行boostrap.bat(Windows)/boostrap.sh(Linux)
- 命令行带参数运行b2编译脚本
- 添加include目录和lib目录到你的编译器
- 详细过程参见`附录-环境配置-C++-Boost-Libraty`
## 一. Socket
### 1.1 Socket是什么
工作在OSI七层模型、TCPIP五层模型中的传输层与应用层之间，TCP协议的封装（详见`基础篇-计算机网络原理`）**<font color="red">尽管是否学习`基础篇-计算机网络原理`不影响理解本篇章内容,然而理解基础与本质的重要性远大于能够使用，因此还是建议在学习本篇章前完成对`基础篇-计算机网络原理`的学习</font>**
### 1.2 Socket通信基本流程
#### 1.2.1 流程描述（文字）
##### 服务端  
- socket  创建插槽
- bind  绑定本机IP结构体
- listen  监听连接请求
- accept  接受请求并分配一个新插槽用于通信(因为服务器要同时面对多个客户端，不能直接把监听那个套接字堵塞)
- read write 通信
##### 客户端  
- socket 创建插槽
- connect 把插槽怼进服务器（连接到服务器）
- read write 通信
##### 通信结束后双方断开
close 结束连接
#### 1.2.2 流程描述（图）

[![..//Image\ Source/Socket1.2.2.png]([..//Image\ Source/Socket1.2.2.png](https://github.com/AirLongDian/EarthOLTransferProps-EngineersLegacyRolls/blob/main/%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%9A%84%E4%BC%A0%E6%89%BF%E5%8D%B7/Image%20Source/Socket1.2.2.png?raw=true))](https://github.com/AirLongDian/EarthOLTransferProps-EngineersLegacyRolls/blob/main/%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%9A%84%E4%BC%A0%E6%89%BF%E5%8D%B7/Image%20Source/Socket1.2.2.png?raw=true)

#### 1.2.3 快速理解（如果你前俩看不懂就看这个）
###### 医院=服务器
###### 病人=客户端
##### 医院方：
医院创建一个挂号导诊台 socket
挂号导诊台设立在此医院门口 bind  
挂号导诊台等待病人来问诊 listen
当病人来时为其挂号安排科室医生 accept（不能让病人全都堵在导诊台聊天）
病人和科室医生交流诊治 read write
病人走了 close
##### 病人方：
病人患了一种病 socket
病人来到医院寻求诊治 connect
与导诊台分配的医生交流 read write
诊治完走了 close

## 二. IO模型
### 2.1 同步/异步、阻塞/非阻塞
#### 2.1.1 关系图
![[IO2.1.png]]
**NOTE：异步情况下一定非阻塞，所以异步根本没有阻塞概念，<font color=red>异步阻塞与异步非阻塞都是错的</font>，异步就是异步！**
#### 2.2.2 同步与异步
同步与异步是指我们进行一个任务时，任务是否等待执行结果。
同步情况下任务会一直等待到结果，然后返回。
异步情况下任务把请求送达之后直接不管了，等执行结果出来了我们会收到通知，这个时候我们再去取结果。
#### 2.2.3 阻塞与非阻塞
阻塞是当我们进行一个任务时，我们是否等待任务结果。
阻塞情况下我们一直等到任务完成结果回来。
非阻塞情况下我们继续进行其他工作并每隔一段时间查看任务是否完成。
如果任务是异步的，根本就不存在等不等任务返回的概念，结果会通过回调方式回来，与请求任务回不回来无关。
#### 2.2.3 快速理解（如果你前三个看不懂就看这个）
###### 我们=老板
###### 任务=实习生小王
###### 阻塞
老板：小王，你去财务那里把工资报表拿过来。
然后老板坐在办公室等报表回来。
###### 非阻塞
老板：小王，你去财务那里把工资报表拿过来。
然后老板该干啥干啥去了，
过一会再回来看看小王把报表拿回来没
###### 异步
老板：小王，你一会和财务说一下，尽快把工资报表发我邮箱
然后老板和小王该干啥干啥去了，过了一段时间老板邮箱收到了报表
###### 为什么异步没有阻塞非阻塞的概念
老板：小王，你一会和财务说一下，尽快把工资报表发我邮箱
然后老板啥都不干了，坐在办公室等小王带着报表回来。
这河狸吗？这不河狸！！
小王告诉财务就该干啥干啥去了，报表在你邮箱里，你在办公室等小王干什么呢？

## 三. 基本的基于Socket 的客户端与服务端实现（Linux环境）

### 3.1 基本服务端实现
```C++
#include <iostream>
#include <unistd.h>
#include <sys/socket.h>
#include <string>
#include <arpa/inet.h>
#include <cstring>
using namespace::std;
int main()
{
	// make socket
	
	int soct1 = socket(AF_INET,SOCK_STREAM,IPPROTO_TCP);
	if (soct1 == -1)
	{
		std::cerr<<"create socket  fail!"<<std::endl;
		return -1;
	}

	// init ip address struct
	struct sockaddr_in saddrin1;
	saddrin1.sin_family = AF_INET;
	saddrin1.sin_port = htons(12345);
	saddrin1.sin_addr.s_addr = INADDR_ANY;
	// bind socket to host
	int ret = bind(soct1,(sockaddr*)&saddrin1,sizeof(saddrin1));
	if(ret == -1)
	{
		std::cout<<"bind failled!"<<std::endl;
		return -1;
	}
	// set listen
	ret =listen(soct1,10);
	if(ret == -1)
	{
		std::cout<<"listen failled!"<<std::endl;
		return -1;
	}
	// init ip address
	struct sockaddr_in saddrin2;
	socklen_t addrlen =sizeof(saddrin2);
	// access connect
	int acc1 = accept(soct1,(sockaddr*)&saddrin2,&addrlen);
	if(ret == -1)
	{
		std::cout<<"connect failled!"<<std::endl;
		return -1;
	}

	// show c_ip addr
	char ip[32];
	cout<<inet_ntop(AF_INET,&saddrin2.sin_addr.s_addr,ip,sizeof(ip))<<"\n\n";
	// make read buffer
	char buff[1024];
	// read
	while(true)
	{
		int len = recv(acc1,buff,sizeof(buff),0);
		if(len>0)
		{
			cerr<<buff<<endl;
			memset(buff,0,sizeof(buff));
			sprintf(buff,"hello customer!");
			send(acc1,buff,strlen(buff)+1,0);
		}
		else if(len = 0)
		{
			break;
		}
		else
		{
			cerr<<"ERR";
			break;
		}
	}
	// clean connect
	close(soct1);
	close(acc1);

	return 0;
}

```

### 3.2 基本客户端实现

```C++
#include <iostream>
#include <arpa/inet.h>
#include <unistd.h>
#include <cstring>
int main()
{
	// make socket
	int soct1=socket(AF_INET,SOCK_STREAM,0);
	if(soct1 == -1)
	{
		std::cerr<<"create socket failed!"<<std::endl;
		return -1;
	}
	// init ip address
	struct sockaddr_in host;
	host.sin_family = AF_INET;
	host.sin_port = htons(12345);
	host.sin_addr.s_addr = INADDR_ANY;
	//connect
	int flag=connect(soct1,(sockaddr*)&host,sizeof(sockaddr));
	if (flag==-1)
	{
		std::cerr<<"connect failed"<<std::endl;
		return -1;
	}
	char buffer[1024];
	//communicate
	while (true)
	{	
		sprintf(buffer,"hello server!");
		send(soct1,buffer,strlen(buffer)+1,0);
		//reset buffer
		memset(buffer,0,sizeof(buffer));
		int rec1=recv(soct1,buffer,sizeof(buffer),0);
		if (rec1>0)
		{
			std::cerr<<buffer<<std::endl;
		}
		else if (rec1=0)
		{
			std::cerr<<"server disconnect"<<std::endl;
		}
		else
		{
			std::cerr<<"send failed"<<std::endl;
		}
		sleep(1);
	}
	close(soct1);
	
}

```
