---
title: 공간-Code First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122597"
---
# <a name="spatial---code-first"></a><span data-ttu-id="58d12-102">공간-Code First</span><span class="sxs-lookup"><span data-stu-id="58d12-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="58d12-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="58d12-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="58d12-105">비디오 및 단계별 연습에는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="58d12-106">또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="58d12-107">새 데이터베이스를 만들려면이 연습에서는 Code First 사용 하지만 사용할 수도 있습니다 [기존 데이터베이스에 Code First](~/ef6/modeling/code-first/workflows/existing-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="58d12-108">공간 형식 지원 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="58d12-109">공간 형식, 열거형 및 테이블 반환 함수 같은 새 기능을 사용 하려면 해야 대상 지정 하는.NET Framework 4.5를 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="58d12-110">Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="58d12-111">공간 데이터 형식을 사용 하 여 공간 지원에는 Entity Framework 공급자를 사용 해야 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="58d12-112">참조 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="58d12-113">기본 공간 데이터 유형은 두 가지가: geography 및 geometry 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="58d12-114">타원 데이터를 저장 하는 지리 데이터 형식 (예를 들어, GPS 위도 및 경도 좌표)입니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="58d12-115">기 하 도형 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="58d12-116">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="58d12-116">Watch the video</span></span>
<span data-ttu-id="58d12-117">이 비디오에는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="58d12-118">또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="58d12-119">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="58d12-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="58d12-120">**비디오**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="58d12-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="58d12-121">필수 조건</span><span class="sxs-lookup"><span data-stu-id="58d12-121">Pre-Requisites</span></span>

<span data-ttu-id="58d12-122">Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="58d12-123">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="58d12-123">Set up the Project</span></span>

1.  <span data-ttu-id="58d12-124">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="58d12-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="58d12-125">에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**</span><span class="sxs-lookup"><span data-stu-id="58d12-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="58d12-126">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿</span><span class="sxs-lookup"><span data-stu-id="58d12-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="58d12-127">입력 **SpatialCodeFirst** 고 프로젝트의 이름으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="58d12-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="58d12-128">Code First를 사용 하 여 새 모델을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-128">Define a New Model using Code First</span></span>

<span data-ttu-id="58d12-129">Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="58d12-130">아래 코드는 University 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-130">The code below defines the University class.</span></span>

<span data-ttu-id="58d12-131">대학교는 DbGeography 형식의 위치 속성이 들어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="58d12-132">DbGeography 형식을 사용 하려면 System.Data.Entity 어셈블리에 대 한 참조를 추가 하 고도 문을 사용 하 여 System.Data.Spatial를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="58d12-133">Program.cs 파일을 열고 다음 using 문을 파일의 맨 위에 있는:</span><span class="sxs-lookup"><span data-stu-id="58d12-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="58d12-134">Program.cs 파일에 다음 University 클래스 정의 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="58d12-135">정의 DbContext 파생 형식</span><span class="sxs-lookup"><span data-stu-id="58d12-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="58d12-136">엔터티를 정의 하는 것 외에도 DbContext에서 파생 되며 DbSet을 노출 하는 클래스를 정의 해야&lt;TEntity&gt; 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="58d12-137">DbSet&lt;TEntity&gt; 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="58d12-138">런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="58d12-139">DbContext 형식과 DbSet EntityFramework 어셈블리에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="58d12-140">EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="58d12-141">솔루션 탐색기에서 프로젝트 이름을 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="58d12-142">선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="58d12-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="58d12-143">NuGet 패키지 관리 대화 상자에서 선택 합니다 **온라인** 탭을 선택 합니다 **EntityFramework** 패키지.</span><span class="sxs-lookup"><span data-stu-id="58d12-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="58d12-144">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="58d12-144">Click **Install**</span></span>

<span data-ttu-id="58d12-145">Note는 EntityFramework 어셈블리 외에도 System.ComponentModel.DataAnnotations 어셈블리에 대 한 참조도 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="58d12-146">추가 다음 Program.cs 파일의 맨 위에 있는 문을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="58d12-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="58d12-147">Program.cs의 상황에 맞는 정의 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="58d12-148">유지 하 고 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="58d12-149">Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="58d12-150">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="58d12-151">두 가지 새로운 University 개체를 컨텍스트에 추가 하는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="58d12-152">공간 속성 DbGeography.FromText 메서드를 사용 하 여 초기화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="58d12-153">지리 지점 나타낸 WellKnownText 메서드에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="58d12-154">다음 코드는 데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-154">The code then saves the data.</span></span> <span data-ttu-id="58d12-155">그런 다음는 LINQ 쿼리 해당 위치가 지정된 된 위치에 가장 가까운 University 개체를 반환 하는 생성 되 고 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="58d12-156">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-156">Compile and run the application.</span></span> <span data-ttu-id="58d12-157">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="58d12-158">생성된 된 데이터베이스 뷰</span><span class="sxs-lookup"><span data-stu-id="58d12-158">View the Generated Database</span></span>

<span data-ttu-id="58d12-159">처음으로 응용 프로그램을 실행 하면 Entity Framework를 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="58d12-160">Visual Studio 2012를 설치 했으므로 LocalDB 인스턴스에서 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="58d12-161">기본적으로 Entity Framework에 이름을 파생 컨텍스트의 정규화 된 이름 뒤에 오는 하는 데이터베이스 (이 예제는 **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="58d12-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="58d12-162">사용할 기존 데이터베이스를 이후 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="58d12-163">참고로 하는 경우 변경 내용을 모델 데이터베이스가 만들어진 후에 사용 하는 Code First 마이그레이션을 데이터베이스 스키마를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="58d12-164">참조 [Code First 새 데이터베이스로](~/ef6/modeling/code-first/workflows/new-database.md) 마이그레이션을 사용 하 여의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="58d12-165">데이터베이스 및 데이터를 보려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="58d12-166">Visual Studio 2012 주 메뉴에서 선택 **뷰**  - &gt; **SQL Server 개체 탐색기**합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="58d12-167">LocalDB 서버 목록에 없는 경우에서 마우스 오른쪽 단추를 클릭 **SQL Server** 선택한 **SQL Server 추가** 기본값을 사용 하 여 **Windows 인증** 에 연결 하는 LocalDB 인스턴스</span><span class="sxs-lookup"><span data-stu-id="58d12-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="58d12-168">LocalDB 노드를 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="58d12-169">펼치려면 합니다 **데이터베이스** 폴더를 찾아 새 데이터베이스가 표시를 합니다 **대학** 테이블</span><span class="sxs-lookup"><span data-stu-id="58d12-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="58d12-170">데이터를 보고, 테이블을 마우스 오른쪽 단추로 클릭 합니다. 선택 **데이터 보기**</span><span class="sxs-lookup"><span data-stu-id="58d12-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="58d12-171">요약</span><span class="sxs-lookup"><span data-stu-id="58d12-171">Summary</span></span>

<span data-ttu-id="58d12-172">이 연습에서는 Entity Framework Code First를 사용 하 여 공간 형식을 사용 하는 방법에 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="58d12-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 