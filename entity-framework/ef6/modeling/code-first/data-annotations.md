---
title: First 데이터 주석-EF6 코드
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122803"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="24448-102">Code First 데이터 주석</span><span class="sxs-lookup"><span data-stu-id="24448-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="24448-103">**EF4.1 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 4.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="24448-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="24448-105">이 페이지에 있는 콘텐츠는 및 Julie lerman 작성 원래 작성 된 문서 (\<http://thedatafarm.com>)합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="24448-106">Entity Framework Code First EF를 쿼리 하는 데 사용 하는 모델을 나타내는 사용자 고유의 도메인 클래스를 사용 하면 추적 및 업데이트 함수 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="24448-107">코드는 먼저 구성 보다 규칙으로 참조 하는 프로그래밍 패턴을 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="24448-108">즉, 해당 코드는 먼저 가정 클래스 EF를 사용 하는 규칙에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="24448-109">이런 경우 EF가 작업을 수행 해야 하는 세부 정보를 파악 하는 일을 할 수 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="24448-110">그러나 클래스에서 이러한 규칙을 따르지 않으면 경우 구성이 필요한 정보를 사용 하 여 EF 수 있도록 클래스에 추가할 수가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="24448-111">먼저 코드는 클래스에 다음이 구성을 추가 하려면 두 가지 방법으로 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="24448-112">DataAnnotations를 호출 하는 단순한 특성 하나 사용 하 고 다른 하나는 코드에서 명령적으로 구성을 설명 하는 방법을 제공 하는 Fluent API는 먼저 코드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="24448-113">이 문서에서는 중점적으로 DataAnnotations (System.ComponentModel.DataAnnotations 네임 스페이스)에서 사용 하 여 구성 클래스-가장 일반적으로 필요한 구성이 강조 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="24448-114">DataAnnotations 다양 한 클라이언트 쪽 유효성 검사에 대 한 동일한 주석을 활용 하 여 이러한 응용 프로그램을 허용 하는 ASP.NET MVC와 같은.NET 응용 프로그램에서 인식 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="24448-115">모델</span><span class="sxs-lookup"><span data-stu-id="24448-115">The model</span></span>

<span data-ttu-id="24448-116">설명 하겠습니다 코드 클래스의 간단한 쌍을 사용 하 여 첫 번째 DataAnnotations: 블로그 및 게시물.</span><span class="sxs-lookup"><span data-stu-id="24448-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="24448-117">있는 그대로 블로그 및 게시물 클래스를 편리 하 게 코드 첫 번째 규칙 따르고 없습니다 조정 EF를 사용 하는 데 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="24448-118">하지만 EF에 클래스 및 동의어가 매핑되는 데이터베이스에 대 한 자세한 정보를 제공 하는 주석을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="24448-119">Key</span><span class="sxs-lookup"><span data-stu-id="24448-119">Key</span></span>

<span data-ttu-id="24448-120">Entity Framework는 엔터티를 추적에 사용 되는 키 값을 가진 모든 엔터티를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="24448-121">코드는 처음에 의존 하는 규칙 중 하나는 각 코드 첫 번째 클래스의 키 속성이 의미 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="24448-122">명명 된 클래스 이름과 같은 "BlogId" "Id"를 결합 하는 하나 또는 "Id" 속성에 대해 확인 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="24448-123">속성은 데이터베이스의 기본 키 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="24448-124">블로그 및 게시물 클래스는 모두이 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="24448-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="24448-125">그러나 않은 될까요?</span><span class="sxs-lookup"><span data-stu-id="24448-125">But what if they didn’t?</span></span> <span data-ttu-id="24448-126">블로그 name을 사용 하는 경우에 어떻게 *PrimaryTrackingKey* 심지어 대신 *foo*?</span><span class="sxs-lookup"><span data-stu-id="24448-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="24448-127">코드 먼저 찾을 수 없는 경우이 규칙과 일치 하는 속성을 키 속성이 있어야 하는 Entity Framework의 요구 사항으로 인해 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="24448-128">EntityKey로 사용할 속성을 지정 하려면 키 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="24448-129">접하는 경우 먼저 코드를 사용 하는 것은 데이터베이스 생성 기능, 블로그 테이블 PrimaryTrackingKey 기본적으로 Id로도 정의 되어 있는 기본 키 열을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="24448-131">복합 키</span><span class="sxs-lookup"><span data-stu-id="24448-131">Composite keys</span></span>

