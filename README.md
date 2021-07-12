# LC_Net5Advanced
## 阅读源码方法和目标
1 自上而下，先整体再细节

2 望文生义，该猜猜该过过

3 理解设计模式，抓住核心套路

目标：理解流程---扩展定制---掌握心法
## ASP.NET Core粗略全貌
1 Program

2 Startup

3 M-V-C

4 appsettings

看源码之前，起码知道是干啥的
## 启动流程
1 准备HostBuilder 

2 Build()

3 HostRun
## Logger组件-常规扩展
![image](https://user-images.githubusercontent.com/26539681/125220866-e4e6d800-e2f9-11eb-8dbf-855a38e257d6.png)
![image](https://user-images.githubusercontent.com/26539681/125220762-ca146380-e2f9-11eb-8720-05f407d018a5.png)
## Option基本操作
解决依赖注入时，需要传递指定数据问题，不是自行获取，而是能上端集中配置

Option多种注册方式 1 Configure 2 ConfigureAll 3 PostConfigure 4 PostConfigureAll 5 AddOptions

![image](https://user-images.githubusercontent.com/26539681/125222143-12348580-e2fc-11eb-81bb-fbac1c52f0c2.png)

Option三种获取方式

1 IOptions<EmailOption> 简单粗暴不更新，就IOptions
  
2 IOptionsMonitor<EmailOption>  需要更新IOptionsMonitor
  
3 IOptionsSnapshot<EmailOption> IOptionsSnapshot(除非单次请求内要求保证不变)
![image](https://user-images.githubusercontent.com/26539681/125222415-8707bf80-e2fc-11eb-8f49-5c72834c5453.png)

