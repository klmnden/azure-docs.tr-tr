---
title: "Azure SQL veritabanı - otomatik ayarlama | Microsoft Docs"
description: "Azure SQL veritabanını SQL sorgusunun analiz eder ve otomatik olarak kullanıcı iş yüküne uyum sağlar."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 09/19/2017
ms.author: jovanpop
ms.openlocfilehash: 10f8cca8e519b28f0fe29605419b860f73e04a87
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL veritabanı'nda otomatik ayarlama

Azure SQL veritabanı ve otomatik olarak yürütülen sorguları izleyen tam olarak yönetilen veri hizmeti veritabanı iş yükü performansını artırır veritabanıdır. Azure SQL veritabanı sahip yerleşik [otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) otomatik olarak ayarlamak ve dinamik olarak İş yükünüzün veritabanına uyarlama tarafından sorgularınızı performansını Intelligence mekanizması. Azure SQL veritabanı'nda otomatik ayarlama, Azure SQL veritabanındaki sorgu performansını iyileştirmek için etkinleştirebilirsiniz en önemli özelliklerden biri olabilir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="automatic-tuning-options"></a>Otomatik ayarlama seçenekleri

[Otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) Azure SQL veritabanı'nda kullanılabilir seçenekler şunlardır:
 1. **CREATE INDEX** İş yükünüzün performansını artırabilir, dizinler oluşturur ve sorguların performansını artırmak doğrular dizinleri tanımlar.
 2. **DROP INDEX** yedekli ve yinelenen dizinler ve uzun süre içinde kullanılmadı dizinleri tanımlar.
 3. **PLAN REGRESYON düzeltme** , önceki iyi planı yavaş yürütme planı kullanarak SQL sorguları tanımlar ve bilinen son iyi plan gerileyen plan yerine kullanılır.

Azure SQL veritabanı tanımlayan **CREATE INDEX**, **DROP INDEX**, ve **planlama REGRESYON düzeltme** veritabanınızı en iyi duruma getirebilirsiniz ve Azure'da gösterir önerileri Portal. Konumundaki değiştirilmelidir dizinleri tanımlaması hakkında daha fazla bilgi bulmak [dizin önerileri Azure Portalı'nda bulmak](sql-database-advisor-portal.md). Ya da önerileri Portalı'nı kullanarak el ile uygulayabilirsiniz veya otomatik olarak önerileri uygulamak, değişiklikten sonra iş yükünü izlemek için Azure SQL veritabanı sağlar ve önerisi, İş yükünüzün performansını geliştirilmiş doğrulayın.

Seçenekleri ayarlama otomatik bağımsız olarak açmak veya kapatmak veritabanı başına açılabilir veya bunlar kullanılabilir mantıksal sunucusunda yapılandırılan ve sunucudan ayarlarını devralır her veritabanı üzerinde uygulanabilir. Yapılandırma [otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) sunucu ve sunucu veritabanlarında ayarları devralma seçenekleri yöntemi üzerinde otomatik ayarlama seçenekleri yönetimini basitleştirir olduğundan otomatik ayarı yapılandırmak için önerilen bir çok sayıda veritabanı.

Bu makalede adımları için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md) Azure portalını kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı'nda otomatik olarak ayarlamayı etkinleştirmek ve işleminizi iş yükü tam olarak yönetmek otomatik ayarlama özelliği sağlar için bkz: [otomatik ayarlamayı etkinleştirmek](sql-database-automatic-tuning-enable.md).
- El ile ayarlamayı kullanacak şekilde gözden geçirebilirsiniz [önerileri Azure portalında ayarlama](sql-database-advisor-portal.md) ve el ile sorgularınızı performansını olanları uygulayın.
- Daha fazla ayarladığını yerleşik zekaya hakkında bilgi edinin [Azure SQL veritabanı](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Daha fazla bilgi edinin [otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) Azure SQL Database ve SQL Server 2017.
