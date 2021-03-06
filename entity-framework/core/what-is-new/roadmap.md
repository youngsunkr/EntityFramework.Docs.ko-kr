---
title: Entity Framework Core 로드맵
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: a12d628a28515f0c6710bfa59bc6dcdf41fcb58b
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688591"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core 로드맵

> [!IMPORTANT]
> 이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.

### <a name="ef-core-22"></a>EF Core 2.2

이 릴리스에는 여러 버그 수정 및 일부 새로운 기능이 포함됩니다. 이 릴리스는 2018년 말로 예정되어 있습니다. 이 릴리스에 대한 자세한 내용은 [EF Core 2.2의 새로운 기능](xref:core/what-is-new/ef-core-2.2)에 포함되어 있습니다. 

### <a name="ef-core-30"></a>EF Core 3.0

주요 EF Core 릴리스를 .NET Core 3.0 및 ASP.NET 3.0에 맞출 계획이지만 아직 [릴리스 계획 프로세스](#release-planning-process)가 완성되지 않았습니다.

[Issue Tracker에서 이 쿼리](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)를 사용하여 3.0에 할당된 작업 항목을 임시로 확인하세요.

## <a name="schedule"></a>일정

EF Core에 대한 일정은 [.NET Core 일정](https://github.com/dotnet/core/blob/master/roadmap.md) 및 [ASP.NET Core 일정](https://github.com/aspnet/Home/wiki/Roadmap)과 동기화됩니다.

## <a name="backlog"></a>백로그

Issue Tracker의 [백로그 마일스톤](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)에는 언젠가 작업할 문제 또는 커뮤니티의 누군가가 해결할 수 있을 것으로 생각하는 문제가 포함되어 있습니다.
고객은 이러한 문제에 대해 의견을 제출하고 투표할 수 있습니다.
이러한 문제를 작업하려는 참가자는 문제에 대한 접근 방법을 토론하는 것부터 시작하는 것이 좋습니다.

특정 버전의 EF Core에서 제공된 기능에 대해 Microsoft가 작업한다는 보장은 없습니다.
모든 소프트웨어 프로젝트, 우선 순위, 릴리스 일정 및 사용 가능한 리소스는 언제든지 변경될 수 있습니다.
그러나 특정 시기에 문제를 해결하려는 경우 백로그 마일스톤이 아닌 릴리스 마일스톤에 해당 문제를 할당합니다.
Microsoft는 [릴리스 계획 프로세스](#release-planning-process)의 일부로써 백로그 마일스톤과 릴리스 마일스톤 간에 일상적으로 문제를 이동합니다.

문제를 해결할 계획이 없는 경우 문제를 종결할 가능성이 있습니다.
그러나 새로운 정보가 있는 경우 이전에 종결한 문제를 다시 고려할 수 있습니다.

## <a name="release-planning-process"></a>릴리스 계획 프로세스

특정 릴리스로 들어가는 특정 기능 선택 방법에 대한 질문을 종종 받습니다.
확실히 백로그는 릴리스 계획으로 자동으로 변환되지 않습니다.
EF6에서 기능의 존재 여부도 기능이 EF Core에서 구현되어야 함을 자동으로 의미하지 않습니다.

릴리스를 계획하기 위해 수행하는 전체 프로세스를 자세히 설명하기는 어렵습니다.
대부분의 경우 특정 기능, 기회 및 우선 순위에 대해 이야기하며 프로세스 자체는 모든 릴리스와 함께 진행됩니다.
그러나 다음에 작업할 항목을 결정할 때 답변하려는 일반적인 질문은 요약할 수 있습니다.

1. **얼마나 많은 개발자가 해당 기능을 사용하고 해당 응용 프로그램/환경을 얼마나 더 좋게 만들 수 있을까요?** 이 질문에 답변하기 위해 Microsoft는 많은 소스에서 피드백을 수집하며, 문제에 대한 의견 및 투표는 이러한 소스 중 하나입니다.

2. **이 기능을 아직 구현하지 않은 경우 사람들이 사용할 수 있는 해결 방법은 무엇인가요?** 예를 들어 많은 개발자는 네이티브 다대다 지원 부족의 문제를 해결하기 위해 조인 테이블을 매핑할 수 있습니다. 분명히 일부 개발자는 그렇게 하고 싶어 하지 않지만 많은 개발자가 그렇게 할 수 있으며, 이는 결정의 한 요인으로 고려됩니다.

3. **이 기능의 구현은 다른 기능의 구현으로 가까이 다가가도록 하는 EF Core의 아키텍처를 진화시키나요?** Microsoft는 다른 기능의 구성 요소로 작동하는 기능을 선호하는 경향이 있습니다. 예를 들어 속성 모음 엔터티는 다대다 지원을 지향하는 데 도움이 되며, 엔터티 생성자는 지연 로드 지원을 사용하도록 설정했습니다. 

4. **기능은 확장성 지점인가요?** 확장성 지점을 통해 개발자는 해당 동작에 연결하고 누락된 기능을 보완할 수 있으므로 Microsoft는 확장성 지점을 선호하는 경향이 있습니다. 

5. **다른 제품과 함께 사용할 경우 기능의 시너지 효과는 무엇인가요?** Microsoft는 EF Core를 .NET Core, 최신 버전의 Visual Studio, Microsoft Azure 등과 같은 다른 제품과 함께 사용하도록 하거나 그러한 환경을 크게 향상하는 기능을 선호합니다.

6. **사람들이 기능에 사용할 수 있는 기술은 무엇이며 이러한 리소스를 가장 잘 활용하는 방법은 무엇인가요?** EF 팀의 각 구성원과 커뮤니티 참가자는 서로 다른 영역에서 다양한 수준의 경험이 있으므로 그에 따라 계획해야 합니다. GroupBy 번역 또는 다대다와 같은 특정 기능을 작업하기 위해 “모든 인력을 동원”하려고 해도 이는 현실성이 떨어질 것입니다.

이전에 언급한 것처럼 프로세스는 모든 릴리스에서 진행됩니다.
향후 커뮤니티 구성원이 릴리스 계획에 대한 의견을 제공하는 기회를 더 많이 추가하겠습니다.
예를 들어 기능과 릴리스 계획 자체의 초안 디자인을 더 쉽게 검토할 수 있도록 하고 싶습니다.