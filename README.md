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
## ASP.NET Core管道模型
连接点？ 管道模型！何谓管道模型？就是个委托！
  
管道模型---承上启下---连接kestrel与MVC---Kestrel监听请求，解析得到HttpContext---MVC就是处理HttpContext(Request&Response)连接点(管道)其实是个委托---RequestDelegate---接受HttpContext参数，处理后得到结果，都在HttpContext里面折腾。
俄罗斯套娃：多层委托嵌套----达到俄罗斯套娃效果---方便扩展管道就是委托！动态组装---随意指定环节轻松扩展---这就是委托嵌套
![image](https://user-images.githubusercontent.com/26539681/125222811-4e1c1a80-e2fd-11eb-9b86-e2dcf9470b00.png)

### 全家桶 VS 自选
Http请求的处理是蛮复杂---不光是生成个HTML---处理Cookie-Session---Token—缓存---重要的是业务步骤---一起很多个步骤—部分是通用的
  
ASP.NET和MVC管道---搭建框架---完成通用部分且提供扩展--基于事件event扩展---配置齐全直接用—但是会付出额外成本
  
ASP.NET Core管道---自选式---只有基本骨架，需要自行配置—要什么组装什么---Pay for what you use
![image](https://user-images.githubusercontent.com/26539681/125223207-f92cd400-e2fd-11eb-9824-053f75268eee.png)

### Middleware各种扩展源码篇
1 Use 2 Run 3 UseWhen 4 Map 5 MapWhen 
  
这些都是对基础Use方法的封装，终结就是不调用Next，后3个都是开了独立的Branch，然后执行
![image](https://user-images.githubusercontent.com/26539681/125223428-56288a00-e2fe-11eb-9c0d-d1069dde0334.png)

## (重点)标准中间件封装 AddMiddleware + UseMiddleware + Options
1 基本结构认知 2 Add集中注册 3 Use扩展类 4 Options传值
  
写一个浏览器校验—如果是Chrome就正常响应，否则返回
![image](https://user-images.githubusercontent.com/26539681/125223637-b0294f80-e2fe-11eb-9b57-dde0f5949643.png)
![image](https://user-images.githubusercontent.com/26539681/125223679-be776b80-e2fe-11eb-8501-11fb873ee8d3.png)
## Stream读取问题
中间件去扩展MVC，其实不能随便写Response—因为请求响应已完成，里面的Content-Length已固定，所以不能改---那怎么改？---OnStarting可以修改Header---但是我想读写Request和Response
  
1 请求体读取问题
  
2 响应体读取问题
![image](https://user-images.githubusercontent.com/26539681/125224422-309c8000-e300-11eb-82aa-afe58111ee34.png)

## 常见中间件解读
待续...

## 中间件放置顺序问题-》是建立在理解源码的基础上
待续...

希望为.net开源社区尽绵薄之力，探lu者###一直在探索前进的路上###（QQ:529987528）