<span data-ttu-id="24448-132">Entity Framework는 복합 키-둘 이상의 속성으로 구성 된 기본 키를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="24448-133">예를 들어에 기본 키가 PassportNumber 및 IssuingCountry 조합을 Passport 클래스가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="24448-134">위 클래스 InvalidOperationExceptions 내용의; 얻게 EF 모델에 사용 하려는 경우</span><span class="sxs-lookup"><span data-stu-id="24448-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="24448-135">*복합 기본 키 순서 지정 'Passport' 형식에 대 한 알 수 없습니다. 복합 기본 키에 대 한 순서를 지정 하는 ColumnAttribute 또는 HasKey 메서드를 사용 합니다.*</span><span class="sxs-lookup"><span data-stu-id="24448-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="24448-136">복합 키에 있는 경우 Entity Framework를 사용 하면 키 속성 순서를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="24448-137">순서를 지정 하는 열 주석을 사용 하 여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="24448-138">순서 값은 상대 (대신 인덱스 기반) 값을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="24448-139">예를 들어 100과 200 허용 되 1과 2를 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="24448-140">복합 외래 키를 가진 엔터티가 있는 경우 해당 기본 키 속성에 사용 하는 순서는 동일한 열을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="24448-141">동일 정확한 값에 할당 해야만 상대적 순서 외래 키 속성 내의 **순서** 일치 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="24448-142">예를 들어, 다음 클래스에 3-4 사용할 수 있습니다 1 및 2 대신.</span><span class="sxs-lookup"><span data-stu-id="24448-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="24448-143">필수</span><span class="sxs-lookup"><span data-stu-id="24448-143">Required</span></span>

<span data-ttu-id="24448-144">필요한 주석 특정 속성이 필수임을 EF를 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="24448-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="24448-145">Title 속성에 필요한 추가 하면 EF 및 MVC가 속성에 데이터가 있는지 확인 합니다. 강제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="24448-146">없는 추가 하지 않은 코드 또는 태그 변경 내용을 응용 프로그램에서 MVC 응용 프로그램이 실행 클라이언트 쪽 유효성 검사, 속성 및 주석 이름을 사용 하 여 메시지를 동적으로 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="24448-148">Required 특성 매핑된 속성을 nullable이 아닌 만들어 생성된 된 데이터베이스를 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="24448-149">"Null 아님" 제목 필드 변경 되었는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="24448-150">경우에 따라 속성이 필요한 경우에 null이 아닌 수를 데이터베이스에 열 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="24448-151">예를 들어 경우 TPH 상속 전략 데이터를 사용 하 여 여러 형식에 대 한에 저장 됩니다 단일 테이블.</span><span class="sxs-lookup"><span data-stu-id="24448-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="24448-152">파생된 형식이 필요한 속성을 포함 하는 경우 열 있으므로 할 수 없습니다 nullable이 아닌 계층의 모든 형식이 아닌이 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="24448-154">MinLength 및 MaxLength</span><span class="sxs-lookup"><span data-stu-id="24448-154">MaxLength and MinLength</span></span>

<span data-ttu-id="24448-155">MinLength 및 MaxLength 특성을 방금 수행한 것 처럼 사용 하 여 필요한 추가 속성 유효성 검사를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="24448-156">길이 요구 사항을 BloggerName 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="24448-157">이 예제에는 특성을 결합 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="24448-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="24448-158">MaxLength 주석 10 속성의 길이 설정 하 여 데이터베이스에 영향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="24448-160">클라이언트 쪽 주석 MVC 및 EF 4.1 서버 쪽 주석 둘 다 적용 됩니다는 오류 메시지를 동적으로 다시 작성이 유효성 검사: "필드 BloggerName '10'의 최대 길이 사용 하 여 문자열 또는 배열 형식 이어야 합니다." 해당 메시지가 약간 깁니다.</span><span class="sxs-lookup"><span data-stu-id="24448-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="24448-161">여러 주석 ErrorMessage 특성을 사용 하 여 오류 메시지를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="24448-162">또한 필요한 주석 ErrorMessage를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="24448-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="24448-164">NotMapped</span></span>

<span data-ttu-id="24448-165">코드의 첫 번째 규칙 데이터베이스에서 지원 되는 데이터 형식의 모든 속성 나타나는 것을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="24448-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="24448-166">하지만 응용 프로그램의 경우가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="24448-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="24448-167">예를 들어 제목과 BloggerName 필드를 기반으로 하는 코드를 만드는 블로그 클래스의 속성이 있을 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="24448-168">해당 속성이 동적으로 만들 수 있으며 저장할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="24448-169">이 BlogCode 속성과 같이 NotMapped 주석이 지정 된 데이터베이스에 매핑되지 않는 모든 속성을 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="24448-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="24448-170">ComplexType</span></span>

