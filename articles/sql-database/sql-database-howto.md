---
title: Azure SQL veritabanı yapılandırma | Microsoft Docs
description: Yapılandırma ve Azure SQL veritabanı'nı yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 7db9c6400ac7d235153a59965e34e30d9b809a81
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59359704"
---
# <a name="how-to-use-azure-sql-database"></a>Azure SQL veritabanı nasıl kullanılır

Bu bölümde çeşitli kılavuzları, betikleri ve Azure SQL veritabanınızı yönetip yardımcı olabilecek açıklamalar bulabilirsiniz. Belirli nasıl yapılır kılavuzlarını da bulabilirsiniz [tek veritabanı](sql-database-howto-single-database.md) ve [yönetilen örneği](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Veri yükleme

- [Tek veritabanı veya Azure içinde havuza veritabanı kopyalama](sql-database-copy.md)
- [Veritabanını BACPAC içeri aktarma](sql-database-import.md)
- [Bir veritabanını BACPAC için dışarı aktarma](sql-database-export.md)
- [BCP ile veri yükleme](sql-database-load-from-csv-with-bcp.md)
- [ADF ile veri yükleme](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Veri eşitleme

- [SQL Data Sync](sql-database-sync-data.md)
- [Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
- [Şema değişikliklerini çoğaltın](sql-database-update-sync-schema.md)
- [OMS ile izleme](sql-database-sync-monitor-oms.md)
- [Data Sync için en iyi uygulamalar](sql-database-best-practices-data-sync.md)
- [Data Sync sorunlarını giderme](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>İzleme ve ayarlama

- [El ile ayarlama](sql-database-performance-guidance.md)
- [Performansını izlemek için Dmv'leri kullanma](sql-database-monitoring-with-dmvs.md)
- [Sorgu deposu performansını izlemek için kullanın](sql-database-operate-query-store.md)
- [Intelligent Insights ile performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Akıllı Öngörüler Tanılama Günlüğü kullanın](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Bellek içi OLTP alanı izleme](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Genişletilmiş olaylar

- [Genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)
- [Olay dosyasına Store genişletilmiş olaylar](sql-database-xevent-code-event-file.md)
- [Halka arabelleği içine Store genişletilmiş olaylar](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Yapılandırma özellikleri

- [Azure AD kimlik doğrulamasını yapılandırma](sql-database-aad-authentication-configure.md)
- [Koşullu erişimi yapılandırma](sql-database-conditional-access.md)
- [Çok faktörlü AAD kimlik doğrulaması](sql-database-ssms-mfa-authentication.md)
- [Çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md)
- [Zamana bağlı bekletme ilkesini yapılandırma](sql-database-temporal-tables-retention-policy.md)
- [BYOK ile TDE yapılandırma](transparent-data-encryption-byok-azure-sql-configure.md)
- [TDE BYOK anahtarlarını döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [TDE koruyucusuna Kaldır](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Bellek içi OLTP’yi yapılandırma](sql-database-in-memory-oltp-migration.md)
- [Azure Otomasyonu yapılandırma](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Uygulama geliştirme

- [Bağlantı](sql-database-libraries.md)
- [Spark Bağlayıcısı kullanma](sql-database-spark-connector.md)
- [Uygulamanın kimliğini doğrulama](sql-database-client-id-keys.md)
- [Hata iletileri](sql-database-develop-error-messages.md)
- [Daha iyi performans için toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md)
- [Bağlantı kılavuzu](sql-database-connectivity-issues.md)
- [DNS diğer adları](dns-alias-overview.md)
- [DNS diğer adı PowerShell Kurulumu](dns-alias-powershell.md)
- [Bağlantı noktaları - ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C ve C++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Uygulamaları tasarlama

- [Olağanüstü durum kurtarma tasarımı](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Elastik havuzlar için Tasarım](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Uygulama yükseltmeleri için Tasarım](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Çok kiracılı SaaS uygulamaları tasarlama

- [SaaS tasarım desenleri](saas-tenancy-app-design-patterns.md)
- [SaaS video dizinleyicisi](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS uygulama güvenliği](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [nasıl yapılır kılavuzları için yönetilen örnekleri](sql-database-howto-managed-instance.md).
- Daha fazla bilgi edinin [nasıl yapılır kılavuzları, tek veritabanları için](sql-database-howto-single-database.md).
