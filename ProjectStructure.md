# 🏗️ 项目模板目录结构指南

基于7个专业NuGet类库的WPF企业级应用项目标准目录结构 (基于BCRobot.ICA项目实践)。

## 📁 项目目录结构

```
YourProject/
├── YourProject.WPF/                       # 主WPF应用程序
│   ├── App.xaml                           # 应用程序入口和资源定义 ⭐
│   ├── App.xaml.cs                        # 应用程序启动配置和依赖注入 ⭐
│   ├── YourProject.csproj                 # 项目文件
│   ├── appsettings.json                   # 应用程序配置文件
│   ├── AssemblyInfo.cs                    # 程序集信息
│   ├── HostModule.cs                      # 主机模块配置
│   ├── SdData.cs                          # 共享数据模型
│   ├── Topics.cs                          # 主题定义
│   │
│   ├── View/                              # 视图层
│   │
│   ├── ViewModel/                         # 视图模型层
│   │
│   ├── Model/                             # 数据模型
│   │
│   ├── Services/                          # 业务服务
│   │   ├── Interfaces/                    # 服务接口
│   │   └── Implementation/                # 服务实现
│   │
│   ├── HostService/                       # 主机服务
│   │   └── Background/                    # 后台服务
│   │
│   ├── Automation/                        # 自动化工作流
│   │
│   ├── Devices/                           # 设备管理
│   │
│   ├── Interface/                         # 接口定义
│   │   ├── IDeviceService.cs              # 设备服务接口
│   │   ├── IMachineStatusService.cs       # 机器状态服务接口
│   │   └── IWorkflowService.cs            # 工作流服务接口
│   │
│   ├── Converter/                         # 数据转换器
│   │   └── ValueConverters/               # 值转换器
│   │
│   ├── Configs/                           # 配置文件
│   │
│   ├── Asset/                             # 资产文件
│   │   ├── Images/                        # 图片资源
│   │   ├── Icons/                         # 图标字体
│   │   └── Styles/                        # 样式文件
│   │
│   └── Halcon/                            # Halcon相关
│       ├── Scripts/                       # 脚本文件
│       └── Templates/                     # 模板文件
│
├── OtherModules/                          # 其他项目模块
│   ├── RobotModbusTCPServer/             # Modbus TCP服务器
│   └── ProjectFiles/                     # 项目文件
│
├── docs/                                # 文档目录
│   ├── API/                            # API文档
│   ├── UserGuide/                      # 用户指南
│   ├── DeveloperGuide/                 # 开发者指南
│   └── Architecture/                   # 架构文档
│
├── .gitignore                          # Git忽略文件
├── YourProject.sln                     # 解决方案文件
├── Directory.Build.props               # 全局项目属性
├── README.md                           # 项目说明文档
```

## 📋 目录功能说明

### 🎯 核心项目层次

| 层次 | 目录 | 职责 | 依赖的NuGet包 |
|------|------|------|---------------|
| **表示层** | `src/YourProject.WPF/` | UI界面、用户交互、视图逻辑 | WPFMultiLanguage, WPFProject.Share |
| **业务层** | `src/YourProject.Core/` | 业务逻辑、领域模型、服务接口 | WPFLibrary.Automation, ProjectLibrary.EventBus |
| **基础设施层** | `src/YourProject.Infrastructure/` | 数据访问、外部服务、配置管理 | WPFProject.Authority, ProjectLibrary.Communication, ModbusMessageToolkit |

### 🔧 关键功能模块

#### 1. **多语言国际化** (`Resources/Languages/`)
- 基于 `WPFMultiLanguage` 包
- 支持动态语言切换
- XAML声明式翻译绑定

#### 2. **权限管理** (`Core/Services/Authority/`)
- 基于 `WPFProject.Authority` 包
- 用户认证、角色管理、权限控制
- SqlSugar ORM 集成

#### 3. **设备通信** (`Core/Communication/`)
- 基于 `ProjectLibrary.Communication` 和 `ModbusMessageToolkit`
- Modbus TCP/RTU、串口通信
- 设备状态监控和数据采集

