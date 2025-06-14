# NuGet 类库完整使用文档

本文档详细介绍了7个专业 .NET 类库的功能特性与使用方法，涵盖WPF应用开发、设备通信、自动化工作流、事件处理等多个领域。

## 📑 目录

1. [WPFMultiLanguage - WPF多语言国际化类库](#1-wpfmultilanguage---wpf多语言国际化类库)
2. [WPFProject.Authority - 权限管理与认证类库](#2-wpfprojectauthority---权限管理与认证类库)
3. [WPFProject.Share - WPF项目共享组件类库](#3-wpfprojectshare---wpf项目共享组件类库)
4. [WPFLibrary.Automation - 企业级自动化工作流类库](#4-wpflibraryautomation---企业级自动化工作流类库)
5. [ModbusMessageToolkit - Modbus协议通信工具包](#5-modbusmessagetoolkit---modbus协议通信工具包)
6. [ProjectLibrary.Communication - 多协议设备通信类库](#6-projectlibrarycommunication---多协议设备通信类库)
7. [ProjectLibrary.EventBus - 事件总线与消息传递类库](#7-projectlibraryeventbus---事件总线与消息传递类库)

---

## 1. WPFMultiLanguage - WPF多语言国际化类库

### 🎯 功能概述
专为WPF应用程序设计的多语言国际化解决方案，提供完整的文本翻译、语言切换和本地化支持。

### ✨ 核心特性
- **XAML声明式翻译**：直接在XAML中进行文本绑定和翻译
- **动态语言切换**：运行时实时切换应用程序语言
- **多参数绑定支持**：支持复杂格式化文本的翻译
- **代码级翻译**：在C#代码中进行字符串翻译
- **UI元素批量翻译**：自动翻译整个依赖对象树

### 📖 使用指南

#### 1. XAML中的使用

**添加命名空间引用：**
```xml
xmlns:i18n="clr-namespace:WPFProject.MultiLanguage;assembly=WPFProject.MultiLanguage"
```

**基础文本翻译：**
```xml
<TextBlock Text="{i18n:Str 欢迎使用系统}" />
```

**格式化文本绑定：**
```xml
<TextBox Text="{i18n:Binding Title, StringFormat=当前标题：{0}}" />
```

**多参数复合绑定：**
```xml
<TextBlock Text="{i18n:MultiBinding 用户{0}在{1}执行了{2}操作, 
    Arg1={Binding UserName}, 
    Arg2={Binding Timestamp}, 
    Arg3={Binding Action}}" />
```

#### 2. C#代码中的使用

**字符串翻译：**
```csharp
string message = "操作成功";
string translatedMessage = message.Translate();
```

**动态语言切换：**
```csharp
// 切换到英文
TranslationCore.SwitchLanguage("en-US");
// 切换到中文
TranslationCore.SwitchLanguage("zh-CN");
```

**批量UI翻译：**
```csharp
// 翻译整个窗口的所有文本元素
UiTranslator.Instance.Translate(this);
// 翻译特定控件
UiTranslator.Instance.Translate(myUserControl);
```

### 🚀 高级功能

**语言切换事件监听：**
```csharp
TranslationCore.LanguageChanged += (sender, newLanguage) => {
    // 处理语言切换后的逻辑
    Console.WriteLine($"语言已切换到：{newLanguage}");
};
```

---

## 2. WPFProject.Authority - 权限管理与认证类库

### 🎯 功能概述
基于SqlSugar的企业级权限管理系统，提供完整的用户认证、角色管理和权限控制功能。

### ✨ 核心特性
- **SqlSugar ORM集成**：高性能数据库操作支持
- **SQLite数据库支持**：轻量级本地数据存储
- **SHA256加密算法**：安全的密码加密机制
- **自动数据库初始化**：一键创建数据库结构
- **超级管理员账户**：预置系统管理员

### 📖 使用指南

#### 基础服务访问
```csharp
// 获取权限服务实例
var authorityService = AuthorityService.Instance;
```

#### 用户认证示例
```csharp
// 用户登录验证
bool isAuthenticated = await authorityService.AuthenticateAsync(username, password);

// 创建新用户
var newUser = new User 
{
    Username = "testuser",
    Password = "password123",
    Email = "test@example.com"
};
await authorityService.CreateUserAsync(newUser);

// 分配角色
await authorityService.AssignRoleAsync(userId, roleId);
```

#### 权限检查
```csharp
// 检查用户权限
bool hasPermission = await authorityService.CheckPermissionAsync(userId, "READ_DATA");

// 获取用户角色
var userRoles = await authorityService.GetUserRolesAsync(userId);
```

---

## 3. WPFProject.Share - WPF项目共享组件类库

### 🎯 功能概述
WPF项目的公共组件和工具类集合，提供项目间复用的核心功能模块。

### ✨ 主要特性
- **通用控件库**：常用WPF控件的封装和扩展
- **工具类集合**：数据转换、验证、格式化等工具
- **样式模板**：统一的UI样式和控件模板
- **扩展方法**：便捷的.NET扩展方法

---

## 4. WPFLibrary.Automation - 企业级自动化工作流类库

### 🎯 功能概述
专业的自动化工作流引擎，支持复杂业务流程的建模、执行和监控。适用于工业自动化、业务流程自动化等场景。

### ✨ 核心架构组件

#### 🔧 Router - 工作流路由器
- 管理工作步骤的执行顺序
- 提供启动、停止、重置功能
- 工作流状态监控和事件通知

#### 🔧 WorkStepBase - 工作步骤基类
- 标准化的步骤生命周期
- 内置状态机支持
- 异常处理和错误恢复

#### 🔧 WorkStateMachine - 状态机引擎
- 复杂状态转换逻辑
- 条件分支和循环支持
- 异步状态处理

#### 🔧 WorkingContext - 工作上下文
- 步骤间数据传递
- 执行状态跟踪
- 取消令牌支持

### 📖 快速开始指南

#### 1. 定义工作上下文
```csharp
public class ProductionContext : WorkingContextBase
{
    public string ProductId { get; set; }
    public int BatchSize { get; set; }
    public List<string> ProcessedItems { get; set; } = new();
    public DateTime StartTime { get; set; }
}
```

#### 2. 定义状态枚举
```csharp
public enum ProductionStates
{
    [Description("初始化生产线")]
    InitializeProduction,
    [Description("准备原材料")]
    PrepareMaterials,
    [Description("开始生产")]
    StartProduction,
    [Description("质量检测")]
    QualityCheck,
    [Description("包装产品")]
    PackageProduct,
    [Description("生产完成")]
    ProductionComplete
}
```

#### 3. 实现工作步骤
```csharp
public class ProductionStep : WorkStepBase<ProductionStates, ProductionContext>
{
    public override string Name => "产品生产流程";

    public override void Configure(WorkStateMachine<ProductionStates, ProductionContext> workAutomation)
    {
        workAutomation
            .ConfigureBoundary(ProductionStates.InitializeProduction, ProductionStates.ProductionComplete)
            .ConfigureFunction(ProductionStates.InitializeProduction, async (context) =>
            {
                context.StartTime = DateTime.Now;
                await InitializeProductionLineAsync(context);
                return ProductionStates.PrepareMaterials;
            })
            .ConfigureFunction(ProductionStates.PrepareMaterials, async (context) =>
            {
                bool materialsReady = await PrepareMaterialsAsync(context);
                return materialsReady ? ProductionStates.StartProduction : ProductionStates.PrepareMaterials;
            })
            .ConfigureFunction(ProductionStates.StartProduction, async (context) =>
            {
                await ProduceItemAsync(context);
                return context.ProcessedItems.Count < context.BatchSize 
                    ? ProductionStates.StartProduction 
                    : ProductionStates.QualityCheck;
            })
            .ConfigureFunction(ProductionStates.QualityCheck, async (context) =>
            {
                bool qualityOk = await PerformQualityCheckAsync(context);
                return qualityOk ? ProductionStates.PackageProduct : ProductionStates.StartProduction;
            })
            .ConfigureFunction(ProductionStates.PackageProduct, async (context) =>
            {
                await PackageProductsAsync(context);
                return ProductionStates.ProductionComplete;
            });
    }

    public override async Task<bool> CanExecute(ProductionContext context)
    {
        // 检查生产条件
        return await CheckProductionPrerequisitesAsync(context);
    }

    public override async Task<bool> Enter(ProductionContext context)
    {
        await LogAsync($"开始生产批次：{context.ProductId}");
        return true;
    }

    public override async Task<bool> Exit(ProductionContext context)
    {
        await LogAsync($"生产批次完成：{context.ProductId}，耗时：{DateTime.Now - context.StartTime}");
        return true;
    }

    public override async Task OnError(ProductionContext context, Exception exception)
    {
        await LogErrorAsync($"生产过程发生错误：{exception.Message}", exception);
        // 实现错误恢复逻辑
    }
}
```

#### 4. 创建和执行工作流
```csharp
public async Task RunProductionWorkflowAsync()
{
    // 创建工作上下文
    var context = new ProductionContext
    {
        ProductId = "PROD-001",
        BatchSize = 100
    };

    // 创建路由器
    var router = new Router<ProductionContext>(context);

    // 订阅工作流事件
    router.OnWorkCompleted += (s, e) => Console.WriteLine("生产流程完成！");
    router.OnWorkStatusChanged += (s, e) => Console.WriteLine($"工作状态：{router.IsRunning}");

    // 添加工作步骤
    router.AddStep(new ProductionStep());

    try
    {
        // 启动工作流
        router.Reset();
        router.Start();

        // 等待完成
        while (router.IsRunning)
        {
            await Task.Delay(1000);
            // 可以在这里更新UI或执行其他监控逻辑
        }
    }
    finally
    {
        router.Dispose();
    }
}
```

### 🔧 工作步骤生命周期详解

```mermaid
graph TD
    A[Initialize] --> B[CanExecute?]
    B -->|Yes| C[Enter]
    B -->|No| H[Skip Step]
    C --> D[Working Loop]
    D --> E[OnWorking]
    E --> F[State Machine Execute]
    F --> G[OnWorked]
    G -->|Continue| D
    G -->|Complete| I[Exit]
    I --> J[Step Complete]
    
    C -->|Error| K[OnError]
    D -->|Error| K
    K --> L[Error Handled]
```

### 🚀 高级特性

#### RouterManager - 多工作流管理
```csharp
var manager = new RouterManager();

// 注册多个工作流
manager.AddRouter("Production", productionRouter);
manager.AddRouter("QualityControl", qualityRouter);
manager.AddRouter("Shipping", shippingRouter);

// 批量启动
foreach (var router in manager)
{
    var workflowRouter = manager.GetRouter(router.Key);
    workflowRouter?.Start();
}
```

---

## 5. ModbusMessageToolkit - Modbus协议通信工具包

### 🎯 功能概述
企业级Modbus协议通信解决方案，提供高性能的数据序列化、协议解析和设备通信功能。

### ✨ 核心特性
- **协议自动化处理**：智能的Modbus数据序列化/反序列化
- **灵活配置支持**：多种字节序和地址映射配置
- **高性能架构**：基于表达式树的高效数据处理
- **泛型设计**：类型安全的协议定义
- **异步操作**：全面的异步IO支持

### 📖 使用指南

#### 1. 安装配置
```bash
Install-Package ModbusMessageToolkit
```

#### 2. 定义协议结构
```csharp
public struct IndustrialProtocol
{
    [Address(0)]
    public bool EmergencyStop { get; set; }

    [Address(1)]
    public bool ProductionRunning { get; set; }

    [Address(4)]
    public int Temperature { get; set; }

    [Address(8)]
    public uint ProductionCount { get; set; }

    [Address(12)]
    public float Speed { get; set; }

    [Address(16)]
    public short Pressure { get; set; }
}
```

#### 3. 服务配置
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddModbusMessageService();
    services.AddSingleton<IndustrialProtocol>();
    services.AddTransient<IProtocolSerialize<IndustrialProtocol>, IndustrialProtocolSerializer>();
    services.AddMessageConfigurationService(typeof(Int), ByteOrder.BADC);
}
```

#### 4. 设备连接与操作
```csharp
public class IndustrialControlService
{
    private readonly IMessageBuilder<IndustrialProtocol> _messageBuilder;
    private readonly IModbusClient _modbusClient;

    public async Task ConnectToDeviceAsync()
    {
        string connectionString = "Protocol=1;IP=192.168.1.100;Port=502;Endian=2;ReadTimeout=5000;WriteTimeout=5000";
        await _modbusClient.ConnectAsync(connectionString);
    }

    public async Task StartProductionAsync()
    {
        // 启动生产线
        await _messageBuilder
            .CreateWriteMapping()
            .Property(p => p.ProductionRunning).Value(true)
            .Property(p => p.EmergencyStop).Value(false)
            .CommitAsync();
    }

    public async Task<IndustrialProtocol> ReadAllDataAsync()
    {
        return await _messageBuilder.ReadProtocolAsync();
    }

    public async Task<int> ReadTemperatureAsync()
    {
        return await _messageBuilder.ReadProtocolAsync(p => p.Temperature);
    }

    public async Task UpdateProductionParametersAsync(float speed, short pressure)
    {
        await _messageBuilder
            .CreateWriteMapping()
            .Property(p => p.Speed).Value(speed)
            .Property(p => p.Pressure).Value(pressure)
            .CommitAsync();
    }
}
```

---

## 6. ProjectLibrary.Communication - 多协议设备通信类库

### 🎯 功能概述
统一的设备通信框架，支持Modbus、TCP/IP、串口等多种通信协议，提供设备管理、数据监控和心跳机制。

### ✨ 核心特性
- **多协议支持**：Modbus RTU/TCP、TCP/IP、Serial Port
- **统一设备管理**：集中式设备连接和访问
- **实时数据监控**：位数据变化和条件监控
- **心跳保持机制**：自动维护设备连接状态
- **字节序处理**：完整的字节序转换工具
- **可插拔架构**：支持自定义通信客户端

### 📖 连接字符串配置

#### Modbus RTU
```json
{
  "ModbusRTUConnectionString": "Protocol=0;PortName=COM1;DataBits=8;Parity=None;StopBits=One;BaudRate=9600;Endian=BigEndian;ReadTimeout=3000;WriteTimeout=3000"
}
```

#### Modbus TCP
```json
{
  "ModbusTCPConnectionString": "Protocol=1;IP=192.168.1.100;Port=502;Endian=BigEndian;ReadTimeout=3000;WriteTimeout=3000"
}
```

#### 串口通信
```json
{
  "SerialPortConnectionString": "PortName=COM3;DataBits=8;Parity=None;StopBits=One;BaudRate=115200;ReadTimeout=3000;WriteTimeout=3000"
}
```

#### TCP客户端
```json
{
  "TcpConnectionString": "IP=192.168.1.200;Port=8080;ReadTimeout=5000;WriteTimeout=5000"
}
```

### 📖 使用指南

#### 1. 设备管理器初始化
```csharp
using ProjectLibrary.Communication.Implement;
using ProjectLibrary.Communication.Interface;

// 创建设备管理器
IDeviceManager deviceManager = new DeviceManager();
```

#### 2. Modbus TCP设备通信
```csharp
public class ModbusDeviceService
{
    private readonly IDeviceManager _deviceManager;
    private readonly string _connectionString = "Protocol=1;IP=192.168.1.100;Port=502;Endian=BigEndian;ReadTimeout=3000;WriteTimeout=3000";

    public async Task InitializeDeviceAsync()
    {
        // 创建Modbus客户端
        IClient modbusClient = new FluentModbusClient();
        _deviceManager.AddDevice("PLC01", modbusClient);

        // 连接设备
        var client = _deviceManager.GetDevice<FluentModbusClient>("PLC01");
        await client.ConnectAsync(_connectionString);
    }

    public async Task<Memory<byte>> ReadHoldingRegistersAsync(byte unitId, ushort address, ushort count)
    {
        var client = _deviceManager.GetDevice<FluentModbusClient>("PLC01");
        return await client.ReadHoldingRegistersAsync(unitId, address, count);
    }

    public async Task WriteRegistersAsync(byte unitId, ushort address, ushort[] values)
    {
        var client = _deviceManager.GetDevice<FluentModbusClient>("PLC01");
        await client.WriteMultipleRegistersAsync(unitId, address, values);
    }
}
```

#### 3. 串口设备通信
```csharp
public class SerialDeviceService
{
    private readonly IDeviceManager _deviceManager;

    public async Task InitializeSerialDeviceAsync()
    {
        IClient serialClient = new PlugableSerialPort();
        _deviceManager.AddDevice("Sensor01", serialClient);

        string connectionString = "PortName=COM3;BaudRate=115200;DataBits=8;Parity=None;StopBits=One";
        var client = _deviceManager.GetDevice<PlugableSerialPort>("Sensor01");
        
        client.Connect(connectionString);
    }

    public async Task<string> SendCommandAsync(string command)
    {
        var client = _deviceManager.GetDevice<PlugableSerialPort>("Sensor01");
        
        // 发送命令
        byte[] commandBytes = Encoding.UTF8.GetBytes(command + "\r\n");
        client.Write(commandBytes);
        
        // 读取响应
        byte[] buffer = new byte[1024];
        int bytesRead = client.Read(buffer);
        return Encoding.UTF8.GetString(buffer, 0, bytesRead);
    }
}
```

#### 4. 数据监控服务
```csharp
public class DeviceMonitoringService
{
    private readonly BitMonitorManager _bitMonitor;

    public DeviceMonitoringService()
    {
        _bitMonitor = new BitMonitorManager();
        SetupMonitoring();
    }

    private void SetupMonitoring()
    {
        // 监控特定地址的位变化
        _bitMonitor.AddMonitorAddressChanged(100, (data) =>
        {
            Console.WriteLine($"地址100的值从{data.OldValue}变为{data.NewValue}");
            if (data.NewValue) // 设备启动
            {
                OnDeviceStarted();
            }
        });

        // 监控报警状态
        _bitMonitor.AddMonitorAddress(200, true, (data) =>
        {
            if (data.Value) // 报警触发
            {
                OnAlarmTriggered(data);
            }
        });
    }

    public void UpdateMonitorData(byte[] deviceData)
    {
        _bitMonitor.CheckData(deviceData);
    }

    private void OnDeviceStarted()
    {
        Console.WriteLine("设备已启动，开始监控流程");
        // 实现设备启动后的逻辑
    }

    private void OnAlarmTriggered(MonitorDataModel<bool> alarmData)
    {
        Console.WriteLine($"报警触发！地址：{alarmData.Address}，时间：{alarmData.Timestamp}");
        // 实现报警处理逻辑
    }
}
```

#### 5. 心跳监控
```csharp
public class DeviceHeartbeatService
{
    private readonly HeartbeatInvoker _heartbeat;
    private readonly IClient _device;

    public DeviceHeartbeatService(IClient device)
    {
        _device = device;
        _heartbeat = new HeartbeatInvoker();
        _heartbeat.HeartbeatInterval = TimeSpan.FromSeconds(30);
        _heartbeat.OnHeartbeat += CheckDeviceConnection;
    }

    public void StartHeartbeat()
    {
        _heartbeat.StartHeartbeat();
    }

    public void StopHeartbeat()
    {
        _heartbeat.StopHeartbeat();
    }

    private async Task CheckDeviceConnection()
    {
        try
        {
            // 发送心跳包
            await _device.WriteAsync(new byte[] { 0x01, 0x02, 0x03 });
            Console.WriteLine("心跳检测正常");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"心跳检测失败：{ex.Message}");
            // 实现重连逻辑
            await ReconnectDeviceAsync();
        }
    }

    private async Task ReconnectDeviceAsync()
    {
        // 设备重连逻辑
        Console.WriteLine("尝试重新连接设备...");
    }
}
```

### 🔧 字节序处理工具
```csharp
public class ByteOrderExample
{
    public void DemonstrateByteOrderOperations()
    {
        // 整数转字节数组
        int value = 0x12345678;
        
        // 默认字节序 (ABCD)
        byte[] defaultBytes = ByteUnit.GetBytes(value);
        // 结果: [0x12, 0x34, 0x56, 0x78]
        
        // BADC字节序
        byte[] badcBytes = ByteUnit.GetBytes(value, ByteOrder.BADC);
        // 结果: [0x34, 0x12, 0x78, 0x56]
        
        // 字节序转换
        byte[] originalBytes = { 0x11, 0x22, 0x33, 0x44 };
        byte[] reorderedBytes = ByteUnit.SwitchByteOrder(originalBytes, ByteOrder.DCBA);
        // 结果: [0x44, 0x33, 0x22, 0x11]
        
        // 字节数组合并
        byte[] array1 = { 0x01, 0x02 };
        byte[] array2 = { 0x03, 0x04 };
        byte[] combined = ByteUnit.CombineBytes(array1, array2);
        // 结果: [0x01, 0x02, 0x03, 0x04]
    }
}
```

---

## 7. ProjectLibrary.EventBus - 事件总线与消息传递类库

### 🎯 功能概述
高性能的内存事件总线系统，提供松耦合的组件间通信机制，支持发布-订阅模式的消息传递。

### ✨ 核心特性
- **发布-订阅模式**：解耦的事件驱动架构
- **类型安全**：强类型事件和订阅者
- **异步处理**：非阻塞的事件处理机制
- **错误恢复**：内置的异常处理和错误事件
- **生命周期管理**：自动的订阅者生命周期管理

### 📖 核心接口

#### IEventBus
主要的事件总线接口，提供发布和订阅功能。

#### AtioEventBus
`IEventBus`的默认实现类，提供完整的事件总线功能。

### 🔧 内置事件主题
```csharp
namespace ProjectLibrary.EventBus
{
    public class Topics
    {
        public const string EVENT_DEADED = "localeventbus.topic://EVENT_DEADED";
        public const string SUBSCRIBER_CRASHED = "localeventbus.topic://SUBSCRIBER_CRASHED";
        public const string TOPIC_MATCHING_CRASHED = "localeventbus.topic://TOPIC_MATCHING_CRASHED";
    }
}
```

### 📖 使用指南

#### 1. 定义事件消息
```csharp
// 用户操作事件
public class UserActionEvent
{
    public string UserId { get; set; }
    public string Action { get; set; }
    public DateTime Timestamp { get; set; }
    public object Data { get; set; }
}

// 系统状态事件
public class SystemStatusEvent
{
    public string ComponentName { get; set; }
    public SystemStatus Status { get; set; }
    public string Message { get; set; }
}

public enum SystemStatus
{
    Online,
    Offline,
    Warning,
    Error
}
```

#### 2. 创建事件总线服务
```csharp
public class EventBusService
{
    private readonly IEventBus _eventBus;

    public EventBusService()
    {
        _eventBus = new AtioEventBus();
        SubscribeToSystemEvents();
    }

    private void SubscribeToSystemEvents()
    {
        // 订阅系统错误事件
        _eventBus.Subscribe<EventDeadedMessage>(Topics.EVENT_DEADED, OnEventDeaded);
        _eventBus.Subscribe<SubscriberCrashedMessage>(Topics.SUBSCRIBER_CRASHED, OnSubscriberCrashed);
        _eventBus.Subscribe<TopicMatchingCrashedMessage>(Topics.TOPIC_MATCHING_CRASHED, OnTopicMatchingCrashed);
    }

    private void OnEventDeaded(EventDeadedMessage message)
    {
        Console.WriteLine($"事件处理失败：{message.EventType} - {message.Reason}");
        // 实现事件恢复逻辑
    }

    private void OnSubscriberCrashed(SubscriberCrashedMessage message)
    {
        Console.WriteLine($"订阅者崩溃：{message.SubscriberName} - {message.Exception}");
        // 实现订阅者重启逻辑
    }

    private void OnTopicMatchingCrashed(TopicMatchingCrashedMessage message)
    {
        Console.WriteLine($"主题匹配失败：{message.Topic} - {message.Error}");
        // 实现主题匹配错误处理
    }

    public void PublishUserAction(string userId, string action, object data = null)
    {
        var userEvent = new UserActionEvent
        {
            UserId = userId,
            Action = action,
            Timestamp = DateTime.Now,
            Data = data
        };

        _eventBus.Publish("user.action", userEvent);
    }

    public void PublishSystemStatus(string component, SystemStatus status, string message = null)
    {
        var statusEvent = new SystemStatusEvent
        {
            ComponentName = component,
            Status = status,
            Message = message
        };

        _eventBus.Publish("system.status", statusEvent);
    }
}
```

#### 3. 应用服务集成
```csharp
public class ApplicationService
{
    private readonly EventBusService _eventBus;

    public ApplicationService(EventBusService eventBus)
    {
        _eventBus = eventBus;
        SetupEventSubscriptions();
    }

    private void SetupEventSubscriptions()
    {
        // 订阅用户操作事件
        _eventBus.Subscribe<UserActionEvent>("user.action", OnUserAction);
        
        // 订阅系统状态事件
        _eventBus.Subscribe<SystemStatusEvent>("system.status", OnSystemStatusChanged);
    }

    private async void OnUserAction(UserActionEvent userEvent)
    {
        Console.WriteLine($"用户 {userEvent.UserId} 执行了操作：{userEvent.Action}");
        
        // 根据不同操作执行相应逻辑
        switch (userEvent.Action)
        {
            case "LOGIN":
                await HandleUserLoginAsync(userEvent);
                break;
            case "LOGOUT":
                await HandleUserLogoutAsync(userEvent);
                break;
            case "DATA_EXPORT":
                await HandleDataExportAsync(userEvent);
                break;
        }
    }

    private void OnSystemStatusChanged(SystemStatusEvent statusEvent)
    {
        Console.WriteLine($"组件 {statusEvent.ComponentName} 状态变更为：{statusEvent.Status}");
        
        if (statusEvent.Status == SystemStatus.Error)
        {
            // 触发错误处理流程
            HandleComponentError(statusEvent);
        }
    }

    private async Task HandleUserLoginAsync(UserActionEvent loginEvent)
    {
        // 更新用户在线状态
        // 记录登录日志
        // 发送欢迎消息
        _eventBus.PublishSystemStatus("UserService", SystemStatus.Online, 
            $"用户 {loginEvent.UserId} 已登录");
    }

    private async Task HandleDataExportAsync(UserActionEvent exportEvent)
    {
        try
        {
            // 执行数据导出逻辑
            await ExportDataAsync(exportEvent.Data);
            
            // 发布导出完成事件
            _eventBus.PublishUserAction(exportEvent.UserId, "EXPORT_COMPLETED", 
                new { ExportTime = DateTime.Now, Status = "Success" });
        }
        catch (Exception ex)
        {
            _eventBus.PublishSystemStatus("ExportService", SystemStatus.Error, 
                $"数据导出失败：{ex.Message}");
        }
    }
}
```

#### 4. 跨模块通信示例
```csharp
// 生产模块
public class ProductionModule
{
    private readonly IEventBus _eventBus;

    public ProductionModule(IEventBus eventBus)
    {
        _eventBus = eventBus;
    }

    public async Task StartProductionAsync(string productId)
    {
        // 开始生产
        _eventBus.Publish("production.started", new 
        {
            ProductId = productId,
            StartTime = DateTime.Now
        });

        // 生产过程...
        await Task.Delay(5000);

        // 生产完成
        _eventBus.Publish("production.completed", new 
        {
            ProductId = productId,
            CompletedTime = DateTime.Now,
            Quantity = 100
        });
    }
}

// 质检模块
public class QualityModule
{
    private readonly IEventBus _eventBus;

    public QualityModule(IEventBus eventBus)
    {
        _eventBus = eventBus;
        
        // 订阅生产完成事件
        _eventBus.Subscribe<dynamic>("production.completed", OnProductionCompleted);
    }

    private async void OnProductionCompleted(dynamic productionData)
    {
        Console.WriteLine($"开始质检产品：{productionData.ProductId}");
        
        // 执行质检
        bool qualityPassed = await PerformQualityCheckAsync(productionData.ProductId);
        
        // 发布质检结果
        _eventBus.Publish("quality.checked", new 
        {
            ProductId = productionData.ProductId,
            QualityPassed = qualityPassed,
            CheckTime = DateTime.Now
        });
    }
}

// 库存模块
public class InventoryModule
{
    private readonly IEventBus _eventBus;

    public InventoryModule(IEventBus eventBus)
    {
        _eventBus = eventBus;
        
        // 订阅质检完成事件
        _eventBus.Subscribe<dynamic>("quality.checked", OnQualityChecked);
    }

    private async void OnQualityChecked(dynamic qualityData)
    {
        if (qualityData.QualityPassed)
        {
            // 更新库存
            await UpdateInventoryAsync(qualityData.ProductId);
            
            _eventBus.Publish("inventory.updated", new 
            {
                ProductId = qualityData.ProductId,
                UpdateTime = DateTime.Now
            });
        }
        else
        {
            // 处理不合格产品
            _eventBus.Publish("product.rejected", new 
            {
                ProductId = qualityData.ProductId,
                RejectedTime = DateTime.Now
            });
        }
    }
}
```

---

## 🚀 综合应用示例

### 智能制造系统集成
```csharp
public class SmartManufacturingSystem
{
    private readonly IDeviceManager _deviceManager;
    private readonly EventBusService _eventBus;
    private readonly Router<ProductionContext> _productionRouter;
    private readonly IMessageBuilder<IndustrialProtocol> _modbusBuilder;

    public async Task InitializeSystemAsync()
    {
        // 初始化设备通信
        await SetupDeviceCommunicationAsync();
        
        // 配置事件总线
        SetupEventBus();
        
        // 创建自动化工作流
        SetupAutomationWorkflow();
        
        // 启动系统监控
        StartSystemMonitoring();
    }

    private async Task SetupDeviceCommunicationAsync()
    {
        // 连接PLC
        var plcClient = new FluentModbusClient();
        _deviceManager.AddDevice("MainPLC", plcClient);
        await plcClient.ConnectAsync("Protocol=1;IP=192.168.1.100;Port=502");

        // 连接传感器
        var sensorClient = new PlugableSerialPort();
        _deviceManager.AddDevice("TempSensor", sensorClient);
        sensorClient.Connect("PortName=COM3;BaudRate=115200");
    }

    private void SetupEventBus()
    {
        _eventBus.Subscribe<ProductionEvent>("production.*", OnProductionEvent);
        _eventBus.Subscribe<QualityEvent>("quality.*", OnQualityEvent);
        _eventBus.Subscribe<AlarmEvent>("alarm.*", OnAlarmEvent);
    }

    private void SetupAutomationWorkflow()
    {
        var context = new ProductionContext();
        _productionRouter = new Router<ProductionContext>(context);
        
        _productionRouter.AddStep(new MaterialPreparationStep());
        _productionRouter.AddStep(new ProductionStep());
        _productionRouter.AddStep(new QualityControlStep());
        _productionRouter.AddStep(new PackagingStep());
        
        _productionRouter.OnWorkCompleted += (s, e) => 
        {
            _eventBus.PublishSystemStatus("Production", SystemStatus.Online, "生产周期完成");
        };
    }

    public async Task StartProductionCycleAsync(string orderId)
    {
        try
        {
            // 通过Modbus启动生产线
            await _modbusBuilder
                .CreateWriteMapping()
                .Property(p => p.ProductionRunning).Value(true)
                .Property(p => p.EmergencyStop).Value(false)
                .CommitAsync();

            // 启动自动化工作流
            _productionRouter.Reset();
            _productionRouter.Start();

            // 发布生产开始事件
            _eventBus.PublishUserAction("SYSTEM", "PRODUCTION_STARTED", new { OrderId = orderId });
        }
        catch (Exception ex)
        {
            _eventBus.PublishSystemStatus("Production", SystemStatus.Error, ex.Message);
        }
    }
}
```

---

## 📋 总结与建议

### 🎯 类库选择建议

| 应用场景 | 推荐类库组合 |
|---------|-------------|
| **WPF桌面应用** | WPFMultiLanguage + WPFProject.Authority + WPFProject.Share |
| **工业自动化** | WPFLibrary.Automation + ProjectLibrary.Communication + ModbusMessageToolkit |
| **设备监控系统** | ProjectLibrary.Communication + ProjectLibrary.EventBus + WPFLibrary.Automation |
| **数据采集平台** | ModbusMessageToolkit + ProjectLibrary.Communication + ProjectLibrary.EventBus |

### 🔧 最佳实践

1. **统一异常处理**：在所有类库的集成点实现统一的异常处理策略
2. **配置管理**：使用配置文件管理连接字符串和系统参数
3. **日志记录**：集成日志框架记录关键操作和错误信息
4. **性能监控**：监控设备通信和工作流执行的性能指标
5. **资源管理**：正确实现IDisposable模式，及时释放资源

### 📈 扩展建议

- **自定义工作步骤**：基于WorkStepBase创建符合业务需求的工作步骤
- **协议扩展**：实现IProtocolSerialize接口支持自定义通信协议
- **事件扩展**：定义业务相关的事件消息类型
- **UI组件扩展**：基于WPFProject.Share开发可复用的UI组件

这套类库提供了从底层设备通信到上层业务逻辑的完整解决方案，支持构建现代化的工业级应用程序。通过合理的架构设计和模块化开发，可以大大提高开发效率和系统的可维护性。