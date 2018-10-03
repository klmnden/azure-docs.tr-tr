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
ms.date: 10/02/2018
ms.openlocfilehash: f2b1a8e18917c1c045c715bd3424d0bbfc0cdc67
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045402"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL veritabanı yönetilen örneği'nın kaynak sınırları genel bakış

Bu makale, Azure SQL veritabanı yönetilen örneği'nın kaynak sınırları genel bir bakış sağlar ve varsayılan abonelik sınırlarını artırmak için isteği oluşturma bilgilerini sağlar. 

> [!NOTE]
> Tek tek aboneliğe özgü olmayan diğer yönetilen örnek sınırlamalar için bkz. [sanal çekirdek tabanlı satın alma modeli](sql-database-managed-instance.md#vcore-based-purchasing-model) ve [yönetilen örneğe hizmet katmanları](sql-database-managed-instance.md#managed-instance-service-tiers).

## <a name="default-subscription-level-limits-per-region"></a>Bölge başına varsayılan abonelik düzeyinde sınırlar

Yönetilen örnek, şu anda dağıtım abonelikleri yalnızca aşağıdaki türlerini destekler:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Bulut hizmeti sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> Bu sınırlama yalnızca bazı abonelik türleri için destek geçicidir.

Yönetilen örneği iki Azure bölgesi başına abonelik düzeyinde sınırlar varsayılan yok. Farklı bir abonelik türlerini farklı bölgesel sınırlara sahiptir. Bu sınırlar, sorun türü ile aboneliğin Azure portalında özel destek isteği oluşturarak artırılabilir **kota**:

- **Alt ağı sınırına**: yönetilen örnekleri dağıtıldığı alt ağ sayısı
- **Örnek sayısı sınırı**: bölge başına örnek sayısı

> [!IMPORTANT]
> Dağıtımlarınızı planlarken (nedeniyle ek artıklık için) bir iş kritik (BC) örneği genellikle bir genel amaçlı (GP) örneği daha fazla kapasite x 4 tükettiğini göz önünde bulundurun. Böylece, hesaplamalarınızda 1 GP örneği için = 1 örnek birim ve 1 BC örneği = 4 örneği birimi. Varsayılan limitleri karşı tüketimini analiz kolaylaştırmak için yönetilen örnekler dağıtıldığı bölgede tüm alt ağlar arasındaki örneği birimleri özetlemek ve abonelik türünüz için örneği birim sınırlarıyla sonuçlarını karşılaştırın.

## <a name="default-limits-by-subscription-type"></a>Varsayılan abonelik türüne göre sınırları

|Abonelik türü| Yönetilen örnek alt sınırı | En fazla örnek sayısı |GP sayısı üst sınırı yönetilen örnekleri *|BC sayısı üst sınırı yönetilen örnekleri *|
| :---| :--- | :--- |:--- |:--- |
|Kullandıkça öde|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 **|12 **|12 **|3 **|

\* "Örnek units" bir bölgedeki toplam sayısı 4 hiçbir zaman aşıyor. böylece ya da 1 BC veya bir alt ağda 4 GP örnekleri dağıtabilirsiniz.

** Başka bir hizmet katmanındaki hiçbir örneği varsa Maksimum sayıda örnek bir hizmet katmanında uygulanır. GP ve BC örnekleri aynı alt ağ içinde karışık planladığınız durumlarda aşağıdaki bölümde izin verilen birleşimleri için referans olarak kullanın. Basit bir kural olarak 3 alt toplam sayısını aşamaz ve 12 örneği birimlerin toplam sayısını aşamaz.

## <a name="deployment-options-for-gp-and-bc-deployments-within-the-same-subnet"></a>Aynı alt ağ içinde GP ve BC dağıtımları için dağıtım seçenekleri

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

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>SQL yönetilen örneği için daha büyük bir kota edinme

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