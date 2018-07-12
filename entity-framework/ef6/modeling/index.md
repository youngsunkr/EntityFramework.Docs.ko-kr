---
title: 모델 만들기 - EF6
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
caps.latest.revision: 3
ms.openlocfilehash: e1eb4077a1fd77c66bb3e992e1d25a5de4fcc64c
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913514"
---
# <a name="creating-a-model"></a><span data-ttu-id="c64bb-102">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="c64bb-102">Creating a Model</span></span>

<span data-ttu-id="c64bb-103">EF 모델은 응용 프로그램 클래스 및 속성이 데이터베이스 테이블과 열에 매핑하는 방식에 대한 세부 정보를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-103">An EF model stores the details about how application classes and properties map to database tables and columns.</span></span> <span data-ttu-id="c64bb-104">EF 모델을 만드는 방법에는 크게 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-104">There are two main ways to create an EF model:</span></span>

- <span data-ttu-id="c64bb-105">**Code First 사용**: 개발자는 모델을 지정하는 코드를 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-105">**Using Code First**: The developer writes code to specify the model.</span></span> <span data-ttu-id="c64bb-106">EF는 런타임에 개발자가 제공한 엔터티 클래스 및 추가 모델 구성을 기반으로 모델과 매핑을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-106">EF generates the models and mappings at runtime based on entity classes and additional model configuration provided by the developer.</span></span>

- <span data-ttu-id="c64bb-107">**EF 디자이너 사용**: 개발자는 EF 디자이너를 사용하여 모델을 지정하는 상자와 선을 그립니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-107">**Using the EF Designer**: The developer draws boxes and lines to specify the model using the EF Designer.</span></span> <span data-ttu-id="c64bb-108">그 결과로 생성되는 모델은 EDMX 확장을 사용하여 파일에 XML로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-108">The resulting model is stored as XML in a file with the EDMX extension.</span></span> <span data-ttu-id="c64bb-109">응용 프로그램의 도메인 개체는 일반적으로 개념적 모델에서 자동으로 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-109">The application's domain objects are typically generated automatically from the conceptual model.</span></span>

## <a name="ef-workflows"></a><span data-ttu-id="c64bb-110">EF 워크플로</span><span class="sxs-lookup"><span data-stu-id="c64bb-110">EF workflows</span></span>

<span data-ttu-id="c64bb-111">두 방법 모두 기존 데이터베이스를 대상으로 지정하거나 새 데이터베이스를 만드는 데 사용할 수 있으며, 그 결과로 4가지 워크플로를 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-111">Both of these approaches can be used to target an existing database or create a new database, resulting in 4 different workflows.</span></span>
<span data-ttu-id="c64bb-112">어떤 것이 가장 적합한지 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="c64bb-112">Find out about which one is best for you:</span></span>  

