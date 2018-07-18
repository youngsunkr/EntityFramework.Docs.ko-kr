---
title: WPF-EF6 사용 하 여 데이터 바인딩
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122306"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="e7197-102">WPF 사용 하 여 데이터 바인딩</span><span class="sxs-lookup"><span data-stu-id="e7197-102">Databinding with WPF</span></span>
<span data-ttu-id="e7197-103">이 단계별 연습에는 POCO 형식 "마스터-세부 정보" 폼에 WPF 컨트롤에 바인딩하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="e7197-104">Entity Framework Api를 사용 하 여 데이터베이스에서 데이터를 사용 하 여 개체를 채우기, 변경 내용 추적 및 데이터베이스에 데이터를 유지 하는 응용 프로그램.</span><span class="sxs-lookup"><span data-stu-id="e7197-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="e7197-105">모델에 일 대 다 관계에 참여 하는 두 가지 형식을 정의 합니다. **범주** (주\\마스터) 및 **제품** (종속\\세부 정보).</span><span class="sxs-lookup"><span data-stu-id="e7197-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="e7197-106">그런 다음 Visual Studio 도구는 WPF 컨트롤 모델에서 정의 된 형식에 바인딩할 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="e7197-107">WPF 데이터 바인딩 프레임 워크에 관련 된 개체 사이 탐색할 수 있습니다: 세부 정보 뷰에서 해당 자식 데이터를 사용 하 여 업데이트를 사용 하면 마스터 뷰에 행을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="e7197-108">스크린샷 및이 연습의 코드 샘플은 Visual Studio 2013에서 가져옵니다 하지만 Visual Studio 2012 또는 Visual Studio 2010을 사용 하 여이 연습을 완료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="e7197-109">WPF 데이터 원본 만들기에 대 한 'Object' 옵션을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="e7197-110">이전 버전의 Entity Framework를 사용 하 여 우리가 하는 데 사용 하는 것이 좋습니다 합니다 **데이터베이스** EF 디자이너를 사용 하 여 만든 모델을 기반으로 하는 새 데이터 원본을 만드는 경우 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="e7197-111">이 디자이너는 ObjectContext에서 파생 된 컨텍스트 및 EntityObject에서 파생 된 엔터티 클래스를 생성 하기 때문 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="e7197-112">데이터베이스 옵션을 사용 하 여이 API 화면의 상호 작용 하기 위한 가장 좋은 코드를 작성 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="e7197-113">Visual Studio 2012 및 Visual Studio 2013 용 EF 디자이너에는 간단한 POCO 엔터티 클래스와 함께 DbContext에서 파생 되는 컨텍스트를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="e7197-114">Visual Studio 2010을 사용 하 여이 연습의 뒷부분에서 설명 된 대로 DbContext를 사용 하는 코드 생성 템플릿을으로 전환 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="e7197-115">DbContext API 화면을 사용 하는 경우 사용 해야 합니다 **개체** 이 연습에 표시 된 대로 새 데이터 원본을 만들 때 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="e7197-116">필요한 경우 있습니다 [ObjectContext 기반 코드 생성으로 되돌리려면](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) EF 디자이너를 사용 하 여 만든 모델에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e7197-117">필수 조건</span><span class="sxs-lookup"><span data-stu-id="e7197-117">Pre-Requisites</span></span>

