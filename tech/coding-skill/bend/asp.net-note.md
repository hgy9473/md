# ASP.NET Core MVC 学习笔记

## ASP.NET Core 简介
 
> ASP.NET Core 是一个开源和跨平台的框架，用于构建基于Web的现代互联网连接应用程序，例如Web应用程序，IoT应用程序和移动后端。 ASP.NET Core应用程序可以在.NET Core或完整的.NET Framework上运行。 它的架构旨在为部署到云或在本地运行的应用程序提供优化的开发框架。 它由模块化组件组成，开销最小，因此您可以在构建解决方案时保持灵活性。 您可以在Windows，Mac和Linux上跨平台开发和运行ASP.NET核心应用程序。
>
> https://github.com/aspnet/AspNetCore

## 文件的伺服

* 传统的 ASP.NET
  - 根目录的文件都被伺服
  - 某些重要文件不会被伺服
  - 黑名单策略

* ASP.NET Core
  - 只有 wwwroot 被伺服
  - 白名单策略

## 依赖注入，IOC容器

- 生命周期
  - Transient: 每次请求都会创建新的实例
  - Scoped: 每次 web 请求都会创建一个实例
  - Singleton: 一旦被创建实例，就一直使用这个实例，直到应用停止

## 依赖注入的好处

- 不用去管生命周期
- 类型之间没有依赖


## Web 请求的处理流程

浏览器 -- IIS/Nginx/Apache -- .NET Core -- Programs.cs Main -- Web应用 Kestrel -- Log -- Auth -- MVC -- Static Files 

## 常见中间件添加
- AddMvc() useMvc()
- UseStatusCodePages()
- UseStatusCodePagesWithRedirects()
- UseStaticFiles()

## ASP.NET Core 环境变量

- Developemnt
- Production
- Staging 准备上线
- 自定义 

可以根据不同的环境变量调用不同的 Startup 类

## MVC

- Controller
- Action
- Filter
- Model Binding
- Routing
- Attribute





## 建立项目

* 在 Visual Studio 2017 中新建项目

new -> project -> Vsiaul C# -> Web -> APS.NET Core Web Application -> Empty



* 使用 dotnet CLI 新建项目

检查 dotnet 是否可用

```c
//////// 
λ dotnet
/*
Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET Core information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes.

path-to-application:
  The path to an application .dll file to execute.
*/
//////// 
λ dotnet --info
/*
.NET Core SDK（反映任何 global.json）:
 Version:   2.1.507
 Commit:    e8520940d7

运行时环境:
 OS Name:     Windows
 OS Version:  10.0.18362
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\2.1.507\

Host (useful for support):
  Version: 2.1.11
  Commit:  d6a5616240

.NET Core SDKs installed:
  1.1.14 [C:\Program Files\dotnet\sdk]
  2.1.202 [C:\Program Files\dotnet\sdk]
  2.1.507 [C:\Program Files\dotnet\sdk]

.NET Core runtimes installed:
  Microsoft.AspNetCore.All 2.1.11 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.App 2.1.11 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 1.0.16 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 1.1.13 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 2.0.9 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 2.1.11 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]

To install additional .NET Core runtimes or SDKs:
  https://aka.ms/dotnet-download
*/
//////// 
λ dotnet --help
/*
.NET 命令行工具 (2.1.507)
使用情况: dotnet [runtime-options] [path-to-application] [arguments]

执行 .NET Core 应用程序。

runtime-options:
  --additionalprobingpath <path>     要探测的包含探测策略和程序集的路径。
  --additional-deps <path>           指向其他 deps.json 文件的路径。
  --fx-version <version>             要用于运行应用程序的安装版共享框架的版本。
  --roll-forward-on-no-candidate-fx  已启用“不前滚到候选共享框架”。

path-to-application:
  要执行的应用程序 .dll 文件的路径。

使用情况: dotnet [sdk-options] [command] [command-options] [arguments]

执行 .NET Core SDK 命令。

sdk-options:
  -d|--diagnostics  启用诊断输出。
  -h|--help         显示命令行帮助。
  --info            显示 .NET Core 信息。
  --list-runtimes   显示安装的运行时。
  --list-sdks       显示安装的 SDK。
  --version         显示使用中的 .NET Core SDK 版本。

SDK 命令:
  add               将包或引用添加到 .NET 项目。
  build             生成 .NET 项目。
  build-server      与由生成版本启动的服务器进行交互。
  clean             清理 .NET 项目的生成输出。
  help              显示命令行帮助。
  list              列出 .NET 项目的项目引用。
  migrate           将 project.json 项目迁移到 MSBuild 项目。
  msbuild           运行 Microsoft 生成引擎(MSBuild)命令。
  new               创建新的 .NET 项目或文件。
  nuget             提供其他 NuGet 命令。
  pack              创建 NuGet 包。
  publish           发布 .NET 项目进行部署。
  remove            从 .NET 项目中删除包或引用。
  restore           还原 .NET 项目中指定的依赖项。
  run               生成并运行 .NET 项目输出。
  sln               修改 Visual Studio 解决方案文件。
  store             在运行时包存储中存储指定的程序集。
  test              使用 .NET 项目中指定的测试运行程序运行单元测试。
  tool              安装或管理扩展 .NET 体验的工具。
  vstest            运行 Microsoft 测试引擎(VSTest)命令。

捆绑工具中的其他命令:
  dev-certs         创建和管理开发证书。
  ef                Entity Framework Core 命令行工具。
  sql-cache         SQL Server 缓存命令行工具。
  user-secrets      管理开发用户密码。
  watch             启动文件观察程序，它会在文件发生更改时运行命令。

运行 "dotnet [command] --help"，获取有关命令的详细信息。
*/
//////// 
λ dotnet new
/*
欢迎使用 .NET Core!
---------------------
若要详细了解 NET Core: https://aka.ms/dotnet-docs
请使用 “dotnet --help”查看可用的命令或访问: https://aka.ms/dotnet-cli-docs

遥测
---------
.NET Core 工具收集用法数据，以便帮助改善用户体验。数据是匿名的，且不包括命令行参数。数据由 Microsoft 收集并与社区共
享。可使用喜欢的 shell 将环境变量 DOTNET_CLI_TELEMETRY_OPTOUT 设置为 “1” 或 “true”，从而选择推出遥测。

若要深入了解 .NET Core CLI 工具遥测，请访问 https://aka.ms/dotnet-cli-telemetry

ASP.NET Core
------------
已成功安装 ASP.NET Core HTTPS 开发证书。
要信任证书，请运行 "dotnet dev-certs https --trust"(仅限 Windows 和 macOS)。要在其他平台上建立信任，请参阅特定于平台
的文档。
有关配置 HTTPS 的详细信息，请参阅 https://go.microsoft.com/fwlink/?linkid=848054。
正在准备...
使用情况: new [选项]

选项:
  -h, --help          显示有关此命令的帮助。
  -l, --list          列出包含指定名称的模板。如果未指定名称，请列出所有模板。
  -n, --name          正在创建输出的名称。如果未指定任何名称，将使用当前目录的名称。
  -o, --output        要放置生成的输出的位置。
  -i, --install       安装源或模板包。
  -u, --uninstall     卸载一个源或模板包。
  --nuget-source      指定在安装期间要使用的 NuGet 源。
  --type              基于可用的类型筛选模板。预定义的值为 "project"、"item" 或 "other"。
  --force             强制生成内容，即使该内容会更改现有文件。
  -lang, --language   根据语言筛选模板，并指定要创建的模板的语言。


模板                                                短名称                语言                标记

----------------------------------------------------------------------------------------------------------------------------
Console Application                               console            [C#], F#, VB      Common/Console

Class library                                     classlib           [C#], F#, VB      Common/Library

Unit Test Project                                 mstest             [C#], F#, VB      Test/MSTest

NUnit 3 Test Project                              nunit              [C#], F#, VB      Test/NUnit

NUnit 3 Test Item                                 nunit-test         [C#], F#, VB      Test/NUnit

xUnit Test Project                                xunit              [C#], F#, VB      Test/xUnit

Razor Page                                        page               [C#]              Web/ASP.NET

MVC ViewImports                                   viewimports        [C#]              Web/ASP.NET

MVC ViewStart                                     viewstart          [C#]              Web/ASP.NET

ASP.NET Core Empty                                web                [C#], F#          Web/Empty

ASP.NET Core Web App (Model-View-Controller)      mvc                [C#], F#          Web/MVC

ASP.NET Core Web App                              razor              [C#]              Web/MVC/Razor Pages

ASP.NET Core with Angular                         angular            [C#]              Web/MVC/SPA

ASP.NET Core with React.js                        react              [C#]              Web/MVC/SPA

ASP.NET Core with React.js and Redux              reactredux         [C#]              Web/MVC/SPA

Razor Class Library                               razorclasslib      [C#]              Web/Razor/Library/Razor Class Library
ASP.NET Core Web API                              webapi             [C#], F#          Web/WebAPI

global.json file                                  globaljson                           Config

NuGet Config                                      nugetconfig                          Config

Web Config                                        webconfig                            Config

Solution File                                     sln                                  Solution


Examples:
    dotnet new mvc --auth Individual
    dotnet new sln
    dotnet new --help

*/

```

选择文件夹，开始新建项目
```c
λ dotnet new web
/*
已成功创建模板“ASP.NET Core Empty”。

正在处理创建后操作...
正在 H:\study\dotnet\FromCli\FromCli.csproj 上运行 "dotnet restore"...
  正在还原 H:\study\dotnet\FromCli\FromCli.csproj 的包...
  正在生成 MSBuild 文件 H:\study\dotnet\FromCli\obj\FromCli.csproj.nuget.g.props。
  正在生成 MSBuild 文件 H:\study\dotnet\FromCli\obj\FromCli.csproj.nuget.g.targets。
  H:\study\dotnet\FromCli\FromCli.csproj 的还原在 1.82 sec 内完成。

还原成功。
*/

// 运行项目
λ dotnet run
/*
从 H:\study\dotnet\FromCli\Properties\launchSettings.json 使用启动设置...
Hosting environment: Development
Content root path: H:\study\dotnet\FromCli
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
*/
```

## 项目文件解读

文件结构
```c
// dotnet CLI 项目的文件结构
/*
├─bin
│  └─Debug
│      └─netcoreapp2.1
├─obj
│  └─Debug
│      └─netcoreapp2.1
├─Properties
├─appsetttings.Development.json
├─appsetting.json
├─FromCli.csproj
├─Program.cs
├─Startup.cs
*/

// Visual Studio 2017 项目在此基础上多了一个 *.sln 文件。
```

- `Program.cs` 和 `Startup.cs` 两个文件负责管理程序的启动和配置。

- `FromCli.csproj` 在 Visual Studio 2017 中需要右击项目才能找到打开按钮。这个文件包含构建项目的详细信息，如 SDK, .NET Core 版本, NuGet 包等。

- IConfiguration 配置信息的来源
  - 命令行参数
  - 系统环境变量
  - User Secrets
  - appsettings.json






## 参考

【1】 《ASP.NET Core MVC 2.x 全面教程》. 杨旭 . https://www.bilibili.com/video/av38392956?t=877&p=1
【2】 《ASP.NET Core MVC 入门教程》. 杨旭 . https://www.bilibili.com/video/av33728783/?spm_id_from=333.788.videocard.0


