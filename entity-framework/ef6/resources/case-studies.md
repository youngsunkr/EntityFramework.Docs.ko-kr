---
title: Entity Framework-EF6에 대 한 사례 연구
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490885"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Entity Framework 용 Microsoft 사례 연구
이 페이지에서 사례 연구는 Entity Framework를 사용 했는지는 몇 가지 실제 프로덕션 프로젝트를 선택 합니다.
> [!NOTE]
> 이러한 사례 연구의 자세한 버전은 Microsoft 웹 사이트에서 사용할 수 없게 됩니다. 따라서 링크 제거 되었습니다.

## <a name="epicor"></a>Epicor
Epicor는 (400 개 이상의 개발자)와 150 개 이상의 국가에서 기업용 계획 ERP (Enterprise Resource) 솔루션을 개발 하는 대규모 글로벌 소프트웨어 회사입니다.
에 서비스 지향 아키텍처 (SOA).NET Framework를 사용 하 여 해당 주력 제품인 Epicor 9에 기반 합니다.
팀은 Visual Studio 2010 및.NET Framework 4.0으로 업그레이드 하기로 언어 통합 쿼리 (LINQ), 및도 해당 백 엔드 SQL 서버의 로드를 줄이려면 하려는 대 한 지원을 제공 하기 위해 다양 한 고객의 요청에 직면 합니다.
이러한 목표를 달성 하 고 개발 및 유지 관리를 크게 간소화할 수 있었습니다 Entity Framework 4.0을 사용 합니다.
특히 Entity Framework의 다양 한 T4 지원을 사용 하는 생성 된 코드의 완전 한 제어 및 미리 컴파일된 쿼리 및 캐싱 같은 성능 저장 기능에 자동으로 빌드할 수 있도록 합니다.

> "기존 코드를 사용 하 여 최근에 일부 성능 테스트를 수행 하는 것을 90%가 SQL Server에 요청을 줄일 수 있었습니다.
이것이 ADO.NET Entity Framework 4 때문입니다. " – Erik Johnson, Vice President 제품 조사  

## <a name="veracity-solutions"></a>정확성 솔루션
유지 관리 하 고 장기 통해 확장 하기 어려울 수 하려는 이벤트 계획 소프트웨어 시스템을 획득 하지 정확성 솔루션 Silverlight 4를 기반으로 강력 하 고 사용 하기 쉬운 리치 인터넷 응용 프로그램으로 다시 작성 Visual Studio 2010을 사용 합니다.
코드 중복을 방지 하 고 계층 전체에서 공통 유효성 검사 및 인증 논리에 대 한 허용 하는 Entity Framework 위에 서비스 계층을 신속 하 게 작성할 수 있었습니다.NET RIA Services를 사용 합니다.  

> "우리 판매 된 Entity Framework에서 처음 도입 되었습니다 및 Entity Framework 4는 더 나은 것으로 입증 된 경우.
도구는 향상 된 및 개념적 모델, 저장소 모델 및 이러한 모델 간의 매핑을 정의 하는.edmx 파일을 조작 하는 것이 쉽습니다... Entity Framework를 사용 하 여 하루에 작동 하는 데이터 액세스 계층 이동할 수 등을 하면서 아웃 빌드합니다.
Entity Framework는 사실상 데이터 액세스 계층. 모르겠습니다 누구나 사용 하지 않습니다. 왜. " – Joe McBride, 수석 개발자

## <a name="nec-display-solutions-of-america"></a>America NEC 표시 솔루션
NEC는 광고주 및 네트워크 소유자 및 자체 수익 증가 하는 솔루션을 사용 하 여 위치 기반 디지털 광고를 위해 시장 입력 하고자 했습니다.
이 작업을 수행 하기 위해 기존 ad 캠페인에서 필요한 수동 프로세스를 자동화 하는 웹 응용 프로그램의 쌍을 출시 합니다.
사이트는 ASP.NET, Silverlight 3, AJAX 및 SQL Server 2008에 게 데이터 액세스 계층에서 Entity Framework와 함께 WCF를 사용 하 여 빌드됩니다.

> "SQL Server를 사용 하 여 생각 했던 실시간은 업무용 응용 프로그램에서 정보를 항상 사용할 수 있도록 안정성에 대 한 정보를 사용 하 여 광고 및 네트워크를 제공 하기 위해 우리가 찾던 처리량을 가져올 수"-Mike Corcoran 이사 IT

## <a name="darwin-dimensions"></a>다윈 차원
다양 한 Microsoft 기술을 사용 하 다윈 팀은로 Evolver-소비자 게임, 애니메이션 및 소셜 네트워킹 페이지의 사용에 대 한 뛰어난, 실제 아바타를 만드는 데 사용할 수 있는 온라인 아바타 포털을 만듭니다.
팀은 생산성의 장점을 함께 Entity Framework 및 Windows WF (Workflow Foundation) 및 Windows Server AppFabric (뛰어난 메모리 내 응용 프로그램 캐시)와 같은 구성 요소에서 풀, 35% 절감에 멋진 제품을 제공할 수 있었습니다. 개발 시간입니다.
불구 하 고 팀 멤버에 걸쳐 분할할 여러 국가 매주 릴리스를 사용 하 여 민첩 한 개발 프로세스를 따라 팀.

 > "기술 위해서 기술을 만들지 시도 합니다. 신생 기업는 시간과 비용을 저장 하는 기술을 활용 하는 것이 중요 한 것입니다.
 .NET이 빠르고 비용 효율적인 개발에 대 한 선택입니다. " – Zachary Olsen, 설계자  

## <a name="silverware"></a>Silverware
15 년 이상 중소기업 식당 그룹에 대 한 판매 시점 관리 (POS) 솔루션을 개발 환경의 Silverware의 개발 팀 설정 큰 유치 하기 위해 더 많은 엔터프라이즈 수준 기능을 사용 하 여 해당 제품을 향상 시키기 위해 식당 체인입니다.
최신 버전의 Microsoft의 개발 도구를 사용 하 수 있었던 4 번 이전 보다 빠르게 새로운 솔루션을 빌드합니다.
LINQ 및 및 보고 요구를 쉽게 이동할 Crystal Reports에서 SQL Server 2008 및 SQL Server Reporting Services (SSRS) 해당 데이터 저장소에 대 한 Entity Framework 등의 주요 새 기능.

> "효과적인 데이터 관리 SilverWare –의 성공에 대 한 키 이며이 때문에 SQL Reporting을 채택 하기로 결정 했습니다." -Nicholas Romanidis, 이사 IT 소프트웨어 엔지니어링 /
