---
title: Azure SQL veritabanı DTU tabanlı kaynak sınırları tek veritabanları | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı'nda tek veritabanları için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/20/2019
ms.openlocfilehash: 0e4d87ee0d0d09a84e960d511ded87dc226515ea
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762676"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları

Bu makalede, Azure SQL veritabanı tek veritabanı DTU tabanlı satın alma modeli kullanarak için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli kaynak sınırları için elastik havuzlar için bkz: [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md). Sanal çekirdek tabanlı kaynak sınırları için bkz: [sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md). Farklı satın alma modeli hakkında daha fazla bilgi için bkz. [modelleri ve hizmet katmanlarını satın](sql-database-purchase-models.md).

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Tek veritabanı: Depolama boyutlarına ve işlem boyutları

Aşağıdaki tablolarda her hizmet katmanında tek bir veritabanı için kullanılabilir kaynakları göster ve işlem boyutu. Hizmet katmanı, işlem boyutu ve depolama alanı miktarı kullanarak tek veritabanı için ayarlayabileceğiniz [Azure portalında](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases), veya [REST API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Bkz. yönergeler ve önemli noktalar ölçekleme için [tek bir veritabanının ölçeğini](sql-database-single-database-scale.md)

### <a name="basic-service-tier"></a>Temel hizmet katmanı

| **İşlem boyutu** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Dahil edilen depolama alanı (GB) | 2 |
| En fazla depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| Maks. eş zamanlı çalışan (istek) | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

### <a name="standard-service-tier"></a>Standart hizmet katmanı

| **İşlem boyutu** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maks. DTU | 10 | 20 | 50 | 100 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| Maks. eş zamanlı çalışan (istek)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanında (devam)

| **İşlem boyutu** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maks. DTU | 200 | 400 | 800 | 1600 | 3000 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| Maks. eş zamanlı çalışan (istek)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı

| **İşlem boyutu** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Dahil edilen depolama alanı (GB) | 500 | 500 | 500 | 500 | 4096* | 4096* |
| En fazla depolama seçenekleri (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096* | 4096* |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maks. eş zamanlı çalışan (istek)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

\* 1024 GB 4096 GB artışlarla 256 GB

> [!IMPORTANT]
> 1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Kuzey Çin, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, US DoD bölgeler ve ABD kamu orta. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır.  Daha fazla bilgi için [P11 P15 geçerli sınırlamalar](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).  
> [!NOTE]
> İçin `tempdb` limitleri bkz [tempdb sınırları](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database).

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için sanal çekirdek kaynak limitleri için bkz. [sanal çekirdek tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md)
- Elastik havuzlar için sanal çekirdek kaynak limitleri için bkz. [sanal çekirdek tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md)
- Elastik havuzlar için DTU kaynak limitleri için bkz. [DTU tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md)
- Yönetilen örnek için kaynak limitleri için bkz [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Bir veritabanı sunucusunda kaynak sınırları hakkında daha fazla bilgi için bkz [kaynak sınırları üzerinde bir SQL veritabanı sunucusuna genel bakış](sql-database-resource-limits-database-server.md) sunucu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
