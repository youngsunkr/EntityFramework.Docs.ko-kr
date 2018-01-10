---
title: "ASP.NET Core에서 시작 - 새 데이터베이스 - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>ASP.NET Core에서 새 데이터베이스로 EF Core 시작

이 연습에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 응용 프로그램을 빌드합니다. 마이그레이션을 사용하여 EF Core 모델에서 데이터베이스를 만듭니다. 더 많은 Entity Framework Core 자습서를 보려면 [추가 리소스](#additional-resources)를 참조하세요.

이 자습서에는 다음이 필요합니다.
* [Visual Studio 201715.3](https://www.visualstudio.com/downloads/) 및 다음 워크로드:
  * **ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)
  * **.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)을 볼 수 있습니다.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Visual Studio 2017에서 새 프로젝트 만들기

* **파일 > 새로 만들기 > 프로젝트**
* 왼쪽 메뉴에서 **설치됨 > 템플릿 > Visual C# > .NET Core**를 선택합니다.
* **새 ASP.NET Core 웹 응용 프로그램**을 선택합니다.
* 이름으로 **EFGetStarted.AspNetCore.NewDb**를 입력하고 **확인**을 클릭합니다.
* **새 ASP.NET Core 웹 응용 프로그램** 대화 상자에서:
  * 드롭다운 목록에서 **.NET Core** 및 **ASP.NET Core 2.0** 옵션이 선택되어 있는지 확인합니다.
  * **웹 응용 프로그램(모델-뷰-컨트롤러)** 프로젝트 템플릿을 선택합니다.
  * **인증**이 **인증 없음**으로 설정되었는지 확인합니다.
  * **확인**을 클릭합니다.

경고: **인증**에 **없음** 대신 **개별 사용자 계정**을 사용하는 경우 Entity Framework Core 모델은 `Models\IdentityModel.cs`의 프로젝트에 추가됩니다. 이 연습에서 학습할 기술을 사용하면 두 번째 모델을 추가하거나 기존 모델을 확장하여 엔터티 클래스를 포함하도록 선택할 수 있습니다.

## <a name="install-entity-framework-core"></a>Entity Framework Core 설치

대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

일부 Entity Framework Core Tools를 사용하여 EF Core 모델에서 데이터베이스를 만듭니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

일부 ASP.NET Core 스캐폴딩 도구를 사용하여 나중에 컨트롤러와 뷰를 만듭니다. 따라서 이 디자인 패키지도 설치합니다.

* `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` 실행

## <a name="create-the-model"></a>모델 만들기

모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.

* **Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.
* 이름으로 **Model.cs**를 입력하고 **확인**을 클릭합니다.
* 파일의 내용을 다음 코드로 바꿉니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

참고: 일반적으로 실제 앱에서는 모델의 각 클래스를 별도의 파일에 저장합니다. 간단한 설명을 위해 모든 클래스를 이 학습서에 대한 하나의 파일에 저장합니다.

## <a name="register-your-context-with-dependency-injection"></a>종속성 주입에 컨텍스트 등록

서비스(예: `BloggingContext`)는 응용 프로그램 시작 중에 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)에 등록됩니다. 이러한 서비스(예: MVC 컨트롤러)가 필요한 구성 요소에는 생성자 매개 변수 또는 속성을 통해 이러한 서비스가 제공됩니다.

MVC 컨트롤러에서 `BloggingContext`를 사용하려면 이 항목을 서비스로 등록합니다.

* **Startup.cs**를 엽니다.
* 다음 `using` 문을 추가합니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

`AddDbContext` 메서드를 추가하여 서비스로 등록합니다.

* `ConfigureServices` 메서드에 다음 코드를 추가합니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

참고: 일반적으로 실제 앱은 연결 문자열을 구성 파일에 저장합니다. 간단한 설명을 위해 여기에서는 코드로 정의합니다. 자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.

## <a name="create-your-database"></a>데이터베이스 만들기

모델을 만든 후 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 사용하여 데이터베이스를 만들 수 있습니다.

* PMC를 엽니다.

  **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**
* `Add-Migration InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다. `The term 'add-migration' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.
* `Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다. 이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.

## <a name="create-a-controller"></a>컨트롤러 만들기

프로젝트에서 스캐폴딩을 사용하도록 설정합니다.

* **솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.
* **최소 종속성**을 선택하고 **추가**를 클릭합니다.
* *ScaffoldingReadMe.txt* 파일을 무시하거나 삭제할 수 있습니다.

이제 스캐폴딩이 사용 가능하므로 `Blog` 엔터티에 대한 컨트롤러를 스캐폴딩할 수 있습니다.

* **솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.
* **보기 포함 MVC 컨트롤러, Entity Framework**를 선택하고 **확인**을 클릭합니다.
* **모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.
* **추가**를 클릭합니다.


## <a name="run-the-application"></a>응용 프로그램 실행

F5 키를 눌러 앱을 실행하고 테스트합니다.

* `/Blogs`로 이동합니다.
* 만들기 링크를 사용하여 일부 블로그 항목을 만듭니다. 세부 정보를 테스트하고 링크를 삭제합니다.

![image](_static/create.png)

![image](_static/index-new-db.png)

## <a name="additional-resources"></a>추가 리소스

* [EF - SQLite를 사용하여 새 데이터베이스 만들기](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.
* [Mac 또는 Linux의 ASP.NET Core MVC 소개](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Visual Studio의 ASP.NET Core MVC 소개](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio를 사용하여 ASP.NET Core 및 Entity Framework Core 시작](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)