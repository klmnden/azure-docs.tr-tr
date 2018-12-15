---
title: Azure SQL veritabanı yapılandırma | Microsoft Docs
description: Yapılandırma ve Azure SQL veritabanı'nı yönetme hakkında bilgi edinin.
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
ms.openlocfilehash: b4dd21324591075d7625a82fbbb661c4a8e84b1d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440509"
---
# <a name="how-to-use-azure-sql-database"></a>Azure SQL veritabanı nasıl kullanılır

Bu bölümde çeşitli kılavuzları, betikleri ve Azure SQL veritabanınızı yönetip yardımcı olabilecek açıklamalar bulabilirsiniz. Belirli nasıl yapılır kılavuzlarını da bulabilirsiniz [tek veritabanı](sql-database-howto-single-database.md) ve [yönetilen örneği](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Veri yükleme

- [Azure içinde tek bir veritabanını kopyalama](https://docs.microsoft.com/azure/sql-database/sql-database-copy)
- [Veritabanını BACPAC içeri aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-import)
- [Bir veritabanını BACPAC için dışarı aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-export)
- [BCP ile veri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-load-from-csv-with-bcp)
- [ADF ile veri yükleme](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Veri eşitleme

- [SQL Data Sync'i](https://docs.microsoft.com/azure/sql-database/sql-database-sync-data)
- [Veri Eşitleme Aracısı](https://docs.microsoft.com/azure/sql-database/sql-database-data-sync-agent)
- [Şema değişikliklerini çoğaltın](https://docs.microsoft.com/azure/sql-database/sql-database-update-sync-schema)
- [OMS ile izleme](https://docs.microsoft.com/azure/sql-database/sql-database-sync-monitor-oms)
- [Data Sync için en iyi uygulamalar](https://docs.microsoft.com/azure/sql-database/sql-database-best-practices-data-sync)
- [Data Sync sorunlarını giderme](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>İzleme ve ayarlama

-  [El ile ayarlama](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
- [Performansını izlemek için Dmv'leri kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
- [Sorgu deposu performansını izlemek için kullanın](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)
- [Intelligent Insights ile performans sorunlarını giderme](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance)
- [Akıllı Öngörüler Tanılama Günlüğü kullanın](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-use-diagnostics-log)
- [Bellek içi OLTP alanı izleme](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>Genişletilmiş olaylar

- [Genişletilmiş olaylar](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)
- [Olay dosyasına Store genişletilmiş olaylar](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-event-file)
- [Halka arabelleği içine Store genişletilmiş olaylar](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>Yapılandırma özellikleri

- [Azure AD kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)
- [Koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- [Çok faktörlü AAD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Çok faktörlü kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure)
- [Zamana bağlı bekletme ilkesini yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
- [BYOK ile TDE yapılandırma](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure)
- [TDE BYOK anahtarlarını döndürme](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-key-rotation)
- [TDE koruyucusuna Kaldır](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)
- [Bellek içi OLTP’yi yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-migration)
- [Azure Otomasyonu yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>Uygulama geliştirme

- [Bağlantı](https://docs.microsoft.com/azure/sql-database/sql-database-libraries)
- [Spark Bağlayıcısı kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-spark-connector)
- [Uygulamanın kimliğini doğrulama](https://docs.microsoft.com/azure/sql-database/sql-database-client-id-keys)
- [Hata iletileri](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)
- [Daha iyi performans için toplu işlem kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)
- [Bağlantı kılavuzu](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)
- [DNS diğer adları](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)
- [DNS diğer adı PowerShell Kurulumu](https://docs.microsoft.com/azure/sql-database/dns-alias-powershell)
- [Bağlantı noktaları - ADO.NET](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C ve C++](https://docs.microsoft.com/azure/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](https://docs.microsoft.com/azure/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>Uygulamaları tasarlama

- [Olağanüstü durum kurtarma tasarımı](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [Elastik havuzlar için Tasarım](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [Uygulama yükseltmeleri için Tasarım](https://docs.microsoft.com/azure/sql-database/sql-database-manage-application-rolling-upgrade)

### <a name="design-multi-tenant-saas-applications"></a>Çok kiracılı SaaS uygulamaları tasarlama

- [SaaS tasarım desenleri](https://docs.microsoft.com/azure/sql-database/saas-tenancy-app-design-patterns)
- [SaaS video dizinleyicisi](https://docs.microsoft.com/azure/sql-database/saas-tenancy-video-index-wingtip-brk3120-20171011)
- [SaaS uygulama güvenliği](https://docs.microsoft.com/azure/sql-database/saas-tenancy-elastic-tools-multi-tenant-row-level-security)

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [nasıl yapılır kılavuzları yönetilen örneği'nde](sql-database-howto-managed-instance.md).
- Daha fazla bilgi edinin [nasıl yapılır kılavuzları, tek bir veritabanında](sql-database-howto-single-database.md).
