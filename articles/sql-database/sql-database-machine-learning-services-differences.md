---
title: Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) için temel farklılıklar
description: Bu konuda, Azure SQL veritabanı Machine Learning Hizmetleri (R ile) ve SQL Server Machine Learning Hizmetleri arasındaki önemli farklılıkları açıklar.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: 92785015a1ce122b8301b56fa62d122c8d95180c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64725045"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Machine Learning Hizmetleri (Önizleme) Azure SQL veritabanında SQL Server arasındaki temel farklılıklar

Azure SQL veritabanı Machine Learning Hizmetleri (R ile) işlevselliğini (önizlemede) benzer [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Bazı temel farklar aşağıda verilmiştir.

> [!IMPORTANT]
> Azure SQL veritabanı Machine Learning Hizmetleri şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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

R kaynaklarında sınırlamak mümkün değildir [kaynak İdarecisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzu.

Genel Önizleme sırasında R kaynakları en fazla %20 SQL veritabanı kaynaklarının ayarlanır ve bağımlı, hangi hizmet katmanı seçin. Daha fazla bilgi için [Azure SQL veritabanı'nın satın alma modeli](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için kullanılabilir bellek yetersiz ise bir hata iletisi alırsınız. Genel hata iletileri şunlardır:

- İstek Kimliği için 'R' betiği için çalışma zamanı ile iletişim kurulamıyor: ***. Lütfen 'R' çalışma zamanı gereksinimlerini kontrol edin
- HRESULT 0x80004004 ile 'sp_execute_external_script' yürütülürken 'R' betik hatası oluştu. ... Dış betik hatası oluştu: "... C işlev 'R_AllocStringBuffer' (0 Mb) bellek ayrılamadı."
- Dış betik hatası oluştu: Hata: boyutunun vektör ayrılamıyor.

Bellek kullanımı üzerinde ne kadar bağımlı R betikleriniz ve paralel sorgular çalıştırılmasını sayısı kullanılır. Yukarıdaki hataları alırsanız, bu sorunu çözmek için daha yüksek bir hizmet katmanına veritabanına ölçeklendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: genel bakış [R (Önizleme) ile Azure SQL veritabanı Machine Learning Hizmetleri](sql-database-machine-learning-services-overview.md).
- R sorguyu Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) kullanmayı öğrenin için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-connect-query-r.md).
- Bazı basit R betikleriyle çalışmaya başlamak için bkz. [oluşturma ve Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde basit R betikleri çalıştırma](sql-database-quickstart-r-create-script.md).
