---
title: SSDL 사양-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122722"
---
# <a name="ssdl-specification"></a><span data-ttu-id="8a321-102">SSDL 사양</span><span class="sxs-lookup"><span data-stu-id="8a321-102">SSDL Specification</span></span>
<span data-ttu-id="8a321-103">SSDL(저장소 스키마 정의 언어)은 Entity Framework 응용 프로그램의 저장소 모델을 설명하는 XML 기반 언어입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="8a321-104">Entity Framework 응용 프로그램에서 저장소 모델 메타 데이터는 System.Data.Metadata.Edm.StoreItemCollection 인스턴스로 (SSDL로 작성).ssdl 파일에서 로드 되 고에서 메서드를 사용 하 여 액세스할 수 합니다 System.Data.Metadata.Edm.MetadataWorkspace 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="8a321-105">Entity Framework 저장소 모델 메타 데이터를 사용 하 여 저장소 관련 명령 개념적 모델에 대 한 쿼리를 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="8a321-106">Entity Framework Designer (EF 디자이너)는 디자인 타임에는.edmx 파일의 저장소 모델 정보를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="8a321-107">빌드 시 Entity Designer는.edmx 파일의 정보는 필요한 Entity Framework에서 런타임에.ssdl 파일을 만들려면 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="8a321-108">SSDL 버전은 XML 네임스페이스로 식별됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="8a321-109">SSDL 버전</span><span class="sxs-lookup"><span data-stu-id="8a321-109">SSDL Version</span></span> | <span data-ttu-id="8a321-110">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="8a321-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="8a321-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="8a321-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="8a321-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="8a321-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="8a321-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="8a321-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="8a321-114">Association 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-114">Association Element (SSDL)</span></span>

