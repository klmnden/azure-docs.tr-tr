---
title: Azure SQL veritabanı sürüm notları | Microsoft Docs
description: Yeni özellikler ve geliştirmeler Azure SQL veritabanı hizmeti ve Azure SQL veritabanı belgeleri hakkında bilgi edinin
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: sstein
ms.openlocfilehash: d527c4fed9c43e62d815078c049d4d8e6f8a46b7
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787708"
---
# <a name="sql-database-release-notes"></a>SQL veritabanı sürüm notları

Bu makalede, SQL veritabanı hizmeti ve SQL veritabanı belgeleri geliştirmeleri ve yeni özellikleri listeler. SQL veritabanı hizmet geliştirmeleri için Ayrıca bkz: [SQL veritabanı hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=sql-database). Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Özellikleri Genel önizlemeye sunuldu

| Özellik | Ayrıntılar |
| ---| --- |
| Tek veritabanları ve elastik havuzlar ile hızlandırılmış veritabanı kurtarma | Bilgi için [hızlandırılmış veritabanı kurtarma](sql-database-accelerated-database-recovery.md).|
|Farklı yaklaşık sayısı|Bilgi için [yaklaşık ayrı Say](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Toplu iş modu Rowstore (altında uyumluluk düzeyi 150)|Bilgi için [Rowstore toplu iş modu](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Veri bulma ve sınıflandırma  |Bilgi için [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md).|
| Elastik veritabanı işleri | Bilgi için [oluşturun, yapılandırın ve elastik işleri yönetme](elastic-jobs-overview.md). |
| Esnek sorgular | Bilgi için [esnek sorgu genel bakış](sql-database-elastic-query-overview.md). |
| Elastik işlemler | [Bulut veritabanlarında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md). |
| Örneği harmanlamasıyla yönetilen örnekleri |Bilgi için [Azure SQL veritabanı'nda yönetilen örnek oluşturma için PowerShell kullanarak Azure Resource Manager şablonu ile](./scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md).|
|(Altında uyumluluk düzeyi 150) bellek ataması geri bildirimi (satır modu)|Bilgi için [bellek ataması geri bildirimi (satır modu)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Azure portalındaki sorgu Düzenleyicisi |Bilgi için [bağlanmak ve veri sorgulamak için Azure portalında SQL sorgu Düzenleyicisi'ni kullanın](sql-database-connect-query-portal.md).|
| R Hizmetleri / makine öğrenimi ile tek veritabanları ve elastik havuzlar |Bilgi için [Machine Learning Hizmetleri Azure SQL veritabanı'nda](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
| Bırakılan veritabanı yönetilen örneği ile yeniden oluşturun |Bilgi için [Azure SQL yönetilen örneği'nde bırakılan veritabanlarını yeniden oluşturma](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266).|
| Çoğaltma ile yönetilen örnekleri |Bilgi için [bir Azure SQL veritabanı yönetilen örnek veritabanında çoğaltma yapılandırma](replication-with-sql-database-managed-instance.md).|
| Sunucusuz işlem katmanı | Bilgi için [sunucusuz SQL veritabanı (Önizleme)](sql-database-serverless.md).|
|SQL analizi|Bilgi için [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).|
|Tablo değişkeni ertelenmiş derleme (altında uyumluluk düzeyi 150)|Bilgi için [tablo değişkeni ertelenmiş derleme](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| Yönetilen örnek tehdit algılama |Bilgi için [yapılandırma tehdit algılama, Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-threat-detection.md).|
| Yönetilen örnek için saat dilimi desteği|Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği saat diliminde](sql-database-managed-instance-timezone.md).|
| Saydam veri şifrelemesi (TDE) ile Getir bilgisayarınızı kendi anahtarını (BYOK) yönetilen örnekleri |Bilgi için [Azure SQL saydam veri şifrelemesi ile Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar: Destek kendi anahtarını Getir](transparent-data-encryption-byok-azure-sql.md).|
| &nbsp; |

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
|Hiper ölçekli hizmet katmanı genel kullanıma sunuldu| Daha fazla bilgi için [hiper ölçekli hizmet katmanı için en fazla 100 TB](sql-database-service-tier-hyperscale.md) ve [yüksek performans için Azure veritabanı iş yüklerinizi hiper ölçekli ile ölçeklendirme](https://azure.microsoft.com/blog/get-high-performance-scaling-for-your-azure-database-workloads-with-hyperscale/).|
|Sunucusuz bilgi işlem katmanı genel önizlemede kullanıma | Daha fazla bilgi için [sunucusuz SQL veritabanı (Önizleme)](sql-database-serverless.md).|
| Sanal çekirdek tabanlı satın alma modelini kullanan veritabanları için artırılmış işlem günlüğü hızları ve hedef IOPS| Daha fazla bilgi için [sanal çekirdek tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases) ve [kaynak sınırları tek veritabanları için DTU tabanlı satın alma modeli kullanarak](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases).
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| GA sürümü için güncelleştirilmiş hiper ölçekli hizmet katmanı belgeleri| Daha fazla bilgi için [hiper ölçekli hizmet katmanı için en fazla 100 TB](sql-database-service-tier-hyperscale.md).|
|Genel Önizleme sürümü ile sunulan sunucusuz bilgi işlem katmanı belgeleri| Daha fazla bilgi için [sunucusuz SQL veritabanı (Önizleme)](sql-database-serverless.md).|
| &nbsp; |

## <a name="april-2019"></a>Nisan 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| Ortak uç noktalar için yönetilen örnek genel önizlemeye sunuldu| Daha fazla bilgi için [kullanarak Azure SQL veritabanı yönetilen örnek genel uç noktası ile güvenli bir şekilde](sql-database-managed-instance-public-endpoint-securely.md).|
| Saat dilimi desteği yönetilen örnek genel önizlemeye sunuldu| Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği saat diliminde](sql-database-managed-instance-timezone.md).|
| Sürüm güvenliğini sağlama Azure SQL veritabanları yönetilen kimliklerle ikinci genel önizlemeye sunuldu| Bkz: [güvenliğini sağlama, Azure SQL veritabanları yönetilen kimliklerle yalnızca kolaylaştı](https://azure.microsoft.com/blog/securing-azure-sql-databases-with-managed-identities-just-got-easier/).|
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| Ortak uç noktalar için yönetilen örnek genel önizlemeye sunuldu| Daha fazla bilgi için [kullanarak Azure SQL veritabanı yönetilen örnek genel uç noktası ile güvenli bir şekilde](sql-database-managed-instance-public-endpoint-securely.md).|
| Saat dilimi desteği yönetilen örnek genel önizlemeye sunuldu| Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği saat diliminde](sql-database-managed-instance-timezone.md). |
| Azure SQL veritabanı'nda Kaynak İdaresi | Daha fazla bilgi için [kaynak İdaresi Azure SQL veritabanı'nda](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/). || &nbsp; |

## <a name="march-2019"></a>Mart 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| Azure SQL veritabanı için okuma ölçeği genişletme desteği genel kullanıma yönelik | Daha fazla bilgi için [okuma ölçeği genişletme](sql-database-read-scale-out.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| Tek veritabanları için eklenen günlük sınırları|Daha fazla bilgi için [tek veritabanı sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md).|
| Elastik havuzlara ve havuza alınmış veritabanlarını eklenen günlük sınırları|Daha fazla bilgi için [elastik havuz sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md).|
| Ek işlem günlüğü oranı idare| Eklenen yeni içerikleri [işlem günlüğü oranı idare](sql-database-resource-limits-database-server.md#transaction-log-rate-governance).|
| Tek veritabanları ve elastik havuzlar az.sql modülü kullanmak için güncelleştirilmiş PowerShell örnekleri | Daha fazla bilgi için [tek veritabanları ve elastik havuzlar için PowerShell örnekleri](sql-database-powershell-samples.md#single-database-and-elastic-pools).|
| &nbsp; |

## <a name="february-2019"></a>Şubat 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
|Sürdürülebilir bir çevrimiçi dizin oluşturma genel kullanıma sunulmuştur| Daha fazla bilgi için [Create Index](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql).|
|Yönetilen örnek geliştirilmiş rota tabloları desteği| Daha fazla bilgi için [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements).|
|Yönetilen örneğinde desteklenen veritabanını yeniden adlandırma | Daha fazla ayrıntı için [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) ve [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql) söz dizimi.|
|Stream Analytics için başvuru veri kaynağı olarak SQL veritabanı. | Daha fazla bilgi için [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) ve [Azure Stream Analytics artık Azure SQL veritabanı başvuru veri girişi olarak desteklemektedir](https://azure.microsoft.com/blog/azure-stream-analytics-now-supports-azure-sql-database-as-reference-data-input/).|
|Veri geçiş Yardımcısı, yönetilen örnek için destek ekler. |Daha fazla bilgi için [DMA'da yenilikler](https://docs.microsoft.com/sql/dma/dma-whatsnew).|
|SQL Server Geçiş Yardımcısı, yönetilen örnek için hedef hazır olma durumu değerlendirmesi için destek ekler. | Daha fazla bilgi için [SQL Server Geçiş Yardımcısı](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant).
|Veri geçiş hizmeti Amazon RDS yönetilen örneğine geçişi destekler. | Daha fazla bilgi için [Öğreticisi: RDS SQL Server'ı Azure SQL veritabanı'na geçirme veya Azure SQL veritabanı yönetilen örneği çevrimiçi DMS kullanarak](../dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
|Örnek dağıtım seçeneği açıklamalar yönetilen ekleme|Yönetilen örnek dağıtım seçenekleri tek veritabanı ve elastik havuz için Uygulanabilirlik açıklamak için birçok makale güncelleştirildi. |
|DTU tabanlı satın alma modeli güncelleştirilmiş tempdb boyutları | Daha fazla bilgi için [SQL veritabanı'nda veritabanı Tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
|Güncelleştirilmiş içeri ve dışarı aktarma bacpac dosyasını yönetilen örnek desteği| Daha fazla bilgi için [BACPAC alma](sql-database-import.md) ve [dışarı aktarmak için BACPAC](sql-database-export.md). |
| &nbsp; |

## <a name="january-2019"></a>Ocak 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| İşlem kaynakları için ek ayrıntı düzeyi seçenekleri | Genel amaçlı ve iş açısından kritik hizmet katmanları için [tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) artık daha fazla ayrıntılı işlem seçeneğiniz vardır.|
| Azure portalında yönetilen örnek için Denetim kayıtlarını görüntüleme | Görüntüleme [Denetim kayıtlarını yönetilen örnekleri için](sql-database-managed-instance-auditing.md) Azure portalı artık desteklenmektedir.|
| Gelişmiş tehdit algılama özelliği gelişmiş veri güvenliği için yeniden adlandırıldı | Gelişmiş tehdit algılama özelliği olarak yeniden adlandırıldı [gelişmiş veri güvenliği](sql-advanced-threat-protection.md) tek veritabanları, elastik havuzlar ve yönetilen örnekleri. |
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| Yönetilen örnekleri ve işlemsel çoğaltma | Kullanma hakkında ek makale [işlem çoğaltma ile yönetilen örnekleri](replication-with-sql-database-managed-instance.md). |
| Yönetilen örnek öğretici Azure AD'ye eklendi | Bu [Azure AD ile yönetilen örnek](sql-database-managed-instance-aad-security-tutorial.md) yönetilen örnek güvenlik Azure AD oturum açma bilgilerinin kullanılması yapılandırmak ve sınamak için sahip olduğunuz gösterilmektedir. |
| Transact-SQL betiklerini kullanarak işlem otomasyonu için güncelleştirilmiş içerik | Güncelleştirilen ve kullanmak için içerik açıklığa kavuşturuldu [Transact-SQL betiklerini kullanarak proje Otomasyon](sql-database-job-automation-overview.md) tek veritabanları, elastik havuzlar ve yönetilen örnekleri. |
| Güncelleştirilmiş yönetilen örnek için güvenlik içeriği | Güncelleştirilen ve içeriği açıklığa kavuşturuldu [yönetilen örnek için güvenlik modeli](sql-database-security-overview.md), benzerlikler ve karşıtlıklar tek veritabanları ve elastik havuzlar için güvenlik modeli açın. |
| Tüm Hızlı başlangıçlar ve öğreticilerle yenilenir | Tüm Hızlı başlangıçları ve öğreticileri, [belgeleri](https://docs.microsoft.com/azure/sql-database) güncelleştirilmiş ve Azure portalında değişikliklerle eşleştirmek için yenilenir. |
| Eklenen hızlı genel bakış kılavuzları | Hızlı genel bakış Kılavuzu eklenen [tek veritabanları](sql-database-quickstart-guide.md) ve [yönetilen örnekleri](sql-database-managed-instance-quickstart-guide.md). |
| Ek SQL veritabanı terimler sözlüğü | Bu [terimleri sözlüğü](sql-database-glossary-terms.md) makalede, SQL veritabanı hüküm ve bağlam terimini açıklayan birincil kavramsal sayfasına bağlantı eksiksiz bir listesini sağlar. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>İçerik gelişimine katkıda bulunmasını

Açık kaynak Azure SQL belgeleri kümesidir. Açık çalışmanın bazı avantajları vardır:

- Açık kaynak depolar, hangi belgelere en çok ihtiyaç olduğuna hakkında geri bildirim almak için açık planlayın.
- Açık kaynak depolar, ilk sürümde en faydalı içeriği yayımlamak için açık olarak gözden geçirin.
- Açık kaynak depolar, içeriği sürekli olarak geliştirmenize daha kolay hale getirmek için açık olarak güncelleştirin.

Azure SQL veritabanı belgeleri içeriklerine katkıda bulunmak için bkz: [Microsoft Docs katkıda bulunan Kılavuzu Genel Bakış](https://docs.microsoft.com/contribute/). Kullanıcı deneyimi üzerinde [docs.microsoft.com](https://docs.microsoft.com/) tümleşir [GitHub](https://github.com/) doğrudan daha kolay yapmak için iş akışları. Başlayın [görüntülediğiniz belgeyi düzenleyerek](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). Ya da tarafından Yardım [yeni konuları inceleyerek](https://docs.microsoft.com/contribute/#review-open-prs), veya [kalite sorunları oluşturarak](https://docs.microsoft.com/contribute/#create-quality-issues).
