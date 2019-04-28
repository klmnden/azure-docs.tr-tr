---
title: SQL veri ambarı'nda kullanıcı tanımlı şemalar kullanarak | Microsoft Docs
description: T-SQL kullanıcı tanımlı şemalar çözümleri geliştirmek için Azure SQL veri ambarı'nda kullanma hakkında ipuçları.
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 04/17/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: ae017461767a207deae1d990980258a1f661df3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61439154"
---
# <a name="using-user-defined-schemas-in-sql-data-warehouse"></a>SQL veri ambarı'nda kullanıcı tanımlı şemalar kullanma
T-SQL kullanıcı tanımlı şemalar çözümleri geliştirmek için Azure SQL veri ambarı'nda kullanma hakkında ipuçları.

## <a name="schemas-for-application-boundaries"></a>Uygulama sınırları için şemalar

Geleneksel veri ambarları, ayrı veritabanları genellikle iş yükü, etki alanı veya güvenlik dayanarak uygulama sınırları oluşturmak için kullanın. Örneğin, geleneksel bir SQL Server veri ambarı Hazırlama veritabanı, veri ambarı veritabanını ve bazı veri reyonu veritabanı içerebilir. Bu topolojide, her veritabanı bir iş yükü ve mimarisinde güvenlik sınırı olarak çalışır.

Bunun aksine, SQL veri ambarı, tüm veri ambarı iş yükünün bir veritabanı içinde çalışır. Veritabanı birleştirmeler izin verilmez. Bu nedenle SQL veri ambarı, bir veritabanı içinde depolanacak ambar tarafından kullanılan tüm tablolar bekliyor.

> [!NOTE]
> SQL veri ambarı, herhangi bir türdeki çapraz veritabanı sorguları desteklemez. Sonuç olarak, bu düzen yararlanarak veri ambarı uygulamaları gözden geçirilmesi gerekir.
> 
> 

## <a name="recommendations"></a>Öneriler
Kullanıcı tanımlı şemalar kullanarak iş yüklerini, güvenlik, etki alanı ve işlevsel sınırları birleştirilmesi için öneriler şunlardır

1. Tüm veri ambarı iş yükü çalıştırmak için bir SQL veri ambarı veritabanını kullan
2. Bir SQL veri ambarı veritabanını kullanmak üzere mevcut veri ambarı ortamınızı birleştirin
3. Gücünden yararlanarak **kullanıcı tanımlı şemalar** veritabanları kullanılarak daha önce uygulanan sınırı sağlamak için.

Temiz bir maskeleme görüntüsü sahip kullanıcı tanımlı şemalar daha önce kullanılmamış durumunda. Yalnızca eski bir veritabanı adı, kullanıcı tanımlı şemalar SQL veri ambarı veritabanındaki için temel olarak kullanın.

Şemaları zaten kullandıysanız, birkaç seçeneğiniz vardır:

1. Eski şema adları kaldırın ve yeni başlangıç
2. Eski şema adları tablo adı için eski şema adı tarafından önceden bekleyen koru
3. Eski şema yapısı yeniden oluşturmak için ek bir şema tablosunda üzerinden görünümlerini uygulayarak eski şema adlarını korur.

> [!NOTE]
> İlk denetimi seçeneği 3 en cazip seçeneği gibi görünebilir. Ancak, ayrıntılı olarak Şeytan olur. Görünümleri yalnızca SQL veri ambarı'nda okunur. Veri veya tabloda değişiklik karşı temel tablo gerçekleştirilmesi gerekir. Seçenek 3, sisteminize görünümleri katmanı da tanıtılmaktadır. Görünümler içinde Mimarinizi zaten kullanıyorsanız bu bazı ek düşünce vermek isteyebilirsiniz.
> 
> 

### <a name="examples"></a>Örnekler:
Kullanıcı tanımlı şemalar veritabanı adlarına göre uygulayın

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Eski şema adlarına göre önceden bekleyen bunları tablo adı için korur. Şemalar, iş yükü sınırlarını kullanın.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Görünümleri kullanarak eski şema adlarını korur

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Şema stratejisi herhangi bir değişiklik güvenlik modelinin bir gözden geçirme veritabanı için gerekir. Çoğu durumda şema düzeyinde izinler atayarak güvenlik modelini basitleştirmek mümkün olabilir. Daha ayrıntılı izinler gerekli olduğunda, veritabanı rolleri kullanabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