<span data-ttu-id="8a321-115">**연결** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 외래 키 제약 조건에 참여 하는 테이블 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="8a321-116">필수 자식 End 요소가 두 연결의 end에 있는 테이블과 각 끝의 복합성을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="8a321-117">선택적 ReferentialConstraint 요소 참여 하는 열 뿐만 아니라 연결의 주 끝 및 종속 끝을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="8a321-118">없으면 **ReferentialConstraint** 요소가, AssociationSetMapping 요소를 사용 하 여 연결에 대 한 열 매핑을 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="8a321-119">합니다 **연결** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-120">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-121">끝 (정확히 2 개)</span><span class="sxs-lookup"><span data-stu-id="8a321-121">End (exactly two)</span></span>
-   <span data-ttu-id="8a321-122">ReferentialConstraint (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="8a321-123">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-124">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-124">Applicable Attributes</span></span>

<span data-ttu-id="8a321-125">다음 표에서 설명에 적용할 수 있는 특성을 **연결** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="8a321-126">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-126">Attribute Name</span></span> | <span data-ttu-id="8a321-127">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-127">Is Required</span></span> | <span data-ttu-id="8a321-128">값</span><span class="sxs-lookup"><span data-stu-id="8a321-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-129">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-129">**Name**</span></span>       | <span data-ttu-id="8a321-130">예</span><span class="sxs-lookup"><span data-stu-id="8a321-130">Yes</span></span>         | <span data-ttu-id="8a321-131">기본 데이터베이스에서 해당하는 외래 키 제약 조건의 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-132">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **연결** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="8a321-133">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-134">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-135">예</span><span class="sxs-lookup"><span data-stu-id="8a321-135">Example</span></span>

<span data-ttu-id="8a321-136">다음 예제와 **연결** 사용 하는 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders**  foreign key 제약 조건:</span><span class="sxs-lookup"><span data-stu-id="8a321-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="8a321-137">AssociationSet 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="8a321-138">합니다 **AssociationSet** 요소 (SSDL) 저장소 스키마 정의 언어에 기본 데이터베이스에서 두 테이블 간의 외래 키 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="8a321-139">외래 키 제약 조건에 관여 하는 테이블 열을 Association 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="8a321-140">**연결** 해당 하는 요소를 지정 **AssociationSet** 요소에 지정 된를 **연결** 특성을 **AssociationSet**  요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="8a321-141">SSDL 연결 집합 AssociationSetMapping 요소 CSDL 연결 집합에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="8a321-142">그러나 지정 된 CSDL 연결에 대 한 CSDL 연결 집합 정의 된 경우 해당 ReferentialConstraint 요소를 사용 하 여 **AssociationSetMapping** 요소가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="8a321-143">이 경우 경우는 **AssociationSetMapping** 요소가 있는지를 정의 하는 매핑이 재정의 합니다 **ReferentialConstraint** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="8a321-144">합니다 **AssociationSet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-145">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-146">끝 (0 개 또는 2)</span><span class="sxs-lookup"><span data-stu-id="8a321-146">End (zero or two)</span></span>
-   <span data-ttu-id="8a321-147">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-148">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-148">Applicable Attributes</span></span>

<span data-ttu-id="8a321-149">다음 표에서 설명에 적용할 수 있는 특성을 **AssociationSet** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="8a321-150">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-150">Attribute Name</span></span>  | <span data-ttu-id="8a321-151">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-151">Is Required</span></span> | <span data-ttu-id="8a321-152">값</span><span class="sxs-lookup"><span data-stu-id="8a321-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-153">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-153">**Name**</span></span>        | <span data-ttu-id="8a321-154">예</span><span class="sxs-lookup"><span data-stu-id="8a321-154">Yes</span></span>         | <span data-ttu-id="8a321-155">연결 집합이 나타내는 외래 키 제약 조건의 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="8a321-156">**연결**</span><span class="sxs-lookup"><span data-stu-id="8a321-156">**Association**</span></span> | <span data-ttu-id="8a321-157">예</span><span class="sxs-lookup"><span data-stu-id="8a321-157">Yes</span></span>         | <span data-ttu-id="8a321-158">외래 키 제약 조건에 참여하는 열을 정의하는 연결의 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-159">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **AssociationSet** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="8a321-160">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-161">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-162">예</span><span class="sxs-lookup"><span data-stu-id="8a321-162">Example</span></span>

<span data-ttu-id="8a321-163">에서는 다음 예제는 **AssociationSet** 나타내는 요소는 `FK_CustomerOrders` 기본 데이터베이스의 외래 키 제약 조건:</span><span class="sxs-lookup"><span data-stu-id="8a321-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="8a321-164">CollectionType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="8a321-165">합니다 **CollectionType** 요소 (SSDL) 저장소 스키마 정의 언어에서 함수의 반환 형식이 컬렉션 임을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="8a321-166">합니다 **CollectionType** ReturnType 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="8a321-167">컬렉션 형식의 RowType 자식 요소를 사용 하 여 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="8a321-168">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **CollectionType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="8a321-169">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-170">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-171">예</span><span class="sxs-lookup"><span data-stu-id="8a321-171">Example</span></span>

<span data-ttu-id="8a321-172">다음 예제에서는 사용 하는 함수를 **CollectionType** 함수 행 컬렉션을 반환 하는지 지정 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="8a321-173">CommandText 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="8a321-174">합니다 **CommandText** 요소 (SSDL) 저장소 스키마 정의 언어에서는 데이터베이스에서 실행 되는 SQL 문을 정의할 수 있는 Function 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="8a321-175">합니다 **CommandText** 요소를 사용 하면 데이터베이스에서 저장된 프로시저에 유사한 기능을 추가할 수 있지만 정의 **CommandText** 저장소 모델의 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="8a321-176">합니다 **CommandText** 요소는 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="8a321-177">본문을 **CommandText** 요소는 기본 데이터베이스에 대 한 유효한 SQL 문 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="8a321-178">적용할 수 없는 특성을 **CommandText** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-179">예</span><span class="sxs-lookup"><span data-stu-id="8a321-179">Example</span></span>

<span data-ttu-id="8a321-180">에서는 다음 예제는 **함수** 자식 요소 **CommandText** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="8a321-181">노출 된 **UpdateProductInOrder** ObjectContext의 메서드를 개념적 모델로 가져와서 역할도 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="8a321-182">DefiningQuery 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="8a321-183">합니다 **DefiningQuery** 요소 (SSDL) 저장소 스키마 정의 언어에서를 사용 하면 기본 데이터베이스에서 직접 SQL 문을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="8a321-184">합니다 **DefiningQuery** 요소는 일반적으로 데이터베이스 뷰처럼 사용 되지만 뷰는 데이터베이스 대신 저장소 모델에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="8a321-185">에 정의 된 뷰를 **DefiningQuery** 요소 EntitySetMapping 요소를 통해 개념적 모델의 엔터티 형식에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="8a321-186">이러한 매핑은 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-186">These mappings are read-only.</span></span>  

<span data-ttu-id="8a321-187">다음 SSDL 구문의 선언을 보여 줍니다.는 **EntitySet** 뒤에 **DefiningQuery** 에 뷰를 검색 하는 데 사용 하는 쿼리를 포함 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="8a321-188">뷰를 통한 읽기 / 쓰기 시나리오를 사용 하도록 설정 하려면 Entity Framework에서 저장된 프로시저를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="8a321-189">데이터를 검색 하 고 저장된 프로시저에서 처리 하는 변경에 대 한 기본 테이블로 데이터 원본 뷰 또는 Entity SQL 뷰를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="8a321-190">사용할 수는 **DefiningQuery** Microsoft SQL Server Compact 3.5를 대상으로 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="8a321-191">SQL Server Compact 3.5는 저장된 프로시저를 지원 하지 않습니다, 유사한 기능을 구현할 수 있습니다 합니다 **DefiningQuery** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="8a321-192">이 요소는 프로그래밍 언어에 사용되는 데이터 형식과 데이터 소스의 데이터 형식 간에 불일치를 해결하기 위한 저장 프로시저를 만드는 데도 유용하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="8a321-193">작성할 수는 **DefiningQuery** 매개 변수는 특정 집합을 사용 하 고 다음 데이터를 삭제 하는 저장된 프로시저 예를 들어, 매개 변수를 다른 집합을 사용 하 여 저장된 프로시저를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="8a321-194">Dependent 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="8a321-195">합니다 **종속** 저장소 스키마 정의 언어 (SSDL)에 참조 제약 조건이 라고도 하는 외래 키 제약 조건의 종속 끝을 정의 하는 ReferentialConstraint 요소에 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="8a321-196">합니다 **종속** 요소는 기본 키 열 (또는 열)을 참조 하는 테이블의 열 (또는 열)을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="8a321-197">**PropertyRef** 요소 참조 되는 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="8a321-198">보안 주체에 지정 된 열에서 참조 하는 기본 키 열을 지정 하는 요소는 **종속** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="8a321-199">합니다 **종속** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-200">PropertyRef (하나 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8a321-201">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-202">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-202">Applicable Attributes</span></span>

<span data-ttu-id="8a321-203">다음 표에서 설명에 적용할 수 있는 특성을 **종속** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="8a321-204">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-204">Attribute Name</span></span> | <span data-ttu-id="8a321-205">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-205">Is Required</span></span> | <span data-ttu-id="8a321-206">값</span><span class="sxs-lookup"><span data-stu-id="8a321-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-207">**역할**</span><span class="sxs-lookup"><span data-stu-id="8a321-207">**Role**</span></span>       | <span data-ttu-id="8a321-208">예</span><span class="sxs-lookup"><span data-stu-id="8a321-208">Yes</span></span>         | <span data-ttu-id="8a321-209">같은 값을 **역할** (사용) 하는 경우 해당 End 요소의 특성, 그렇지 않으면 테이블의 이름을 포함 하는 참조 하는 열입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-210">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **종속** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="8a321-211">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-212">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-213">예</span><span class="sxs-lookup"><span data-stu-id="8a321-213">Example</span></span>

<span data-ttu-id="8a321-214">다음 예제에서는 사용 하는 연결 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders** 외래 키 제약 조건입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8a321-215">**종속** 요소를 지정 합니다 **CustomerId** 열의 합니다 **순서** 테이블 제약 조건의 종속 끝으로.</span><span class="sxs-lookup"><span data-stu-id="8a321-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="8a321-216">Documentation 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="8a321-217">합니다 **설명서** 부모 요소에 정의 된 개체에 대 한 정보를 제공 하는 저장소 스키마 정의 언어 (SSDL)에 요소를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="8a321-218">합니다 **설명서** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-219">**요약**: 부모 요소에 대 한 간략 한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="8a321-220">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-220">(zero or one element)</span></span>
-   <span data-ttu-id="8a321-221">**LongDescription**: 부모 요소에 대 한 자세한 설명입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="8a321-222">(0개 또는 한 개의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-223">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-223">Applicable Attributes</span></span>

<span data-ttu-id="8a321-224">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **설명서** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="8a321-225">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-226">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-227">예</span><span class="sxs-lookup"><span data-stu-id="8a321-227">Example</span></span>

<span data-ttu-id="8a321-228">다음 예제는 **설명서** EntityType 요소의 자식 요소로 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="8a321-229">End 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-229">End Element (SSDL)</span></span>

<span data-ttu-id="8a321-230">합니다 **최종** 기본 데이터베이스의 테이블과 외래 키 제약 조건의 한 end에 있는 행의 수를 지정 하는 저장소 스키마 정의 언어 (SSDL)에 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="8a321-231">합니다 **최종** Association 요소 또는 AssociationSet 요소의 자식 요소일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="8a321-232">각 경우에 가능한 자식 요소와 적용 가능한 특성은 서로 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="8a321-233">Association 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="8a321-234">**끝** 요소 (자식으로는 **연결** 요소) 테이블 및 사용 하 여 외래 키 제약 조건의 end에 있는 행의 수를 지정 합니다 **형식** 및 **복합성** 각각 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="8a321-235">외래 키 제약 조건의 End는 SSDL 연결의 일부로 정의되며 SSDL 연결에는 정확히 두 개의 End가 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="8a321-236">**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-237">설명서 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8a321-238">OnDelete (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="8a321-239">Annotation 요소 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a321-240">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-240">Applicable Attributes</span></span>

<span data-ttu-id="8a321-241">다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **연결** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="8a321-242">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-242">Attribute Name</span></span>   | <span data-ttu-id="8a321-243">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-243">Is Required</span></span> | <span data-ttu-id="8a321-244">값</span><span class="sxs-lookup"><span data-stu-id="8a321-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="8a321-245">**Type**</span></span>         | <span data-ttu-id="8a321-246">예</span><span class="sxs-lookup"><span data-stu-id="8a321-246">Yes</span></span>         | <span data-ttu-id="8a321-247">외래 키 제약 조건의 end에 있는 SSDL 엔터티 집합의 정규화 된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="8a321-248">**역할**</span><span class="sxs-lookup"><span data-stu-id="8a321-248">**Role**</span></span>         | <span data-ttu-id="8a321-249">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-249">No</span></span>          | <span data-ttu-id="8a321-250">값을 **역할** (사용) 하는 경우 해당 ReferentialConstraint 요소 또는 종속 요소의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="8a321-251">**다중성**</span><span class="sxs-lookup"><span data-stu-id="8a321-251">**Multiplicity**</span></span> | <span data-ttu-id="8a321-252">예</span><span class="sxs-lookup"><span data-stu-id="8a321-252">Yes</span></span>         | <span data-ttu-id="8a321-253">**1**하십시오 **0..1**, 또는 **\*** 외래 키 제약 조건의 end에 있을 수 있는 행의 수에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="8a321-254">**1** 외래 키 제약 조건 end에는 정확히 하나의 행이 있음을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="8a321-255">**0..1** 나타내는 외래 키 제약 조건 end에 0 개 이상의 행이 존재 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="8a321-256">**\*** 외래 키 제약 조건 end에 0, 1 또는 행이 더 있는지를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-257">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="8a321-258">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-259">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="8a321-260">예</span><span class="sxs-lookup"><span data-stu-id="8a321-260">Example</span></span>

<span data-ttu-id="8a321-261">에서는 다음 예제는 **연결** 정의 하는 요소는 **FK\_CustomerOrders** foreign key 제약 조건.</span><span class="sxs-lookup"><span data-stu-id="8a321-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8a321-262">**복합성** 각각에 지정 된 값 **끝** 요소에는 많은 행을 나타내는 **주문** 테이블의 행과 연결할 수 있습니다는 **고객**  테이블에 있지만 하나의 행에는 **고객** 테이블의 행과 연결할 수 있습니다는 **주문** 테이블.</span><span class="sxs-lookup"><span data-stu-id="8a321-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="8a321-263">또한 합니다 **OnDelete** 요소에서 모든 행을 나타냅니다는 **주문** 테이블에서 특정 행을 참조 하는 **고객** 경우 테이블은 삭제 됩니다의 행을 합니다 **고객** 테이블이 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="8a321-264">AssociationSet 요소의 자식인 End 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="8a321-265">합니다 **끝** 요소 (자식으로는 **AssociationSet** 요소) 기본 데이터베이스의 외래 키 제약 조건의 한 end에 있는 테이블을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="8a321-266">**최종** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-267">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-268">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a321-269">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-269">Applicable Attributes</span></span>

<span data-ttu-id="8a321-270">다음 표에서 특성을 적용할 수 있습니다는 **끝** 의 자식인 경우 요소는 **AssociationSet** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="8a321-271">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-271">Attribute Name</span></span> | <span data-ttu-id="8a321-272">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-272">Is Required</span></span> | <span data-ttu-id="8a321-273">값</span><span class="sxs-lookup"><span data-stu-id="8a321-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="8a321-274">**EntitySet**</span></span>  | <span data-ttu-id="8a321-275">예</span><span class="sxs-lookup"><span data-stu-id="8a321-275">Yes</span></span>         | <span data-ttu-id="8a321-276">외래 키 제약 조건의 end에 있는 SSDL 엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="8a321-277">**역할**</span><span class="sxs-lookup"><span data-stu-id="8a321-277">**Role**</span></span>       | <span data-ttu-id="8a321-278">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-278">No</span></span>          | <span data-ttu-id="8a321-279">값 중 하나는 **역할** 하나에 지정 된 특성 **끝** 의 해당 Association 요소는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-280">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **최종** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="8a321-281">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-282">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="8a321-283">예</span><span class="sxs-lookup"><span data-stu-id="8a321-283">Example</span></span>

<span data-ttu-id="8a321-284">에서는 다음 예제는 **EntityContainer** 요소를 **AssociationSet** 요소 두 개가 **끝** 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="8a321-285">EntityContainer 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="8a321-286">**EntityContainer** 요소 (SSDL) 저장소 스키마 정의 언어에서 Entity Framework 응용 프로그램에서 기본 데이터 원본의 구조를 설명 합니다: SSDL 엔터티 집합 (EntitySet 요소에 정의 됨)에서 테이블을 나타내는 데이터베이스에 SSDL 엔터티 형식 (EntityType 요소에 정의 됨) 테이블의 행을 나타내고 연결 집합 (AssociationSet 요소에 정의 됨) 데이터베이스에서 foreign key 제약 조건입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="8a321-287">EntityContainerMapping 요소를 통해 개념적 모델 엔터티 컨테이너를 저장소 모델 엔터티 컨테이너를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="8a321-288">**EntityContainer** 요소 0 개 이상의 Documentation 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="8a321-289">경우는 **설명서** 요소가 있는지, 다른 모든 자식 요소 앞에 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="8a321-290">**EntityContainer** 요소 (나열 된 순서로) 0 개 이상의 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="8a321-291">EntitySet</span></span>
-   <span data-ttu-id="8a321-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="8a321-292">AssociationSet</span></span>
-   <span data-ttu-id="8a321-293">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-294">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-294">Applicable Attributes</span></span>

<span data-ttu-id="8a321-295">아래 표에서 설명에 적용할 수 있는 특성을 **EntityContainer** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="8a321-296">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-296">Attribute Name</span></span> | <span data-ttu-id="8a321-297">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-297">Is Required</span></span> | <span data-ttu-id="8a321-298">값</span><span class="sxs-lookup"><span data-stu-id="8a321-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="8a321-299">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-299">**Name**</span></span>       | <span data-ttu-id="8a321-300">예</span><span class="sxs-lookup"><span data-stu-id="8a321-300">Yes</span></span>         | <span data-ttu-id="8a321-301">엔터티 컨테이너의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-301">The name of the entity container.</span></span> <span data-ttu-id="8a321-302">이 이름에는 마침표(.)가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-303">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityContainer** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="8a321-304">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-305">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-306">예</span><span class="sxs-lookup"><span data-stu-id="8a321-306">Example</span></span>

<span data-ttu-id="8a321-307">다음 예제는 **EntityContainer** 두 엔터티 집합 및 하나의 연결 집합을 정의 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="8a321-308">엔터티 형식 및 연결 형식 이름은 개념적 모델 네임스페이스 이름으로 정규화됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="8a321-309">EntitySet 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="8a321-310">**EntitySet** 테이블 또는 뷰의 원본 데이터베이스의 저장소 스키마 정의 언어 (SSDL) 요소를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="8a321-311">SSDL에서 EntityType 요소는 테이블 또는 뷰의 행을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="8a321-312">**EntityType** 특성을 **EntitySet** 요소는 SSDL 엔터티 집합의 행을 나타내는 특정 SSDL 엔터티 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="8a321-313">EntitySetMapping 요소는 CSDL 엔터티 집합 및 SSDL 엔터티 집합 간의 매핑을 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="8a321-314">합니다 **EntitySet** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-315">설명서 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8a321-316">DefiningQuery (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="8a321-317">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-318">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-318">Applicable Attributes</span></span>

<span data-ttu-id="8a321-319">다음 표에서 설명에 적용할 수 있는 특성을 **EntitySet** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8a321-320">(여기에 나열 되지) 일부 특성을 사용 하 여 정규화 할 수 있습니다 합니다 **저장할** 별칭입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="8a321-321">이러한 특성은 모델을 업데이트 하는 경우 모델 업데이트 마법사에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="8a321-322">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-322">Attribute Name</span></span> | <span data-ttu-id="8a321-323">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-323">Is Required</span></span> | <span data-ttu-id="8a321-324">값</span><span class="sxs-lookup"><span data-stu-id="8a321-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-325">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-325">**Name**</span></span>       | <span data-ttu-id="8a321-326">예</span><span class="sxs-lookup"><span data-stu-id="8a321-326">Yes</span></span>         | <span data-ttu-id="8a321-327">엔터티 집합의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="8a321-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="8a321-328">**EntityType**</span></span> | <span data-ttu-id="8a321-329">예</span><span class="sxs-lookup"><span data-stu-id="8a321-329">Yes</span></span>         | <span data-ttu-id="8a321-330">엔터티 집합에 포함되는 인스턴스의 엔터티 형식에 대한 정규화된 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="8a321-331">**스키마**</span><span class="sxs-lookup"><span data-stu-id="8a321-331">**Schema**</span></span>     | <span data-ttu-id="8a321-332">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-332">No</span></span>          | <span data-ttu-id="8a321-333">데이터베이스 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="8a321-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="8a321-334">**Table**</span></span>      | <span data-ttu-id="8a321-335">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-335">No</span></span>          | <span data-ttu-id="8a321-336">데이터베이스 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="8a321-337">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntitySet** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="8a321-338">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-339">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-340">예</span><span class="sxs-lookup"><span data-stu-id="8a321-340">Example</span></span>

<span data-ttu-id="8a321-341">에서는 다음 예제는 **EntityContainer** 를 가진 두 요소가 **EntitySet** 요소와 하나 **AssociationSet** 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="8a321-342">EntityType 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="8a321-343">**EntityType** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 뷰나 테이블의 행을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="8a321-344">테이블 또는 뷰의 행 발생 하는 SSDL의 EntitySet 요소를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="8a321-345">**EntityType** 특성을 **EntitySet** 요소는 SSDL 엔터티 집합의 행을 나타내는 특정 SSDL 엔터티 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="8a321-346">SSDL 엔터티 형식과 CSDL 엔터티 형식 간의 매핑은 EntityTypeMapping 요소에 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="8a321-347">합니다 **EntityType** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-348">설명서 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8a321-349">키 (0 개 이상의 요소)</span><span class="sxs-lookup"><span data-stu-id="8a321-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="8a321-350">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-351">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-351">Applicable Attributes</span></span>

<span data-ttu-id="8a321-352">아래 표에서 설명에 적용할 수 있는 특성을 **EntityType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="8a321-353">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-353">Attribute Name</span></span> | <span data-ttu-id="8a321-354">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-354">Is Required</span></span> | <span data-ttu-id="8a321-355">값</span><span class="sxs-lookup"><span data-stu-id="8a321-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-356">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-356">**Name**</span></span>       | <span data-ttu-id="8a321-357">예</span><span class="sxs-lookup"><span data-stu-id="8a321-357">Yes</span></span>         | <span data-ttu-id="8a321-358">엔터티 형식의 이름.</span><span class="sxs-lookup"><span data-stu-id="8a321-358">The name of the entity type.</span></span> <span data-ttu-id="8a321-359">이 값은 일반적으로 엔터티 형식이 나타내는 행이 있는 테이블의 이름과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="8a321-360">이 값에는 마침표(.)가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-361">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **EntityType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="8a321-362">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-363">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-364">예</span><span class="sxs-lookup"><span data-stu-id="8a321-364">Example</span></span>

<span data-ttu-id="8a321-365">다음 예제는 **EntityType** 두 속성이 있는 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="8a321-366">Function 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-366">Function Element (SSDL)</span></span>

<span data-ttu-id="8a321-367">합니다 **함수** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스에 존재 하는 저장된 프로시저를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="8a321-368">합니다 **함수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-369">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-370">매개 변수 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="8a321-371">CommandText (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="8a321-372">ReturnType (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="8a321-373">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="8a321-374">반환 함수에 대 한 형식을 사용 하 여 지정 해야 합니다 **ReturnType** 요소 또는 **ReturnType** 특성 (아래 참조) 중 하나만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="8a321-375">저장소 모델에 지정된 저장 프로시저를 응용 프로그램의 개념적 모델로 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="8a321-376">자세한 내용은 [저장 프로시저를 사용 하 여 쿼리](~/ef6/modeling/designer/stored-procedures/query.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="8a321-377">합니다 **함수** 요소는 저장소 모델에서 사용자 지정 함수 정의를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-378">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-378">Applicable Attributes</span></span>

<span data-ttu-id="8a321-379">다음 표에서 설명에 적용할 수 있는 특성을 **함수** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8a321-380">(여기에 나열 되지) 일부 특성을 사용 하 여 정규화 할 수 있습니다 합니다 **저장할** 별칭입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="8a321-381">이러한 특성은 모델을 업데이트 하는 경우 모델 업데이트 마법사에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="8a321-382">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-382">Attribute Name</span></span>             | <span data-ttu-id="8a321-383">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-383">Is Required</span></span> | <span data-ttu-id="8a321-384">값</span><span class="sxs-lookup"><span data-stu-id="8a321-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-385">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-385">**Name**</span></span>                   | <span data-ttu-id="8a321-386">예</span><span class="sxs-lookup"><span data-stu-id="8a321-386">Yes</span></span>         | <span data-ttu-id="8a321-387">저장 프로시저의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8a321-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="8a321-388">**ReturnType**</span></span>             | <span data-ttu-id="8a321-389">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-389">No</span></span>          | <span data-ttu-id="8a321-390">저장 프로시저의 반환 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="8a321-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="8a321-391">**Aggregate**</span></span>              | <span data-ttu-id="8a321-392">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-392">No</span></span>          | <span data-ttu-id="8a321-393">**True 이면** 저장된 프로시저는 집계 값을 반환 하는 경우이 고, 그렇지 **False**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="8a321-394">**기본 제공**</span><span class="sxs-lookup"><span data-stu-id="8a321-394">**BuiltIn**</span></span>                | <span data-ttu-id="8a321-395">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-395">No</span></span>          | <span data-ttu-id="8a321-396">**True** 함수는 기본 제공 하는 경우<sup>1</sup> 함수를 그렇지 않으면 **False**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="8a321-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="8a321-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="8a321-398">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-398">No</span></span>          | <span data-ttu-id="8a321-399">저장 프로시저의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8a321-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="8a321-400">**NiladicFunction**</span></span>        | <span data-ttu-id="8a321-401">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-401">No</span></span>          | <span data-ttu-id="8a321-402">**True 이면** 함수는 무항 이면<sup>2</sup> ; 함수 **False** 그렇지 않은 경우.</span><span class="sxs-lookup"><span data-stu-id="8a321-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="8a321-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="8a321-403">**IsComposable**</span></span>           | <span data-ttu-id="8a321-404">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-404">No</span></span>          | <span data-ttu-id="8a321-405">**True** 기본 구성 가능 함수 이면<sup>3</sup> ; 함수 **False** 그렇지 않은 경우.</span><span class="sxs-lookup"><span data-stu-id="8a321-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="8a321-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="8a321-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="8a321-407">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-407">No</span></span>          | <span data-ttu-id="8a321-408">함수 오버로드를 확인하는 데 사용되는 형식 의미 체계를 정의하는 열거형입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="8a321-409">열거형은 함수 정의별로 공급자 매니페스트에 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="8a321-410">기본값은 **AllowImplicitConversion**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="8a321-411">**스키마**</span><span class="sxs-lookup"><span data-stu-id="8a321-411">**Schema**</span></span>                 | <span data-ttu-id="8a321-412">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-412">No</span></span>          | <span data-ttu-id="8a321-413">저장 프로시저가 정의된 스키마의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="8a321-414"><sup>1</sup> 기본 제공 함수는 데이터베이스에 정의 된 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="8a321-415">저장소 모델에 정의 된 함수에 대 한 내용은 CommandText 요소 (SSDL)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="8a321-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="8a321-416"><sup>2</sup> 무항 함수는 매개 변수를 수락 하 고 호출 하는 경우 괄호가 필요 하지 않습니다는 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="8a321-417"><sup>3</sup> 두 함수는 한 함수의 출력이 다른 함수의 입력 수 있으면 구성 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="8a321-418">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **함수** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="8a321-419">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-420">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-421">예</span><span class="sxs-lookup"><span data-stu-id="8a321-421">Example</span></span>

<span data-ttu-id="8a321-422">에서는 다음 예제는 **함수** 해당 하는 요소는 **UpdateOrderQuantity** 저장 프로시저.</span><span class="sxs-lookup"><span data-stu-id="8a321-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="8a321-423">저장 프로시저는 두 매개 변수를 받아들이며 값을 반환하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="8a321-424">Key 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-424">Key Element (SSDL)</span></span>

<span data-ttu-id="8a321-425">합니다 **키** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스에서 테이블의 기본 키를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="8a321-426">**키** 테이블의 행을 나타내는 EntityType 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="8a321-427">에 기본 키가 정의 합니다 **키** 에 정의 된 하나 이상의 속성 요소를 참조 하 여 요소를 **EntityType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="8a321-428">합니다 **키** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-429">PropertyRef (하나 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8a321-430">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-430">Annotation elements</span></span>

<span data-ttu-id="8a321-431">적용할 수 없는 특성을 **키** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-432">예</span><span class="sxs-lookup"><span data-stu-id="8a321-432">Example</span></span>

<span data-ttu-id="8a321-433">다음 예제는 **EntityType** 요소 하나의 속성을 참조 하는 키를 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="8a321-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="8a321-434">OnDelete 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="8a321-435">합니다 **OnDelete** 요소 (SSDL) 저장소 스키마 정의 언어에서 외래 키 제약 조건에 관여 하는 행이 삭제 될 때 데이터베이스 동작을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="8a321-436">작업 설정 된 경우 **Cascade**, 삭제 되는 행을 참조 하는 행도 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="8a321-437">작업 설정 된 경우 **None**, 후 삭제 되는 행을 참조 하는 행도 삭제 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="8a321-438">**OnDelete** 최종 요소의 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="8a321-439">**OnDelete** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-440">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-441">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-442">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-442">Applicable Attributes</span></span>

<span data-ttu-id="8a321-443">다음 표에서 설명에 적용할 수 있는 특성을 **OnDelete** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="8a321-444">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-444">Attribute Name</span></span> | <span data-ttu-id="8a321-445">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-445">Is Required</span></span> | <span data-ttu-id="8a321-446">값</span><span class="sxs-lookup"><span data-stu-id="8a321-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-447">**작업**</span><span class="sxs-lookup"><span data-stu-id="8a321-447">**Action**</span></span>     | <span data-ttu-id="8a321-448">예</span><span class="sxs-lookup"><span data-stu-id="8a321-448">Yes</span></span>         | <span data-ttu-id="8a321-449">**Cascade** 나 **None**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-449">**Cascade** or **None**.</span></span> <span data-ttu-id="8a321-450">(값 **Restricted** 유효 하지만 동일한 동작 **None**.)</span><span class="sxs-lookup"><span data-stu-id="8a321-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-451">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **OnDelete** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="8a321-452">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-453">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-454">예</span><span class="sxs-lookup"><span data-stu-id="8a321-454">Example</span></span>

<span data-ttu-id="8a321-455">에서는 다음 예제는 **연결** 정의 하는 요소는 **FK\_CustomerOrders** foreign key 제약 조건.</span><span class="sxs-lookup"><span data-stu-id="8a321-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8a321-456">**OnDelete** 요소에서 모든 행을 나타냅니다는 **주문** 테이블에서 특정 행을 참조 하는 합니다 **고객** 경우 테이블은 삭제 됩니다 합니다 행**고객** 테이블이 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="8a321-457">Parameter 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="8a321-458">합니다 **매개 변수** (SSDL) 저장소 스키마 정의 언어에서 요소는 데이터베이스의 저장된 프로시저에 대 한 매개 변수를 지정 하는 Function 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="8a321-459">합니다 **매개 변수** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-460">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-461">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-462">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-462">Applicable Attributes</span></span>

<span data-ttu-id="8a321-463">아래 표에서 설명에 적용할 수 있는 특성을 **매개 변수** 요소.</span><span class="sxs-lookup"><span data-stu-id="8a321-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="8a321-464">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-464">Attribute Name</span></span> | <span data-ttu-id="8a321-465">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-465">Is Required</span></span> | <span data-ttu-id="8a321-466">값</span><span class="sxs-lookup"><span data-stu-id="8a321-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-467">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-467">**Name**</span></span>       | <span data-ttu-id="8a321-468">예</span><span class="sxs-lookup"><span data-stu-id="8a321-468">Yes</span></span>         | <span data-ttu-id="8a321-469">매개 변수의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="8a321-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="8a321-470">**Type**</span></span>       | <span data-ttu-id="8a321-471">예</span><span class="sxs-lookup"><span data-stu-id="8a321-471">Yes</span></span>         | <span data-ttu-id="8a321-472">매개 변수 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="8a321-473">**모드**</span><span class="sxs-lookup"><span data-stu-id="8a321-473">**Mode**</span></span>       | <span data-ttu-id="8a321-474">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-474">No</span></span>          | <span data-ttu-id="8a321-475">**In**, **초과**, 또는 **InOut** 매개 변수는 입력, 출력 또는 입출력 매개 변수 인지에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="8a321-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8a321-476">**MaxLength**</span></span>  | <span data-ttu-id="8a321-477">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-477">No</span></span>          | <span data-ttu-id="8a321-478">매개 변수의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="8a321-479">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="8a321-479">**Precision**</span></span>  | <span data-ttu-id="8a321-480">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-480">No</span></span>          | <span data-ttu-id="8a321-481">매개 변수의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="8a321-482">**배율**</span><span class="sxs-lookup"><span data-stu-id="8a321-482">**Scale**</span></span>      | <span data-ttu-id="8a321-483">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-483">No</span></span>          | <span data-ttu-id="8a321-484">매개 변수의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="8a321-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="8a321-485">**SRID**</span></span>       | <span data-ttu-id="8a321-486">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-486">No</span></span>          | <span data-ttu-id="8a321-487">시스템 공간 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="8a321-488">공간 형식 매개 변수에 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="8a321-489">자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-490">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **매개 변수** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="8a321-491">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-492">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-493">예</span><span class="sxs-lookup"><span data-stu-id="8a321-493">Example</span></span>

<span data-ttu-id="8a321-494">에서는 다음 예제는 **함수** 를 가진 두 요소가 **매개 변수** 입력된 매개 변수를 지정 하는 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="8a321-495">Principal 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="8a321-496">합니다 **주** 저장소 스키마 정의 언어 (SSDL)에 참조 제약 조건이 라고도 하는 외래 키 제약 조건의 주 끝을 정의 하는 ReferentialConstraint 요소에 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="8a321-497">합니다 **주** 다른 열 (또는 열)에서 참조 되는 테이블의 기본 키 열 (또는 열)를 지정 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="8a321-498">**PropertyRef** 요소 참조 되는 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="8a321-499">Dependent 요소에 지정 된 기본 키 열을 참조 하는 열을 지정 합니다 **주** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="8a321-500">합니다 **주** 요소 (나열 된 순서로)는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a321-501">PropertyRef (하나 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8a321-502">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-503">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-503">Applicable Attributes</span></span>

<span data-ttu-id="8a321-504">다음 표에서 설명에 적용할 수 있는 특성을 **주** 요소.</span><span class="sxs-lookup"><span data-stu-id="8a321-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="8a321-505">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-505">Attribute Name</span></span> | <span data-ttu-id="8a321-506">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-506">Is Required</span></span> | <span data-ttu-id="8a321-507">값</span><span class="sxs-lookup"><span data-stu-id="8a321-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-508">**역할**</span><span class="sxs-lookup"><span data-stu-id="8a321-508">**Role**</span></span>       | <span data-ttu-id="8a321-509">예</span><span class="sxs-lookup"><span data-stu-id="8a321-509">Yes</span></span>         | <span data-ttu-id="8a321-510">같은 값을 **역할** (사용) 하는 경우 해당 End 요소의 특성, 그렇지 않으면 테이블의 이름을 포함 하는 참조 된 열입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-511">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **주** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="8a321-512">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-513">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-514">예</span><span class="sxs-lookup"><span data-stu-id="8a321-514">Example</span></span>

<span data-ttu-id="8a321-515">다음 예제에서는 사용 하는 연결 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders** 외래 키 제약 조건입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8a321-516">**주** 요소를 지정 합니다 **CustomerId** 열의 **고객** 테이블 제약 조건의 주 끝으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="8a321-517">Property 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-517">Property Element (SSDL)</span></span>

<span data-ttu-id="8a321-518">합니다 **속성** 요소 (SSDL) 저장소 스키마 정의 언어에서 기본 데이터베이스의 테이블에 열을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="8a321-519">**속성** 요소가 테이블의 행을 나타내는 EntityType 요소의 자식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="8a321-520">각 **속성** 에 정의 된 요소를 **EntityType** 요소는 열을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="8a321-521">A **속성** 요소는 모든 자식 요소를 포함할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-522">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-522">Applicable Attributes</span></span>

<span data-ttu-id="8a321-523">다음 표에서 설명에 적용할 수 있는 특성을 **속성** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="8a321-524">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-524">Attribute Name</span></span>            | <span data-ttu-id="8a321-525">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-525">Is Required</span></span> | <span data-ttu-id="8a321-526">값</span><span class="sxs-lookup"><span data-stu-id="8a321-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-527">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-527">**Name**</span></span>                  | <span data-ttu-id="8a321-528">예</span><span class="sxs-lookup"><span data-stu-id="8a321-528">Yes</span></span>         | <span data-ttu-id="8a321-529">해당 열의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="8a321-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="8a321-530">**Type**</span></span>                  | <span data-ttu-id="8a321-531">예</span><span class="sxs-lookup"><span data-stu-id="8a321-531">Yes</span></span>         | <span data-ttu-id="8a321-532">해당 열의 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="8a321-533">**Null 허용**</span><span class="sxs-lookup"><span data-stu-id="8a321-533">**Nullable**</span></span>              | <span data-ttu-id="8a321-534">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-534">No</span></span>          | <span data-ttu-id="8a321-535">**True 이면** (기본값) 또는 **False** 해당 열에 null 값을 사용할 수 있는지 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="8a321-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="8a321-536">**DefaultValue**</span></span>          | <span data-ttu-id="8a321-537">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-537">No</span></span>          | <span data-ttu-id="8a321-538">해당 열의 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8a321-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8a321-539">**MaxLength**</span></span>             | <span data-ttu-id="8a321-540">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-540">No</span></span>          | <span data-ttu-id="8a321-541">해당 열의 최대 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="8a321-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="8a321-542">**FixedLength**</span></span>           | <span data-ttu-id="8a321-543">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-543">No</span></span>          | <span data-ttu-id="8a321-544">**True 이면** 나 **False** 고정된 길이 문자열로 해당 열 값이 저장될지 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="8a321-545">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="8a321-545">**Precision**</span></span>             | <span data-ttu-id="8a321-546">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-546">No</span></span>          | <span data-ttu-id="8a321-547">해당 열의 전체 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="8a321-548">**배율**</span><span class="sxs-lookup"><span data-stu-id="8a321-548">**Scale**</span></span>                 | <span data-ttu-id="8a321-549">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-549">No</span></span>          | <span data-ttu-id="8a321-550">해당 열의 소수 자릿수입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="8a321-551">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="8a321-551">**Unicode**</span></span>               | <span data-ttu-id="8a321-552">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-552">No</span></span>          | <span data-ttu-id="8a321-553">**True 이면** 나 **False** 유니코드 문자열로 해당 열 값이 저장될지 여부에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="8a321-554">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="8a321-554">**Collation**</span></span>             | <span data-ttu-id="8a321-555">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-555">No</span></span>          | <span data-ttu-id="8a321-556">데이터 소스에 사용될 데이터 정렬 순서를 지정하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="8a321-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="8a321-557">**SRID**</span></span>                  | <span data-ttu-id="8a321-558">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-558">No</span></span>          | <span data-ttu-id="8a321-559">시스템 공간 참조 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="8a321-560">공간 형식의 속성에 대해서만 유효 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="8a321-561">자세한 내용은 [SRID](http://en.wikipedia.org/wiki/SRID) 하 고 [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="8a321-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="8a321-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="8a321-563">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-563">No</span></span>          | <span data-ttu-id="8a321-564">**None**, **Identity** (하는 경우 해당 열 값이 데이터베이스에서 생성 된 id), 또는 **계산 됨** (하는 경우 해당 열 값은 데이터베이스에서 계산).</span><span class="sxs-lookup"><span data-stu-id="8a321-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="8a321-565">RowType 속성에 대해 유효 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-566">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **속성** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="8a321-567">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-568">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-569">예</span><span class="sxs-lookup"><span data-stu-id="8a321-569">Example</span></span>

<span data-ttu-id="8a321-570">다음 예제는 **EntityType** 두 개의 자식 **속성** 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="8a321-571">PropertyRef 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="8a321-572">합니다 **PropertyRef** 요소 (SSDL) 저장소 스키마 정의 언어에서를 나타내는 속성이 다음 역할 중 하나가 수행 되는 EntityType 요소에 정의 된 속성을 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="8a321-573">테이블의 기본 키의 일부 여야 하는 **EntityType** 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="8a321-574">하나 이상의 **PropertyRef** 기본 키를 정의 하는 요소를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="8a321-575">자세한 내용은 키 요소를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="8a321-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="8a321-576">참조 제약 조건의 종속 또는 주 끝.</span><span class="sxs-lookup"><span data-stu-id="8a321-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="8a321-577">자세한 내용은 ReferentialConstraint 요소를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="8a321-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="8a321-578">합니다 **PropertyRef** 요소는 다음과 같은 자식 요소를 하나만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="8a321-579">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-580">Annotation 요소</span><span class="sxs-lookup"><span data-stu-id="8a321-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-581">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-581">Applicable Attributes</span></span>

<span data-ttu-id="8a321-582">아래 표에서 설명에 적용할 수 있는 특성을 **PropertyRef** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="8a321-583">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-583">Attribute Name</span></span> | <span data-ttu-id="8a321-584">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-584">Is Required</span></span> | <span data-ttu-id="8a321-585">값</span><span class="sxs-lookup"><span data-stu-id="8a321-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="8a321-586">**이름**</span><span class="sxs-lookup"><span data-stu-id="8a321-586">**Name**</span></span>       | <span data-ttu-id="8a321-587">예</span><span class="sxs-lookup"><span data-stu-id="8a321-587">Yes</span></span>         | <span data-ttu-id="8a321-588">참조된 속성의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="8a321-589">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **PropertyRef** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="8a321-590">그러나 사용자 지정 특성은 CSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8a321-591">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-592">예</span><span class="sxs-lookup"><span data-stu-id="8a321-592">Example</span></span>

<span data-ttu-id="8a321-593">에서는 다음 예제는 **PropertyRef** 에 정의 된 속성을 참조 하 여 기본 키를 정의 하는 데 사용 되는 요소는 **EntityType** 요소.</span><span class="sxs-lookup"><span data-stu-id="8a321-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="8a321-594">ReferentialConstraint 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="8a321-595">합니다 **ReferentialConstraint** 요소 (SSDL) 저장소 스키마 정의 언어에 기본 데이터베이스에는 참조 무결성 제약 조건이 라고도 하는 외래 키 제약을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="8a321-596">제약 조건의 주 끝 및 종속 끝 각각 보안 주체 및 종속 자식 요소에 의해 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="8a321-597">주 끝과 종속 끝에 참여 하는 열은 PropertyRef 요소를 사용 하 여 참조 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="8a321-598">합니다 **ReferentialConstraint** 요소는 Association 요소의 선택적 자식 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="8a321-599">경우는 **ReferentialConstraint** 요소에 지정 된 외래 키 제약 조건에 매핑할 사용 하지 않으면 합니다 **연결** 이 작업을 수행 하려면 요소를 사용 해야 하는 AssociationSetMapping 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="8a321-600">합니다 **ReferentialConstraint** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a321-601">설명서 (0 또는 1)</span><span class="sxs-lookup"><span data-stu-id="8a321-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8a321-602">보안 주체 (1 개만)</span><span class="sxs-lookup"><span data-stu-id="8a321-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="8a321-603">종속 (1 개만)</span><span class="sxs-lookup"><span data-stu-id="8a321-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="8a321-604">Annotation 요소 (0 개 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-605">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-605">Applicable Attributes</span></span>

<span data-ttu-id="8a321-606">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReferentialConstraint** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="8a321-607">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-608">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-609">예</span><span class="sxs-lookup"><span data-stu-id="8a321-609">Example</span></span>

<span data-ttu-id="8a321-610">다음 예제와 **연결** 사용 하는 요소를 **ReferentialConstraint** 참여 하는 열을 지정 하는 요소는 **FK\_CustomerOrders**  foreign key 제약 조건:</span><span class="sxs-lookup"><span data-stu-id="8a321-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="8a321-611">ReturnType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="8a321-612">합니다 **ReturnType** (SSDL) 저장소 스키마 정의 언어에서 요소에 정의 된 함수에 대 한 반환 형식을 지정 하는 **함수** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="8a321-613">함수 반환 형식으로 지정할 수도 있습니다는 **ReturnType** 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="8a321-614">함수의 반환 형식이 지정 된 된 **형식** 특성 또는 **ReturnType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="8a321-615">합니다 **ReturnType** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="8a321-616">CollectionType (1 개)</span><span class="sxs-lookup"><span data-stu-id="8a321-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="8a321-617">주석 특성 (사용자 지정 XML 특성)을 개수에 관계 없이 적용할 수 있습니다 합니다 **ReturnType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="8a321-618">그러나 사용자 지정 특성은 SSDL에 예약된 XML 네임스페이스에 속할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8a321-619">두 사용자 지정 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-620">예</span><span class="sxs-lookup"><span data-stu-id="8a321-620">Example</span></span>

<span data-ttu-id="8a321-621">다음 예에서는 **함수** 는 행의 컬렉션을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="8a321-622">RowType 요소 (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="8a321-623">A **RowType** 반환는 명명 되지 않은 구조를 정의 하는 저장소 스키마 정의 언어 (SSDL)의 요소 형식 저장소에 정의 된 함수에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="8a321-624">A **RowType** 요소는 자식 요소입니다 **CollectionType** 요소:</span><span class="sxs-lookup"><span data-stu-id="8a321-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="8a321-625">A **RowType** 요소는 다음 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="8a321-626">속성 (하나 이상)</span><span class="sxs-lookup"><span data-stu-id="8a321-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="8a321-627">예</span><span class="sxs-lookup"><span data-stu-id="8a321-627">Example</span></span>

<span data-ttu-id="8a321-628">다음 예제에서는 사용 하는 저장소 함수를 **CollectionType** 는 반환 행 컬렉션을 지정 하는 요소 (에 지정 된 대로 합니다 **RowType** 요소).</span><span class="sxs-lookup"><span data-stu-id="8a321-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="8a321-629">스키마 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="8a321-630">합니다 **스키마** (SSDL) 저장소 스키마 정의 언어에서 요소는 저장소 모델 정의의 루트 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="8a321-631">이 요소에는 저장소 모델을 구성하는 개체, 함수 및 컨테이너에 대한 정의가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="8a321-632">합니다 **스키마** 요소 0 개 이상의 자식 요소를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="8a321-633">형식 연결</span><span class="sxs-lookup"><span data-stu-id="8a321-633">Association</span></span>
-   <span data-ttu-id="8a321-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="8a321-634">EntityType</span></span>
-   <span data-ttu-id="8a321-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="8a321-635">EntityContainer</span></span>
-   <span data-ttu-id="8a321-636">기능</span><span class="sxs-lookup"><span data-stu-id="8a321-636">Function</span></span>

<span data-ttu-id="8a321-637">합니다 **스키마** 요소를 사용 합니다 **Namespace** 저장소 모델의 엔터티 형식 및 연결 개체에 대 한 네임 스페이스를 정의 하는 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="8a321-638">네임스페이스 내에서 두 개체의 이름이 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="8a321-639">저장소 모델 네임 스페이스의 XML 네임 스페이스와에서 다릅니다. 합니다 **스키마** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="8a321-640">저장소 모델 네임 스페이스 (정의 된 대로 합니다 **Namespace** 특성)은 엔터티 형식 및 연결 형식에 대 한 논리적 컨테이너입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="8a321-641">XML 네임 스페이스 (나타난 합니다 **xmlns** 특성)의 **스키마** 요소는 자식 요소 및 특성에 대 한 기본 네임 스페이스를 **스키마** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="8a321-642">폼의 XML 네임 스페이스 http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (여기서 YYYY, MM은 연도 및 월 각각)는 SSDL 용으로 예약 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="8a321-643">사용자 지정 요소 및 특성은 이러한 형식의 네임스페이스에 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a321-644">적용 가능한 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-644">Applicable Attributes</span></span>

<span data-ttu-id="8a321-645">아래 표에서 특성을 설명에 적용할 수는 **스키마** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="8a321-646">특성 이름</span><span class="sxs-lookup"><span data-stu-id="8a321-646">Attribute Name</span></span>            | <span data-ttu-id="8a321-647">필수 여부</span><span class="sxs-lookup"><span data-stu-id="8a321-647">Is Required</span></span> | <span data-ttu-id="8a321-648">값</span><span class="sxs-lookup"><span data-stu-id="8a321-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-649">**네임스페이스**</span><span class="sxs-lookup"><span data-stu-id="8a321-649">**Namespace**</span></span>             | <span data-ttu-id="8a321-650">예</span><span class="sxs-lookup"><span data-stu-id="8a321-650">Yes</span></span>         | <span data-ttu-id="8a321-651">저장소 모델의 네임스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-651">The namespace of the storage model.</span></span> <span data-ttu-id="8a321-652">값을 **Namespace** 특성 형식의 정규화 된 이름을 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="8a321-653">예를 들어 경우는 **EntityType** 라는 *고객* ExampleModel.Store 네임 스페이스의 정규화 된 이름을를 **EntityType** 는 ExampleModel.Store.Customer입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="8a321-654">다음 문자열 값으로 사용할 수 없습니다는 **Namespace** 특성: **System**를 **일시적인**, 또는 **Edm**합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="8a321-655">값을 **Namespace** 특성에 대 한 값과 같을 수 없습니다는 **Namespace** CSDL 스키마 요소의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="8a321-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="8a321-656">**Alias**</span></span>                 | <span data-ttu-id="8a321-657">아니요</span><span class="sxs-lookup"><span data-stu-id="8a321-657">No</span></span>          | <span data-ttu-id="8a321-658">네임스페이스 이름 대신 사용되는 식별자입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="8a321-659">예를 들어 경우는 **EntityType** 라는 *고객* ExampleModel.Store 네임 스페이스 및 값에는 **별칭** 특성이 *StorageModel*, StorageModel.Customer를 사용 하 여 정규화 된 이름으로는 **EntityType입니다.**</span><span class="sxs-lookup"><span data-stu-id="8a321-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="8a321-660">**공급자**</span><span class="sxs-lookup"><span data-stu-id="8a321-660">**Provider**</span></span>              | <span data-ttu-id="8a321-661">예</span><span class="sxs-lookup"><span data-stu-id="8a321-661">Yes</span></span>         | <span data-ttu-id="8a321-662">데이터 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="8a321-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="8a321-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="8a321-664">예</span><span class="sxs-lookup"><span data-stu-id="8a321-664">Yes</span></span>         | <span data-ttu-id="8a321-665">반환할 공급자 매니페스트를 공급자에게 나타내는 토큰입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="8a321-666">정의된 토큰의 형식은 없으며</span><span class="sxs-lookup"><span data-stu-id="8a321-666">No format for the token is defined.</span></span> <span data-ttu-id="8a321-667">토큰의 값은 공급자가 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="8a321-668">SQL Server 공급자 매니페스트 토큰에 대 한 자세한 내용은 Entity Framework 용 SqlClient를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="8a321-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="8a321-669">예</span><span class="sxs-lookup"><span data-stu-id="8a321-669">Example</span></span>

<span data-ttu-id="8a321-670">다음 예제와 **스키마** 포함 하는 요소는 **EntityContainer** 요소, 두 **EntityType** 요소와 **연결** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="8a321-671">주석 특성</span><span class="sxs-lookup"><span data-stu-id="8a321-671">Annotation Attributes</span></span>

<span data-ttu-id="8a321-672">SSDL(저장소 스키마 정의 언어)의 주석 특성은 저장소 모델의 요소에 대한 추가 메타데이터를 제공하는 저장소 모델의 사용자 지정 XML 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="8a321-673">주석 특성은 올바른 XML 구조를 가져야 할 뿐 아니라 다음 제약 조건을 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="8a321-674">주석 특성은 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="8a321-675">두 주석 특성의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="8a321-676">두 개 이상의 주석 특성을 지정된 SSDL 요소에 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="8a321-677">Annotation 요소에 포함 된 메타 데이터 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 런타임에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-678">예</span><span class="sxs-lookup"><span data-stu-id="8a321-678">Example</span></span>

<span data-ttu-id="8a321-679">다음 예제에서는 주석 특성이 적용 하는 EntityType 요소는 **OrderId** 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="8a321-680">예제도 추가 되는 주석 요소를 표시 합니다 **EntityType** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="8a321-681">Annotation 요소(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="8a321-682">SSDL(저장소 스키마 정의 언어)의 Annotation 요소는 저장소 모델에서 저장소 모델에 대한 추가 메타데이터를 제공하는 사용자 지정 XML 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="8a321-683">Annotation 요소는 올바른 XML 구조를 가져야 함은 물론 다음 제약 조건이 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="8a321-684">Annotation 요소는 SSDL용으로 예약된 XML 네임스페이스에 속하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="8a321-685">두 Annotation 요소의 정규화된 이름은 서로 같을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="8a321-686">Annotation 요소는 제공된 SSDL 요소의 다른 모든 자식 요소 뒤에 나타나야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="8a321-687">두 개 이상의 Annotation 요소가 제공된 SSDL 요소의 자식이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="8a321-688">.NET Framework 버전 4부터 annotation 요소에 포함 된 메타 데이터에 액세스할 수 있습니다 런타임 System.Data.Metadata.Edm 네임 스페이스의 클래스를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="8a321-689">예</span><span class="sxs-lookup"><span data-stu-id="8a321-689">Example</span></span>

<span data-ttu-id="8a321-690">다음 예제에서는 주석 요소에는 EntityType 요소를 보여 줍니다 (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="8a321-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="8a321-691">또한이 예제에서는 주석 특성을 적용 합니다 **OrderId** 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="8a321-692">패싯(SSDL)</span><span class="sxs-lookup"><span data-stu-id="8a321-692">Facets (SSDL)</span></span>

<span data-ttu-id="8a321-693">저장소 스키마 정의 언어 (SSDL)의 패싯은 속성 요소에 지정 된 열 형식에 제약 조건을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="8a321-694">패싯의 XML 특성으로 구현 됩니다 **속성** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="8a321-695">다음 표에서는 SSDL에서 지원되는 패싯에 대해 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="8a321-696">패싯</span><span class="sxs-lookup"><span data-stu-id="8a321-696">Facet</span></span>           | <span data-ttu-id="8a321-697">설명</span><span class="sxs-lookup"><span data-stu-id="8a321-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a321-698">**데이터 정렬**</span><span class="sxs-lookup"><span data-stu-id="8a321-698">**Collation**</span></span>   | <span data-ttu-id="8a321-699">속성 값에 대한 비교 및 순서 지정 작업을 수행할 때 사용할 데이터 정렬 순서 또는 정렬 순서를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="8a321-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="8a321-700">**FixedLength**</span></span> | <span data-ttu-id="8a321-701">열 값 길이가 다양할 수 있는지 여부를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="8a321-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8a321-702">**MaxLength**</span></span>   | <span data-ttu-id="8a321-703">열 값의 최대 길이를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="8a321-704">**전체 자릿수**</span><span class="sxs-lookup"><span data-stu-id="8a321-704">**Precision**</span></span>   | <span data-ttu-id="8a321-705">형식의 속성에 대 한 **10 진수**, 속성 값을 가질 수 있습니다 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="8a321-706">형식의 속성에 대 한 **시간**를 **DateTime**, 및 **DateTimeOffset**, 열 값의 초의 소수 부분 자릿수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="8a321-707">**배율**</span><span class="sxs-lookup"><span data-stu-id="8a321-707">**Scale**</span></span>       | <span data-ttu-id="8a321-708">열 값에 대한 소수점 오른쪽의 자릿수를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="8a321-709">**유니코드**</span><span class="sxs-lookup"><span data-stu-id="8a321-709">**Unicode**</span></span>     | <span data-ttu-id="8a321-710">열 값을 유니코드로 저장할지 여부를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8a321-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |