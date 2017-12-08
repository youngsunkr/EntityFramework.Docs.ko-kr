---
title: "EF 코어 2-EF 코어를 이전 버전에서 업그레이드"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 0bd1ea2476621f826cca7d4a526a49a1b902acf8
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>EF 코어 2.0 이전 버전에서 응용 프로그램 업그레이드

## <a name="procedures-common-to-all-applications"></a>모든 응용 프로그램에 공통 된 프로시저

EF 코어 2.0으로 기존 응용 프로그램을 업데이트 해야 할 수 있습니다.

1. 응용 프로그램을 지 원하는 표준.NET 2.0의 대상.NET 플랫폼을 업그레이드 합니다. 참조 [지원 되는 플랫폼](../platforms/index.md) 내용을 확인 합니다.

2. EF 코어 2.0와 호환 되는 대상 데이터베이스에 대 한 공급자를 식별 합니다. 참조 [2.0 데이터베이스 공급자를 필요로 하는 EF 코어 2.0](#ef-core-20-requires-a-20-database-provider) 아래 합니다.

3. 2.0 (런타임 및 도구) 모든 EF 코어 패키지를 업그레이드 합니다. 참조 [EF 코어 설치](../get-started/install/index.md) 내용을 확인 합니다.

4. 주요 변경 내용에 대 한 보정을 위해 필요한 코드 변경을 수행 합니다. 참조는 [주요 변경 내용](#breaking-changes) 자세한 내용을 보려면 아래 섹션.

## <a name="aspnet-core-applications"></a>ASP.NET Core 응용 프로그램

1. 특히 참조는 [응용 프로그램의 서비스 공급자를 초기화 하기 위한 새 패턴](#new-way-of-getting-application-services) 아래에서 설명 합니다.

> [!TIP]  
> 이 새 패턴을 적극 권장 하 고 Entity Framework Core 마이그레이션 같은 제품 기능에 대 한 작업 순서에 필요한 응용 프로그램을 2.0 업데이트를 도입 합니다. 다른 일반적인 대신 [구현 *IDesignTimeDbContextFactory\<TContext >*](configuring-dbcontext.md#using-idesigntimedbcontextfactorytcontext)합니다.

2. ASP.NET Core 2.0을 대상으로 응용 프로그램 EF 코어 2.0을 사용 하 여 제 3 자 데이터베이스 공급자 외에도 추가 종속성 없이 수 있습니다. 그러나 이전 버전의 ASP.NET 코어를 대상으로 응용 프로그램 EF 코어 2.0을 사용 하려면 ASP.NET 코어 2.0으로 업그레이드 해야 합니다. ASP.NET Core 응용 프로그램을 2.0 업그레이드에 대 한 자세한 내용은 참조 [주체에서 ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/)합니다.

## <a name="breaking-changes"></a>주요 변경 사항

크게 우리의 기존 Api 및 2.0의 동작을 조정할 수 있는 기회를 길을 합니다. 기존 응용 프로그램 코드를 수정 필요할 수 있는 몇 가지 기능이 향상 되었습니다, 그리고 하지만 것으로 생각 하는 대부분의 응용 프로그램에 영향을 다시 컴파일 및 최소한 안내 변경 되지 않는 Api를 교체할 수만 필요한 대부분의 경우 낮은 지정 됩니다.

### <a name="new-way-of-getting-application-services"></a>응용 프로그램 서비스를 가져오는 새로운 방법

ASP.NET Core 웹 응용 프로그램에 대 한 권장된 패턴 2.0 EF 코어 1.x에 사용 되는 디자인 타임 논리를 중단 하는 방식에서에 대 한 업데이트 되었습니다. 이전에 디자인 타임에 EF 코어는를 호출 하려고 `Startup.ConfigureServices` 응용 프로그램의 서비스 공급자에 액세스 하기 위해 직접 합니다. ASP.NET Core 2.0 구성 외부의 초기화는 `Startup` 클래스입니다. 일반적으로 EF 코어를 사용 하 여 응용 프로그램, 구성에서 하므로 자신의 연결 문자열을 액세스할 `Startup` 자체적으로 충분 하지 않습니다. ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우에 EF 핵심 도구를 사용 하는 경우 다음과 같은 오류가 나타날 수 있습니다.

> 매개 변수가 없는 생성자가 없습니다 'ApplicationContext'에서 찾았습니다. 'ApplicationContext'에 매개 변수가 없는 생성자를 추가 하거나 구현 추가 ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' 'ApplicationContext'와 같은 어셈블리에

새 디자인 타임 후크 ASP.NET 코어 2.0의 기본 서식 파일에 추가 되었습니다. 정적 `Program.BuildWebHost` 메서드를 사용 하면 디자인 타임에 응용 프로그램의 서비스 공급자에 액세스 하는 EF 코어입니다. ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 업데이트 해야 합니다 `Program` 클래스 다음과 유사 하 게 합니다.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>이름을 바꿀 IDbContextFactory

다양 한 응용 프로그램 패턴을 지원 하 고 사용자가 방법 보다 더 많은 제어를 제공 하기 위해 자신의 `DbContext` 사용 되는 디자인 타임에 있는, 이전에 제공는 `IDbContextFactory<TContext>` 인터페이스입니다. 디자인 타임에 EF 핵심 도구는 프로젝트에서이 인터페이스의 구현 찾아 만드는 데 사용할 `DbContext` 개체입니다.

이 인터페이스는 일부 사용자가 다시 다른 사용 하 여 잘못 된 것 같다고는 매우 개괄적인 이름을 갖고 `DbContext`-시나리오 만들기. EF 도구는 다음 디자인 타임에 구현을 사용 하려고 할 때 문제가 생기지 않도록 보호 하 고 같은 명령을 발생 된 `Update-Database` 또는 `dotnet ef database update` 실패 합니다.

이 인터페이스의 강력한 디자인 타임 의미 체계를 통신 하려면 이름을 하도록 `IDesignTimeDbContextFactory<TContext>`합니다.

2.0 릴리스는 `IDbContextFactory<TContext>` 계속 존재 하지만 사용 되지 않는 것으로 표시 합니다.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions 제거

위에서 설명한 ASP.NET 코어 2.0 변경으로 인해 것을 발견 하는 `DbContextFactoryOptions` 새 필요 없게 된 `IDesignTimeDbContextFactory<TContext>` 인터페이스입니다. 대신 사용 해야 하는 대체 방법을 다음과 같습니다.

DbContextFactoryOptions | 대체
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>디자인 타임 작업 디렉터리 변경

ASP.NET Core 2.0 변경에서 사용 하는 작업 디렉터리에도 필요 `dotnet ef` 응용 프로그램을 실행 하는 경우 Visual Studio에서 사용 하는 작업 디렉터리에 맞게 합니다. 이 한 눈에 띄는 부작용은 파일 이름을 이제 출력 디렉터리가 아닌을 프로젝트 디렉터리에 상대적인 예전 처럼 해당 SQLite 합니다.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF 코어 2.0 2.0 데이터베이스 공급자를 필요로합니다.

EF 코어 2.0에 대 한 많은 간편 하 게 사용 작업과 방식으로 데이터베이스 공급자의 향상 된 기능이 되었습니다. 즉, 1.0. x 및 1.1.x 공급자 EF 코어 2.0과 함께 작동 하지 않습니다.

EF 팀에서 제공 되는 SQL Server 및 SQLite 공급자 및 2.0 버전을 사용할 수는 2.0의 일환으로 릴리스 합니다. 에 대 한 타사 오픈 소스 공급자 [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), 및 [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) 2.0에 대 한 업데이트 하는 중입니다. 다른 모든 공급자에 대 한 공급자 작성자에 게 문의 하십시오.

### <a name="logging-and-diagnostics-events-have-changed"></a>이벤트 로깅 및 진단 변경

참고: 이러한 변경 내용은 대부분의 응용 프로그램 코드가 영향을 주지 않아야 합니다.

로 보낸 메시지에 대 한 이벤트 Id는 [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0에서 변경 되었습니다. 이벤트 Id는 이제 EF 핵심 코드에서 고유 합니다. 이러한 메시지 이제도 표준 패턴을 따가 구조적된 로깅 예를 들어 MVC 사용 합니다.

로 거 범주 함께 변경 합니다. 이제는 잘 알려진 범주 집합을 통해 액세스는 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs)합니다.

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) 해당 이벤트 이제 사용 하 여 동일한 이벤트 ID 이름이 `ILogger` 메시지입니다. 이벤트 페이로드는 모든 명목 형식에서 파생 된 [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs)합니다.

이벤트 Id, 페이로드 유형 및 범주에 문서화 된는 [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) 및 [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) 클래스입니다.

Id도 Microsoft.EntityFrameworkCore.Infraestructure에서 새 Microsoft.EntityFrameworkCore.Diagnostics 네임 스페이스로 이동 했습니다.

### <a name="ef-core-relational-metadata-api-changes"></a>EF 코어 관계형 메타 데이터 API 변경

EF 코어 2.0은 이제 다른 빌드할 [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) 사용 중인 각 다른 공급자에 대 한 합니다. 이 일반적으로 응용 프로그램에 투명 합니다. 액세스 하는 모든 하위 수준의 메타 데이터 Api의 단순화 된 버전 때문에이 _일반적인 관계형 메타 데이터 개념_ 항상 호출을 통해 이루어집니다 `.Relational` 대신 `.SqlServer`, `.Sqlite`등입니다. 예를 들어 1.1.x 다음과 같은 코드:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

다음과 같이 이제 작성 합니다.

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

와 같은 메서드를 사용 하는 대신 `ForSqlServerToTable`, 확장 메서드는 이제 사용 중인 현재 공급자에 따라 조건부 코드를 작성 하는 사용할 수 있습니다. 예:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

이 변경에 대해 정의 된 Api/메타에만 적용 됩니다 _모든_ 관계형 공급자입니다. API 및 메타 데이터 동일 하 게 유지 때만 단일 공급자에만 적용 됩니다. 예를 들어 클러스터형된 인덱스는 SQL Sever 관련이 있으므로 `ForSqlServerIsClustered` 및 `.SqlServer().IsClustered()` 계속 사용 해야 합니다.

### <a name="dont-take-control-of-the-ef-service-provider"></a>EF 서비스 공급자의 제어권을 확보 하지 마십시오

EF 코어를 사용 하 여 내부 `IServiceProvider` (즉, 종속성 주입 컨테이너)의 내부 구현에 대 한 합니다. 응용 프로그램 EF 핵심을 만들고 같은 특수 한 경우를 제외 하 고이 공급자를 관리할 수 있어야 합니다. 에 대 한 호출을 제거 방법을 고려해 `UseInternalServiceProvider`합니다. 응용 프로그램에서 호출할 필요가 경우 `UseInternalServiceProvider`하십시오 [문제를 신고](https://github.com/aspnet/EntityFramework/Issues) 시나리오를 처리 하는 다른 방법을 조사할 수 있도록 합니다.

호출 `AddEntityFramework`, `AddEntityFrameworkSqlServer`, 등 응용 프로그램 코드에 필요 하지 않는 한 `UseInternalServiceProvider` 라고도 합니다. 제거에 대 한 모든 기존 호출 `AddEntityFramework` 또는 `AddEntityFrameworkSqlServer`등 `AddDbContext` 이전과 같은 방식으로 사용 계속 해야 합니다.

### <a name="in-memory-databases-must-be-named"></a>메모리 내 데이터베이스 이름을 지정 해야

모든 메모리 내 데이터베이스 이름 대신 합니다 전역 명명 되지 않은 메모리 내 데이터베이스를 제거 했습니다. 예:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

이 만듭니다/에서는 "MyDatabase" 라는 이름의 데이터베이스가 사용 합니다. 경우 `UseInMemoryDatabase` 동일한 메모리 내 데이터베이스 사용 됩니다, 상황에 맞는 여러 인스턴스에서 공유할 수 있도록 동일한 이름의 다시 호출 됩니다.

### <a name="read-only-api-changes"></a>읽기 전용 API 변경

`IsReadOnlyBeforeSave``IsReadOnlyAferSave`, 및 `IsStoreGeneratedAlways` 사용 하지 않는 및으로 대체 된 [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) 및 [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55)합니다. 이러한 동작은 모든 속성 (뿐만 아니라 저장소 생성 속성)에 적용 되 고 데이터베이스 행에 삽입 하는 경우 속성의 값을 사용할지 방법을 결정 (`BeforeSaveBehavior`) 행 만든 데이터베이스를 기존 업데이트 또는 (`AfterSaveBehavior`).

속성으로 표시 [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (예: 계산된 열)에 기본적으로 값을 무시할 현재 속성에 설정 합니다. 즉, 저장소 생성 값을 항상 모든 값 설정 또는 추적 된 엔터티의 수정 된 여부에 관계 없이 얻을 수 있습니다. 이 다른 설정 하 여 변경할 수 있습니다 `Before\AfterSaveBehavior`합니다.

### <a name="new-clientsetnull-delete-behavior"></a>새 ClientSetNull 삭제 동작

이전 릴리스에서 [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) 했습니다 엔터티에 대 한 동작 컨텍스트에서 추적 이상의 닫힌 일치 `SetNull` 의미 합니다. EF 코어 2.0에서는 새 `ClientSetNull` 선택적 관계에 대 한 기본 동작을 도입 했습니다. 이 때문에 `SetNull` 추적 된 엔터티에 대 한 의미 체계 및 `Restrict` EF 코어를 사용 하 여 만든 데이터베이스에 대 한 동작입니다. 경험에 따르면 이러한 항목은 추적 된 엔터티 및 데이터베이스에 대 한 예상/유용한 동작 합니다. `DeleteBehavior.Restrict`이제 선택적 관계에 대해 설정 된 경우 추적 된 엔터티에 대 한 인식 됩니다.

### <a name="provider-design-time-packages-removed"></a>공급자 디자인 타임 패키지 제거

`Microsoft.EntityFrameworkCore.Relational.Design` 패키지 제거 되었습니다. 관련 내용을로 통합 된 `Microsoft.EntityFrameworkCore.Relational` 및 `Microsoft.EntityFrameworkCore.Design`합니다.

이 공급자 디자인 타임 패키지에 전파합니다. 이러한 패키지 (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`등) 제거 된 멤버와 해당 내용을 주요 공급자 패키지로 통합 합니다.

사용할 수 있도록 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold` EF 코어 2.0에서 하면 단일 공급자 패키지를 참조 합니다.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```