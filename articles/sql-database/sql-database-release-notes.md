---
title: Azure SQL veritabanı sürüm notları | Microsoft Docs
description: Yeni özellikler ve geliştirmeler Azure SQL veritabanı hizmeti ve Azure SQL veritabanı belgeleri hakkında bilgi edinin
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: carlrab
ms.openlocfilehash: 9b961436c81282381f963d16c6c6dd5f289d1259
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495114"
---
# <a name="sql-database-release-notes"></a>SQL veritabanı sürüm notları

Bu makalede, SQL veritabanı hizmeti ve SQL veritabanı belgeleri geliştirmeleri ve yeni özellikleri listeler. SQL veritabanı hizmet geliştirmeleri için Ayrıca bkz: [SQL veritabanı hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=sql-database). Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Özellikleri Genel önizlemeye sunuldu

| Özellik | Ayrıntılar |
| ---| --- |
| Esnek veritabanı işleri | Bilgi için [oluşturun, yapılandırın ve elastik işleri Yönet](elastic-jobs-overview.md) |
| Elastik işlemler | [Bulut veritabanlarında dağıtılmış işlemler](sql-database-elastic-transactions-overview.md) |
| Esnek sorgular | Bilgi için [esnek sorgu genel bakış](sql-database-elastic-query-overview.md) |
| Çoğaltma ile yönetilen örnekleri |Bilgi için [bir Azure SQL veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma](replication-with-sql-database-managed-instance.md)|
| Örneği harmanlamasıyla yönetilen örnekleri |Bilgi için [kullanım PowerShell'i Azure Resource Manager şablonu ile Azure SQL veritabanı'nda yönetilen örnek oluşturma](./scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| R Hizmetleri / makine öğrenimi ile tek veritabanları ve elastik havuzlar |Bilgi için [Azure SQL veritabanı'nda Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database)|
| Tek veritabanları ve elastik havuzlar ile hızlandırılmış veritabanı kurtarma | Bilgi için [hızlandırılmış veritabanı kurtarma](sql-database-accelerated-database-recovery.md)|
| Veri bulma ve sınıflandırma  |Bilgi için [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md)|
| Saydam veri şifrelemesi (TDE) ile Getir bilgisayarınızı kendi anahtarını (BYOK) yönetilen örnekleri |Bilgi için [Azure SQL saydam veri şifrelemesi ile Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar: Destek kendi anahtarını Getir](transparent-data-encryption-byok-azure-sql.md)|
| Bırakılan veritabanı yönetilen örneği ile yeniden oluşturun |Bilgi için [Azure SQL yönetilen örneği'nde bırakılan veritabanlarını yeniden oluşturma](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266)|
| Yönetilen örnek tehdit algılama |Bilgi için [yapılandırma tehdit algılama, Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-threat-detection.md)|
| Tek veritabanları ile hiper ölçekli hizmet katmanları |Bilgi için [hiper ölçekli hizmet katmanı için en fazla 100 TB](sql-database-service-tier-hyperscale.md)|
| Azure portalındaki sorgu Düzenleyicisi |Bilgi için [bağlanmak ve veri sorgulamak için Azure portalında SQL sorgu Düzenleyicisi'ni kullanın](sql-database-connect-query-portal.md)|
|Farklı yaklaşık sayısı|Bilgi için [yaklaşık ayrı Say](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing)|
|Toplu iş modu Rowstore (altında uyumluluk düzeyi 150)|Bilgi için [Rowstore toplu iş modu](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore)|
|(Altında uyumluluk düzeyi 150) bellek ataması geri bildirimi (satır modu)|Bilgi için [bellek ataması geri bildirimi (satır modu)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback)|
|Tablo değişkeni ertelenmiş derleme (altında uyumluluk düzeyi 150)|Bilgi için [tablo değişkeni ertelenmiş derleme](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation)|
|SQL analizi|Bilgi için [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md)|
| Yönetilen örnek için saat dilimi desteği|Daha fazla bilgi için [saat diliminde Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-timezone.md)|
|||

## <a name="march-2019"></a>Mart 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| Genel kullanım: Azure SQL Veritabanı için okuma amaçlı ölçeği genişletme desteği | Daha fazla bilgi için [okuma ölçeği genişletme](sql-database-read-scale-out.md)|
| &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| Yönetilen örnek için saat dilimi desteği|Daha fazla bilgi için [saat diliminde Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-timezone.md)|
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
|Stream Analytics için başvuru veri kaynağı olarak SQL veritabanı. | Daha fazla bilgi için [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).|
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
| Yönetilen örnekleri ve işlemsel çoğaltma | Kullanma hakkında ek makale [işlem çoğaltma ile yönetilen örnekleri](replication-with-sql-database-managed-instance.md) |
| Yönetilen örnek öğretici Azure AD'ye eklendi | Bu [Azure AD ile yönetilen örnek](sql-database-managed-instance-aad-security-tutorial.md) yönetilen örnek güvenlik Azure AD oturum açma bilgilerinin kullanılması yapılandırmak ve sınamak için sahip olduğunuz gösterilmektedir. |
| Transact-SQL betiklerini kullanarak işlem otomasyonu için güncelleştirilmiş içerik | Güncelleştirilen ve kullanmak için içerik açıklığa kavuşturuldu [Transact-SQL betiklerini kullanarak proje Otomasyon](sql-database-job-automation-overview.md) tek veritabanları, elastik havuzlar ve yönetilen örnekleri |
| Güncelleştirilmiş yönetilen örnek için güvenlik içeriği | Güncelleştirilen ve içeriği açıklığa kavuşturuldu [yönetilen örnek için güvenlik modeli](sql-database-security-overview.md), benzerlikler ve karşıtlıklar oturum tek veritabanları ve elastik havuzlar için güvenlik modeli |
| Tüm Hızlı başlangıçlar ve öğreticilerle yenilenir | Tüm Hızlı başlangıçları ve öğreticileri, [belgeleri](https://docs.microsoft.com/azure/sql-database) güncelleştirilmiş ve Azure portalında değişikliklerle eşleştirmek için yenilendi |
| Eklenen hızlı genel bakış kılavuzları | Hızlı genel bakış Kılavuzu eklenen [tek veritabanları](sql-database-quickstart-guide.md) ve [yönetilen örnekler](sql-database-managed-instance-quickstart-guide.md) |
| Ek SQL veritabanı terimler sözlüğü | Bu [terimleri sözlüğü](sql-database-glossary-terms.md) makalede, SQL veritabanı hüküm ve bağlam terimini açıklayan birincil kavramsal sayfasına bağlantı eksiksiz bir listesini sağlar. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>İçerik gelişimine katkıda bulunmasını

Açık kaynak Azure SQL belgeleri kümesidir. Açık çalışmanın bazı avantajları vardır:

- Açık kaynak depolar, hangi belgelere en çok ihtiyaç olduğuna hakkında geri bildirim almak için açık planlayın.
- Açık kaynak depolar, ilk sürümde en faydalı içeriği yayımlamak için açık olarak gözden geçirin.
- Açık kaynak depolar, içeriği sürekli olarak geliştirmenize daha kolay hale getirmek için açık olarak güncelleştirin.

Azure SQL veritabanı belgeleri içeriklerine katkıda bulunmak için bkz: [Microsoft Docs katkıda bulunan Kılavuzu Genel Bakış](https://docs.microsoft.com/contribute/). Kullanıcı deneyimi üzerinde [docs.microsoft.com](https://docs.microsoft.com/) tümleşir [GitHub](https://github.com/) doğrudan daha kolay yapmak için iş akışları. Başlayın [görüntülediğiniz belgeyi düzenleyerek](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). Ya da tarafından Yardım [yeni konuları inceleyerek](https://docs.microsoft.com/contribute/#review-open-prs), veya [kalite sorunları oluşturarak](https://docs.microsoft.com/contribute/#create-quality-issues).
