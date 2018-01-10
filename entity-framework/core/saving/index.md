---
title: "데이터 저장 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="saving-data"></a>데이터 저장

각각의 컨텍스트 인스턴스에는 데이터베이스에 기록해야 하는 변경 내용의 추적을 담당하는 `ChangeTracker`가 있습니다. 엔터티 클래스 인스턴스를 변경하면 해당 변경이 `ChangeTracker`에 기록된 다음 `SaveChanges`를 호출할 때 데이터베이스에 기록됩니다. 데이터베이스 공급자는 변경 내용을 데이터베이스 특정 작업으로 변환하는 것을 담당합니다(예: 관계형 데이터베이스에 대한 `INSERT`, `UPDATE` 및 `DELETE` 명령).