|                                           | <span data-ttu-id="c64bb-113">코드만 작성하려 합니다...</span><span class="sxs-lookup"><span data-stu-id="c64bb-113">I just want to write code...</span></span>                                                                                                                   | <span data-ttu-id="c64bb-114">디자이너를 사용하고 싶습니다...</span><span class="sxs-lookup"><span data-stu-id="c64bb-114">I want to use a designer...</span></span>                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c64bb-115">**새 데이터베이스를 만들려는 경우**</span><span class="sxs-lookup"><span data-stu-id="c64bb-115">**I am creating a new database**</span></span>          | [<span data-ttu-id="c64bb-116">**Code First**를 사용하여 코드에서 모델을 정의한 다음, 데이터베이스를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-116">Use **Code First** to define your model in code and then generate a database.</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)           | [<span data-ttu-id="c64bb-117">**Model First**를 사용하여 상자와 선을 이용해 모델을 정의한 다음, 데이터베이스를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-117">Use **Model First** to define your model using boxes and lines and then generate a database.</span></span>](~/ef6/modeling/designer/workflows/model-first.md)   |
| <span data-ttu-id="c64bb-118">**기존 데이터베이스에 액세스해야 하는 경우**</span><span class="sxs-lookup"><span data-stu-id="c64bb-118">**I need to access an existing database**</span></span> | [<span data-ttu-id="c64bb-119">**Code First**를 사용하여 기존 데이터베이스에 매핑하는 코드 기반 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-119">Use **Code First** to create a code based model that maps to an existing database.</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md) | [<span data-ttu-id="c64bb-120">**Database First**를 사용하여 기존 데이터베이스에 매핑하는 상자 및 선 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-120">Use **Database First** to create a boxes and lines model that maps to an existing database.</span></span>](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a><span data-ttu-id="c64bb-121">비디오 보기: 어떤 EF 워크플로를 사용해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="c64bb-121">Watch the video: What EF workflow should I use?</span></span>

<span data-ttu-id="c64bb-122">이 짧은 비디오에서는 워크플로 간 차이점과 적합한 워크플로를 찾는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-122">This short video explains the differences, and how to find the one that is right for you.</span></span>

<span data-ttu-id="c64bb-123">**작성자**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="c64bb-123">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="c64bb-124">![WhichWorkflow_Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV(ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)</span><span class="sxs-lookup"><span data-stu-id="c64bb-124">![WhichWorkflow_Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)</span></span>

<span data-ttu-id="c64bb-125">비디오를 시청한 후에도 EF 디자이너와 Code First 중 무엇을 사용할지 결정하지 못했다면 둘 다 공부해 보세요.</span><span class="sxs-lookup"><span data-stu-id="c64bb-125">If after watching the video you still don't feel comfortable deciding if you want to use the EF Designer or Code First, learn both!</span></span>

## <a name="a-look-under-the-hood"></a><span data-ttu-id="c64bb-126">내부 살펴보기</span><span class="sxs-lookup"><span data-stu-id="c64bb-126">A look under the hood</span></span>

<span data-ttu-id="c64bb-127">Code First와 EF 디자이너 중에서 무엇을 사용하든, EF 모델에는 항상 다음과 같은 여러 구성 요소가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-127">Regardless of whether you use Code First or the EF Designer, an EF model always has several components:</span></span>

- <span data-ttu-id="c64bb-128">바로 응용 프로그램의 도메인 개체 또는 엔터티 형식 자체.</span><span class="sxs-lookup"><span data-stu-id="c64bb-128">The application's domain objects or entity types themselves.</span></span> <span data-ttu-id="c64bb-129">개체 레이어라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-129">This is often referred to as the object layer</span></span>

- <span data-ttu-id="c64bb-130">도메인 관련 엔터티 형식 및 관계로 구성되며 [엔터티 데이터 모델](~/ef6/resources/glossary.md#entity-data-model)을 사용하여 설명되는 개념적 모델.</span><span class="sxs-lookup"><span data-stu-id="c64bb-130">A conceptual model consisting of domain-specific entity types and relationships, described using the [Entity Data Model](~/ef6/resources/glossary.md#entity-data-model).</span></span> <span data-ttu-id="c64bb-131">이 레이어는 _conceptual_을 의미하는 "C"로 부르기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-131">This layer is often referred to with the letter "C", for _conceptual_.</span></span>

- <span data-ttu-id="c64bb-132">데이터베이스에 정의된 테이블, 열 및 관계를 나타내는 저장소 모델.</span><span class="sxs-lookup"><span data-stu-id="c64bb-132">A storage model representing tables, columns and relationships as defined in the database.</span></span> <span data-ttu-id="c64bb-133">이 레이어는 _storage_를 의미하는 "S"로 부르기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-133">This layer is often referred to with the later "S", for _storage_.</span></span>  

- <span data-ttu-id="c64bb-134">개념적 모델과 데이터베이스 스키마 간의 매핑.</span><span class="sxs-lookup"><span data-stu-id="c64bb-134">A mapping between the conceptual model and the database schema.</span></span> <span data-ttu-id="c64bb-135">이 매핑을 "C-S" 매핑이라고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-135">This mapping is often referred to as "C-S" mapping.</span></span>

<span data-ttu-id="c64bb-136">EF의 매핑 엔진은 "C-S" 매핑을 활용하여 만들기, 읽기, 업데이트, 삭제 등 엔터티에 대한 작업을 데이터베이스 테이블에 대한 동등한 작업으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-136">EF's mapping engine leverages the "C-S" mapping to transform operations against entities - such as create, read, update, and delete - into equivalent operations against tables in the database.</span></span>

<span data-ttu-id="c64bb-137">개념적 모델과 응용 프로그램 개체 간의 매핑을 "O-C" 매핑이라고 부르기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-137">The mapping between the conceptual model and the application's objects is often referred to as "O-C" mapping.</span></span> <span data-ttu-id="c64bb-138">"C-S" 매핑과 비교하면, "O-C" 매핑은 암시적인 일대일 매핑입니다. 개념적 모델에 정의된 엔터티, 속성 및 관계가 .NET 개체의 셰이프 및 형식과 일치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-138">Compared to the "C-S" mapping, "O-C" mapping is implicit and one-to-one: entities, properties and relationships defined in the conceptual model are required to match the shapes and types of the .NET objects.</span></span> <span data-ttu-id="c64bb-139">EF4 이상부터 계층 레이어는 EF에 대한 종속성 없이 속성이 있는 간단한 개체로 구성될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-139">From EF4 and beyond, the objects layer can be composed of simple objects with properties without any dependencies on EF.</span></span> <span data-ttu-id="c64bb-140">이를 보통 POCO(Plain Old CLR Object)라고 부르며 형식 및 속성 매핑은 이름 일치 규칙에 따라 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-140">These are usually referred to as Plain-Old CLR Objects (POCO) and mapping of types and properties is performed base on name matching conventions.</span></span> <span data-ttu-id="c64bb-141">기존의 EF 3.5에서는 엔터티가 EntityObject 클래스에서 파생되어야 하고 "O-C" 매핑을 구현하기 위한 EF 특성을 갖고 있어야 하는 등 개체 레이어와 관련된 제한이 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="c64bb-141">Previously, in EF 3.5 there were specific restrictions for the object layer, like entities having to derive from the EntityObject class and having to carry EF attributes to implement the "O-C" mapping.</span></span>