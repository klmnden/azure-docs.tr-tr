---
title: Azure SQL veritabanı - Tekliyi yapılandırmak nasıl | Microsoft Docs
description: Yapılandırma ve Azure SQL veritabanı - tek veritabanını yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: d34853220e423e73c6ca8cf7c76ba616b815b8bd
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440656"
---
# <a name="how-to-use-single-database"></a>Tek veritabanı nasıl kullanılır

Bu bölümde çeşitli kılavuzları, betikleri ve Azure SQL veritabanı - tek veritabanı yönetip yardımcı olabilecek açıklamalar bulabilirsiniz.

## <a name="migrate"></a>Geçiş

- [SQL veritabanı'na geçirme](sql-database-cloud-migrate.md) – yönetilen örneğe geçiş araçları ve önerilen geçiş işlemi hakkında bilgi edinin.
- Bilgi edinmek için nasıl [geçişten sonra SQL veritabanını yönetme](sql-database-manage-after-migration.md).

## <a name="configure-features"></a>Yapılandırma özellikleri

- [İşlem çoğaltma yapılandırma](replication-to-sql-database.md) tarihinizden veritabanları arasında çoğaltmak için.
- [Tehdit algılamayı yapılandırma](sql-database-threat-detection.md) Azure SQL veritabanı, SQL ekleme veya şüpheli konumlardan erişim gibi şüpheli etkinlikleri tanımlamak izin vermek için.
- [Dinamik veri maskelemeyi yapılandırma](sql-database-dynamic-data-masking-get-started-portal.md) hassas verilerinizi korumak için.
- [Yedek saklama yapılandırma](sql-database-long-term-backup-retention-configure.md) Azure Blob Depolama alanında Yedeklemelerinizin tutulacağı bir veritabanı için. Var olan bir alternatifi olarak [(kullanım dışı) Azure kasası kullanarak yedekleme bekletmeyi yapılandırma](sql-database-long-term-backup-retention-configure-vault.md) yaklaşım.
- [Coğrafi çoğaltmayı yapılandırma](sql-database-geo-replication-portal.md) başka bir bölgede veritabanınızın bir kopyasını tutmak için.
- [Coğrafi çoğaltmalar için güvenliği yapılandırma](sql-database-geo-replication-security-config.md).

## <a name="monitor-and-tune-your-database"></a>İzleme ve veritabanınızı ayarlayın

- [Otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md) Azure SQL veritabanı'nın İş yükünüzün performansını en iyi duruma izin vermek için.
- [Otomatik ayarlama e-posta bildirimlerini etkinleştirme](sql-database-automatic-tuning-email-notifications.md) ayarlama önerileri hakkında bilgi almak için.
- [Performans önerilerini uygulama](sql-database-advisor-portal.md) ve veritabanınızı iyileştirin.
- [Uyarı oluşturma](sql-database-insights-alerts-portal.md) Azure SQL veritabanı'ndan bildirimleri almak için.
- [Bağlantı sorunlarını giderme](sql-database-troubleshoot-common-connection-issues.md) bazı uygulamalar ve veritabanı arasında bağlantı sorunları fark ederseniz. Ayrıca [bağlantı sorunları için kaynak durumu](sql-database-resource-health.md).
- [Dosya alanı yönetmek](sql-database-file-space-management.md) veritabanı depolama alanı kullanımı izlemek için.

## <a name="query-distributed-data"></a>Sorgu dağıtılmış verileri

- [Dikey olarak bölümlenmiş verileri Sorgulama](sql-database-elastic-query-getting-started-vertical.md) birden çok veritabanı genelinde.
- [Ölçeklendirilmiş veri katmanı arasında rapor](sql-database-elastic-query-horizontal-partitioning.md).
- [Farklı şemalarla tablolar üzerinden sorgu](sql-database-elastic-query-vertical-partitioning.md).

## <a name="elastic-database-jobs"></a>Elastik Veritabanı İşleri

- [Oluşturma ve yönetme](elastic-jobs-powershell.md) PowerShell kullanarak elastik veritabanı işleri.
- [Oluşturma ve yönetme](elastic-jobs-tsql.md) Transact-SQL kullanarak elastik veritabanı işleri.
- [Eski esnek işten geçirme](elastic-jobs-migrate.md).

## <a name="database-sharding"></a>Veritabanı parçalama

- [Yükseltme elastik veritabanı istemci Kitaplığı](sql-database-elastic-scale-upgrade-client-library.md).
- [Parçalı uygulama oluşturma](sql-database-elastic-scale-get-started.md).
- [Yatay olarak parçalı verileri Sorgulama](sql-database-elastic-query-getting-started.md).
- Çalıştırma [çok parçalı sorgular](sql-database-elastic-scale-multishard-querying.md).
- [Parçalı verileri taşıma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
- [Güvenlik Yapılandırma](sql-database-elastic-scale-split-merge-security-configuration.md) veritabanı parçalardaki.
- [Parça ekleme](sql-database-elastic-scale-add-a-shard.md) geçerli veritabanı parçalarını od ayarlayın.
- [Parça eşleme sorunlarını düzeltme](sql-database-elastic-database-recovery-manager.md).
- [Parçalı veritabanını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).
- [Sayaç oluşturma](sql-database-elastic-database-perf-counters.md).
- [Kullanım entity framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) sorgu parçalı veriler için.
- [Kullanım Dapper framework](sql-database-elastic-scale-working-with-dapper.md) sorgu parçalı veriler için.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [yönetilen örneği'nde nasıl yapılır kılavuzları](sql-database-howto-managed-instance.md)
