---
title: Entity Framework Core 로드맵
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 5aef679df2ecdfe7f59458c8994d0d17b4a889ff
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
# <a name="entity-framework-core-roadmap"></a><span data-ttu-id="fe642-102">Entity Framework Core 로드맵</span><span class="sxs-lookup"><span data-stu-id="fe642-102">Entity Framework Core Roadmap</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe642-103">이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="fe642-104">EF Core 2.1의 첫 번째 미리 보기는 2018년 2월에 출시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-104">The first preview of EF Core 2.1 was released on February 2018.</span></span> <span data-ttu-id="fe642-105">이제 [EF Core 2.1의 새로운 기능](xref:core/what-is-new/ef-core-2.1)에서 이 릴리스에 대한 자세한 정보를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-105">You can now find more information about this release in [What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1).</span></span>

<span data-ttu-id="fe642-106">EF Core 2.1의 추가 미리 보기를 월별로, 2018년 두 번째 분기에 마지막 릴리스를 출시하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-106">We intend to release additional previews of EF Core 2.1 monthly, and a final release on the second calendar quarter of 2018.</span></span>

<span data-ttu-id="fe642-107">2.1 이후의 다음 릴리스에 대한 [릴리스 계획](#release-planning-process)을 완료하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-107">We have not completed the [release planning](#release-planning-process) for the next release after 2.1.</span></span>

## <a name="schedule"></a><span data-ttu-id="fe642-108">일정</span><span class="sxs-lookup"><span data-stu-id="fe642-108">Schedule</span></span>

<span data-ttu-id="fe642-109">EF Core에 대한 일정은 [.NET Core 일정](https://github.com/dotnet/core/blob/master/roadmap.md) 및 [ASP.NET Core 일정](https://github.com/aspnet/Home/wiki/Roadmap)과 동기화됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-109">The schedule for EF Core is in-sync with the [.NET Core schedule](https://github.com/dotnet/core/blob/master/roadmap.md) and [ASP.NET Core schedule](https://github.com/aspnet/Home/wiki/Roadmap).</span></span>

## <a name="backlog"></a><span data-ttu-id="fe642-110">백로그</span><span class="sxs-lookup"><span data-stu-id="fe642-110">Backlog</span></span>

<span data-ttu-id="fe642-111">문제 추적기에서 [백로그 중요 시점](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)을 사용하여 문제 및 기능의 자세한 목록을 유지 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-111">We use the [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in our issue tracker to maintain a detailed list of issues and features.</span></span> <span data-ttu-id="fe642-112">고객은 이에 대해 의견을 내고 투표할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-112">Customers can comment and up-vote these.</span></span>

<span data-ttu-id="fe642-113">합리적으로 일부 시점에 작업할 것을 예상하거나 커뮤니티의 일원이 문제를 제시할 수 있는 문제를 열린 상태로 두려는 경향이 있지만 [릴리스 계획 프로세스](#release-planning-process)의 일부로 특정 중요 시점에 해당 항목을 할당할 때까지 특정 시간에서 해결할 의도를 의미하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-113">We tend to leave issues open that we reasonably expect we will work on at some point, or that someone from the community could tackle, but that does not imply the intent to resolve them in a specific timeframe until we assign them to a specific milestone as part of our [release planning process](#release-planning-process).</span></span>

<span data-ttu-id="fe642-114">기능을 구현할 계획이 없는 경우 문제를 종결할 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-114">If we do not plan to ever implement a feature, we will likely close the issue.</span></span> <span data-ttu-id="fe642-115">종료한 문제는 이에 대한 새 정보를 얻는 경우 나중에 다시 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-115">An issue that we closed can be reconsidered at a later point if we obtain new information about it.</span></span>

<span data-ttu-id="fe642-116">즉, 기능 X가 시간/릴리스 Y로 확인될 것이라고 말할 수 있는 미래에 대한 충분한 정보가 없습니다. 모든 소프트웨어 프로젝트, 우선 순위, 릴리스 일정 및 사용 가능한 리소스는 언제든지 변경될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-116">All that said, we don’t have enough information about the future to be able to say that feature X will be resolved by time/release Y. As in all software projects, priorities, release schedules, and available resources can change at any point.</span></span>

## <a name="release-planning-process"></a><span data-ttu-id="fe642-117">릴리스 계획 프로세스</span><span class="sxs-lookup"><span data-stu-id="fe642-117">Release planning process</span></span>

<span data-ttu-id="fe642-118">특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-118">We often get questions about how we choose specific features to go into a particular release.</span></span> <span data-ttu-id="fe642-119">백로그는 분명히 릴리스 계획으로 자동으로 번역하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-119">Our backlog certainly does not automatically translate into release plans.</span></span> <span data-ttu-id="fe642-120">EF6에서 기능의 존재 여부도 기능이 EF Core에서 구현되어야 함을 자동으로 의미하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-120">The presence of a feature in EF6 also does not automatically mean that the feature needs to be implemented in EF Core.</span></span>

<span data-ttu-id="fe642-121">부분적으로 많은 부분이 특정 기능, 기회 및 우선 순위를 논의하고, 부분적으로 프로세스 자체는 일반적으로 모든 릴리스로 진화하기 때문에 여기에서 릴리스를 계획하도록 따르는 전체 프로세스를 자세히 설명하는 것은 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-121">It is difficult to detail here the whole process we follow to plan a release, partly because a lot of it is discussing the specific features, opportunities and priorities, and partly because the process itself usually evolves with every release.</span></span> <span data-ttu-id="fe642-122">그러나 다음에 작업할 것을 결정하는 경우 답하려는 일반적인 질문을 요약하는 것은 상대적으로 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-122">However, it is relatively easy to summarize the common questions we try to answer when deciding what to work on next:</span></span>

1. <span data-ttu-id="fe642-123">**얼마나 많은 개발자가 해당 기능을 사용하고 해당 응용 프로그램/환경을 얼마나 더 좋게 만들 수 있을까요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-123">**How many developers we think will use the feature and how much better will it make their applications/experience?**</span></span> <span data-ttu-id="fe642-124">많은 소스의 피드백을 여기로 집계합니다. 문제에 대한 의견 및 투표는 이러한 소스 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-124">We aggregate feedback from many sources into this — Comments and votes on issues is one of those sources.</span></span>

2. <span data-ttu-id="fe642-125">**이 기능을 아직 구현하지 않는 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-125">**What are the workarounds people can use if we don’t implement this feature yet?**</span></span> <span data-ttu-id="fe642-126">예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-126">For example, many developers are able to map a join table in order to work around lack of native many-to-many support.</span></span> <span data-ttu-id="fe642-127">물론, 모든 개발자가 이를 수행할 수 없지만 다수가 수행할 수 있으며 중요한 요인입니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-127">Obviously, not all developers can do this, but many can, and this is a factor which counts.</span></span>

3. <span data-ttu-id="fe642-128">**이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-128">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="fe642-129">다른 기능에 대한 빌딩 블록으로 작동하는 기능을 선호하는 경향이 있습니다. 예를 들어 소유 형식에 대해 수행된 테이블 분할을 통해 TPT 지원으로 이동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-129">We tend to favor features that act as building blocks for other features—for example, the table splitting that was done for owned types helps us move towards TPT support.</span></span>

4. <span data-ttu-id="fe642-130">**기능은 확장성 지점인가요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-130">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="fe642-131">확장성 지점을 통해 개발자는 더욱 쉽게 해당 동작에 연결하고 이러한 방식으로 누락된 기능의 일부를 가져올 수 있으므로 확장성 지점을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-131">We tend to favor extensibility points because they enable developers to more easily hook in their own behaviors and get some of the missing functionality that way.</span></span> <span data-ttu-id="fe642-132">이 중 일부를 지연 로드 작업에 대한 시작으로 수행하려고 계획 중입니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-132">We’re planning to do some of this as a start to the lazy loading work.</span></span>

5. <span data-ttu-id="fe642-133">**다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-133">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="fe642-134">EF Core를 다른 제품과 함께 사용하도록 하거나 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품 사용의 환경을 크게 향상시키는 기능을 선호하는 경향이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-134">We tend to favor features that allow EF Core to be used with other products or to significantly improve the experience of using other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, etc.</span></span>

6. <span data-ttu-id="fe642-135">**기능에 사용할 수 있는 사람들의 능력은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?**</span><span class="sxs-lookup"><span data-stu-id="fe642-135">**What are the capabilities of the people available to work on a feature, and how to best leverage these resources?**</span></span> <span data-ttu-id="fe642-136">EF 팀의 각 멤버와 커뮤니티 참여자는 서로 다른 영역에서 다양한 수준의 경험을 가지고 있으며 그에 따라 계획해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-136">Each member of the EF team and even our community contributors have different levels of experience in different areas and we have to plan accordingly.</span></span> <span data-ttu-id="fe642-137">GroupBy 번역 또는 다대다와 같은 특정 기능에 대해 작업하기 위해 “모두 손을 모아 도와야 한다”는 것을 원했음에도 불구하고 이는 실용적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe642-137">Even if we wanted to have “all hands on deck” to work on a specific feature like GroupBy translations, or many-to-many, that wouldn’t be practical.</span></span>

<span data-ttu-id="fe642-138">이전에 언급했듯이 이 프로세스는 모든 릴리스를 진화시키고, 향후에 개발자 커뮤니티의 멤버를 위한 더 많은 기회를 추가하여 릴리스 계획에 대한 입력을 제공하려고 합니다(예: 기능 및 릴리스 계획 자체의 제안된 초안을 검토하기 쉽도록).</span><span class="sxs-lookup"><span data-stu-id="fe642-138">As mentioned before, this process evolves on every release, and in the future we would like to add more opportunities for members of the developer community to provide inputs into release plans, e.g. by making it easier to review proposed drafts of the features and of the release plan itself.</span></span>