<span data-ttu-id="e7197-118">이 연습을 완료 하려면 Visual Studio 2012 또는 Visual Studio 2010 설치, Visual Studio 2013이 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="e7197-119">Visual Studio 2010을 사용 하는 경우 NuGet을 설치 하려면 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="e7197-120">자세한 내용은 [NuGet 설치](http://docs.nuget.org/docs/start-here/installing-nuget)합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="e7197-121">응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="e7197-121">Create the Application</span></span>

-   <span data-ttu-id="e7197-122">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-122">Open Visual Studio</span></span>
-   <span data-ttu-id="e7197-123">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="e7197-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="e7197-124">선택 **Windows** 왼쪽된 창에서와 **WPFApplication** 오른쪽 창에서</span><span class="sxs-lookup"><span data-stu-id="e7197-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="e7197-125">입력 **WPFwithEFSample** 이름으로</span><span class="sxs-lookup"><span data-stu-id="e7197-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="e7197-126">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="e7197-127">Entity Framework NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="e7197-128">솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **WinFormswithEFSample** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="e7197-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="e7197-129">선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="e7197-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="e7197-130">NuGet 패키지 관리 대화 상자에서 선택 합니다 **Online** 탭을 선택 합니다 **EntityFramework** 패키지</span><span class="sxs-lookup"><span data-stu-id="e7197-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="e7197-131">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="e7197-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="e7197-132">EntityFramework 어셈블리 외에도 System.ComponentModel.DataAnnotations에 대 한 참조도 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="e7197-133">프로젝트 System.Data.Entity에 대 한 참조가 있으면 다음 제거 됩니다 EntityFramework 패키지를 설치한 경우.</span><span class="sxs-lookup"><span data-stu-id="e7197-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="e7197-134">System.Data.Entity 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="e7197-135">모델 정의</span><span class="sxs-lookup"><span data-stu-id="e7197-135">Define a Model</span></span>

<span data-ttu-id="e7197-136">이 연습에서는 있습니다 Code First 또는 EF 디자이너를 사용 하 여 모델을 구현 하도록 선택 했습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="e7197-137">다음 두 섹션 중 하나를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="e7197-138">옵션 1: Code First를 사용 하 여 모델 정의</span><span class="sxs-lookup"><span data-stu-id="e7197-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="e7197-139">이 섹션에는 모델과 Code First를 사용 하 여 연결된 된 데이터베이스를 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="e7197-140">다음 섹션을 건너뜁니다 (**옵션 2: Database First를 사용 하 여 모델 정의)** 사용 하려는 Database First 순서를 반대로 바꿀 경우 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 설계 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="e7197-141">Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="e7197-142">새 클래스를 추가 합니다 **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="e7197-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="e7197-143">프로젝트 이름을 마우스 오른쪽 단추로 클릭</span><span class="sxs-lookup"><span data-stu-id="e7197-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="e7197-144">선택 **추가할**, 다음 **새 항목**</span><span class="sxs-lookup"><span data-stu-id="e7197-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="e7197-145">선택 **클래스** enter **제품** 클래스 이름</span><span class="sxs-lookup"><span data-stu-id="e7197-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="e7197-146">대체는 **제품** 클래스 정의 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="e7197-147">**제품** 속성에는 **범주** 클래스 및 **범주** 속성에는 **제품** 클래스는 탐색 속성.</span><span class="sxs-lookup"><span data-stu-id="e7197-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="e7197-148">Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="e7197-149">엔터티를 정의 하는 것 외에도 DbContext에서 파생 되며 DbSet을 노출 하는 클래스를 정의 해야&lt;TEntity&gt; 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="e7197-150">DbSet&lt;TEntity&gt; 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="e7197-151">런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="e7197-152">새 **ProductContext** 다음 정의 사용 하 여 프로젝트에 클래스:</span><span class="sxs-lookup"><span data-stu-id="e7197-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="e7197-153">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="e7197-154">Database First를 사용 하 여 모델을 정의 하는 옵션 2:</span><span class="sxs-lookup"><span data-stu-id="e7197-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="e7197-155">이 섹션에서는 Database First 리버스 엔지니어링을 사용 하는 방법을 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="e7197-156">이전 섹션을 완료 하는 경우 (**옵션 1: Code First를 사용 하 여 모델 정의)**, 그런 다음이 섹션을 건너뛰고 바로 이동 합니다 **지연 로드** 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="e7197-157">기존 데이터베이스를 만들려면</span><span class="sxs-lookup"><span data-stu-id="e7197-157">Create an Existing Database</span></span>

<span data-ttu-id="e7197-158">일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="e7197-159">Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="e7197-160">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="e7197-161">Visual Studio 2012를 사용 하는 경우를 만들 수는 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="e7197-162">데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="e7197-163">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="e7197-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="e7197-164">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="e7197-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="e7197-165">Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우</span><span class="sxs-lookup"><span data-stu-id="e7197-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="e7197-167">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결 하 고 입력 **제품** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="e7197-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="e7197-170">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="e7197-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="e7197-172">새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="e7197-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="e7197-173">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="e7197-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="e7197-174">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="e7197-174">Reverse Engineer Model</span></span>

<span data-ttu-id="e7197-175">모델을 만들려면 Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="e7197-176">**프로젝트-&gt; 새 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="e7197-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="e7197-177">선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**</span><span class="sxs-lookup"><span data-stu-id="e7197-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="e7197-178">입력 **ProductModel** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="e7197-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="e7197-179">그러면는 **엔터티 데이터 모델 마법사**</span><span class="sxs-lookup"><span data-stu-id="e7197-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="e7197-180">선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="e7197-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="e7197-182">첫 번째 섹션에서 만든 데이터베이스에 연결을 선택, 입력 **ProductContext** 연결 문자열 및 클릭의 이름으로 **다음**</span><span class="sxs-lookup"><span data-stu-id="e7197-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="e7197-184">모든 테이블을 가져오고 '마침' 클릭 '테이블' 옆의 확인란을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="e7197-186">리버스 엔지니어링 프로세스가 완료 되 면 새 모델 프로젝트에 추가 되 고 Entity Framework 디자이너에서 확인할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="e7197-187">또한 App.config 파일을 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="e7197-188">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="e7197-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="e7197-189">Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하 여 EF 디자이너를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="e7197-190">EF 디자이너에서 모델의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="e7197-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="e7197-191">선택 **온라인 템플릿을** 검색에 대 한 확인 하 고 왼쪽된 메뉴에서 **DbContext**</span><span class="sxs-lookup"><span data-stu-id="e7197-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="e7197-192">선택 합니다 **EF 6.x C에 대 한 DbContext 생성기\#를** 입력 **ProductsModel** 이름으로 추가 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="e7197-193">데이터 바인딩에 대 한 코드 생성 업데이트</span><span class="sxs-lookup"><span data-stu-id="e7197-193">Updating code generation for data binding</span></span>

<span data-ttu-id="e7197-194">EF T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="e7197-195">Visual Studio를 함께 사용 하거나 Visual Studio 갤러리에서 다운로드 한 템플릿에 범용 사용이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="e7197-196">즉, 이러한 템플릿에서 생성 한 엔터티 간단한 ICollection&lt;T&gt; 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="e7197-197">그러나 데이터를 수행 하는 경우 WPF를 사용 하 여 바인딩 것을 사용 하는 것이 바람직 **ObservableCollection** WPF를 추적할 수는 컬렉션에 대 한 변경 내용을 컬렉션 속성에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="e7197-198">이 위해 ObservableCollection 사용 하도록 템플릿을 수정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="e7197-199">엽니다는 **솔루션 탐색기** 찾고 **ProductModel.edmx** 파일</span><span class="sxs-lookup"><span data-stu-id="e7197-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="e7197-200">찾을 합니다 **ProductModel.tt** ProductModel.edmx 파일 아래에 중첩 될는 파일</span><span class="sxs-lookup"><span data-stu-id="e7197-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="e7197-202">Visual Studio 편집기에서 열려는 ProductModel.tt 파일을 두 번 클릭</span><span class="sxs-lookup"><span data-stu-id="e7197-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="e7197-203">찾기 및 바꾸기 2 개 "**ICollection**"with"**ObservableCollection**"입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="e7197-204">이러한 줄 296 및 484에서 대략적으로 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="e7197-205">찾기 및 바꾸기 첫 번째로 나타나는 "**HashSet**"with"**ObservableCollection**"입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="e7197-206">이 항목은 약 50 줄에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="e7197-207">**그렇지 않은** HashSet 코드 나중에 두 번째 항목으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="e7197-208">찾기 및 바꾸기만 발생 "**System.Collections.Generic**"with"**System.Collections.ObjectModel**"입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="e7197-209">이 대략 424 줄에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="e7197-210">ProductModel.tt 파일을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="e7197-211">이 인해 다시 생성 하는 엔터티에 대 한 코드를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="e7197-212">코드를 자동으로 다시 생성 하지 않습니다, 경우 ProductModel.tt 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="e7197-213">이제 (함은 ProductModel.tt 아래에 중첩) Category.cs 파일을 열 경우 제품 컬렉션 형식에 표시 되어야 **ObservableCollection&lt;제품&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="e7197-214">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="e7197-215">지연 로드</span><span class="sxs-lookup"><span data-stu-id="e7197-215">Lazy Loading</span></span>

<span data-ttu-id="e7197-216">**제품** 속성에는 **범주** 클래스 및 **범주** 속성에는 **제품** 클래스는 탐색 속성.</span><span class="sxs-lookup"><span data-stu-id="e7197-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="e7197-217">Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="e7197-218">EF는 관련된 엔터티 로드 데이터베이스에서 자동으로 처음 탐색 속성에 액세스 하는 옵션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="e7197-219">이 유형의 로드 (지연 로딩 이라고 함)는 각 탐색 속성에 액세스 하는 처음으로 별도 쿼리가 실행 됩니다 데이터베이스에 대 한 내용이 없는 경우 컨텍스트에서 알아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="e7197-220">POCO 엔터티 형식을 사용 하는 경우 EF는 런타임에 파생된 프록시 형식의 인스턴스를 만들고 다음 로드 후크를 추가 하려면 클래스에서 가상 속성을 재정의 하 여 지연 로딩을 달성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="e7197-221">관련된 개체의 지연 로드를 가져오려면 선언 해야 탐색으로 속성 getter **공용** 하 고 **가상** (**Overridable** Visual basic에서), 있습니다 클래스가 아니어야 하 고 **봉인** (**NotOverridable** Visual basic에서).</span><span class="sxs-lookup"><span data-stu-id="e7197-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="e7197-222">데이터베이스를 사용 하는 경우 첫 번째 탐색 속성이 자동으로 지연 로드를 사용 하도록 설정 하려면 가상 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="e7197-223">같은 이유로 가상 탐색 속성을 확인 하기로 코드 첫 번째 섹션에서</span><span class="sxs-lookup"><span data-stu-id="e7197-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="e7197-224">컨트롤에 개체 바인딩</span><span class="sxs-lookup"><span data-stu-id="e7197-224">Bind Object to Controls</span></span>

<span data-ttu-id="e7197-225">이 WPF 응용 프로그램에 대 한 데이터 원본으로 모델에 정의 된 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="e7197-226">두 번 클릭 **MainWindow.xaml** 기본 폼을 열려면 솔루션 탐색기에서</span><span class="sxs-lookup"><span data-stu-id="e7197-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="e7197-227">주 메뉴에서 선택 **프로젝트-&gt; 새 데이터 소스 추가...**</span><span class="sxs-lookup"><span data-stu-id="e7197-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="e7197-228">(Visual Studio 2010에서 선택 해야 **데이터-&gt; 새 데이터 소스 추가...** )</span><span class="sxs-lookup"><span data-stu-id="e7197-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="e7197-229">데이터 원본 Typewindow 선택에서 선택 **개체** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="e7197-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="e7197-230">데이터 개체 대화 상자 선택에서 펼침 합니다 **WPFwithEFSample** 두 시간과 선택 **범주**</span><span class="sxs-lookup"><span data-stu-id="e7197-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="e7197-231">*선택 하지 않아도 됩니다는 **제품** 데이터 원본을 통해 얻게 되므로 **제품**의 속성에는 **범주** 데이터 원본*</span><span class="sxs-lookup"><span data-stu-id="e7197-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="e7197-233">클릭 **완료 합니다.**</span><span class="sxs-lookup"><span data-stu-id="e7197-233">Click **Finish.**</span></span>
-   <span data-ttu-id="e7197-234">데이터 소스 창 MainWindow.xaml 창 옆에 있는 열릴 *데이터 소스 창에서 표시 되지 않으면, 선택 **뷰-&gt; 다른 Windows-&gt; 데이터 원본***</span><span class="sxs-lookup"><span data-stu-id="e7197-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="e7197-235">고정 아이콘을 하므로 데이터 소스 창에서는 자동 숨기기 단추를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="e7197-236">창의 창이 이미 열려 있는 경우 새로 고침 단추를 적중 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSources](~/ef6/media/datasources.png)

-   <span data-ttu-id="e7197-238">선택 된 * * 범주 * * 데이터 원본 및 폼에 놓습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-238">Select the **Category **data source and drag it on the form.</span></span>

<span data-ttu-id="e7197-239">다음에서는이 소스를 끌 때 발생 했습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="e7197-240">합니다 **categoryViewSource** 리소스 및 * * categoryDataGrid * * 컨트롤이 XAML에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-240">The **categoryViewSource** resource and the** categoryDataGrid** control were added to XAML.</span></span> <span data-ttu-id="e7197-241">DataViewSources에 대 한 자세한 내용은 참조 하세요. http://bea.stollnitz.com/blog/?p=387합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="e7197-242">부모 모눈 요소의 DataContext 속성 설정 된 "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="e7197-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="e7197-243">합니다 **categoryViewSource** 외부를 바인딩 소스로 사용 되는 리소스\\부모 Grid 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="e7197-244">내부 Grid 요소는 다음 부모 표 (categoryDataGrid의 ItemsSource 속성 "{Binding}"로 설정 됨)에서 DataContext 값을 상속 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="e7197-245">세부 정보 표를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-245">Adding a Details Grid</span></span>

<span data-ttu-id="e7197-246">이제 표 보겠습니다 범주를 표시 하려면 연결 된 제품을 표시 하려면 세부 정보 표를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="e7197-247">선택 합니다 * * 제품 * * 아래에서 속성을 * * 범주 * * 데이터 원본 및 폼에 놓습니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-247">Select the **Products **property from under the **Category **data source and drag it on the form.</span></span>
    -   <span data-ttu-id="e7197-248">합니다 **categoryProductsViewSource** 리소스와 **productDataGrid** 표 XAML에 추가 됩니다</span><span class="sxs-lookup"><span data-stu-id="e7197-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="e7197-249">이 리소스에 대 한 바인딩 경로 Products로 설정</span><span class="sxs-lookup"><span data-stu-id="e7197-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="e7197-250">WPF 데이터 바인딩 프레임 워크는 선택한 범주와 관련 된 제품만 표시 되도록 **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="e7197-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="e7197-251">도구 상자에서 끌어 **단추** 폼에 로그온 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="e7197-252">설정 된 **이름** 속성을 **buttonSave** 하며 **콘텐츠** 속성을 **저장**합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="e7197-253">폼은 다음과 유사 하 게 같아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-253">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="e7197-255">데이터 상호 작용을 처리 하는 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="e7197-256">주 창에 일부 이벤트 처리기를 추가할 차례입니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="e7197-257">XAML 창에서 클릭 합니다  **&lt;창** 요소인 주 창 선택</span><span class="sxs-lookup"><span data-stu-id="e7197-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="e7197-258">에 **속성** 창 **이벤트** 오른쪽 위에 있는 다음 두 번 클릭의 오른쪽에 있는 텍스트 상자를 **Loaded** 레이블</span><span class="sxs-lookup"><span data-stu-id="e7197-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="e7197-260">추가할 수도 **클릭** 에 대 한 이벤트를 **저장** 단추는 디자이너의 저장 단추를 두 번 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="e7197-261">그러면 코드 숨김 양식,에서는 이제는 ProductContext를 사용 하 여 데이터 액세스를 수행 하는 코드를 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="e7197-262">아래와 같이 MainWindow에 대 한 코드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="e7197-263">코드의 장기 실행 인스턴스를 선언 **ProductContext**합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="e7197-264">합니다 **ProductContext** 개체는 쿼리 및 데이터베이스에 데이터를 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="e7197-265">합니다 **Dispose**()는 **ProductContext** 인스턴스 이라고 재정의 된 **OnClosing** 메서드.</span><span class="sxs-lookup"><span data-stu-id="e7197-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="e7197-266">코드 주석을 코드 수행 작업에 대 한 세부 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-266">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="e7197-267">WPF 응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="e7197-267">Test the WPF Application</span></span>

-   <span data-ttu-id="e7197-268">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-268">Compile and run the application.</span></span> <span data-ttu-id="e7197-269">Code First에서 사용 되는 경우 확인할 수 있습니다는 **WPFwithEFSample.ProductContext** 데이터베이스 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="e7197-270">아래 표의 위쪽 표 및 제품 이름에 범주 이름 입력 *입력 하지 않으면 아무 것도 ID 열에 데이터베이스에서 기본 키를 생성 하기 때문에*</span><span class="sxs-lookup"><span data-stu-id="e7197-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Screen1](~/ef6/media/screen1.png)

-   <span data-ttu-id="e7197-272">키를 눌러 합니다 **저장할** 데이터베이스로 데이터를 저장 하려면 단추</span><span class="sxs-lookup"><span data-stu-id="e7197-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="e7197-273">DbContext의를 호출한 후 **SaveChanges**Id ()는 데이터베이스에서 생성 된 값으로 채워집니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="e7197-274">호출 했으므로 **새로 고침**한 후 () **SaveChanges**()를 **DataGrid** 컨트롤도 새 값으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e7197-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Screen2](~/ef6/media/screen2.png)