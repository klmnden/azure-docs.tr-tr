---
title: Azure SQL veritabanı kaynak limitleri - yönetilen örnek | Microsoft Docs
description: Bu makalede yönetilen örnekleri için Azure SQL veritabanı kaynak limitleri genel bir bakış sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop
manager: craigg
ms.date: 10/03/2018
ms.openlocfilehash: 6ae9b3f71cb5328c7f4a7ee8e43ec0aff8b5fcec
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48267793"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL veritabanı yönetilen örneği'nın kaynak sınırları genel bakış

Bu makale, Azure SQL veritabanı yönetilen örneği'nın kaynak sınırları genel bir bakış sağlar ve varsayılan bölgesel abonelik limitleri artırmak için isteği oluşturma bilgilerini sağlar. 

> [!NOTE]
> Diğer yönetilen örnek sınırlamalar için bkz. [sanal çekirdek tabanlı satın alma modeli](sql-database-managed-instance.md#vcore-based-purchasing-model) ve [yönetilen örneğe hizmet katmanları](sql-database-managed-instance.md#managed-instance-service-tiers).

## <a name="instance-level-resource-limits"></a>Örnek düzeyinde kaynak sınırları

Yönetilen örnek, özelliklerine ve temel alınan altyapı ve mimari bağlıdır kaynak sınırları vardır. Sınırları donanım oluşturma ve hizmet katmanına bağlıdır.

### <a name="hardware-generation-characteristics"></a>Donanım oluşturma özellikleri

Azure SQL veritabanı yönetilen örneği, iki donanım oluşturma (4. nesil ve 5. nesil) üzerinde dağıtılabilir. Donanım Nesilleri aşağıdaki tabloda açıklanan farklı özelliklere sahiptir:

|   | **4. nesil** | **5. nesil** |
| --- | --- | --- |
| Donanım | Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri, bağlı SSD sanal çekirdek = 1 PP (fiziksel çekirdek) | Intel E5-2673 v4 (Broadwell) 2.3 GHz işlemcileri, hızlı eNVM SSD, sanal çekirdek = 1 LP (hiper iş parçacığı) |
| İşlem | 8, 16, 24 sanal çekirdek | 8, 16, 24, 32, 40, 64, 80 sanal çekirdekler |
| Bellek | Sanal çekirdek başına 7 GB | Sanal çekirdek başına 5.5 GB |
| En fazla depolama alanı (iş açısından kritik) | 1TB | 1TB, 2TB çekirdek sayısına bağlı olarak 4TB |

### <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Yönetilen örnek, iki hizmet katmanıyla - genel amaçlı ve iş açısından kritik (genel Önizleme) sahiptir. Bu katmanlar, aşağıdaki tabloda açıklandığı gibi farklı özellikleri sunar:

| **Özellik** | **Genel amaçlı** | **İş açısından kritik (Önizleme)** |
| --- | --- | --- |
| Sanal çekirdek sayısı\* | 4. nesil: 8, 16, 24<br/>5. nesil: 8, 16, 24, 32, 40, 64, 80 | 4. nesil: 8, 16, 24, 32 <br/> 5. nesil: 8, 16, 24, 32, 40, 64, 80 |
| Bellek | 4. nesil: 56GB - 156GB<br/>5. nesil: 44GB - 440GB<br/>\*Orantılı sanal çekirdek sayısı | 4. nesil: 56GB - 156GB <br/> 5. nesil: 44GB - 440GB<br/>\*Orantılı sanal çekirdek sayısı |
| En büyük depolama boyutu | 8 TB | Gen 4: 1 TB <br/> 5. nesil: <br/>-1 TB 8, 16 sanal çekirdek<br/>-24 sanal çekirdek 2 TB<br/>-32, 40, 64 4 TB 80 sanal çekirdekler |
| Veritabanı başına maks. depolama | Örnek başına en fazla depolama boyutu tarafından belirlenir. | Örnek başına en fazla depolama boyutu tarafından belirlenir. |
| En fazla örnek başına veritabanı sayısı | 100 | 100 |
| Örnek başına en fazla veritabanı dosyaları | En fazla 280 | Sınırsız |
| En fazla depolama IOPS bekleniyor | Veri dosyası başına 500-7500 IOPS ([veri dosya boyutuna mı bağımlı](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)). | Temel alınan SSD hızınıza bağlıdır. |

## <a name="supported-regions"></a>Desteklenen bölgeler

Yalnızca yönetilen Instanced oluşturulabilir [desteklenen bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database). Şu anda desteklenmeyen bir bölgede bir yönetilen örnek oluşturmak istiyorsanız, aşağıdakileri yapabilirsiniz [Azure portalı üzerinden destek isteği Gönder](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Yönetilen örnek, şu anda dağıtım abonelikleri yalnızca aşağıdaki türlerini destekler:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Bulut hizmeti sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> Bu sınırlama geçicidir. Yeni Abonelik türleriyle gelecekte etkinleştirilecektir.

## <a name="regional-resource-limitations"></a>Bölgesel bir kaynak sınırlamaları

Kaynak bölge başına sınırlı sayıda desteklenen abonelik türlerini içerebilir. Yönetilen örnek, bir abonelik türü türüne bağlı olarak Azure bölgesi başına iki varsayılan sınırlara sahiptir:

- **Alt ağı sınırına**: tek bir bölgede yönetilen örnekleri dağıtıldığı alt ağların sayısı.
- **Örnek sayısı sınırı**: tek bir bölgede dağıtılan örnekleri sayısı.

Aşağıdaki tabloda desteklenen abonelikler için varsayılan bölgesel sınırlar gösterilmektedir:

|Abonelik türü| Yönetilen örnek alt sınırı | En fazla örnek sayısı |GP sayısı üst sınırı yönetilen örnekleri *|BC sayısı üst sınırı yönetilen örnekleri *|
| :---| :--- | :--- |:--- |:--- |
|Kullandıkça öde|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 **|12 **|12 **|3 **|

\* "Örnek units" alt toplam sayısı 4 hiçbir zaman aşıyor. böylece ya da 1 BC veya bir alt ağda 4 GP örnekleri dağıtabilirsiniz.

** Başka bir hizmet katmanındaki hiçbir örneği varsa Maksimum sayıda örnek bir hizmet katmanında uygulanır. GP ve BC örnekleri aynı alt ağ içinde karışık planladığınız durumlarda aşağıdaki bölümde izin verilen birleşimleri için referans olarak kullanın. Basit bir kural olarak 3 alt toplam sayısını aşamaz ve 12 örneği birimlerin toplam sayısını aşamaz.

Bu sınırlar, özel oluşturarak artırılabilir [Azure portalında bir destek isteği](#obtaining-a-larger-quota-for-sql-managed-instance) geçerli bölgede daha fazla yönetilen örnek gerekiyorsa. Alternatif olarak, destek istekleri göndermeden yeni yönetilen örnekler başka bir Azure bölgesinde oluşturabilirsiniz.

> [!IMPORTANT]
> Dağıtımlarınızı planlarken (nedeniyle ek artıklık için) bir iş kritik (BC) örneği genellikle bir genel amaçlı (GP) örneği daha fazla kapasite x 4 tükettiğini göz önünde bulundurun. Böylece, hesaplamalarınızda 1 GP örneği için = 1 örnek birim ve 1 BC örneği = 4 örneği birimi. Varsayılan limitleri karşı tüketimini analiz kolaylaştırmak için yönetilen örnekler dağıtıldığı bölgede tüm alt ağlar arasındaki örneği birimleri özetlemek ve abonelik türünüz için örneği birim sınırlarıyla sonuçlarını karşılaştırın.

### <a name="deployment-options-for-gp-and-bc-deployments-within-the-same-subnet"></a>Aynı alt ağ içinde GP ve BC dağıtımları için dağıtım seçenekleri

Aşağıdaki örnek, boş olmayan alt ağlar ile dağıtım durumları kapsar ve hizmet katmanları, GP ve BC karma.

|Alt ağ sayısı|alt ağ 1|Alt ağı 2|Alt ağ 3|
|:---|:---|:---|:---|
|1|0 BC ve 12 GP kadar<br>1 BC ve 8 GP kadar<br>BC 2 ve 4 GP kadar<br>3 BC|Yok| Yok|
|2|0 BC, en fazla 4 GP|0 BC, en fazla 8 GP<br>1 BC, en fazla 4 GP<br>2 BC|Yok|
|2|1 BC|0 BC, en fazla 8 GP<br>1 BC, en fazla 4 GP<br>2 BC|Yok|
|2|2 BC|0 BC, en fazla 8 GP<br>1 BC, en fazla 4 GP<br>2 BC|Yok|
|3|BC 1, 0 GP|BC 1, 0 GP|0 BC, en fazla 4 GP|
|3|1BC, 0 GP|0 BC, en fazla 4 GP|BC 1, 0 GP|
|3|0 BC, en fazla 4 GP|BC 1, 0 GP|1BC, 0 GP|

### <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>SQL yönetilen örneği için daha büyük bir kota edinme

Daha büyük bir kota alma işlemi başlatmak için:

1. Açık **Yardım + Destek**, tıklatıp **yeni destek isteği**. 

   ![Yardım ve Destek](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Yeni destek isteği için temel bilgiler sekmesinde:
   - İçin **sorun türü**seçin **hizmet ve abonelik sınırlarını (kotalar)**.
   - **Abonelik** bölümünde aboneliğinizi seçin.
   - İçin **kota türü**seçin **SQL veritabanı yönetilen örneği**.
   - İçin **destek planı**, destek planını seçin.

     ![Sorun türü kota](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. **İleri**’ye tıklayın.
4. Yeni destek isteği sorun sekmesinde:
   - İçin **önem derecesi**, sorunun önem derecesini seçin.
   - İçin **ayrıntıları**, hata iletileri de dahil olmak üzere sorununuzla ilgili ek bilgiler sağlayın.
   - İçin **karşıya dosya yükleme**, daha fazla bilgi içeren bir dosya ekleme (en fazla 4 MB).

     ![Sorun ayrıntıları](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Geçerli bir istek içermelidir:
     > - Bölge hangi aboneliği sınırı artırılacak gerekiyor
     > - (Hiçbir alt ağda mevcut gerekiyorsa genişletilecek örnekleri, mevcut alt ağlar kota sonra hizmet katmanında başına gerekli sayısını artırın
     > - (Yeni alt ağlara yönetilen örnekleri dağıtmanız gerekiyorsa), yeni alt ağ sayısını ve yeni alt ağlara hizmet katmanında başına örnek sayısı gereklidir.
5. **İleri**’ye tıklayın.
6. Yeni destek isteği için irtibat bilgileri sekmesinde, tercih edilen iletişim yöntemine (e-posta veya telefon) ve kişi ayrıntılarını girin.
7. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md). 
- Fiyatlandırma bilgileri için bkz: [SQL veritabanı yönetilen örneği fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- İlk yönetilen örneğinizi oluşturma konusunda bilgi almak için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-managed-instance-get-started.md).
