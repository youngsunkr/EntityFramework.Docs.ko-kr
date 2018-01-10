---
title: "Entity Framework를 사용한 구성 요소 테스트 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a>테스트

실제 데이터베이스 I/O 작업에 부담을 주지 않으면서 실제 데이터베이스에 근접 연결하는 무엇인가를 통해 구성 요소를 테스트하고자 할 수 있습니다.

이렇게 하는 데는 두 가지 기본적인 옵션이 있습니다.
 * [SQLite 메모리 내 모드](sqlite.md)에서는 관계형 데이터베이스처럼 작동하는 공급자에 대해 효율적인 테스트를 작성할 수 있습니다.
 * [InMemory 공급자](in-memory.md)는 최소한의 종속성을 갖으면서도 항상 관계형 데이터베이스처럼 작동하지는 않는 가벼운 공급자입니다.