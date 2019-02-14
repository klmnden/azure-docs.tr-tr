---
title: Temel farklılıklar (R ile) Machine Learning Hizmetleri için Azure SQL veritabanı (Önizleme) genel bakış
description: Bu konuda, Azure SQL veritabanı Machine Learning Hizmetleri (R ile) ve SQL Server Machine Learning Hizmetleri arasındaki önemli farklılıkları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: 4350fb0e75f140e120ba6cd2f074ffa1816a8fce
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237493"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-and-sql-server"></a>Azure SQL veritabanı'nda Machine Learning Hizmetleri ve SQL Server arasındaki farklar

Azure SQL Veritabanı'nda sunulan Machine Learning Services (R ile) işlevleri, [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning) ile benzerdir. Bunlar arasındaki bazı temel farklar aşağıda verilmiştir.

## <a name="language-support"></a>Dil desteği

SQL Server, R ve Python aracılığıyla desteği olan [Genişletilebilirlik Çerçevesi](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework). SQL veritabanı, her iki dil desteklemez. Temel farklılıklar şunlardır:

- R SQL veritabanı'nda yalnızca desteklenen dilidir. Python desteği yoktur.
- R 3.4.4 sürümüdür.
- Yapılandırmak için gerek yoktur `external scripts enabled` aracılığıyla `sp_configure`. Olduktan sonra [kaydolan](sql-database-machine-learning-services-overview.md#signup), makine öğrenimi, SQL veritabanı için etkin.

## <a name="package-management"></a>Paket yönetimi

SQL veritabanı ve SQL Server arasındaki farklı iş R paket yönetimi ve yükleme. Bu farklar şunlardır:

- R paketleri aracılığıyla yüklenen [sqlmlutils](https://github.com/Microsoft/sqlmlutils) veya [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Paketleri giden ağ çağrıları gerçekleştirilemiyor. Bu sınırlama benzer [Machine Learning Hizmetleri için varsayılan güvenlik duvarı kuralları](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) SQL Server'da, SQL veritabanı'nda değiştirilemez, ancak.
- Dış çalışma zamanları (Java gibi) bağımlı veya yükleme veya kullanım için işletim sistemi API'lerine erişmesi gereken paketler desteği yoktur.

## <a name="resource-governance"></a>Kaynak idaresi

R kaynaklarında sınırlamak mümkün değildir [kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzu. R kaynakları SQL veritabanı kaynaklarının yüzdesi ve bağlıdır, hangi hizmet katmanı seçin. Daha fazla bilgi için [Azure SQL veritabanı'nın satın alma modeli](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

## <a name="security-isolation"></a>Güvenlik yalıtımı

Azure SQL veritabanı'nda SQL Platform Soyutlama Katmanı (SQLPAL) dış işlemler için yalıtım sağlar. Bu yalıtım, ek bir R betikleri çalıştırma için güvenlik katmanı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics) genel bilgi için belgelere bakın
- Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda kullanmayı öğrenmek için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-connect-query-r.md).
- Daha fazla bilgi edinin [SQL Server R dili öğreticiler](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)