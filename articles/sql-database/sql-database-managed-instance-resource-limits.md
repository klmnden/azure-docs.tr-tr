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
ms.reviewer: carlrab, jovanpop, sachinp, sstein
manager: craigg
ms.date: 05/22/2019
ms.openlocfilehash: 7ff8405bba39e274c4f9f0cbacb7c295564c877e
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66303206"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Genel Bakış Azure SQL veritabanı yönetilen örneği kaynak sınırları

Bu makalede, Azure SQL veritabanı yönetilen örneği için kaynak sınırları genel bir bakış sağlar ve bu sınırları için bir artış istemek hakkında bilgi sağlar.

> [!NOTE]
> Desteklenen özellikler ve T-SQL farklılıkları için bkz: deyimleri [özellik farkları](sql-database-features.md) ve [T-SQL deyimi desteği](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Örnek düzeyinde kaynak sınırları

Yönetilen örnek, özelliklerine ve temel alınan altyapı ve mimari bağlı kaynak sınırları vardır. Sınırları donanım oluşturma ve hizmet katmanına bağlıdır.

### <a name="hardware-generation-characteristics"></a>Donanım oluşturma özellikleri

Azure SQL veritabanı yönetilen örnek üzerinde iki donanım Nesilleri dağıtılabilir: 4. nesil ve 5. nesil. Donanım Nesilleri aşağıdaki tabloda açıklanan farklı özelliklere sahiptir:

|   | **4. nesil** | **5. nesil** |
| --- | --- | --- |
| Donanım | Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri, bağlı SSD sanal çekirdek = 1 PP (fiziksel çekirdek) | Intel E5-2673 v4 (Broadwell) 2.3 GHz işlemcileri, hızlı NVMe SSD, sanal çekirdek = 1 LP (hiper iş parçacığı) |
| Sanal çekirdekler | 8, 16, 24 sanal çekirdek | 8, 16, 24, 32, 40, 64, 80 sanal çekirdekler |
| Bellek | Sanal çekirdek başına 7 GB | Sanal çekirdek başına 5.1 GB |
| Maks. bellek içi OLTP bellek | Sanal çekirdek başına 3 GB | Sanal çekirdek 2,6 GB |
| En büyük örnek depolama (genel amaçlı) |  8 TB | 8 TB |
| En büyük örnek depolama (iş açısından kritik) | 1 TB | 1 TB, 2 TB veya 4 TB çekirdek sayısına bağlı olarak |

### <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

İki hizmet katmanıyla - genel amaçlı ve iş açısından kritik yönetilen örnek sahiptir. Bu katmanlar, aşağıdaki tabloda açıklandığı gibi farklı özellikleri sunar:

| **Özelliği** | **Genel amaçlı** | **İş açısından kritik** |
| --- | --- | --- |
| Sanal çekirdek sayısı\* | 4. nesil: 8, 16, 24<br/>5. nesil: 8, 16, 24, 32, 40, 64, 80 | 4. nesil: 8, 16, 24, 32 <br/> 5. nesil: 8, 16, 24, 32, 40, 64, 80 |
| Bellek | 4. nesil: 56 GB - 168 GB (7GB/sanal çekirdek)<br/>5. nesil: 40.8 GB - 408 GB (5.1 GB/sanal çekirdek) | 4. nesil: 56 GB - 168 GB (7GB/sanal çekirdek)<br/>5. nesil: 40.8 GB - 408 GB (5.1 GB/sanal çekirdek) |
| En büyük örnek depolama boyutu | 8 TB | 4. nesil: 1 TB <br/> 5. nesil: <br/>-1 TB 8, 16 sanal çekirdek<br/>-24 sanal çekirdek 2 TB<br/>-32, 40, 64 4 TB 80 sanal çekirdekler |
| Veritabanı başına maks. depolama | Örnek başına en fazla depolama boyutu tarafından belirlenir. | Örnek başına en fazla depolama boyutu tarafından belirlenir. |
| En fazla örnek başına veritabanı sayısı | 100 | 100 |
| Örnek başına en fazla veritabanı dosyaları | En fazla 280 | Veritabanı başına 32.767 dosyaları |
| Veri/günlük IOPS (yaklaşık) | 500 - 7.500 dosya başına<br/>\*[Dosya boyutuna bağlıdır](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11 K - 110 K (1375/sanal çekirdek) |
| Günlük aktarım hızı | Sanal çekirdek başına 3 MB/sn<br/>Örnek başına en çok 22 MB/sn | Sanal çekirdek başına 4 MB/sn<br/>Örnek başına en fazla 48 MB/sn|
| Veri aktarım hızı (yaklaşık) | 100 - dosya başına 250 MB/sn<br/>\*[Dosya boyutuna bağlıdır](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | |
| GÇ gecikmesi (yaklaşık) | 5-10 ms | 1-2 ms |
| En fazla tempDB boyutu | 192 - 1,920 GB (24 GB sanal çekirdek başına) | Maksimum örnek depolama boyutuyla sınırlı hiçbir kısıtlama- |
| En fazla oturum | 30000 | 30000 |

**Notları**:

- Kullanıcı ve sistem veritabanları hem verileri hem de günlük dosyası boyutu en fazla depolama boyutu sınırı ile karşılaştırıldığında örnek depolama boyutu bulunmaktadır. Kullanım <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> toplam alanı veritabanı tarafından kullanılan belirlemek için sistem görünümü. Hata günlüklerini kalıcı değildir ve boyutu dahil değildir. Yedekleme depolama boyutu dahil edilmez.
- Ayrıca aktarım hızı ve IOPS açıkça yönetilen örneği ile sınırlı değildir sayfa boyutuna bağlıdır.

## <a name="supported-regions"></a>Desteklenen bölgeler

Yalnızca yönetilen örnekleri oluşturulabilir [desteklenen bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Şu anda desteklenmeyen bir bölgede yönetilen örnek oluşturma için [Azure portalından bir destek isteği gönderin](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Yönetilen örnek, şu anda dağıtım abonelikleri yalnızca aşağıdaki türlerini destekler:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Bulut hizmeti sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Kurumsal geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)

> [!NOTE]
> Bu sınırlama geçicidir. Yeni Abonelik türleriyle gelecekte etkinleştirilecektir.

## <a name="regional-resource-limitations"></a>Bölgesel bir kaynak sınırlamaları

Kaynak bölge başına sınırlı sayıda desteklenen abonelik türlerini içerebilir. Yönetilen örnek, bir abonelik türü türüne bağlı olarak Azure bölgesi başına iki varsayılan sınırlara sahiptir:

- **Alt ağı sınırına**: Tek bir bölgede yönetilen örnekleri dağıtıldığı alt ağlar maksimum sayısı.
- **Örnek sayısı sınırı**: Tek bir bölgede dağıtılan örnekleri sayısı.

> [!Note]
> Bu, varsayılan ayarları ve değil teknik sınırlamalar limitlerdir. Özel bir oluşturarak artan talebi sınırları olabilir [Azure portalında bir destek isteği](#obtaining-a-larger-quota-for-sql-managed-instance) daha geçerli bölgede yönetilen örnekleri gerekiyorsa. Alternatif olarak, destek istekleri göndermeden yeni yönetilen örnekleri başka bir Azure bölgesinde oluşturabilirsiniz.

Aşağıdaki tabloda, desteklenen abonelikler için varsayılan bölgesel sınırlar gösterilmektedir:

|Abonelik türü| Yönetilen örnek alt sınırı | En fazla örnek sayısı |GP sayısı üst sınırı yönetilen örnekleri *|BC sayısı üst sınırı yönetilen örnekleri *|
| :---| :--- | :--- |:--- |:--- |
|Kullandıkça öde|1*|4*|4*|1*|
|CSP |1*|4*|4*|1*|
|Kullandıkça Öde geliştirme ve Test|1*|4*|4*|1*|
|Kurumsal Geliştirme ve Test|1*|4*|4*|1*|
|EA|3**|12**|12**|3**|

\* "Örnek units" alt toplam sayısı 4 hiçbir zaman aşıyor. böylece ya da 1 BC veya bir alt ağda 4 GP örnekleri dağıtabilirsiniz.

** Başka bir hizmet katmanındaki hiçbir örneği varsa Maksimum sayıda örnek bir hizmet katmanında uygulanır. GP ve BC örnekleri aynı alt ağ içinde karışık planlıyorsanız, aşağıdaki bölümde izin verilen birleşimleri için referans olarak kullanın. Basit bir kural olarak 3 alt toplam sayısını aşamaz ve 12 örneği birimlerin toplam sayısını aşamaz.


> [!IMPORTANT]
> Dağıtımlarınızı planlarken (nedeniyle ek artıklık için) bir iş kritik (BC) örneği genellikle bir genel amaçlı (GP) örneği daha fazla kapasite x 4 tükettiğini göz önünde bulundurun. Böylece, hesaplamalarınızda 1 GP örneği için = 1 örnek birim ve 1 BC örneği = 4 örneği birimi. Varsayılan sınırları karşı tüketimini analiz basitleştirmek için yönetilen örnekleri burada dağıtılır ve abonelik türünüz için örneği birim sınırlarıyla sonuçlarını karşılaştırın bu bölgedeki tüm alt ağlar arasındaki örneği birimleri özetler.

## <a name="strategies-for-deploying-mixed-general-purpose-and-business-critical-instances"></a>Karma genel amaçlı ve iş açısından kritik örneği dağıtma stratejileri

[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) abonelikler, GP ve BC örnekleri bileşimlerine sahip olabilir. Ancak, alt ağlar durumlarda yerleşimini ile ilgili bazı kısıtlamalar vardır.

> [!Note]
> [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) ve [bulut hizmeti sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources) abonelik türlerini ya da bir iş açısından kritik olan veya en çok 4 genel amaçlı örnekler için.

Aşağıdaki örnek, boş olmayan alt ağlar ile dağıtım durumları kapsar ve hizmet katmanları, GP ve BC karma.

|Alt ağ sayısı|alt ağ 1|Alt ağı 2|Alt ağ 3|
|:---|:---|:---|:---|
|1|1 BC ve 8 GP kadar<br>BC 2 ve 4 GP kadar|Yok| Yok|
|2|0 BC, en fazla 4 GP|1 BC, en fazla 4 GP<br>2 BC, 0 GP|Yok|
|2|BC 1, 0 GP|0 BC, en fazla 8 GP<br>1 BC, en fazla 4 GP|Yok|
|2|2 BC, 0 GP|0 BC, en fazla 4 GP|Yok|
|3|BC 1, 0 GP|BC 1, 0 GP|0 BC, en fazla 4 GP|
|3|BC 1, 0 GP|0 BC, en fazla 4 GP|0 BC, en fazla 4 GP|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Yönetilen örnek için SQL daha büyük bir kota edinme

Daha fazla yönetilen örnekleri, geçerli bölgede ihtiyacınız varsa, Azure portalını kullanarak kota genişletmek için bir destek isteği gönderin.
Daha büyük bir kota alma işlemi başlatmak için:

1. Açık **Yardım + Destek**, tıklatıp **yeni destek isteği**.

   ![Yardım ve Destek](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Yeni destek isteği için temel bilgiler sekmesinde:
   - İçin **sorun türü**seçin **hizmet ve abonelik sınırlarını (kotalar)** .
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

- Yönetilen örnek hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Fiyatlandırma bilgileri için bkz: [SQL veritabanı yönetilen örneği fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- İlk yönetilen örneğinizi oluşturma konusunda bilgi almak için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-managed-instance-get-started.md).