<span data-ttu-id="24448-171">클래스의 여러 도메인 엔터티를 설명 하 고 전체 엔터티를 설명 하는 해당 클래스를 다음 계층에 일반적이 지 않은 것입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="24448-172">예를 들어 모델 BlogDetails 라는 클래스를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="24448-173">BlogDetails에 모든 형식의 키 속성이 없는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="24448-174">도메인 기반 디자인을 BlogDetails는 값 개체 라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="24448-175">Entity Framework는 복합 형식으로 값 개체를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="24448-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="24448-176">복합 형식은 자체적으로 추적할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="24448-177">그러나 블로그 개체의 일부로 추적 됩니다 BlogDetails 블로그 클래스의 속성으로.</span><span class="sxs-lookup"><span data-stu-id="24448-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="24448-178">먼저이 인식 하는 코드에 대 한 순서로 BlogDetails 클래스 ComplexType로 표시 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="24448-179">이제 해당 블로그 BlogDetails 나타내는 블로그 클래스에서 속성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="24448-180">데이터베이스의 블로그 테이블 블로그 BlogDetail 속성에 포함 된 속성을 포함 하 여 속성을 모두 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="24448-181">기본적으로 각 앞 BlogDetail 복합 형식의 이름을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="24448-183">또 다른 흥미로운 점은 DateCreated 속성 클래스는 nullable이 아닌 날짜/시간으로 정의 했지만, 관련 데이터베이스 필드는 null을 허용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="24448-184">데이터베이스 스키마를 적용 하려는 경우 필요한 주석을 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="24448-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="24448-185">ConcurrencyCheck</span></span>

<span data-ttu-id="24448-186">ConcurrencyCheck 주석을 사용 하면 플래그 하나 이상의 속성에서에서 동시성 검사는 데이터베이스 사용자를 편집 또는 엔터티를 삭제 하는 경우에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="24448-187">EF 디자이너를 사용 하 여 작업 한 적이 속성의 ConcurrencyMode를 Fixed로 설정 된 맞춥니다.</span><span class="sxs-lookup"><span data-stu-id="24448-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="24448-188">ConcurrencyCheck BloggerName 속성에 추가 하 여 작동 하는 방법을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="24448-189">BloggerName 필드 ConcurrencyCheck 주석으로 인해 SaveChanges 호출 되 면 해당 속성의 원래 값 업데이트에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="24448-190">이 명령은 올바른 행 키 값 뿐만 아니라 BloggerName의 원래 값에 대해 필터링 하 여 찾은 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="24448-191">명령을 PrimaryTrackingKey가 있는 행 업데이트를 볼 수 있는 데이터베이스에 전송 업데이트 명령의 중요 한 부분 1 이며 해당 블로그 데이터베이스에서 검색 된 경우 원래 값 이었던 "Julie"의 BloggerName 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="24448-192">사용자가 변경 된 경우 해당 블로그 블로거 이름을 그동안이 업데이트 실패 하 고 처리 해야 하는 DbUpdateConcurrencyException 받게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="24448-193">타임 스탬프</span><span class="sxs-lookup"><span data-stu-id="24448-193">TimeStamp</span></span>

<span data-ttu-id="24448-194">보다 일반적으로 동시성 검사에 대 한 타임 스탬프 또는 rowversion 필드를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="24448-195">하지만 ConcurrencyCheck 주석을 사용 하는 대신 사용할 수 있습니다 보다 구체적인 타임 스탬프 주석으로 속성의 형식은 바이트 배열입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="24448-196">코드 처음으로 처리 될 타임 스탬프 속성 동일한 ConcurrencyCheck 속성 있지만 것도 코드는 먼저 생성 된 데이터베이스 필드를 nullable이 아닌 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="24448-197">지정된 된 클래스의 한 타임 스탬프 속성을 하나만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="24448-198">블로그 클래스에 다음 속성을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="24448-199">먼저 데이터베이스 테이블에서 null이 아닌 타임 스탬프 열을 생성 하는 코드의 결과입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="24448-201">테이블 및 열</span><span class="sxs-lookup"><span data-stu-id="24448-201">Table and Column</span></span>

