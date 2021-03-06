---
title: EF4, EF5, 및 EF6에 대 한 성능 고려 사항
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: c87c1412cb23abf232663d7e4f44eef5f7818ea2
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022391"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>4, 5 및 6 EF에 대 한 성능 고려 사항
David Obando, Eric Dettinger 등에서

게시 날짜: 2012 년 4 월

마지막 업데이트 날짜: 2014 년 5 월

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. 소개

개체-관계형 매핑 프레임 워크는 개체 지향 응용 프로그램에서 데이터 액세스를 위한 추상화를 제공 하는 편리한 방법입니다. .NET 응용 프로그램의 경우 O/RM Entity Framework는 Microsoft의 권장 합니다. 그러나 모든 추상화를 사용 하 여 성능 문제가 될 수 있습니다.

이 백서는 조사에 대 한 팁을 제공 하 고 개발자가 Entity Framework 내부 알고리즘 성능에 영향을 파악 하기 위해 Entity Framework를 사용 하 여 응용 프로그램을 개발 하는 경우 성능 고려 사항 표시에 기록 된 및 Entity Framework를 사용 하는 응용 프로그램의 성능 향상. 다양 한 성능에 좋은 주제 이미 사용할 수 있는 웹에 있으며 가능한 경우 이러한 리소스를 가리키는 하였습니다.

성능이 까다로운 항목이 됩니다. 이 백서는 데 사용 됩니다 리소스로 성능 할 관련 엔터티 프레임 워크를 사용 하는 응용 프로그램에 대 한 결정 합니다. 성능을 보여 주기 위해 몇 가지 테스트 메트릭을 포함 되지만 이러한 메트릭은 응용 프로그램에서 성능 절대 표시기로 사용 되지 않습니다.

실질적으로이 문서에서는 Entity Framework 4는.NET 4.0 및 Entity Framework 5에서 실행 되 고 6.NET 4.5에서 실행 됩니다. Entity Framework 5에 대 한 성능 향상에 많은.NET 4.5를 사용 하 여 제공 되는 핵심 구성 요소 내에 상주 합니다.

Entity Framework 6 대역 외 릴리스의 부족 이며.NET과 함께 제공 되는 Entity Framework 구성 요소에 종속 되지 않습니다. Entity Framework 6.NET 4.0 및.NET 4.5에서 작동 하 고 해당 응용 프로그램에서 Entity Framework 최신 싶지만.NET 4.0에서 업그레이드 하지 않은 사람에 게 큰 성능 이점을 제공할 수 있습니다. 이 문서 작성 당시 사용 가능한 최신 버전을 가리키는 경우이 문서는 Entity Framework 6을 언급 합니다. 6.1.0 버전입니다.

## <a name="2-cold-vs-warm-query-execution"></a>2. 콜드 vs입니다. 웜 쿼리 실행

처음으로 지정된 된 모델에 대 한 모든 쿼리를 실행 하는 Entity Framework는 많은 로드 하 고 모델 유효성 검사는 백그라운드 작업을 수행 합니다. 자주 "콜드" 쿼리로이 첫 번째 쿼리를 참조합니다.  이미 로드 된 모델에 대해 추가 쿼리 "웜" 쿼리로 알려져 및 훨씬 더 빠릅니다.

Entity Framework를 사용 하 여 쿼리를 실행할 때 시간이 소요 된 위치를 간략하게 해 작업 Entity Framework 6에서 개선 되 고 있는 참조 보겠습니다.

**첫 번째 쿼리 실행-콜드 쿼리**

| 사용자 쓰기 코드                                                                                     | 작업                    | EF4 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | 컨텍스트 만들기          | 중간                                                                                                                                                                                                                                                                                                                                                                                                                        | 중간                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | 쿼리 식 만들기 | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                           | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | LINQ 쿼리 실행      | 메타 데이터 로드: 캐시 된 하지만 높은 <br/> --생성을 확인 하는 중: 잠재적으로 매우 높은 하지만 캐시 <br/> 매개 변수 평가: 중간 <br/> --번역을 쿼리 하는 중: 중간 <br/> -구체화 생성: 캐시 되지만 보통 <br/> -쿼리 실행을 데이터베이스: 높아질 수 있으므로 <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 <br/> Id 조회: 중간 | 메타 데이터 로드: 캐시 된 하지만 높은 <br/> --생성을 확인 하는 중: 잠재적으로 매우 높은 하지만 캐시 <br/> 매개 변수 평가: 낮음 <br/> --번역을 쿼리 하는 중: 캐시 되지만 보통 <br/> -구체화 생성: 캐시 되지만 보통 <br/> -쿼리 실행을 데이터베이스: 잠재적으로 높은 (향상 된 상황에 따라 쿼리) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 <br/> Id 조회: 중간 | 메타 데이터 로드: 캐시 된 하지만 높은 <br/> --생성을 확인 하는 중: 캐시 되지만 보통 <br/> 매개 변수 평가: 낮음 <br/> --번역을 쿼리 하는 중: 캐시 되지만 보통 <br/> -구체화 생성: 캐시 되지만 보통 <br/> -쿼리 실행을 데이터베이스: 잠재적으로 높은 (향상 된 상황에 따라 쿼리) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 (EF5 보다 더 빠르고) <br/> Id 조회: 중간 |
| `}`                                                                                                  | Connection.Close          | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                           | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**두 번째 쿼리 실행-웜 쿼리**

| 사용자 쓰기 코드                                                                                     | 작업                    | EF4 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 성능에 미치는 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | 컨텍스트 만들기          | 중간                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 중간                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | 쿼리 식 만들기 | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | LINQ 쿼리 실행      | 메타 데이터 ~~로드~~ 조회: ~~캐시 하지만 높은~~ 낮음 <br/> -볼 ~~세대~~ 조회: ~~잠재적으로 매우 높은 되지만 캐시 된~~ 낮음 <br/> 매개 변수 평가: 중간 <br/> -쿼리 ~~번역~~ 조회: 중간 <br/> -구체화 ~~세대~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> -쿼리 실행을 데이터베이스: 높아질 수 있으므로 <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 <br/> Id 조회: 중간 | 메타 데이터 ~~로드~~ 조회: ~~캐시 하지만 높은~~ 낮음 <br/> -볼 ~~세대~~ 조회: ~~잠재적으로 매우 높은 되지만 캐시 된~~ 낮음 <br/> 매개 변수 평가: 낮음 <br/> -쿼리 ~~번역~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> -구체화 ~~세대~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> -쿼리 실행을 데이터베이스: 잠재적으로 높은 (향상 된 상황에 따라 쿼리) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 <br/> Id 조회: 중간 | 메타 데이터 ~~로드~~ 조회: ~~캐시 하지만 높은~~ 낮음 <br/> -볼 ~~세대~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> 매개 변수 평가: 낮음 <br/> -쿼리 ~~번역~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> -구체화 ~~세대~~ 조회: ~~캐시 되지만 보통~~ 낮음 <br/> -쿼리 실행을 데이터베이스: 잠재적으로 높은 (향상 된 상황에 따라 쿼리) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중간 (EF5 보다 더 빠르고) <br/> Id 조회: 중간 |
| `}`                                                                                                  | Connection.Close          | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


콜드 및 웜 쿼리의 성능 비용을 줄이기 위해 여러 가지 및 다음 섹션에서 살펴보겠습니다 알아보겠습니다. 특히, 모델 뷰 생성 동안 발생된 하는 성능 문제를 완화 하는 데 도움이 됩니다 미리 생성 된 뷰를 사용 하 여 콜드 쿼리에서 로드의 비용을 절감 살펴보겠습니다. 웜 쿼리에 대 한 쿼리 계획 캐싱, 추적 쿼리가 없습니다 및 다른 쿼리 실행 옵션 설명 합니다.

### <a name="21-what-is-view-generation"></a>2.1 뷰 생성 란 무엇입니까?

세대는, 어떤 보기를 이해 하기 위해 "매핑 뷰" 이란 먼저 이해 해야 합니다. 매핑 뷰는 각 엔터티 집합 및 연결에 대 한 매핑을 지정 된 변환이 실행 가능 표현을입니다. 내부적으로 이러한 매핑 뷰 CQTs (정식 쿼리 트리)의 형태를 수행 합니다. 매핑 보기는 다음과 같은 두 종류가 있습니다.

-   뷰를 쿼리 합니다: 데이터베이스 스키마에서 개념적 모델로 이동 하는 데 필요한 변환을 나타냅니다.
-   뷰 업데이트: 개념적 모델에서 데이터베이스 스키마로 이동 하는 데 필요한 변환을 나타냅니다.

개념적 모델의 다양 한 방법으로 데이터베이스 스키마에서 다를 수 있는 점을 염두에 두십시오. 예를 들어 두 개의 서로 다른 엔터티 형식에 대 한 데이터를 저장할 단일 테이블을 사용할 수 있습니다. 중요 한 역할에서 복잡 한 매핑 뷰 상속 및 특수 매핑.

매핑 사양에 따라 이러한 뷰를 컴퓨팅의 과정이 이라고 하는 뷰 생성 합니다. 뷰 생성 수행 될 수 있거나 동적으로 모델을 로드 하거나 빌드 시간에 "미리 생성 된 뷰";를 사용 하 여 Entity SQL 문 C의 형태로 serialize 되는 후자\# 또는 VB 파일입니다.

뷰 생성 되 면 이러한도 유효성이 검사 됩니다. 성능 관점에서 뷰 생성 비용의 대부분은 실제로 유효성 검사는 뷰는 엔터티 간의 연결 의미가 있고 지원 되는 모든 작업에 대 한 올바른 카디널리티를 보장 하는

엔터티 집합을 통해 쿼리를 실행 하는 경우 쿼리는 해당 쿼리 뷰를 함께 사용 하 고이 컴퍼지션의 결과 백업 저장소 이해할 수 있는 쿼리의 표현을 만드는 데 계획 컴파일러를 통해 실행 됩니다. SQL Server에 대 한이 컴파일의 최종 결과 T-SQL SELECT 문이 됩니다. 엔터티 집합을 통해 업데이트 처리는 처음으로 업데이트 보기는 대상 데이터베이스의 DML 문으로 변환 하는 유사한 프로세스를 통해 실행 됩니다.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 뷰 생성 성능에 영향을 주는 요소

뿐만 아니라 뷰 생성 단계의 성능 모델의 크기에 있지만 어떻게 상호 연결 된 모델에 따라 달라 집니다. 두 엔터티 상속 체인 또는 연결을 통해 연결 되어 있으면 연결 이라고 합니다. 마찬가지로 두 테이블이 외래 키를 통해 연결 되어 있으면 해당 연결 됩니다. 연결 된 엔터티 및 스키마의 테이블 수가 커질수록 뷰 생성 비용이 늘어납니다.

생성 하 고 보기의 유효성을 검사 하는 데 사용할 알고리즘은이 개선 하기 위해 일부 최적화 기능을 사용 하지만 최악의 경우 지 수입니다. 성능 저하를 보이는 가장 큰 요인은 다음과 같습니다.

-   엔터티 수 및 이러한 엔터티 간 연결의 양을 참조 모델 크기입니다.
-   많은 유형의 포함 하는 상속 특히 모델 복잡성입니다.
-   외래 키 연결 대신 독립 연결을 사용합니다.

간단한 모델에 대 한 비용을 미리 생성 된 뷰를 사용 하 여 문제 되지 수 있을 만큼 적습니다 수 있습니다. 모델 크기와 복잡성 증가, 일부의 옵션 보기 생성 및 유효성 검사의 비용을 줄이기 위해 사용할 수 있습니다.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>모델을 줄이기 위해 있는 2.3를 사용 하 여 Pre-Generated 보기 로드 시간