#### 4. **自动化工作流** (`Core/Automation/`)
- 基于 `WPFLibrary.Automation` 包
- 状态机驱动的工作流引擎
- 复杂业务流程建模

#### 5. **事件总线** (`Core/Events/`)
- 基于 `ProjectLibrary.EventBus` 包
- 解耦的消息传递机制
- 异步事件处理

### 🚀 最佳实践建议

#### 📁 命名约定
- **命名空间**: 遵循 `Company.Product.Module.SubModule` 格式
- **文件夹**: 使用PascalCase命名
- **文件名**: 与类名保持一致，使用PascalCase

#### 🔄 依赖关系
- **表示层** → **业务层** → **基础设施层**
- 避免循环依赖
- 通过接口实现依赖倒置

#### 📦 模块化设计
- 每个功能模块独立可测试
- 清晰的边界和职责分离
- 支持插件式扩展

#### 🛡️ 安全考虑
- 权限验证在业务层实现
- 敏感配置信息加密存储
- 日志记录不包含敏感信息

## 🔑 核心文件

### ⭐ App.xaml - 应用程序资源定义

```xml
<Application
    x:Class="YourProject.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:ui="http://schemas.lepo.co/wpfui/2022/xaml"
    DispatcherUnhandledException="Application_DispatcherUnhandledException"
    Exit="Application_Exit"
    Startup="Application_Startup">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!-- WPF UI主题 -->
                <ui:ThemesDictionary Theme="Dark" />
                <ui:ControlsDictionary />
                <!-- 共享组件样式 -->
                <ResourceDictionary Source="pack://application:,,,/WPFProject.Share;component/ResourceDictionary/DefaultStyle.xaml" />
            </ResourceDictionary.MergedDictionaries>

            <Style BasedOn="{StaticResource DefaultComboBoxStyle}" TargetType="{x:Type ComboBox}" />
            
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### ⭐ App.xaml.cs - 应用程序启动和配置

```csharp
using System.Windows;
using System.Windows.Threading;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ProjectLibrary.Communication.Interface;
using WPFProject.MultiLanguage;
using WPFProject.Share;
using Wpf.Ui;

namespace YourProject;

public partial class App : Application
{
    private static IHost? _host;
    private Task? _initTask;
    private IEnumerable<IModule>? _modules;

    /// <summary>
    /// 应用程序启动事件
    /// </summary>
    private async void Application_Startup(object sender, StartupEventArgs e)
    {
        // 1. 检查单实例运行
        CheckSingleInstance();
        
        // 2. 异步初始化主机
        _initTask = Task.Run(() =>
        {
            InitializeAssembly();
            InitializeHost();
        });
        
        // 3. 加载多语言翻译
        await TranslationCore.LoadTranslations();

        // 4. 加载主窗口
        await LoadMainWindow();
        
        // 5. 清理旧日志文件
        _ = Task.Run(AppHelper.DeleteOldLogFiles);
    }

    /// <summary>
    /// 初始化程序集模块
    /// </summary>
    private void InitializeAssembly()
    {
        _modules = [new HostModule()];
    }

    /// <summary>
    /// 初始化依赖注入主机
    /// </summary>
    private void InitializeHost()
    {
        var hostBuilder = Host.CreateDefaultBuilder();

        // 配置默认设置
        hostBuilder.ConfigureDefaultConfigration();

        // 注册服务
        hostBuilder.ConfigureServices(services =>
        {
            services.AddDefaultServices();
            services.AddBaseSettingPage();
            services.AddDatabaseService();
            
            // 注册模块服务
            _modules!.ForEach(it => it.RegisterServices(services));
        });

        _host = hostBuilder.Build();
        
        // 主机启动前的初始化
        _modules?.ForEach(n => n.BeforeHostRun(_host.Services));
    }