<span data-ttu-id="24448-202">Code First는 데이터베이스를 만들 수 있도록 됩니다, 경우에 테이블 및 열을 만드는의 이름을 변경 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="24448-203">사용할 수도 있습니다 Code First는 기존 데이터베이스를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="24448-204">그러나 항상 클래스와 도메인 속성의 이름을 테이블 및 데이터베이스의 열 이름과 일치 하는 경우는 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="24448-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="24448-205">필자의 클래스 라는 블로그 고 규칙에 따라 코드 먼저 가정이 블로그 라는 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="24448-206">에 해당 하지 않은 경우 테이블 특성을 사용 하 여 테이블의 이름을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="24448-207">다음 예를 들어, 주석을 지정 하는 테이블 이름이 InternalBlogs 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="24448-208">열 주석의는 자세한 매핑된 열의 특성을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="24448-209">이름, 데이터 형식 또는 열을 테이블에 표시 되는 순서도 규정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="24448-210">열 특성의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="24448-211">열의 DataType DataAnnotation TypeName 특성을 혼동 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="24448-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="24448-212">DataType UI에 사용 되는 주석 인지 하 고 Code First에서 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="24448-213">가 다시 생성 된 후 표에서 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="24448-214">InternalBlogs 테이블 이름 변경 되었습니다 및 복합 형식에서 설명 열 BlogDescription 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="24448-215">주석에서 이름을 지정 하기 때문에 코드 먼저 사용 하지 않습니다 열 이름이 복합 형식의 이름으로 시작 하는 규칙.</span><span class="sxs-lookup"><span data-stu-id="24448-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="24448-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="24448-217">DatabaseGenerated</span></span>

