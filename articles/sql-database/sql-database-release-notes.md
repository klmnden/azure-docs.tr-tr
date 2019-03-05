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
ms.date: 03/04/2019
ms.author: carlrab
ms.openlocfilehash: af0fd591393f9f187a75f79fea980d41085df5cb
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342454"
---
# <a name="sql-database-release-notes"></a>SQL veritabanı sürüm notları

Bu makalede, SQL veritabanı hizmeti ve SQL veritabanı belgeleri geliştirmeleri ve yeni özellikleri listeler. Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="march-2019"></a>Mart 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
| Tek veritabanları için eklenen günlük sınırları|Daha fazla bilgi için [tek veritabanı sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md)|
| Elastik havuzlara ve havuza alınmış veritabanlarını eklenen günlük sınırları|Daha fazla bilgi için [elastik havuz sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md)|
| Ek işlem günlüğü oranı idare| Eklenen yeni içerikleri [işlem günlüğü oranı idare](sql-database-resource-limits-database-server.md#transaction-log-rate-governance)|
| Tek veritabanları ve elastik havuzlar az.sql modülü kullanmak için güncelleştirilmiş PowerShell örnekleri | Daha fazla bilgi için [tek veritabanları ve elastik havuzlar için PowerShell örnekleri](sql-database-powershell-samples.md#single-database-and-elastic-pools).

## <a name="february-2019"></a>Şubat 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
|Sürdürülebilir bir çevrimiçi dizin oluşturma genel kullanıma sunulmuştur| Daha fazla bilgi için [dizin oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql)|
|Yönetilen örnek geliştirilmiş rota tabloları desteği| Daha fazla bilgi için [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements)|

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
|Örnek dağıtım seçeneği açıklamalar yönetilen ekleme|Yönetilen örnek dağıtım seçenekleri tek veritabanı ve elastik havuz için Uygulanabilirlik açıklamak için birçok makale güncelleştirildi |
|DTU tabanlı satın alma modeli güncelleştirilmiş tempdb boyutları | Daha fazla bilgi için [SQL veritabanı'nda veritabanı Tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)|
|Güncelleştirilmiş içeri ve dışarı aktarma bacpac dosyasını yönetilen örnek desteği| Daha fazla bilgi için [BACPAC alma](sql-database-import.md) ve [BACPAC için dışarı aktarma](sql-database-export.md) |

## <a name="january-2019"></a>Ocak 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| İşlem kaynakları için ek ayrıntı düzeyi seçenekleri | Genel amaçlı ve iş açısından kritik hizmet katmanları için [tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) artık daha fazla ayrıntılı işlem seçeneğiniz vardır.|
| Azure portalında yönetilen örnek için Denetim kayıtlarını görüntüleme | Görüntüleme [Denetim kayıtlarını yönetilen örnekleri için](sql-database-managed-instance-auditing.md) Azure portalı artık desteklenmektedir.|
| Gelişmiş tehdit algılama özelliği gelişmiş veri güvenliği için yeniden adlandırıldı | Gelişmiş tehdit algılama özelliği olarak yeniden adlandırıldı [gelişmiş veri güvenliği](sql-advanced-threat-protection.md) tek veritabanları, elastik havuzlar ve yönetilen örnekleri |
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

Azure SQL veritabanı belgeleri içeriklerine katkıda bulunmak için bkz: [Microsoft Docs katkıda bulunan Kılavuzu Genel Bakış](https://docs.microsoft.com/en-us/contribute/). Kullanıcı deneyimi üzerinde [docs.microsoft.com](https://docs.microsoft.com/) tümleşir [GitHub](https://github.com/) doğrudan daha kolay yapmak için iş akışları. Başlayın [görüntülediğiniz belgeyi düzenleyerek](https://docs.microsoft.com/en-us/contribute/#quick-edits-to-existing-documents). Ya da tarafından Yardım [yeni konuları inceleyerek](https://docs.microsoft.com/en-us/contribute/#review-open-prs), veya [kalite sorunları oluşturarak](https://docs.microsoft.com/en-us/contribute/#create-quality-issues).