Entity Framework 6에서 미리 생성 된 뷰를 사용 하는 방법에 대 한 자세한 내용은 방문 [Pre-Generated 매핑 보기](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 Entity Framework Power Tools Community Edition을 사용 하 여 미리 생성 된 뷰

사용할 수는 [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 모델 클래스 파일을 마우스 오른쪽 단추로 클릭 하 고 "뷰 생성"을 선택 하는 Entity Framework 메뉴를 사용 하 여 EDMX 및 Code First 모델의 뷰를 생성할 수 있습니다. Entity Framework Power Tools Community Edition DbContext에서 파생 된 컨텍스트 에서만 작동 합니다.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 EDMGen에 의해 생성 된 모델을 사용 하 여 미리 생성 된 뷰를 사용 하는 방법

EDMGen는.NET과 함께 제공 하 고 Entity Framework 6 있지만 Entity Framework 4 및 5를 사용 하 여 작동 하는 유틸리티입니다. EDMGen을 사용 하면 명령줄에서 모델 파일, 개체 계층 및 뷰를 생성할 수 있습니다. VB 또는 C가 선택한 언어로 뷰 파일을 출력 중 하나로 됩니다\#합니다. 각 엔터티 집합에 대 한 Entity SQL 조각을 포함 하는 코드 파일입니다. 미리 생성 된 뷰를 사용 하려면 단순히 프로젝트에 파일을 포함 합니다.

하는 경우 수동으로 편집 모델에 대 한 스키마 파일을 뷰 파일을 다시 생성 해야 합니다. 사용 하 여 EDMGen을 실행 하 여 이렇게 합니다 **/mode:ViewGeneration** 플래그입니다.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 EDMX 파일을 사용 하 여 Pre-Generated 뷰를 사용 하는 방법

EDMX 파일에 대 한 보기를 생성 하려면 EDMGen을 사용할 수도 있습니다-이전에 참조 된 MSDN 항목-이 작업을 수행 하려면 빌드 전 이벤트를 추가 하는 방법에 설명 하지만이 복잡 한 및 수는 없는 경우도 있습니다. 일반적으로 T4 템플릿을 사용 하 여 모델 edmx 파일의 때 뷰를 생성 하는 것이 쉽습니다.

ADO.NET 팀 블로그는 뷰를 생성 하기 위해 T4 템플릿을 사용 하는 방법에 설명 하는 게시물 ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>)합니다. 이 게시물에 다운로드 하 고 프로젝트에 추가할 수 있는 템플릿을 포함 합니다. 최신 버전의 Entity Framework를 사용 하는 보장 되지 않습니다 있도록 Entity Framework의 첫 번째 버전에 대 한 템플릿을 작성 되었습니다. 그러나 Visual Studio 갤러리 Entity Framework 4 및 5from 뷰 생성 템플릿 집합을 좀 더 최신를 다운로드할 수 있습니다.

-   VB.NET의 경우: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Entity Framework 6을 사용 하는 경우에서 가져올 수 있습니다 뷰 생성 T4 템플릿 아래에 있는 Visual Studio 갤러리 \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>합니다.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 보기 생성의 비용을 절감

디자인 타임 미리 생성 된 뷰를 사용 하 여 이동 비용 (런타임에) 로드 하는 모델에서 뷰 생성을 합니다. 런타임 시 시작 성능이 향상 됩니다을 하는 동안 개발 하는 동안에 보기 생성의 어려움을 여전히 발생할 수 있습니다. 뷰 생성, 컴파일 시간 및 런타임 모두 비용을 절감 하는 데 도움이 되는 몇 가지 추가 요령 있습니다.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 외래 키 연결을 사용 하 여 뷰 생성 비용을 줄일 수

많은 경우 크게 독립 연결에서 외래 키 연결 모델의 연결을 전환 뷰 생성에 소요 된 시간을 개선 하는 위치를 살펴보았습니다.

이러한 향상 된이 기능을 보여 주기 위해 EDMGen을 사용 하 여 Navision 모델의 두 버전을 생성 했습니다. *참고: seeappendix Cfor Navision 모델을 설명 합니다.* Navision 모델은 엔터티 및 이들 간의 관계는 매우 많은 양의 인해이 연습에 대 한 흥미로운입니다.

외래 키 연결을 사용 하 여이 매우 큰 모델의 한 버전을 생성 하 고 독립 연결을 사용 하 여 생성 된 다른 합니다. 그런 다음 각 모델에 대 한 뷰를 생성 하는 데 걸린 기간 시간 초과 되었습니다. Entity Framework 6 테스트 StorageMappingItemCollection 클래스에서 GenerateViews() 메서드를 사용 하는 동안는 뷰를 생성할 클래스 EntityViewGenerator에서에서 GenerateViews() 메서드를 사용 하는 엔터티 Framework5 테스트 합니다. 코드 구조는 Entity Framework 6 코드 베이스에서 발생 한 변경으로 인해이 있습니다.

Entity Framework 5를 사용 하 여 외래 키를 사용 하 여 모델에 대 한 뷰 생성 랩 컴퓨터에서 65 분을 걸렸습니다. Unknown의 얼마나 걸리는 독립 연결을 사용 하는 모델에 대 한 보기를 생성 합니다. 월별 업데이트를 설치 하려면 연구소에서 컴퓨터를 다시 부팅 된 전에 달에 대 한 실행 중인 테스트를 두었습니다.

Entity Framework 6을 사용 하 여 외래 키를 사용 하 여 모델에 대 한 뷰 생성 초가 28 동일한 랩 컴퓨터에서. 독립 연결을 사용 하 여 모델에 대 한 뷰 생성 58 초가 소요 되었습니다. Entity Framework 6에 뷰 생성 코드에서 수행 향상 된 기능 대부분의 프로젝트 빠른 시작 시간을 가져오려면 미리 생성 된 뷰를 필요 하지 않습니다 의미 합니다.

EDMGen 또는 Entity Framework 파워 도구를 사용 하 여 Entity Framework 4 및 5 뷰 미리 생성이 수행 될 수 있는 묶어 표시 하는 것이 반드시 합니다. Entity Framework 6 보기에 대 한 생성 가능 하거나에 설명 된 대로 프로그래밍 방식으로 Entity Framework 파워 도구를 통해 [Pre-Generated 매핑 뷰](~/ef6/fundamentals/performance/pre-generated-views.md)합니다.

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 독립 연결 하는 대신 외래 키를 사용 하는 방법

기본적으로 Fk 얻게 EDMGen 또는 Entity Designer에서 Visual Studio를 사용 하면 메시지를 Fk 및 IAs 전환 하려면 단일 확인란을 선택 하거나 명령줄 플래그입니다.

큰 Code First 모델에 있는 경우 독립 연결을 사용 하 여 해야 동일한 미치는 뷰 생성 합니다. 일부 개발자는이 해당 개체 모델이 오염 시 키 지 수를 고려 하는 경우 종속 개체에 대 한 클래스의 외래 키 속성을 포함 하 여이 영향을 방지할 수 있습니다. 이 주제에 대 한 자세한 정보를 찾을 수 있습니다 \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>합니다.

| 사용 하는 경우      | 방법                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | 두 엔터티 간의 연결을 추가한 후 참조 제약 조건이 있는지 확인 합니다. 참조 제약 조건에는 Entity Framework 독립 연결 대신 외래 키를 사용 하도록 알려 줍니다. 추가 세부 정보를 참조 하세요 \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>합니다. |
| EDMGen          | EDMGen을 사용 하 여 데이터베이스에서 파일을 생성를 외래 키를 적용 하 고 같이 모델에 추가 됩니다. EDMGen에 의해 노출 된 다양 한 옵션에 대 한 자세한 내용은 방문 [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx)합니다.                           |
| Code First      | "관계 규칙" 섹션을 참조 합니다 [코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/built-in.md) Code First를 사용 하는 경우 종속 개체에 대 한 외래 키 속성을 포함 하는 방법에 대 한 정보에 대 한 항목입니다.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 별도 어셈블리 모델 이동

모델에는 응용 프로그램의 프로젝트에 직접 포함 하 고 빌드 전 이벤트 또는 T4 템플릿을 통해 뷰를 생성 하는 경우 뷰 생성 및 유효성 검사 수행 됩니다 프로젝트를 다시 작성 될 때마다 모델이 변경 되지 않은 경우에 합니다. 모델을 별도 어셈블리를 이동 하 고 응용 프로그램의 프로젝트에서 참조 하는 경우 할 수 있습니다 다른 변경 내용을 응용 프로그램 모델을 포함 하는 프로젝트를 다시 빌드하지 않고도.

*참고:*  어셈블리를 클라이언트 프로젝트의 응용 프로그램 구성 파일에 모델에 대 한 연결 문자열을 복사 해야 구분 하 여 모델을 이동할 때.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 edmx 기반 모델의 유효성 검사를 사용 하지 않도록 설정

EDMX 모델 모델을 변경 되지 않더라도 컴파일 타임에 유효성이 검사 됩니다. 모델에 이미 유효성을 검사 하는 경우 속성 창에서 "빌드 시 유효성 검사" 속성을 false로 설정 하 여 컴파일 시간에 유효성 검사를 억제할 수 있습니다. 사용자 매핑 또는 모델을 변경 하면 변경 내용을 확인 하도록 유효성 검사를 일시적으로 다시 사용할 수 있습니다.

성능 향상 된 Entity Framework 6에 대 한 Entity Framework 디자이너 하려고 하는 "빌드 시 유효성 검사"의 비용 보다 훨씬 낮습니다 디자이너의 이전 버전에서 note 합니다.

## <a name="3-caching-in-the-entity-framework"></a>Entity Framework에서 3 개의 캐싱

Entity Framework는 다음과 같은 캐싱 기본 제공 합니다.

1.  개체 캐싱 – ObjectContext 인스턴스 내장 ObjectStateManager 해당 인스턴스를 사용 하 여 검색 된 개체의 메모리에 추적을 유지 합니다. 이 첫 번째 수준의 캐시 라고도 합니다.
2.  쿼리 계획 캐싱-쿼리를 두 번 이상 실행 될 때 생성 된 저장소 명령은 다시 사용 합니다.
3.  메타 데이터 캐싱-동일한 모델에 다른 연결을 통해 모델에 대 한 메타 데이터를 공유 합니다.

기본적으로 특수 한 유형의 데이터베이스에서 검색 된 결과 대 한 캐시를 사용 하 여 Entity Framework 확장할 래핑 공급자를 사용할 수도 있습니다 라고 하는 ADO.NET 데이터 공급자는 EF 캐시 외에도 라고도 두 번째 수준 캐싱 합니다.

### <a name="31-object-caching"></a>3.1 개체 캐싱

기본적으로 엔터티를 쿼리의 결과에 반환 되 면, EF 구체화 직전 ObjectContext는 확인 동일한 키를 가진 엔터티는 ObjectStateManager에 이미 로드 된 경우 합니다. 동일한 키를 가진 엔터티가 이미 있는 경우 EF는 쿼리의 결과에 포함 됩니다. EF는 여전히 데이터베이스에 대해 쿼리를 실행 하 고, 하지만이 동작은 비용 엔터티를 여러 번 구체화의 대부분 무시할 수 있습니다.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 DbContext 찾기를 사용 하 여 개체 캐시에서 엔터티 가져오기

일반 쿼리를 달리 DbSet (EF 4.1에 처음으로 포함 된 Api)의 Find 메서드에서 데이터베이스에 대해 쿼리를 실행 하기 전에 메모리에서 검색을 수행 합니다. 서로 다른 ObjectContext의 두 인스턴스가 다른 인스턴스 두 개 ObjectStateManager에 별도 개체 캐시는 의미는 두는 것이 반드시 합니다.

찾을 기본 키 값을 사용 하 여 컨텍스트에서 추적 하는 엔터티를 찾을 하려고 합니다. 엔터티 컨텍스트에 없는 경우 다음 쿼리를 실행 되며 데이터베이스에 대해 평가 및 엔터티가 컨텍스트 또는 데이터베이스에 없는 경우 null이 반환 됩니다. 찾기도 컨텍스트에 추가 되었지만 아직 데이터베이스에 저장 되지 않은 엔터티를 반환 하는 참고 합니다.

성능 고려 사항 찾기를 사용할 때 수행할 수 있습니다. 기본적으로이 메서드를 호출 데이터베이스에 대 한 커밋 보류 중인 변경 내용을 감지 하기 위해 개체 캐시의 유효성 검사를 트리거할 수 있습니다. 이 프로세스는 매우 개체 또는 개체 캐시에 추가 되 고 큰 개체 그래프를 개체 캐시에 매우 많은 경우에 비용이 많이 들 수 있지만 해제할 수도 있습니다. 특정 경우에 메서드 자동을 사용 하지 않도록 설정 하는 경우 검색 찾기 호출 차이의 규모를 통해 변경 인식할 수 있습니다. 아직 실제로 개체가 때와 캐시에서 개체를 데이터베이스에서 검색 해야 하는 경우 두 번째 규모 인식 됩니다. 5000 엔터티의 부하를 사용 하 여 밀리초 단위로 표시 하는 microbenchmarks 중 일부를 사용 하 여 측정을 사용 하 여 예제 그래프는 다음과 같습니다.

![.NET 4.5 눈금](~/ef6/media/net45logscale.png ".NET 4.5-로그 눈금 간격")

사용 하지 않도록 설정 하는 자동 검색 변경 내용 찾기 예제:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Find 메서드를 사용 하는 경우를 고려해 야 할 것 같습니다.

1.  개체 캐시에 없는 경우 찾기의 이점을 부정 됩니다 있지만 구문은 여전히 키로 쿼리를 보다 간단 합니다.
2.  자동 변경 내용을 검색 하는 경우 사용 가능 한 작업량 또는 양과 개체 캐시의 엔터티 모델의 복잡도 따라 훨씬 더 Find 메서드 비용이 증가할 수 있습니다.

또한만 찾을 수 있음을 명심 찾고자 하는 엔터티를 반환 합니다. 것과 자동으로 로드 연결 된 해당 엔터티가 없는 개체 캐시의 경우. 관련된 엔터티를 검색 해야 할 경우 즉시 로드를 사용 하 여 키 쿼리를 사용할 수 있습니다. 자세한 내용은 참조 하세요. **8.1 지연 로드 vs. 즉시 로드**합니다.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 개체 캐시에 여러 엔터티 성능 문제

개체 캐시 Entity Framework의 전반적인 응답성을 향상 시킬 수 있습니다. 그러나 개체 캐시에 매우 많은 양의 추가 등의 특정 작업에 영향을 줄 수 있습니다 로드할된 엔터티를 제거, 항목, SaveChanges를 찾아보십시오. 특히 DetectChanges에 대 한 호출을 트리거하는 작업은 부정적인 영향을 받지 초대형 개체 캐시 합니다. DetectChanges 개체 그래프의 크기에 따라 직접 결정 해당 성능 되며 개체 상태 관리자를 사용 하 여 개체 그래프를 동기화 합니다. DetectChanges에 대 한 자세한 내용은 참조 하세요. [POCO 엔터티의 변경 내용 추적](https://msdn.microsoft.com/library/dd456848.aspx)합니다.

Entity Framework 6을 사용할 경우 개발자는 AddRange와 RemoveRange 컬렉션에서 반복 하 고 추가 인스턴스당 한 번 호출 하는 대신 DbSet을 직접 호출할 수 있습니다. 범위 메서드를 사용 하 여의 장점은 DetectChanges 비용 엔터티 추가 된 각 엔터티 마다 한 번씩 달리의 전체 집합에 대 한 한 번 유료는 한다는 점입니다.

### <a name="32-query-plan-caching"></a>3.2 쿼리 계획 캐싱

쿼리를 실행 하 고, 처음으로 거치 내부 계획 컴파일러 개념적 쿼리 저장소 명령 (예를 들어 T-SQL을 SQL Server를 실행할 때 실행 되는)으로 변환 합니다.  쿼리 계획 캐싱을 사용 하는 경우 쿼리는 다음에 저장소를 실행 하는 명령 실행, 계획 컴파일러 무시에 대 한 쿼리 계획 캐시에서 직접 검색 됩니다.

쿼리 계획 캐시는 동일한 AppDomain 내에서 ObjectContext 인스턴스 간에 공유 됩니다. 쿼리 계획 캐싱을 활용 하려면 ObjectContext 인스턴스에 저장할 필요가 없습니다.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 몇 가지 알아야 할 쿼리 계획 캐싱

-   모든 쿼리 형식에 대 한 쿼리 계획 캐시를 공유: Entity SQL, LINQ to Entities 및 CompiledQuery 개체입니다.
-   기본적으로 쿼리 계획 캐싱을 사용할 Entity SQL 쿼리에서 ObjectQuery 또는 EntityCommand를 통해 실행 여부를 합니다. 또한 기본적으로에 사용 LINQ to Entities 쿼리에서.NET 4.5에서 Entity Framework 및 Entity Framework 6
    -   쿼리 계획 캐싱 (EntityCommand에 ObjectQuery) EnablePlanCaching 속성을 false로 설정 하 여 비활성화할 수 있습니다. 예를 들어:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   매개 변수가 있는 쿼리에 대 한 매개 변수의 값을 변경도 소진 캐시 된 쿼리. 하지만 캐시에서 다른 항목을 소진 매개 변수 패싯 (예를 들어, 크기, 전체 자릿수 또는 소수)을 변경 합니다.
-   Entity SQL을 사용 하 여 키의 일부인 쿼리 문자열에 표시 합니다. 쿼리를 전혀 변경 기능적으로 동일 쿼리 하는 경우에 다양 한 캐시 항목을 발생 합니다. 여기에 공백 또는 대/소문자를 변경 합니다.
-   LINQ를 사용 하는 경우 키의 일부를 생성 하는 쿼리가 처리 됩니다. LINQ 식을 변경 하는 다른 키를 생성 하므로 됩니다.
-   기타 기술적 제한이 적용 될 수 있습니다. 자세한 내용은 Autocompiled 쿼리를 참조 하세요.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 캐시 제거 알고리즘

내부 알고리즘 작동 됩니다 하는 방법을 파악 해야 하는 경우 쿼리 계획 캐시 사용 안 함 또는 사용을 이해 합니다. 정리 알고리즘은 다음과 같습니다.

1.  캐시에 (800) 항목 집합 수를, 후 (일 분 단위로) 캐시를 비우고 주기적으로 타이머를 시작 합니다.
2.  (최소 자주 – 최근에 사용 됨)는 LFRU 캐시에서 항목이 제거 됩니다 캐시 sweeps 단위로 합니다. 이 알고리즘 고려 적중된 횟수 및 기간 항목을 방출 됩니다 결정 하는 경우.
3.  각 캐시 비우기 끝 캐시 다시 800 항목을 포함합니다.

제거 하는 항목을 결정할 때 모든 캐시 항목 동일 하 게 처리 됩니다. 즉, 한 CompiledQuery에 대 한 저장소 명령에는 Entity SQL 쿼리를 위한 저장소 명령으로 제거 될 가능성이 같습니다.

800 엔터티를 캐시 하지만 캐시의 경우 캐시 제거 타이머 시작 되는 60 초가이 타이머가 시작 된 후 스윕만 됩니다. 즉, 최대 60 초 동안 매우 큰 캐시 커질 수 있습니다.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 테스트 메트릭을 쿼리 계획 캐싱 성능 데모

쿼리 계획 캐싱 응용 프로그램의 성능에 영향을 보여 주기 위해 수행한 테스트 여기 Navision 모델에 대 한 Entity SQL 쿼리 횟수를 실행 했습니다. Navision 모델과 실행 된 쿼리의 종류에 대 한 부록을 참조 하세요. 이 테스트에서는 먼저 쿼리 목록을 반복 하 고 (캐싱 사용) 하는 경우 캐시에 추가 하려면 각각 두 번 실행 합니다. 이 단계가 시간 초과 하지 않습니다. 캐시 비우기; 수행 하도록 허용 하려면 60 초 이상의 주 스레드가 절전 모드에서는 다음으로, 마지막으로 캐시 된 쿼리를 실행 하는 두 번째 목록에 통해 반복 합니다. 또한 쿼리의 각 집합은 정확 하 게 구한 시간 쿼리 계획 캐시에서 지정 된 혜택을 반영 되도록 실행 되기 전에 플러시 그 SQL Server 계획 캐시 합니다.

##### <a name="3231-test-results"></a>3.2.3.1 테스트 결과

| 테스트                                                                   | EF5 캐시 없음 | EF5 캐시 | EF6 캐시 없음 | EF6 캐시 |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| 모든 18723 쿼리를 열거합니다.                                          | 124          | 125.4      | 124.3        | 125.3      |
| 스윕 (먼저 800 쿼리만, 복잡성에 관계 없이)를 방지합니다.  | 41.7         | 5.5        | 40.5         | 5.4        |
| (178 전체-비우기를 방지 하는) AggregatingSubtotals 쿼리만 | 39.5         | 4.5        | 38.1         | 4.6        |

*모든 시간 (초)입니다.*

도덕적-무수 실행 하는 경우 (예를 들어 동적으로 생성 된 쿼리), 고유한 쿼리 캐싱 도움이 되지 않습니다 하 고 캐시의 결과 플러시에서 실제로 사용 하 여 캐시 된 계획에서 가장 이익이 되는 쿼리를 유지할 수 있습니다.

AggregatingSubtotals 쿼리는 상당히 복잡 한 쿼리를 사용 하 여 테스트 했습니다. 예상 대로 더 복잡 한 쿼리는 쿼리 계획을 캐시에서 보면 있는 이점이 많아집니다.

CompiledQuery는 계획 캐시를 사용 하 여 LINQ 쿼리를 실제로 이기 때문에 해당 하는 Entity SQL 쿼리 및 CompiledQuery 비교 유사한 결과가 있어야 합니다. 실제로 앱에 동적 Entity SQL 쿼리는 많은 경우 쿼리를 사용 하 여 캐시를 채우는 효과적 하면 CompiledQueries "디컴파일" 캐시에서 플러시되는 경우에 합니다. 이 시나리오에서는 동적 쿼리는 CompiledQueries 우선 순위에서 캐싱을 사용 하지 않도록 설정 하 여 성능이 향상 될 수 있습니다. 더 나아가 물론 하는 것 대신 동적 쿼리 매개 변수가 있는 쿼리를 사용 하도록 앱을 다시 작성 합니다.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 CompiledQuery를 사용 하 여 LINQ 쿼리를 사용 하 여 성능 향상을 위해

이 테스트 결과 CompiledQuery를 사용 하 여 상태로 만들 수 있는지는 이점은 7 %autocompiled 통한 LINQ 쿼리; 즉,는 데는 7% 적은 시간이 Entity Framework 스택에서; 코드를 실행 합니다. 응용 프로그램에 7% 더 빠르게 됩니다 것이 아닙니다. 일반적으로 작성 하 고 EF 5.0에서 CompiledQuery 개체를 유지 관리의 비용 혜택을 비교할 때 문제가 가치가 없을 수 있습니다. 여러분 다를 수 있습니다, 그리고 따라서 프로젝트가 추가 푸시를 요구 하는 경우이 옵션을 실행 합니다. 참고 CompiledQueries는만 ObjectContext에서 파생 된 모델과 호환 DbContext에서 파생 된 모델을 사용 하 여 호환 되지 않습니다.

만들고를 CompiledQuery 호출에 대 한 자세한 내용은 참조 하세요. [컴파일된 쿼리 (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx)합니다.

CompiledQuery, 즉 사용 요구 사항이 정적 인스턴스 하 고 문제를 작성 한다면를 사용 하는 경우 수행 해야 하는 두 가지 고려 사항이 있습니다. 여기서 이러한 두 가지 고려 사항에 대 한 자세한 설명은 다음과 같습니다.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 정적 CompiledQuery 인스턴스를 사용 합니다.

LINQ 쿼리를 컴파일 시간이 오래 걸리는 프로세스 이므로 데이터베이스에서 데이터를 인출 해야 할 때마다 작업을 수행 하려고 하지 않습니다. CompiledQuery 인스턴스 한 번 컴파일하고 여러 번 실행 수 있지만 주의 기울여야 하 고 확보 될 때마다 반복 해 서이 컴파일하는 대신 동일한 CompiledQuery 인스턴스를 다시 사용 합니다. CompiledQuery 인스턴스를 저장 하는 정적 멤버를 사용 하 되 필요 합니다. 그렇지 않으면 모든 혜택을 볼 수 없습니다.

예를 들어, 페이지에 다음 메서드 본문 선택한 범주에 대 한 제품 표시를 처리 하는 것으로 가정 합니다.

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

이 경우 메서드가 호출 될 때마다 즉석에서 새 CompiledQuery 인스턴스를 만들가 있습니다. 저장소 명령은 쿼리 계획 캐시에서 검색 하 여 성능을 표시 하는 대신 새 인스턴스를 만들 때마다는 CompiledQuery 계획 컴파일러를 통해 전환 됩니다. 사실, 있습니다 됩니다 수 오염 시 키 지 새 CompiledQuery 항목을 사용 하 여 쿼리 계획 캐시 될 때마다 호출 됩니다.

메서드가 호출 될 때마다 동일한 컴파일된 쿼리를 호출 하는 컴파일된 쿼리를 정적 인스턴스를 만들어 원하는 대신 합니다. 한 가지 방법은 개체 컨텍스트의 멤버로 CompiledQuery 인스턴스를 추가 하 여 이기 때문입니다.  할 수 있습니다 다음 가지 작은 클리너를 CompiledQuery 도우미 메서드를 통해 액세스 하 여:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

이 도우미 메서드를 다음과 같이 호출할 수는:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 CompiledQuery를 통해 작성

모든 LINQ 쿼리를 작성 하는 기능은 매우 유용 합니다. 이렇게 하려면 단순히 메서드를 호출 하면 IQueryable 후와 같은 *skip ()* 하거나 *count ()* 합니다. 그러나 수행 하므로 기본적으로 새 IQueryable 개체를 반환 합니다. 기술적으로 CompiledQuery 위에 작성에서 중지 하는 것은, 이렇게 하면 새 IQueryable 개체를 생성 하는 계획 컴파일러를 통해 전달이 다시 필요 합니다.

일부 구성 요소 구성된 IQueryable을 이용 하 게 고급 기능을 사용 하는 개체입니다. 예를 들어 ASP 합니다. NET의 GridView 수 수 데이터 바인딩된 IQueryable 개체 SelectMethod 속성을 통해. GridView 정렬 및 데이터 모델에 대해 페이징 수 있도록이 IQueryable 개체에 대해 구성 합니다. 알 수 있듯이 gridview를 CompiledQuery를 사용 하 여 컴파일된 쿼리를 적중 하지는 하지만 새 autocompiled 쿼리를 생성 합니다.

고객 자문 팀이 해당 "잠재적 성능 문제 사용 하 여 컴파일된 LINQ 쿼리 다시 컴파일" 블로그 게시물에 설명 합니다. <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>합니다.

쿼리 점진적 필터를 추가 경우이를 실행할 수 있는 곳입니다. 예를 들어 선택적 필터 (예: 국가 및 OrdersCount)에 대 한 몇 가지 드롭 다운 목록 사용 하 여 고객에 게 페이지를 해야 합니다. CompiledQuery IQueryable 결과 통해 이러한 필터를 구성할 수 있습니다 하지만 발생할에서 새 쿼리를 실행 하 여 때마다 계획 컴파일러를 통해 이동 합니다.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 다시이 컴파일이이 방지 하려면 가능한 필터를 고려해 야 할 CompiledQuery 다시 작성할 수 있습니다.

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

같은 UI에서 호출 됩니다.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 여기 단점은 생성 된 저장소 명령은 항상 null 확인을 사용 하 여 필터는 있지만 최적화 하기 위해 데이터베이스 서버에 대 한 간단한 여야 합니다.

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 메타 데이터 캐싱

Entity Framework 메타 데이터 캐싱도 지원합니다. 이 오류는 기본적으로 형식 정보 및 데이터베이스에 형식 매핑 정보 동일한 모델에 다른 연결을 통해 캐시 됩니다. 메타 데이터 캐시는 AppDomain 별로 고유 합니다.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 메타 데이터 캐싱 알고리즘

1.  모델에 대 한 메타 데이터 정보는 각 EntityConnection에 대 한 ItemCollection에 저장 됩니다.
    -   참고, 모델의 다른 부분에 대 한 다른 ItemCollection 개체가 있습니다. StoreItemCollections는 데이터베이스 모델에 대 한 정보를 포함 하는 예를 들어, ObjectItemCollection은 데이터 모델에 대 한 정보를 포함합니다. EdmItemCollection 개념적 모델에 대 한 정보를 포함합니다.

2.  두 개의 연결이 동일한 연결 문자열을 사용 하는 경우 동일한 ItemCollection 인스턴스를 공유 합니다.
3.  기능적으로 동등 하지만 텍스트가 다른 연결 문자열은 다른 메타 데이터 캐시에서 될 수 있습니다. 연결 문자열을 단순히 공유 메타 데이터에서 발생 해야 토큰의 순서를 변경 토큰화 수행 했습니다. 하지만 동일한 기능적으로 보이는 두 개의 연결 문자열 평가할 수 없습니다 동일한 것으로 토큰화 후.
4.  ItemCollection 사용 하기 위해 주기적으로 확인 됩니다. 판단 되는 작업 영역에 액세스 하지 않은 최근에, 다음 캐시 비우기에 정리를 위해 표시 됩니다.
5.  EntityConnection을 만드는 것 만으로는 (항목 컬렉션에 연결이 열릴 때까지 초기화 되지 것입니다) 하지만 만들 메타 데이터 캐시를 하면 됩니다. 이 작업 영역 "에 사용 하 여" 없는 캐싱 알고리즘이 결정 될 때까지 메모리에 남아 있습니다.

고객 자문 팀 설명 큰 모델을 사용 하는 경우 "사용 중단"를 방지 하기 위해 ItemCollection에 대 한 참조를 보유 하는 블로그 게시물을 작성 했습니다: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>합니다.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 메타 데이터 캐싱 및 쿼리 계획 캐싱 간의 관계

쿼리 계획 캐시 인스턴스를 저장소 형식의 MetadataWorkspace의 ItemCollection에 상주합니다. 이 캐시 된 저장소 명령이 지정된 MetadataWorkspace를 사용 하 여 인스턴스화된 모든 컨텍스트에 대 한 쿼리는 것을 의미 합니다. 또한는 약간 다른 되며 토큰화 후 일치 하지 않습니다 하는 두 연결 문자열에 있는 경우 해야 다른 쿼리 계획 캐시 인스턴스를 의미 합니다.

### <a name="35-results-caching"></a>3.5 캐싱 결과

결과 캐싱 (라고도 "두 번째 수준 캐싱")를 사용 하 여 로컬 캐시에서 쿼리 결과 유지합니다. 쿼리를 발급 하는 경우 먼저 표시 경우 결과 저장소에 대해 쿼리 하기 전에 로컬로 사용할 수 있습니다. 캐싱 결과 Entity Framework에서 직접 지원 되지 않습니다, 하지만 래핑 공급자를 사용 하 여 보조 수준 캐시를 추가할 가능성이 있습니다. 두 번째 수준 캐시를 사용 하 여 예제 래핑 공급자를 Alachisoft의 됩니다 [NCache를 기반으로 Entity Framework 두 번째 수준 캐시](http://www.alachisoft.com/ncache/entity-framework.html)합니다.

두 번째 수준 캐싱이 이것은 LINQ 식을 평가한 후 위치 (및 funcletized)를 사용 하는 경우 삽입 된 기능 및 쿼리 실행 계획은 계산 또는 첫 번째 수준의 캐시에서 검색 합니다. 다음 두 번째 수준의 캐시는 materialization 파이프라인도 나중에 실행 되므로 원시 데이터베이스 결과만 저장 합니다.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 결과 래핑 공급자를 사용 하 여 캐싱 추가 참조

-   Julie Lerman Windows Server AppFabric caching을 사용 하도록 샘플 래핑 공급자를 업데이트 하는 방법을 포함 하는 "두 번째 수준 캐싱 Entity Framework 및 Windows Azure의" MSDN 문서를 작성 했습니다. [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   팀 블로그 Entity Framework 5에 대 한 캐싱 공급자와 함께 실행 하는 방법을 설명 하는 게시물에 Entity Framework 5를 사용 하 여 작업 하는 경우: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>합니다. 또한 프로젝트에 두 번째 수준 캐싱 추가 자동화 하기 위해 T4 템플릿을 포함 합니다.

## <a name="4-autocompiled-queries"></a>4 Autocompiled 쿼리

실제로 결과 구체화 하기 전에 일련의 단계를 통해 수행 해야 Entity Framework를 사용 하 여 데이터베이스에 대해 쿼리를 실행 하는 경우 이러한 단계를 하나에 쿼리 컴파일입니다. Entity SQL 쿼리 된 있으므로 좋은 성능을 자동으로 캐시 되는 대로 계획 컴파일러를 건너뛸 수 고 대신 캐시 된 계획을 사용 하 여 동일한 쿼리를 실행 하면 두 번째 또는 세 번째에 알려져 있습니다.

Entity Framework 5 to Entities 쿼리에서 LINQ에 대 한 자동 캐싱을 도입 했습니다. 속도 높이려면 CompiledQuery 만들기 Entity Framework의 이전 버전에서 성능 처럼는 일반적이 없게 LINQ to Entities 쿼리의 캐시할 수 있습니다. 캐싱 이제 자동 CompiledQuery 사용 하지 않고, 하므로이 기능은 "autocompiled 쿼리" 라고 합니다. 쿼리 계획 캐시 및 해당 메커니즘에 대 한 자세한 내용은 캐시 된 쿼리 계획을 참조 하세요.

Entity Framework 쿼리 다시 컴파일되도록 해야 하는 경우를 검색 및 쿼리 하기 전에 컴파일된 경우에 호출 될 때 작업을 수행 합니다. 일반적인 쿼리를 다시 컴파일할 수는 조건은 다음과 같습니다.

-   쿼리에 연결 된 MergeOption를 변경 합니다. 캐시 된 쿼리를 사용 하지 않습니다 하 고 계획 컴파일러를 다시 실행 됩니다 대신 새로 만든된 계획 캐시를 가져옵니다.
-   ContextOptions.UseCSharpNullComparisonBehavior의 값을 변경 합니다. 표시 된 MergeOption 변경 하는 것과 같습니다.

기타 조건에서 캐시를 사용 하 여 쿼리를 방지할 수 있습니다. 일반적인 예로:

-   IEnumerable을 사용 하 여&lt;T&gt;합니다. 포함&lt;&gt;(T 값).
-   상수를 사용 하 여 쿼리를 생성 하는 함수를 사용 합니다.
-   비 매핑된 개체의 속성을 사용합니다.
-   컴파일해야 하는 데 필요한 다른 쿼리를 쿼리를 연결 합니다.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 IEnumerable을 사용 하 여&lt;T&gt;합니다. 포함&lt;T&gt;(T 값)

Entity Framework는 IEnumerable을 호출 하는 쿼리를 캐시 하지 않습니다&lt;T&gt;합니다. 포함&lt;T&gt;(T 값) 컬렉션의 값은 일시적으로 간주 되므로 메모리 내 컬렉션에 대 한 합니다. 다음 예제 쿼리에서 하지 캐시 되므로 항상 처리 하는 계획 컴파일러에서:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

결정 하는 크기를 포함 하는 대 한 IEnumerable의 실행 속도가 빠르거나 어떻게 쿼리 참고가 컴파일됩니다. 위의 예제에 표시 된 것과 같은 대규모 컬렉션을 사용 하는 경우 성능이 크게 떨어질 수 있습니다.

Entity Framework 6 포함 최적화 방법 IEnumerable&lt;T&gt;합니다. 포함&lt;T&gt;(T 값) 쿼리를 실행 하는 경우에 작동 합니다. 생성 되는 SQL 코드를 훨씬 빠르게 생성 및 보다 읽기 쉬운 이며 대부분의 경우에도 더 빠르게 실행 서버에 있습니다.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 상수를 사용 하 여 쿼리를 생성 하는 함수를 사용 합니다.

Skip () "," take () "," contains () "및" DefautIfEmpty() LINQ 연산자는 매개 변수를 사용 하 여 SQL 쿼리를 생성 하지 않지만 대신 상수에 전달 된 값을 입력 합니다. 이 때문에 동일한 쿼리를 오염 시 키 지 종료 될 수 있는 쿼리 EF 스택에 데이터베이스 서버에서 캐시를 계획 하 고 동일한 상수 후속 쿼리 실행에 사용 되지 않는 한 reutilized 하지 않습니다. 예를 들어:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

이 예제에서는이 쿼리는 쿼리 id에 대 한 다른 값을 사용 하 여 실행 될 때마다 새 계획으로 컴파일됩니다.

Skip 및 Take 페이징을 수행 하는 경우 사용 하 여 특정 주의 합니다. EF6에서 이러한 메서드는 효과적으로 캐시 된 쿼리 계획을 재사용 가능한 수 있기 때문에 EF를 이러한 메서드에 전달 된 변수를 캡처 Sqlparameter로 변환 하는 람다 오버를 로드가 있습니다. 그러면 다른 상수로 Skip 및 Take를 사용 하 여 각 쿼리 자체 쿼리 계획 캐시 항목을 가져오기는 그렇지 않은 경우 때문 캐시를 더 깔끔하게 유지 합니다.

최적이 아닌 이지만이 클래스의 쿼리를 보여 주기 위해 알리기만 하는 다음 코드를 살펴보세요.

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

이 동일한 코드의 더 빠른 버전을 포함 하는 람다를 사용 하 여 Skip을 호출:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

두 번째 조각은 CPU 시간을 저장 하 고 쿼리 캐시를 오염 방지 하는 쿼리를 실행할 때마다 동일한 쿼리 계획 사용 되기 때문에 최대 11% 더 빠르게 실행할 수 있습니다. 또한 클로저에서 건너뛰기로 매개 변수 이므로 코드도 같습니다이 이제.

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 아닌 매핑된 개체의 속성을 사용 하 여

경우는 쿼리 매개 변수 다음 쿼리는 캐시 되지과 비 매핑된 개체 유형의 속성을를 사용 합니다. 예를 들어:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

이 예제에서는 클래스 NonMappedType 엔터티 모델의 일부가 아닙니다를 가정 합니다. 이 쿼리를 하지 않은 매핑된 형식을 사용 하 여 대신 로컬 변수를 사용 하 여 쿼리를 매개 변수로 쉽게 변경할 수 있습니다.

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

이 경우 캐시 수 쿼리와 쿼리 계획 캐시에서 이익을 합니다.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 다시 컴파일하지 필요로 하는 쿼리가에 연결

위와 동일한 예제를 다시 컴파일되도록 해야 하는 쿼리를 사용 하는 두 번째 쿼리를 해야 하는 경우 전체 두 번째 쿼리도 다시 컴파일됩니다. 이 시나리오를 설명 하는 예는 다음과 같습니다.

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

이 예제에서는 제네릭 메서드이지만 어떻게 firstQuery에 연결 되도록 secondQuery 캐시 수를 보여 줍니다. FirstQuery 했습니다 되지 된 경우 다시 컴파일하지 필요로 하는 쿼리를 다음 secondQuery는 캐시 된 경우.

## <a name="5-notracking-queries"></a>NoTracking 쿼리 5

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 변경 내용 추적을 상태 관리 오버 헤드를 줄이고 사용 하지 않도록 설정

읽기 전용 시나리오에는 ObjectStateManager에 개체를 로드 하는 오버 헤드를 방지 하려면 하는 경우 "No 추적" 쿼리를 실행할 수 있습니다.  쿼리 수준에서 변경 내용 추적을 해제할 수 있습니다.

그러나는 변경 추적을 사용 하지 않도록 설정 하 여 효과적으로 끄는 개체 캐시 note 합니다. 엔터티를 쿼리 하는 경우 materialization ObjectStateManager를 이전에 구체화 된 쿼리 결과 풀링 하 여 건너뛸 수 없습니다 했습니다. 동일한 컨텍스트에서 동일한 엔터티에 대 한 반복적으로 쿼리 하는 경우 성능 변경 내용 추적 사용의 이점이 실제로 표시 될 수 있습니다.

ObjectContext를 사용 하 여 쿼리 하는 경우에 ObjectQuery 및 ObjectSet 인스턴스에 설정 된 후에 구성 된 쿼리는 상위 쿼리의 효과적인 MergeOption 상속 된 MergeOption에 저장 됩니다. DbContext를 사용 하는 경우 DbSet에서 AsNoTracking() 한정자를 호출 하 여 추적을 해제할 수 있습니다.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1에 변경 내용 추적 쿼리 DbContext를 사용 하는 경우 사용 하지 않도록 설정

NoTracking를 쿼리에서 AsNoTracking() 메서드에 대 한 호출을 연결 하 여 쿼리 모드를 전환할 수 있습니다. ObjectQuery를 달리 DbContext api에서는 DbSet 및 DbQuery 클래스는 MergeOption에 대 한 속성을 변경할 수 없습니다.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 ObjectContext를 사용 하 여 쿼리 수준에서 변경 내용 추적을 사용 하지 않도록 설정

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 전체 엔터티 ObjectContext를 사용 하 여 집합에 변경 내용 추적을 사용 하지 않도록 설정

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 NoTracking 쿼리의 성능 이점을 보여 주는 테스트 메트릭

이 테스트는 ObjectStateManager Navision 모델에 대 한 NoTracking 쿼리에 대 한 추적을 비교 하 여 입력 하는 대신 살펴봅니다. Navision 모델과 실행 된 쿼리의 종류에 대 한 부록을 참조 하세요. 이 테스트에서는 쿼리 목록을 반복 하 고 각각 두 번 실행 합니다. "AppendOnly"의 기본 병합 옵션을 사용 하 여 테스트, NoTracking 쿼리를 사용 하 여 한 번 및 한 번의 두 가지 변형이 발생 했습니다. 각 변형 3 번 실행 하 고 실행의 평균 값을 사용 합니다. 테스트 간에 SQL Server에서 쿼리 캐시의 선택을 취소 하 고 다음 명령을 실행 하 여 tempdb를 축소:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

테스트 결과, 중앙값 3 개 실행:

|                        | 없는 추적-작업 집합 | 추적 안 함 – 시간 | 작업 집합에만 추가 | 유일한 – 시간 추가 |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5에는 메모리를 적게 실행의 끝에 Entity Framework 6 보다 해야 합니다. Entity Framework 6에서 사용 하는 추가 메모리에는 추가 메모리 구조 및 새로운 기능 및 더 나은 성능을 사용할 수 있는 코드의 결과입니다.

이기도 메모리 사용 공간에서 분명 한 차이가 ObjectStateManager를 사용 하는 경우. Entity Framework 5는 데이터베이스에서 구체화에서는 모든 엔터티는 추적 하는 경우 해당 공간을 30% 증가 합니다. Entity Framework 6 이렇게 하면 해당 공간 28% 증가 합니다.

시간을 기준으로 Entity Framework 6 보다 큰 여백으로이 테스트에서 Entity Framework 5 뛰어난 성능을 냅니다. Entity Framework 6 거의 16%의 시간 동안 사용 된 Entity Framework 5의 테스트를 완료 합니다. 또한 Entity Framework 5는 ObjectStateManager를 사용 하는 경우 완료 9% 더 많은 시간이 걸립니다. 반면에서 Entity Framework 6에는 3 %ObjectStateManager 사용 하는 경우에 더 많은 시간을 사용 합니다.

## <a name="6-query-execution-options"></a>6 쿼리 실행 옵션

Entity Framework는 여러 가지 방법으로 쿼리를 제공 합니다. 다음 옵션을 살펴보고, 각각의 장단점을 비교 하 고 성능 특징을 검토 드리겠습니다.

-   LINQ to Entities 합니다.
-   추적 안 함 LINQ to Entities 합니다.
-   ObjectQuery 통해 SQL 엔터티입니다.
-   EntityCommand 통해 SQL 엔터티입니다.
-   ExecuteStoreQuery 합니다.
-   SqlQuery 합니다.
-   CompiledQuery 합니다.

### <a name="61-linq-to-entities-queries"></a>6.1 LINQ to Entities 쿼리

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**전문가**

-   CUD 작업에 적합 합니다.
-   완전히 구체화 된 개체입니다.
-   구문을 사용 하 여 작성 하는 가장 간단한 프로그래밍 언어에 빌드됩니다.
-   좋은 성능을 합니다.

**단점**

-   특정 기술 제한 같은:
    -   Entity SQL에서 간단한 OUTER JOIN 문 보다 더 복잡 한 쿼리 패턴 DefaultIfEmpty OUTER JOIN 쿼리를 사용 하 여 발생 합니다.
    -   여전히 사용할 수 없는 일반 패턴 일치와 유사 합니다.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6.2 추적 안 함 LINQ to Entities 쿼리에서

경우 컨텍스트는 ObjectContext 파생 됩니다.

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

경우 컨텍스트는 DbContext 파생 됩니다.

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**전문가**

-   일반 LINQ 쿼리를 통해 성능이 향상 되었습니다.
-   완전히 구체화 된 개체입니다.
-   구문을 사용 하 여 작성 하는 가장 간단한 프로그래밍 언어에 빌드됩니다.

**단점**

-   CUD 작업에 적합 하지 않습니다.
-   특정 기술 제한 같은:
    -   Entity SQL에서 간단한 OUTER JOIN 문 보다 더 복잡 한 쿼리 패턴 DefaultIfEmpty OUTER JOIN 쿼리를 사용 하 여 발생 합니다.
    -   여전히 사용할 수 없는 일반 패턴 일치와 유사 합니다.

Note는 NoTracking 지정 되지 않은 경우에 스칼라 속성을 프로젝션 하는 쿼리 추적 되지 않습니다. 예를 들어:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

이 특정 쿼리 NoTracking, 되 고 명시적으로 지정 하지 않습니다 하지만를 구체화 하지는 때문으로 알려진 개체 상태 관리자 다음 구체화 된 결과 형식 추적 되지 않습니다.

### <a name="63-entity-sql-over-an-objectquery"></a>6.3 엔터티는 ObjectQuery 상의 SQL

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**전문가**

-   CUD 작업에 적합 합니다.
-   완전히 구체화 된 개체입니다.
-   지 원하는 쿼리 계획 캐싱입니다.

**단점**

-   언어에 빌드되는 쿼리 구문을 보다 사용자 오류 발생 가능성이 더는 텍스트 기반 쿼리 문자열을 포함 합니다.

### <a name="64-entity-sql-over-an-entity-command"></a>6.4 엔터티는 엔터티 명령을 통해 SQL

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**전문가**

-   지원 (.NET 4.5에서 다른 모든 쿼리 형식에서 지원 계획 캐싱).NET 4.0에 캐시 된 계획을 쿼리 합니다.

**단점**

-   언어에 빌드되는 쿼리 구문을 보다 사용자 오류 발생 가능성이 더는 텍스트 기반 쿼리 문자열을 포함 합니다.
-   CUD 작업에 적합 하지 않습니다.
-   결과 자동으로 구체화 되지 않은 및 데이터 판독기에서 읽을 수 있어야 합니다.

### <a name="65-sqlquery-and-executestorequery"></a>6.5 SqlQuery 및 ExecuteStoreQuery

데이터베이스에서 SqlQuery:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

DbSet에 SqlQuery:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**전문가**

-   일반적으로 가장 빠른 성능 계획 컴파일러는 무시 되므로입니다.
-   완전히 구체화 된 개체입니다.
-   DbSet에서 사용 될 때 CUD 작업에 적합 합니다.

**단점**

-   쿼리 텍스트는 오류가 발생 하기 쉽습니다.
-   쿼리는 저장소 의미 체계가 개념적 구문을 대신 사용 하 여 특정 백 엔드에 연결 됩니다.
-   상속 존재 하는 경우 기존된 쿼리를 요청 된 형식에 대 한 매핑 조건에 대 한 계정 해야 합니다.

### <a name="66-compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**전문가**

-   일반 LINQ 쿼리를 통해 최대 7% 성능 향상을 제공합니다.
-   완전히 구체화 된 개체입니다.
-   CUD 작업에 적합 합니다.

**단점**

-   복잡성과 오버 헤드를 프로그래밍 증가 합니다.
-   컴파일된 쿼리를 기반으로 작성 하는 경우 성능 향상 손실 됩니다.
-   예를 들어, 익명 형식 프로젝션 CompiledQuery-으로 일부 LINQ 쿼리를 쓸 수 없습니다.

### <a name="67-performance-comparison-of-different-query-options"></a>6.7 다른 쿼리 옵션의 성능 비교

테스트에 컨텍스트를 만드는 적합 하지 않습니다 하는 간단한 microbenchmarks 배치한 것입니다. 5,000 번 통제 된 환경에서 캐시 되지 않은 엔터티 집합에 대 한 쿼리를 측정 했습니다. 이러한 숫자는 경고와 함께 수행할: 응용 프로그램에 의해 생성 된 실제 숫자를 반영 하지 않습니다 하지만 대신 얼마나는 다른 쿼리 옵션을 비교 하면 성능 차이 매우 정확 하 게 측정 사과-에-사과, 새 컨텍스트를 만드는 비용을 제외 합니다.

| EF  | 테스트                                 | 시간 (ms) | 메모리   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | ObjectContext Linq 쿼리             | 2692      | 38277120 |
| EF5 | DbContext Linq 쿼리 추적 안 함     | 2818      | 41840640 |
| EF5 | DbContext Linq 쿼리                 | 2930      | 41771008 |
| EF5 | ObjectContext Linq 쿼리 추적 안 함 | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | ObjectContext Linq 쿼리             | 3074      | 45248512 |
| EF6 | DbContext Linq 쿼리 추적 안 함     | 3125      | 47575040 |
| EF6 | DbContext Linq 쿼리                 | 3420      | 47652864 |
| EF6 | ObjectContext Linq 쿼리 추적 안 함 | 3593      | 45260800 |

![EF5 마이크로 벤치 마크를 5000 웜 반복](~/ef6/media/ef5micro5000warm.png)

![EF6 마이크로 벤치 마크를 5000 웜 반복](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks은 코드의 작은 변화에 매우 민감합니다. 추가 인해 Entity Framework 5의 비용 및 Entity Framework 6의 차이점은이 예에서 [인터 셉 션](~/ef6/fundamentals/logging-and-interception.md) 하 고 [트랜잭션 향상](~/ef6/saving/transactions.md)합니다. 그러나 이러한 microbenchmarks 숫자 Entity Framework 수행 하는 매우 작은 조각으로 증폭 되는 시각을 됩니다. Entity Framework 6에서 Entity Framework 5로 업그레이드 하는 경우 실제 시나리오 웜 쿼리의 성능 회귀를 나타나지 않습니다.

다른 쿼리 옵션의 실제 성능을 비교할 위치를 사용 하 여 다른 쿼리 옵션 범주 이름이 "음료수" 모든 제품을 선택 하는 5 별도 테스트 변형이 만들었습니다. 각 반복에는 컨텍스트를 만드는 비용 및 반환 된 모든 엔터티 구체화 비용이 포함 됩니다. 10 번 반복 시간이 1000 회 반복의 합계를 수행 하기 전에 무제한 실행 됩니다. 각 테스트 실행이 5에서 가져온 중앙값 실행 되는 결과입니다. 자세한 내용은 부록 B 테스트에 대 한 코드를 포함 하는 참조 하세요.

| EF  | 테스트                                        | 시간 (ms) | 메모리   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext 엔터티 명령                | 621       | 39350272 |
| EF5 | 데이터베이스에서 DbContext Sql 쿼리             | 825       | 37519360 |
| EF5 | ObjectContext 저장소 쿼리                   | 878       | 39460864 |
| EF5 | ObjectContext Linq 쿼리 추적 안 함        | 969       | 38293504 |
| EF5 | 개체 쿼리를 사용 하 여 ObjectContext Entity Sql | 1089      | 38981632 |
| EF5 | 컴파일된 쿼리에서 ObjectContext                | 1099      | 38682624 |
| EF5 | ObjectContext Linq 쿼리                    | 1152      | 38178816 |
| EF5 | DbContext Linq 쿼리 추적 안 함            | 1208      | 41803776 |
| EF5 | DbSet에 DbContext Sql 쿼리                | 1414      | 37982208 |
| EF5 | DbContext Linq 쿼리                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext 엔터티 명령                | 480       | 47247360 |
| EF6 | ObjectContext 저장소 쿼리                   | 493       | 46739456 |
| EF6 | 데이터베이스에서 DbContext Sql 쿼리             | 614       | 41607168 |
| EF6 | ObjectContext Linq 쿼리 추적 안 함        | 684       | 46333952 |
| EF6 | 개체 쿼리를 사용 하 여 ObjectContext Entity Sql | 767       | 48865280 |
| EF6 | 컴파일된 쿼리에서 ObjectContext                | 788       | 48467968 |
| EF6 | DbContext Linq 쿼리 추적 안 함            | 878       | 47554560 |
| EF6 | ObjectContext Linq 쿼리                    | 953       | 47632384 |
| EF6 | DbSet에 DbContext Sql 쿼리                | 1023      | 41992192 |
| EF6 | DbContext Linq 쿼리                        | 1290      | 47529984 |


![EF5 웜 쿼리 1000 반복](~/ef6/media/ef5warmquery1000.png)

![EF6 웜 쿼리 1000 반복](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> 완성도 위해는 EntityCommand에 Entity SQL 쿼리를 실행 했습니다 변형 포함 되어 있습니다. 그러나 이러한 쿼리의 결과 구체화 되지 않은, 때문에 비교 반드시 사과-사과입니다. 테스트 비교 공평한 만들어 볼 구체화 대 한 근사치를 포함 합니다.

이 종단 간 경우 Entity Framework 6 보다 성능이 뛰어납니다 Entity Framework 5를 포함 하는 스택의 훨씬 더 밝게 DbContext 초기화 및 빠른 MetadataCollection 여러 부분에 대 한 성능 향상으로 인해&lt;T&gt; 조회 합니다.

## <a name="7-design-time-performance-considerations"></a>7 디자인 시간 성능 고려 사항

### <a name="71-inheritance-strategies"></a>7.1 상속 전략

Entity Framework를 사용 하는 경우 다른 성능 고려 사항에 사용할 상속 전략입니다. Entity Framework에는 3 가지 기본 유형의 상속과 조합을 지원합니다.

-   테이블 당 계층 TPH () – 각 상속 계층 구조에서 특정 형식을 나타내는 판별자 열이 있는 테이블에 지도 설정 하는 위치는 행에 표시 되 고 됩니다.
-   테이블 당 형식 TPT ()-데이터베이스에서 각 유형에 고유한 테이블에 있는 자식 테이블만 부모 테이블을 포함 하지 않는 열을 정의 합니다.
-   테이블 당 클래스 TPC ()-데이터베이스에서 각 유형에 고유한 전체 테이블에 있는 자식 테이블에는 부모 형식에 정의 된 것을 비롯 한 모든 필드를 정의 합니다.

TPT 상속을 사용 하는 모델에서 생성 되는 쿼리를 다른 상속 전략을 저장소에서 긴 실행 시간에 발생할 수 있습니다를 사용 하 여 생성 되는 것 보다 더 복잡 한 됩니다.  일반적으로 TPT 모델에 대해 쿼리를 생성 하 고 결과 개체를 구체화 하려면 시간이 오래 걸립니다.

"성능 고려 사항을 참조 Entity Framework에서 TPT (형식당 하나의 테이블) 상속을 사용 하는 경우" MSDN 블로그 게시: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>합니다.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 Model First 또는 Code First 응용 프로그램에서 TPT 방지

TPT 스키마가 있는 기존 데이터베이스 모델을 만들 때 다양 한 옵션 않아도 됩니다. 하지만 성능 문제에 대 한 TPT 상속 Model First 또는 Code First를 사용 하 여 응용 프로그램을 만들 때 피해 야 합니다.

엔터티 디자이너 마법사의 첫 번째 모델을 사용 하는 경우 모델의 모든 상속 TPT를 발생 합니다. Model First를 사용 하 여 TPH 상속 전략으로 전환 하려는 경우 Visual Studio 갤러리에서 "엔터티 디자이너 데이터베이스 생성 전원 팩" 사용 가능한를 사용할 수 있습니다 ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>)합니다.

Code First 방법으로 상속을 사용 하 여 모델의 매핑을 구성 하는 경우 EF는 기본적으로 TPH를 사용 하는, 않아 상속 계층의 모든 엔터티에 동일한 테이블에 매핑할 수 됩니다. MSDN Magazine의 "코드 엔터티 Framework4.1에서 첫 번째." 문서 "매핑 Fluent API를 사용한" 섹션을 참조 하세요 ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) 대 한 자세한 내용은 합니다.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 모델 생성을 개선 하기 위해 EF4에서 업그레이드 시간

모델의 저장소 계층 (SSDL)를 생성 하는 알고리즘에는 SQL Server 관련 개선은 Visual Studio 2010 SP1을 설치할 때 Entity Framework 5 및 6에서 Entity Framework 4 업데이트로 제공 됩니다. 다음 테스트 결과 Navision 모델 경우이 매우 큰 모델을 생성 하는 경우 향상을 보여 줍니다. 부록 C에 대 한 자세한 내용은 참조 하십시오.

1005 엔터티 집합과 연결 집합이 4227 모델에 포함 되어 있습니다.

| 구성                              | 사용 된 시간의 분석                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010에서는 Entity Framework 4     | SSDL 생성: 2 시간 27 분 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> 1 초 ObjectLayer 생성: <br/> 뷰 생성: 2 h 14 분 |
| Visual Studio 2010 SP1, Entity Framework 4 | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> 1 초 ObjectLayer 생성: <br/> 뷰 생성: 1 시간 53 분   |
| Visual Studio 2013, Entity Framework 5     | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> 1 초 ObjectLayer 생성: <br/> 뷰 생성: 65 분    |
| Visual Studio 2013, Entity Framework 6     | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> 1 초 ObjectLayer 생성: <br/> 뷰 생성: 28 초입니다.   |


주목할 만한 가치가 SSDL을 생성 하는 경우 부하는 거의 완전히 소비 SQL Server 클라이언트 개발 컴퓨터 대기 하는 동안 서버에서 돌아와서 결과 대 한 유휴 상태입니다. Dba는이 개선 주셔서 감사 드리며 특히 해야 합니다. 모델 생성의 전체 비용에 기본적으로 뷰 생성에서 이제는 주목할 만한 이기도 합니다.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7.3 데이터베이스를 사용 하 여 큰 모델을 먼저 분할 및 Model First

모델 크기 증가, 디자이너 화면에 복잡 하 고 사용 하기 어려운 됩니다. 일반적으로 효과적으로 디자이너를 사용 하는 너무 큰 300 개 이상의 엔터티를 사용 하 여 모델을 살펴봅니다. 큰 모델을 분할 하기 위한 여러 옵션을 설명 하는 다음 블로그 게시물: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>합니다.

Entity Framework의 첫 번째 버전에 대 한 게시물 쓴 있지만 단계는 여전히 적용 됩니다.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7.4 엔터티 데이터 소스 컨트롤을 사용 하 여 성능 고려 사항

EntityDataSource 컨트롤을 사용 하 여 웹 응용 프로그램의 성능을 크게 저하 다중 스레드 성능 및 스트레스 테스트 사례 확인 되었습니다. 기본 원인은 EntityDataSource 반복적으로 호출 하는 MetadataWorkspace.LoadFromAssembly 엔터티로 사용할 형식을 검색 하는 웹 응용 프로그램에서 참조 하는 어셈블리에서입니다.

솔루션 파생 된 ObjectContext 클래스의 형식 이름에는 EntityDataSource의 ContextTypeName를 설정 하는 것입니다. 이 엔터티 형식에 대 한 모든 참조 된 어셈블리를 검색 하는 메커니즘을 해제 합니다.

또한 ContextTypeName 필드 설정 리플렉션을 통해 어셈블리에서 형식을 로드할 수 없습니다 하는 경우.NET 4.0에서 EntityDataSource ReflectionTypeLoadException을 throw 하는 위치는 기능적 문제가 발생을 하지 않습니다. 이 문제는.NET 4.5에서 수정 되었습니다.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7.5 POCO 엔터티와 변경 내용 추적 프록시

Entity Framework를 사용 하면 데이터 클래스 자체를 수정 하지 않고 데이터 모델과 함께 사용자 지정 데이터 클래스를 사용할 수 있습니다. 즉, 기존 도메인 개체 등의 POCO(Plain Old CLR Object)를 데이터 모델과 함께 사용할 수 있습니다. 데이터 모델에서 정의 된 엔터티에 매핑되는 이러한 POCO 데이터 클래스 (지 속성 무시 개체 라고도), 대부분의 동일한 쿼리를 지 원하는, 삽입, 업데이트 및 삭제 동작 엔터티 데이터 모델 도구에서 생성 되는 엔터티 형식으로 합니다.

Entity Framework에서 POCO 엔터티의 추적 변경 내용을 자동 지연 로딩 등의 기능을 사용 하도록 설정할 때 사용 되는 POCO 형식에서 파생 된 프록시 클래스를 만들 수도 있습니다. POCO 클래스에는 여기에 설명 된 대로 Entity Framework 프록시를 사용할 수 있도록 특정 요구 사항을 충족 해야 합니다. [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx)합니다.

기회 추적 프록시는 엔터티의 속성에 해당 값이 변경, Entity Framework 엔터티의 실제 상태를 항상 알 수 있도록 때마다 개체 상태 관리자를 알림 나타납니다. 이렇게 하려면 속성의 setter 메서드 본문에 알림 이벤트를 추가 하 고 이러한 이벤트를 처리 하는 개체 상태 관리자입니다. 프록시 만들기 엔터티는 일반적으로 Entity Framework에서 생성 되는 이벤트의 추가 집합으로 인해 프록시가 아닌 POCO 엔터티를 만드는 것 보다 더 비쌉니다.

POCO 엔터티가 프록시 추적 변경 되지 않은, 경우 이전 저장 된 상태의 복사본에 대해 엔터티의 내용을 비교 하 여 변경 내용은 찾습니다. 이 심층 비교 하나도 발생 마지막 비교 이후를 변경 하는 경우에 많은 엔터티 컨텍스트에 있는 경우 또는 엔터티 속성을 매우 많은 양의 경우 시간이 오래 걸리는 프로세스가 됩니다.

요약에서: 성능이 수동 프록시를 만들 때 저하을 지불 하 게 되지만 변경 내용 추적에 엔터티 대부분의 속성 또는 설치한 경우 여러 엔터티 모델의 경우 변경 감지 프로세스의 속도. 소수의 없는 엔터티의 크기 증가 하지 너무 많은 속성을 사용 하 여 엔터티를 변경 내용 추적 프록시가 있는 되지 않을 수 있습니다 훨씬 혜택.

## <a name="8-loading-related-entities"></a>8 로드 관련 엔터티

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 지연 로드 및 즉시 로드

Entity Framework를 대상 엔터티에 관련 된 엔터티를 로드 하는 여러 가지 방법으로 제공 합니다. 예를 들어, 제품에 대 한 데이터를 쿼리할 때 여러 가지 관련된 주문이 로드 될 개체 상태 관리자에 있습니다. 성능 관점에서 관련된 엔터티를 로드할 때 고려해 야 할 가장 큰 질문을 지연 로드 또는 즉시 로드를 사용할지 여부를 수 있습니다.

즉시 로드를 사용 하는 경우 대상 엔터티 집합에 함께 관련된 엔터티가 로드 됩니다. 사용 하 여는 Include 문을 쿼리에 관련 가져오려는 엔터티를 나타냅니다.

지연 로드를 사용 하는 경우 초기 쿼리는 대상 엔터티 집합에만 제공 합니다. 하지만 저장소 관련된 엔터티가 로드에 대해 다른 쿼리는 탐색 속성에 액세스 될 때마다 발생 합니다.

엔터티 로드 된 후 엔터티에 대 한 추가 쿼리는에서 직접 로드 하는 개체 상태 관리자가 지연 로딩 또는 즉시 로드를 사용 하 여.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 지연 로드 및 즉시 로드 중에서 선택 하는 방법

중요 한 사실은 응용 프로그램에 대 한 적합을 내릴 수 있도록 지연 로드 및 즉시 로드 간의 차이점을 이해 하는 것입니다. 대용량 페이로드를 포함할 수 있는 단일 요청 및 데이터베이스에 대 한 여러 요청 간의 관계를 평가 하는 데 도움이 됩니다. 및 사용 하 여 응용 프로그램의 일부에서 선행 로딩과 지연 로딩 다른 부분에 적합할 수 있습니다.

내부에서 발생 하는 예를 들어, 주문 수 및 영국에 거주 하는 고객에 대 한 쿼리 하려는 경우를 가정 합니다.

**즉시 로드를 사용 하 여**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**지연 로드를 사용 하 여**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

모든 고객을 반환 하는 단일 쿼리를 실행 하겠습니다 즉시 로드를 사용할 때와 모든 주문을 합니다. 저장소 명령은 다음과 같습니다.

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

지연 로드를 사용 하는 경우 처음에 다음 쿼리를 실행할 수 있습니다.

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

및 고객의 주문을 탐색 속성에 액세스할 때마다 다음과 같은 다른 쿼리 저장소에 대해 발생 합니다.

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

자세한 내용은 참조는 [관련 개체 로드](https://msdn.microsoft.com/library/bb896272.aspx)합니다.

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 지연 된 로드 및 즉시 로드 치트 시트

있습니다 것은 없습니다를 절대적으로 지연 로드 및 즉시 로드를 선택 합니다. 잘 합리적인된 의사 결정을 수행할 수 있도록 두 전략 간의 차이점을 이해 하려면 먼저를 시도 하세요. 또한 코드는 다음과 같은 시나리오 중 하나에 맞는지 하는 것이 좋습니다.

| 시나리오                                                                    | 이 제안                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 페치된 엔터티의 여러 탐색 속성에 액세스 해야 합니까? | **아니요** -두 옵션은 아마도 사용할 것입니다. 그러나 쿼리에서 가져오는 페이로드가 너무 작은 필요할 것으로 즉시 로드를 사용 하 여 성능상의 이점을 발생할 경우 네트워크 왕복 개체를 구체화 합니다. <br/> <br/> **예** -엔터티에서 탐색 속성에 액세스 해야 할 경우 수행한 여러 개 사용 하면 포함 문을 쿼리에 즉시 로드를 사용 하 여 합니다. 포함 하는 더 많은 엔터티, 클수록 페이로드 쿼리 반환 됩니다. 쿼리를 세 개 이상의 엔터티를 포함 하 되 면 지연 로드를 전환 하는 것이 좋습니다. |
| 런타임 시 데이터 필요할지 정확히 알고 있습니까?                   | **아니요** -지연 로드를 개선 됩니다. 그렇지 않은 경우 필요 하지는 데이터에 대 한 쿼리 될 수 있습니다. <br/> <br/> **예** -즉시 로드는 가장 좋은; 전체 집합을 더 빠르게 로드 하도록 도와줍니다. 쿼리에 필요한 매우 많은 양의 데이터를 인출 하는 경우이 너무 느리면 지연 로드를 대신 시도 합니다.                                                                                                                                                                                                                                                       |
| 데이터베이스 멀리 코드를 실행 하 시겠습니까? (향상 된 네트워크 대기 시간)  | **이상** 네트워크 대기 시간 문제가 없는 경우-지연 로드를 사용 하 여 코드를 단순화 될 수 있습니다. 응용 프로그램의 토폴로지 변경 될 수 있음을, 부여에 대 한 데이터베이스 근접을 사용 하지 있도록 해야 합니다. <br/> <br/> **예** -네트워크 전용인 문제가 시나리오에 잘 맞는 것을 결정할 수 있습니다. 일반적으로 더 적은 왕복 필요 하기 때문에 선행 로딩은 향상 됩니다.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 성능 문제가 여러 포함

서버 응답 시간 문제를 포함 하는 성능 관련 질문을 제기 하는 경우 문제에 대 한 원본은 자주 쿼리와 여러 Include 문입니다. 쿼리에서 관련된 엔터티를 비롯 한 강력한 상태인 동안에 내부적으로 발생 하는 상황을 이해 해야 합니다.

저장소 명령을 생성 하기 위해 내부 계획 컴파일러를 통해 이동 하 여 여러 Include 문 사용 하 여 쿼리에 대 한 비교적 오랜 시간이 걸립니다. 이 시간의 대부분은 결과 쿼리를 최적화 하는 동안 소요 됩니다. 생성 된 저장소 명령은 매핑에 따라 각 포함에 대 한 Outer Join 또는 Union 포함 됩니다. 이와 같은 쿼리 (예를 들어, 이동할의 여러 수준을 사용 하는 경우 페이로드의 중복을 많이 필요한 경우에 특히 대역폭 문제가 acerbate는 단일 페이로드에서 데이터베이스에서 큰 연결 된 그래프에 표시 됩니다. 연결에 일 대 다 방향으로).

쿼리 ToTraceString 사용 및 페이로드 크기를 확인 하려면 SQL Server Management Studio에서 저장소 명령을 실행 하 여 쿼리에 대 한 기본 TSQL에 액세스 하 여 과도 하 게 큰 페이로드를 반환 되는 경우 확인할 수 있습니다. 바로 쿼리에서 Include 문 수를 줄일 하려고 해도 이러한 경우 필요한 데이터를 가져옵니다. 또는 예를 들어 작은 하위 시퀀스로 쿼리를 중단할 수 있습니다.

**시작 하기 전에 쿼리:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**다음 쿼리를 중단 합니다.**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

진행 중인으로 추적 된 쿼리 에서만 작동 합니다이 컨텍스트 identity 확인 및 연결 수정을 자동으로 수행 해야 하는 기능을 활용 합니다.

지연 로드와 마찬가지로 단점이 페이로드가 작을수록에 대 한 더 많은 쿼리 됩니다. 명시적으로 각 엔터티에서 필요한 데이터만 선택 하도록 프로젝션 개별 속성을 사용할 수도 있습니다 하지만 하지를 로드 합니다 엔터티 이때 및 업데이트가 지원 되지 않습니다.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 속성을 지연 로드 하기 위한 해결 방법

현재 entity Framework는 스칼라 또는 복합 속성을 지연 로드를 지원 하지 않습니다. 그러나 BLOB와 같은 큰 개체를 포함 하는 테이블 해야 하는 경우에 사용할 수 있습니다 테이블 분할 큰 속성 별도 엔터티로 구분 하려면. 예를 들어, varbinary photo 열이 포함 된 제품 테이블이 있다고 가정 합니다. 쿼리에서이 속성에 액세스 하려면 자주 필요 하지 않으면, 일반적으로 해야 하는 엔터티의 부품만를 분할 하는 테이블을 사용할 수 있습니다. 제품 사진을 나타내는 엔터티는 명시적으로 필요한 경우에 로드 됩니다.

테이블 분할을 사용 하도록 설정 하는 방법을 보여주는 좋은 리소스는 Gil Fink "테이블 분할에서 Entity Framework" 블로그 게시물: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>합니다.

## <a name="9-other-considerations"></a>9 기타 고려 사항

### <a name="91-server-garbage-collection"></a>9.1 서버 가비지 수집

일부 사용자는 가비지 수집기가 제대로 구성 되지 않은 경우 예상 되는 병렬 처리를 제한 하는 리소스 경합을 경험할 수 있습니다. EF는 다중 스레드 시나리오에서 사용 되는 서버 쪽 체제와 응용 프로그램에서 유사한, 때마다 서버 가비지 수집을 사용 하도록 설정 해야 합니다. 이 작업은 응용 프로그램 구성 파일의 간단한 설정을 통해 수행 됩니다.

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

이 프로그램 스레드 경합 감소 하 고 CPU 포화 시나리오에서 최대 30% 여 처리량을 늘릴 해야 합니다. 일반적인 측면에서 항상 응용 프로그램의 동작 클래식 가비지 컬렉션 (UI 및 클라이언트 쪽 시나리오에 대해 더 잘 조정 된)를 사용 하 여 서버 가비지 컬렉션 뿐만 아니라 테스트 해야 합니다.

### <a name="92-autodetectchanges"></a>9.2 AutoDetectChanges

앞에서 설명한 대로 Entity Framework 개체 캐시에 엔터티가 많은 경우 성능 문제를 표시할 수 있습니다. 추가, 제거, 찾기, 항목 및 SaveChanges를 같은 특정 작업을 많은 양의 개체 캐시 크기 수에 따라 CPU 사용할 수 있는 DetectChanges에 대 한 호출을 트리거합니다. 이 원인은 개체 캐시 하 고 계속 해 서 개체 상태 관리자 봅니다 가능한 생성 된 데이터는 다양 한 시나리오에서 올바른 것으로 보장 됩니다 있도록 컨텍스트에 수행 된 각 작업에 동기화 합니다.

일반적으로 응용 프로그램의 전체 수명 동안 사용 하도록 설정 하는 Entity Framework의 자동 변경 내용 검색을 종료 하는 것이 좋습니다. 시나리오는 부정적인 영향을 받지 높은 CPU 사용량 프로필 원인 DetectChanges에 대 한 호출 임을 나타내기 위해 경우에 AutoDetectChanges 코드의 중요 한 부분에 일시적으로 해제 하는 것이 좋습니다.

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

AutoDetectChanges를 끄기 전에 것이 좋습니다 이해는 Entity Framework 엔터티에서 수행 되는 변경에 대 한 특정 정보를 추적할 수 없게 발생할 수 있습니다. 잘못 처리 하는 경우 응용 프로그램에서 데이터 불일치가 발생할 수 있습니다. AutoDetectChanges 해제에 대 한 자세한 내용은 읽을 \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>합니다.

### <a name="93-context-per-request"></a>9.3 요청당 컨텍스트

Entity Framework 컨텍스트는 최적의 성능을 제공 하기 위해 단기 인스턴스 처럼 사용할 것입니다. 컨텍스트는 짧을 것으로 예상 활성 상태로 유지 하 고 삭제와 같이 매우 간단 하 고 가능한 메타 데이터를 reutilize에 구현 되었습니다. 웹 시나리오에서는이 점을 명심 하 여 단일 요청 기간 보다는 컨텍스트가 제공 되지 것입니다. 마찬가지로, 웹이 아닌 시나리오에서 컨텍스트를 삭제 해야 Entity Framework의 캐싱 여러 수준에 대 한 이해에 따라 합니다. 일반적으로 하나는 응용 프로그램 뿐만 아니라 스레드당 컨텍스트 및 정적 컨텍스트의 수명 동안 컨텍스트 인스턴스가 있으면 피해 야 합니다.

### <a name="94-database-null-semantics"></a>9.4 데이터베이스 null 의미 체계

기본적으로 entity Framework C에는 SQL 코드를 생성 합니다\# null 비교 의미 체계. 다음 예제 쿼리에서를 것이 좋습니다.

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

이 예제에서는 SupplierID 및 단가 같은 엔터티를 null 허용 속성에 대해 null 허용 변수 숫자를 비교 하는 것입니다. 이 쿼리에 대해 생성된 된 SQL 매개 변수 값은 열 값과 같은 경우 또는 매개 변수 및 열 값이 null이 묻습니다. 이 데이터베이스 서버는 null을 처리 하는 방법은 숨기고 일관 된 C 하면\# 환경을 다른 데이터베이스 공급 업체에서 null입니다. 생성된 된 코드는 다소 복잡 하기는 하지만 및 경우에 잘 수행할 수 없는 반면, 비교의 양을 where 쿼리 문을 많은 수 증가 합니다.

이 상황을 처리 하는 한 가지 방법은 데이터베이스 null 의미 체계를 사용 하는 것입니다. 이 수와 다르게 동작할 c는\# 엔터티 프레임 워크에서는 null 값을 처리 하는 데이터베이스 엔진으로 노출 하는 단순한 SQL을 생성 하는 이제 이후 의미 체계를 null입니다. 데이터베이스 null 의미 체계가 활성화 당 상황에 맞는 컨텍스트 구성에 대해 한 단일 구성 줄 수 있습니다.

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

작음에서 중간 크기의 쿼리에서는 표시 되지 경우 성능을 향상 시킵니다 데이터베이스 null 의미 체계를 사용 하는 경우 않지만 차이 많은 수의 잠재적인 null 비교를 사용 하 여 쿼리에 더 눈에 띄는 됩니다.

위 예제 쿼리와 성능 차이가 통제 된 환경에서 실행 하는 microbenchmark의 2% 보다 작은 경우

### <a name="95-async"></a>9.5 비동기

.NET 4.5 이상을 실행 하는 경우 비동기 작업의 entity Framework 6 도입 된 지원. 대부분의 경우 IO 있는 응용 프로그램 관련 경합 비동기 쿼리를 사용 하 여에서 가장 많은 혜택을 작업을 저장 합니다. 응용 프로그램 IO 경합에서 문제가 발생 하지 않는, 경우 비동기 사용은 최상의 경우에서 동기적으로 실행 최악의 경우 또는 비동기 호출, 처럼 동일한 기간에서 결과 반환, 단순히 비동기 작업 실행을 지연 및 추가 tim 추가 e 시나리오의 완료를 합니다.

방문 비동기 응용 프로그램의 성능이 향상 됩니다 하는 경우를 결정 하는 데 도움이 되는 비동기 프로그래밍 작업에 대 한 내용은 [http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx)합니다. Entity Framework에서 비동기 작업의 자세한 사용 방법은 참조 하세요 [비동기 쿼리 및 저장](~/ef6/fundamentals/async.md
)합니다.

### <a name="96-ngen"></a>9.6 NGEN

Entity Framework 6.NET framework의 기본 설치에서 제공 되지 않습니다. 이와 같이 엔터티 프레임 워크 어셈블리에 없는 다른 MSIL 어셈블리와 동일한 JIT'ing 비용에 따라 모든 Entity Framework 코드 임을 의미는 기본적으로 ngen 합니다. 또한 개발 및 프로덕션 환경에서 응용 프로그램의 콜드 시작 하는 동안 F5 환경을 저하 될 수 있습니다이 있습니다. JIT'ing CPU 및 메모리 비용을 절감 하기 위해 적절 하 게 이미지를 Entity Framework ngen는 것이 좋습니다. NGEN 사용 하 여 Entity Framework 6의 시작 성능을 개선 하는 방법에 대 한 자세한 내용은 참조 하세요. [NGen 사용 하 여 시작 성능을 개선](~/ef6/fundamentals/performance/ngen.md)합니다.

### <a name="97-code-first-versus-edmx"></a>9.7 code First EDMX 비교

개체 지향 프로그래밍 및 관계형 데이터베이스 간의 매핑을 개념적 모델 (개체)와 저장소 스키마 (데이터베이스)의 메모리 내 표현 함으로써 간의 임피던스 불일치 문제에 대 한 entity Framework 이유는 두 가지입니다. 간단히는 엔터티 데이터 모델 또는 EDM이 메타이 데이터 라고 합니다. 이 EDM에서 Entity Framework 왕복 데이터 뷰는 데이터베이스에 메모리의 개체에서 파생 되며 다시 합니다.

개념적 모델, 저장소 스키마 및 매핑을 지정 하면 Entity Framework는 EDMX 파일을 사용 하 여 공식적으로 다음 EDM 올바른지 유효성을 검사 하려면 모델 로드 단계만에 (예를 들어 있는지 매핑이 누락), 한 다음 뷰 생성, 다음 보기를 확인 하 고이 메타 데이터는 사용할 수 있습니다. 다음 수는 쿼리 실행 또는 새 데이터를 데이터 저장소에 저장할 수만 있습니다.

본질적으로 Code First 접근 방식은 복잡 한 엔터티 데이터 모델 생성기입니다. Entity Framework는 제공 된 코드에서 EDM을 생성 해야 합니다. 이때 모델에 규칙을 적용 하 고 Fluent API를 통해 모델 구성에 관련 된 클래스를 분석 합니다. EDM을 빌드한 후 Entity Framework 기본적으로 동일 하 게 방식으로가 프로젝트에서 EDMX 파일입니다. 따라서 Code First에서 모델 빌드 Entity framework EDMX에 비해 속도가 느린 시작 시간을 변환 하는 추가 복잡성을 추가 합니다. 비용은 크기와 빌드되는 모델의 복잡성에 따라 완전히 달라 집니다.

EDMX와 Code First를 사용 하도록 선택 때 Code First에서 도입 된 유연성 처음으로 모델을 구축 비용 증가 한다는 것을 알아야 합니다. 응용 프로그램에이 처음 로드의 비용 견딜 수 않으면 일반적으로 Code First 됩니다 이동 하는 기본 방법입니다.

## <a name="10-investigating-performance"></a>10 조사 성능

### <a name="101-using-the-visual-studio-profiler"></a>10.1 Visual Studio Profiler를 사용 하 여

Entity Framework를 사용 하 여 성능 문제를 발생 하는 경우에 응용 프로그램은 해당 시간을 소모 하는 상황을 확인 하려면 Visual Studio에 기본 제공 하는 것 처럼 프로파일러를 사용할 수 있습니다. "ADO.NET Entity Framework-1 부의 성능 알아보기" 블로그 게시물에서 원형 차트를 생성 하는 데에서는 도구입니다 ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework 동작 콜드 및 웜 쿼리 중의 시간을 소모 하는 위치를 보여 주는 합니다.

데이터 및 모델링 고객 자문 팀에서 작성 된 "Visual Studio 2010 Profiler를 사용 하 여 프로 파일링 Entity Framework" 블로그 게시물에서는 성능 문제를 조사 하는 프로파일러 사용 방법의 실제 예제를 보여 줍니다.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Windows 응용 프로그램에 대 한이 게시물 작성 되었습니다. 웹 응용 프로그램을 프로 파일링 하는 경우 Windows 성능 레코더 WPR () 및 Windows 성능 분석기 (WPA) 도구를 Visual Studio에서 작업 보다 더 잘 작동할 수 있습니다. WPR 및 WPA는 Windows 성능 도구 키트는 Windows 평가 및 배포 키트에 포함 된 부분 ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 응용 프로그램/데이터베이스 프로 파일링

Visual Studio에 기본 제공 profiler와 같은 도구는 응용 프로그램은 시간을 소모 하는 상황을 알려 줍니다.  다른 유형의 프로파일러는 사용 가능한 프로덕션 또는 요구 사항에 따라 사전 프로덕션에서 실행 중인 응용 프로그램의 동적 분석을 수행 하 고 일반적인 문제 및 데이터베이스 액세스의 안티 패턴을 찾습니다.

상업적으로 사용할 수 있는 두 가지 프로파일러는 Entity Framework Profiler ( \<http://efprof.com>) ORMProfiler 및 ( \<http://ormprofiler.com>)합니다.

Code First를 사용 하 여 MVC 응용 프로그램을 응용 프로그램을 사용 하는 경우에 StackExchange의 MiniProfiler를 사용할 수 있습니다. Scott Hanselman 블로그가이 도구 설명: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>합니다.

Julie Lerman의 MSDN Magazine 기사 라는 참조 응용 프로그램의 데이터베이스 작업 프로 파일링 하는 방법은 [Entity Framework에서 데이터베이스 작업 프로 파일링](https://msdn.microsoft.com/magazine/gg490349.aspx)합니다.

### <a name="103-database-logger"></a>10.3 데이터베이스로 거

Entity Framework 6을 사용 하는 경우 또한 기본 제공 로깅 기능을 사용 하는 것이 좋습니다. 컨텍스트의 데이터베이스 속성은 간단한 한 줄짜리 구성을 통해 해당 활동을 기록 하도록 할 수 있습니다.

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

이 예제의 데이터베이스 작업 콘솔에 기록 됩니다. 하지만 모든 작업을 호출 하도록 로그 속성을 구성할 수 있습니다&lt;문자열&gt; 위임 합니다.

사용 하도록 설정 하려는 경우 다시 컴파일하면 고 하지 않고 데이터베이스 로깅 Entity Framework 6.1을 사용 하는 또는 응용 프로그램의 web.config 또는 app.config 파일에 인터셉터를 추가 하 여이 수행할 수는 나중에.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

이동 다시 컴파일하지 않고도 로깅을 추가 하는 방법에 대 한 자세한 내용은 \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>합니다.

## <a name="11-appendix"></a>11 부록

### <a name="111-a-test-environment"></a>11.1 1. 테스트 환경

이 환경에는 클라이언트 응용 프로그램에서 별도 컴퓨터에서 데이터베이스를 사용 하 여 2 대의 컴퓨터 설치를 사용 합니다. 컴퓨터를 동일한 랙에 되므로 네트워크 대기 시간은 비교적 적은 있지만 단일 컴퓨터 환경 보다 좀 더 현실적인 합니다.

#### <a name="1111-app-server"></a>11.1.1 앱 서버

##### <a name="11111-software-environment"></a>11.1.1.1 소프트웨어 환경

-   Entity Framework 4 소프트웨어 환경
    -   OS 이름: Windows Server 2008 R2 Enterprise SP1
    -   Visual Studio 2010 – Ultimate입니다.
    -   Visual Studio 2010 SP1 (일부 비교)에 해당 합니다.
-   Entity Framework 5 및 6 소프트웨어 환경
    -   OS 이름: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate입니다.

##### <a name="11112-hardware-environment"></a>11.1.1.2 하드웨어 환경

-   이중 프로세서: intel (r) 제온 CPU L5520 W3530 @ 2.27ghz, 2261 Mhz8 GHz, 4 코어, 84 논리 프로세서.
-   2412 GB RamRAM 합니다.
-   136 GB SCSI250GB SATA 7200 rpm 3 GB/s 드라이브 4 개의 파티션으로 분할 합니다.

#### <a name="1112-db-server"></a>11.1.2 DB 서버

##### <a name="11121-software-environment"></a>11.1.2.1 소프트웨어 환경

-   OS 이름: Windows Server 2008 R28.1 Enterprise SP1
-   SQL Server 2008 R22012 합니다.

##### <a name="11122-hardware-environment"></a>11.1.2.2 하드웨어 환경

-   단일 프로세서: intel (r) CPU L5520 @ 2.27ghz 제온 2261 MhzES-1620 0 3.60, @, 4 코어, 논리 프로세서 8입니다.
-   824 GB RamRAM 합니다.
-   465 GB ATA500GB SATA 7200 rpm 6GB/s 드라이브 4 개의 파티션으로 분할 합니다.

### <a name="112-b-query-performance-comparison-tests"></a>11.2 2. 쿼리 성능 비교 테스트

이러한 테스트를 실행 하는 Northwind 모델 사용 되었습니다. Entity Framework 디자이너를 사용 하 여 데이터베이스에서 생성 되었습니다. 그런 다음 쿼리 실행 옵션의 성능을 비교 하려면 다음 코드를 사용한:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>11.3 C. Navision 모델

Navision 데이터베이스는 Microsoft Dynamics – 탐색 데모에 사용 되는 많은 데이터베이스 1005 엔터티 집합과 연결 집합이 4227 생성된 된 개념적 모델에 포함 되어 있습니다. 테스트에 사용 된 모델은 "평면" – 상속 하지에 추가 되었습니다.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 쿼리 Navision 테스트에 사용

Navision 모델을 사용한 쿼리 목록에 Entity SQL 쿼리는 3 개의 범주가 포함 되어 있습니다.

##### <a name="11311-lookup"></a>11.3.1.1 조회

집계가 없는 간단한 조회 쿼리

-   개수: 16232
-   예제:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

여러 집계 되지만 없습니다 부분합 (단일 쿼리)를 사용 하 여 일반 BI 쿼리

-   개수: 2313
-   예제:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

여기서 MDF\_SessionLogin\_시간\_max ()는 모델에서 정의 됩니다.

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

집계 및 부분합 (모든 공용 구조체)를 통해을 사용 하 여 BI 쿼리

-   개수: 178
-   예제:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