    /// <summary>
    /// 加载主窗口
    /// </summary>
    private async Task LoadMainWindow()
    {
        await _initTask!;

        // 启动后台服务
        _ = Task.Run(async () =>
        {
            InitializeDevices();
            await _host!.StartAsync();
        });

        // 主机启动后的初始化
        _modules?.ForEach(n => n.AfterHostRun(_host!.Services));

        // 创建主窗口
        var window = _host?.Services.GetRequiredService<DefaultMainWindow>();
        window!.OnClosingTask = OnClosing;
        MainWindow = window;

        // 设置窗口数据上下文
        MainWindow!.DataContext = _host?.Services.GetRequiredService<DefaultMainViewModel>();
        window.SmartShow(ShowWindowParameter.Defult());

        // 导航到首页
        var navigationService = _host!.Services.GetRequiredService<INavigationService>();
        navigationService.NavigationFristPage();
        
        // 应用主题
        AppHelper.ApplyTheme();
    }

    /// <summary>
    /// 初始化设备
    /// </summary>
    private void InitializeDevices()
    {
        // 初始化配置
    }

    /// <summary>
    /// 应用程序关闭处理
    /// </summary>
    private async Task OnClosing()
    {
        // 释放设备资源(程序内资源)
    }

    /// <summary>
    /// 获取服务实例
    /// </summary>
    public static T? GetService<T>() where T : class
    {
        return _host!.Services.GetService<T>();
    }

    /// <summary>
    /// 应用程序退出事件
    /// </summary>
    private async void Application_Exit(object sender, ExitEventArgs e)
    {
        await TranslationCore.SaveUntranslationText();
        await _host?.StopAsync();
        ApplicationMutexes.ReleaseInstance();
    }

    /// <summary>
    /// 全局异常处理
    /// </summary>
    private void Application_DispatcherUnhandledException(object sender, DispatcherUnhandledExceptionEventArgs e)
    {
        AppHelper.Instance.Application_DispatcherUnhandledException(e);
    }

    /// <summary>
    /// 检查单实例运行
    /// </summary>
    private static void CheckSingleInstance()
    {
        AppHelper.Instance.CheckSingleInstance();
    }
}
```

## 📋 目录功能说明

### 🎯 核心架构组件

| 组件 | 目录 | 职责 | 依赖的NuGet包 |
|------|------|------|---------------|
| **应用程序入口** | `App.xaml/App.xaml.cs` | 启动配置、依赖注入、资源定义 | 所有NuGet包 |
| **视图层** | `View/` | UI界面、用户交互 | WPFMultiLanguage, WPFProject.Share |
| **业务逻辑** | `Services/`, `ViewModel/` | 业务逻辑、数据绑定 | WPFLibrary.Automation, ProjectLibrary.EventBus |
| **设备管理** | `Devices/` | 设备通信、控制 | ProjectLibrary.Communication, ModbusMessageToolkit |
| **工作流引擎** | `Automation/` | 自动化流程、状态机 | WPFLibrary.Automation |
| **权限系统** | `Services/Authority/` | 用户认证、权限控制 | WPFProject.Authority |

### 🔧 关键配置文件

#### 1. **项目文件** (`YourProject.csproj`)
- NuGet包引用
- 目标框架设置
- 构建配置

#### 2. **应用配置** (`appsettings.json`)
- 数据库连接字符串
- 设备连接配置
- 日志配置
- 功能开关

#### 3. **主机模块** (`HostModule.cs`)
- 服务注册
- 中间件配置
- 生命周期管理

### 🚀 最佳实践建议

#### 🔄 依赖注入
- 所有服务通过DI容器管理
- 接口与实现分离
- 生命周期明确定义

#### 📦 模块化设计
- 功能模块独立
- 清晰的边界分离
- 支持热插拔扩展

#### 🛡️ 错误处理
- 全局异常捕获
- 详细日志记录
- 优雅降级处理

``` c#
    <ItemGroup>
        <PackageReference Include="CameraWrapperSDK" Version="*" />
        <PackageReference Include="GoogoControlCardLibrary" Version="*" />
        <PackageReference Include="ProjectLibrary.Automation" Version="*" />
        <PackageReference Include="ProjectLibrary.Communication" Version="*" />
        <PackageReference Include="ProjectLibrary.Database" Version="*" />
        <PackageReference Include="ProjectLibrary.EventBus" Version="*" />
        <PackageReference Include="ServoSDK" Version="1.3.7" />
        <PackageReference Include="ShareDevice" Version="1.0.0" />
        <PackageReference Include="WPFProject.Share" Version="2.4.6" />
    </ItemGroup>
```