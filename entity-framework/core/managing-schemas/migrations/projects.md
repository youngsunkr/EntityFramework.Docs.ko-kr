---
title: "여러 프로젝트-EF 코어 마이그레이션"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a>별도 프로젝트를 사용 하 여
========================
마이그레이션을 포함 하는 하나의 보다 다른 어셈블리에 저장 하려는 경우 사용자 `DbContext`합니다. 또한 릴리스 간 업그레이드에 대 한 예를 들어 여러 집합이 마이그레이션 유지 하기 위해이 전략, 개발에 대 한 이며 다른 하나를 사용할 수 있습니다.

수행 작업

1. 새 클래스 라이브러리를 만듭니다.

2. DbContext 어셈블리에 대 한 참조를 추가 합니다.

3. 클래스 라이브러리에는 마이그레이션 및 모델 스냅숏 파일을 이동 합니다.
   * 모든 추가 않은 DbContext 프로젝트에 하나를 추가한 다음 이동 합니다.

4. 마이그레이션 어셈블리를 구성 합니다.

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. 시작 어셈블리에서 마이그레이션 어셈블리에 대 한 참조를 추가 합니다.
   * 이렇게 하면 순환 종속성을 하는 경우 클래스 라이브러리의 출력 경로 업데이트 합니다.

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

올바르게 모든 작업을 수행한 경우에 새 마이그레이션 프로젝트에 추가할 수 있습니다.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```