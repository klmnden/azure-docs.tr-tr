---
title: "SQL veri ambarı kullanıcı tanımlı şemalarda | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse'da Transact-SQL şemalarını kullanma ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: dfb58956ad6637cf0f50b4c052ab98fb7c26139d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Kullanıcı tanımlı şemaları SQL veri ambarı
Geleneksel veri ambarları, ayrı veritabanları genellikle iş yükü, etki alanı veya güvenlik dayanarak uygulama sınırları oluşturmak için kullanın. Örneğin, geleneksel SQL Server veri ambarı Hazırlama veritabanı, veri ambarı veritabanı ve bazı veri reyonu veritabanı içerebilir. Bu topolojide, her veritabanı iş yükü ve güvenlik sınırı mimarisinde olarak çalışır.

Bunun aksine, SQL Data Warehouse bir veritabanına içinde tüm veri ambarı iş yükü çalışır. Veritabanı birleştirmeler izin verilmez. Bu nedenle SQL Data Warehouse bir veritabanı içinde depolanması için ambar tarafından kullanılan bütün tablolar bekliyor.

> [!NOTE]
> SQL veri ambarı veritabanları arası sorguları herhangi bir türde desteklemez. Sonuç olarak, bu deseni yararlanan veri ambarı uygulamaları gözden geçirilmesi gerekir.
> 
> 

## <a name="recommendations"></a>Öneriler
Kullanıcı tanımlı şemalarını kullanarak iş yükleri, güvenlik, etki alanı ve işlevsel sınırları birleştirilmesi için öneriler bunlar

1. Bir SQL Data Warehouse veritabanı, tüm veri ambarı iş yükü çalıştırmak için kullanın
2. Bir SQL Data Warehouse veritabanı kullanmak için mevcut veri ambarı ortamınızı birleştirin
3. Dengeleme **kullanıcı tanımlı şemaları** veritabanları kullanılarak daha önce uygulanan sınırı sağlamak için.

Kullanıcı tanımlı şemaları kullanılmadı önceden sonra temiz bir Kurşun varsa. Yalnızca eski veritabanı adını, kullanıcı tanımlı şemaları SQL veri ambarı veritabanındaki için temel olarak kullanın.

Ardından şemaları zaten kullandıysanız, birkaç seçeneğiniz vardır:

1. Eski şema adlarını kaldırın ve yeniden Başlat
2. Eski şema adları tablo adı için eski şema adını tarafından önceden bekleyen korur
3. Eski şema yapısı yeniden oluşturmak için ek bir şema tablosunda üzerinden görünümleri uygulayarak eski şema adlarını korur.

> [!NOTE]
> İlk denetimi seçenek 3 en cazip seçeneği gibi görünebilir. Ancak, Şeytan ayrıntılı olarak vardır. Görünümler, yalnızca SQL veri ambarında salt okunurdur. Tüm veri veya tabloda değişiklik karşı temel tablo gerçekleştirilmesi gerekir. Seçenek 3, aynı zamanda bir katman görünümlerinin sisteminize sunar. Görünümler, mimarisinde zaten kullanıyorsanız bu bazı ek düşünce vermek isteyebilirsiniz.
> 
> 

### <a name="examples"></a>Örnekler:
Kullanıcı tanımlı şemaları veritabanı adlarına göre uygulama

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

Eski şema adları tarafından önceden bekleyen bunları tablo adı korur. Şemalar, iş yükü sınırlarını kullanın.

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
> Şema stratejisi herhangi bir değişiklik gözden geçirme güvenlik modelinin veritabanı için gerekir. Çoğu durumda, şema düzeyinde izinler atayarak güvenlik modeli basitleştirmek olabilir. Daha ayrıntılı izinler gerekli olduğunda, veritabanı rolleri kullanabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