<span data-ttu-id="24448-218">중요 한 데이터베이스 기능은 속성 계산 하는 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="24448-219">Code First 클래스에 매핑하는 경우 계산 열을 포함 하는 테이블, 해당 열을 업데이트 하려면 Entity Framework를 표시 하지 않으려면입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="24448-220">하지만 EF 삽입 하거나 데이터를 업데이트 한 후 데이터베이스에서 해당 값을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="24448-221">DatabaseGenerated 주석을 함께 계산 됨 열거형 클래스에서 해당 속성 플래그를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="24448-222">다른 열거형은 None 및 Id입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="24448-223">그렇지 않은 경우에 사용 해야이 코드 먼저 수 없기 때문에 계산된 열에 대 한 수식을 확인 하려면 기존 데이터베이스를 가리키는 경우, 코드 먼저 생성 되는 데이터베이스 바이트 또는 타임 스탬프 열에 생성 된 데이터베이스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="24448-224">기본적으로 위에 읽기는 정수가 키 속성을 데이터베이스의 id 키가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="24448-225">되는 동일한 방식으로 DatabaseGenerated DatabaseGenerationOption.Identity를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="24448-226">원하지 않는 id 키 수, 하는 경우에 DatabaseGenerationOption.None에 값을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="24448-227">인덱스</span><span class="sxs-lookup"><span data-stu-id="24448-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="24448-228">**EF6.1 이상만** -The 인덱스 특성은 Entity Framework 6.1에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="24448-229">이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="24448-230">사용 하 여 하나 이상의 열에 인덱스를 만들 수 있습니다 합니다 **IndexAttribute**합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="24448-231">하나 이상의 속성에 특성을 추가 인덱스를 만들려면 해당 데이터베이스에 데이터베이스를 만들 때 EF 시키거나 해당 스 캐 폴드 **CreateIndex** Code First 마이그레이션을 사용 하는 경우 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="24448-232">인덱스를 작성 하 고 다음 코드 예를 들어 하면를 **등급** 열의 합니다 **게시물** 데이터베이스의 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="24448-233">기본적으로 인덱스 이름은 **IX\_&lt;속성 이름&gt;**  (IX\_위의 예에서 등급).</span><span class="sxs-lookup"><span data-stu-id="24448-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="24448-234">또한 통해 인덱스의 이름을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="24448-235">다음 예에서는 인덱스의 이름이 있는지를 지정 **PostRatingIndex**합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="24448-236">기본적으로 인덱스는 고유 하지 않은 하지만 사용할 수 있습니다는 **IsUnique** 명명 된 매개 변수 인덱스를 고유 해야 함을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="24448-237">다음 예제에서 고유 인덱스를 소개 합니다는 **사용자**의 로그인 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-237">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="24448-238">다중 열 인덱스</span><span class="sxs-lookup"><span data-stu-id="24448-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="24448-239">여러 열에 걸쳐 있는 인덱스는 지정된 된 테이블에 대 한 여러 인덱스 주석에서 동일한 이름을 사용 하 여 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="24448-240">다중 열 인덱스를 만들면 인덱스의 열 순서를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="24448-241">예를 들어, 다음 코드에서 다중 열 인덱스를 만듭니다 **등급** 하 고 **BlogId** 호출 **IX\_BlogAndRating**합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="24448-242">**BlogId** 인덱스의 첫 번째 열 및 **등급** 두 번째입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="24448-243">관계: InverseProperty 특성과 ForeignKey</span><span class="sxs-lookup"><span data-stu-id="24448-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="24448-244">이 페이지는 Code First 데이터 주석 사용 하 여 모델의 관계를 설정 하는 방법에 대 한 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="24448-245">EF 및 관계를 사용 하 여 데이터 액세스 및 조작 하는 방법에 관계에 대 한 일반적인 정보를 참조 하세요 [관계 및 탐색 속성](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="24448-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="24448-246">모델의 가장 일반적인 관계 코드 첫 번째 규칙을 고려 하지만 도움말 필요한 곳 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="24448-247">게시물의 관계를 사용 하 여 문제를 만든 블로그 클래스의 키 속성의 이름을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="24448-248">데이터베이스를 생성 하는 경우 코드 먼저 Post 클래스 BlogId 속성 표시 고이 클래스 이름과 "Id", 블로그 클래스에 외래 키로 일치 규칙을 통해 인식 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="24448-249">하지만 블로그 클래스에서 BlogId 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="24448-250">이 솔루션은 게시물에는 탐색 속성을 만들고 외래 DataAnnotation 먼저 두 클래스 간의 관계를 구축 하는 방법을 이해 하는 코드를 사용 하 여-Post.BlogId 속성을 사용 하 여-제약 조건을 지정 하는 방법 뿐만 아니라는 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="24448-251">데이터베이스에서 제약 조건을 InternalBlogs.PrimaryTrackingKey Posts.BlogId 사이의 관계를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="24448-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="24448-253">InverseProperty 클래스 간에 여러 관계가 있는 경우 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="24448-254">Post 클래스의 블로그 게시물을 작성 하는 한 추적 하려는 편집한 사용자 뿐만 아니라 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="24448-255">다음은 Post 클래스에 대 한 두 개의 새로운 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="24448-256">이러한 속성으로 참조 하는 Person 클래스에 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="24448-257">Person 클래스에 모든 사용자 및 모든 해당 사용자에 의해 업데이트 게시물에 대해 하나씩 작성 하는 게시물에 대 한 게시물을 다시 탐색 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="24448-258">먼저 코드는 자체적으로 두 개의 클래스에서 속성을 일치 시킬 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="24448-259">게시물에 대 한 데이터베이스 테이블 CreatedBy 사용자에 대 한 하나의 외래 키를 있어야 하 고 4 개는 외래 키 속성 만듭니다 먼저 UpdatedBy 상대방이 했지만 코드에 대 한: Person\_Id, Person\_Id1, CreatedBy\_Id 및 UpdatedBy\_id입니다.</span><span class="sxs-lookup"><span data-stu-id="24448-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="24448-261">이러한 문제를 해결 하려면 속성의 맞춤을 지정 하려면 InverseProperty 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="24448-262">직접에서 PostsWritten 속성 알고는이 형식을 참조 하는 게시물에 있기 때문에 Post.CreatedBy 관계가 빌드됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="24448-263">마찬가지로, PostsUpdated Post.UpdatedBy에 연결 됩니다.</span><span class="sxs-lookup"><span data-stu-id="24448-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="24448-264">및 코드 추가 외래 키를 먼저 만듭니다 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="24448-266">요약</span><span class="sxs-lookup"><span data-stu-id="24448-266">Summary</span></span>

<span data-ttu-id="24448-267">DataAnnotations 뿐만 아니라 코드 첫 번째 클래스를 사용 하 여 클라이언트 및 서버 쪽 유효성 검사를 설명할 수 있습니다 하지만 높이고 되도록 코드 먼저 해당 규칙을 기반으로 클래스에 대 한 가정을 수정도 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="24448-268">DataAnnotations를 사용 하 여 실현할 수 있습니다 없습니다만 데이터베이스 스키마를 생성 하지만 코드 첫 번째 클래스는 기존 데이터베이스에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="24448-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="24448-269">매우 유연한 중일 때 가장에는 일반적으로 구성을 변경할 수 있는 코드 첫 번째 클래스에서 필요한 DataAnnotations 제공 하는 염두에 둡니다.</span><span class="sxs-lookup"><span data-stu-id="24448-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="24448-270">클래스에 지 사례의 일부를 구성 하려면 Code First의 Fluent API 대체 구성 메커니즘은 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="24448-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>