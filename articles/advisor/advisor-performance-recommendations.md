---
title: "Azure Danışmanı performans önerileri | Microsoft Docs"
description: "Azure dağıtımlarınızı performansını iyileştirmek için Danışmanı'nı kullanın."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 995a1f37a3fd68b39c14a95d46109c0f7814018d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-performance-recommendations"></a>Advisor performans önerileri

Azure Danışmanı performans önerileri hızı ve kritik iş uygulamalarının yanıtlama hızını geliştirilmesine yardımcı olun. Performans öneriler danışmanına alın **performans** Danışmanı Pano sekmesi.

![Advisor performans sekmesi](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>SQL DB Danışmanı ile veritabanı performansı

Advisor önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar. SQL Azure veritabanının performansını geliştirmek için öneriler getirmek için SQL Database Advisor ile tümleşir. SQL veritabanı Danışmanı, SQL Azure veritabanının performansını kullanım geçmişiniz çözümleyerek değerlendirir. Ardından, veritabanının tipik iş yükünü çalıştırmak için en uygun öneriler sunar. 

> [!NOTE]
> Önerileri almak için bir veritabanı hakkında kullanım haftada olması gerekir ve bu hafta içinde var. bazı tutarlı etkinlik olması gerekir. SQL veritabanı Danışmanı daha kolay rastgele WINS'e etkinlik için tutarlı bir sorgu modelleri için en iyi duruma getirebilirsiniz.

SQL Database Advisor hakkında daha fazla bilgi için bkz: [SQL veritabanı Danışmanı'nı](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![SQL veritabanı önerileri](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis önbelleği performansı ve güvenilirliği iyileştirmek

Advisor burada performansını olumsuz yönde yüksek bellek kullanımı, sunucu iş yükü, ağ bant genişliği veya çok sayıda istemci bağlantıları tarafından etkilenebilir Redis önbelleği örnekleri tanımlar. Advisor en iyi yöntemler, olası sorunları önlemenize yardımcı olacak öneriler de sağlar. Redis önbelleği öneriler hakkında daha fazla bilgi için bkz: [Redis önbelleği Danışmanı](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Uygulama hizmeti performans ve güvenilirlik geliştirmek

Azure Danışmanı uygulama hizmetleri deneyiminizi geliştirmek ve ilgili platform özelliklerini keşfetmek için en iyi uygulama önerilerini tümleştirir. Uygulama Hizmetleri önerileri örnekleri şunlardır:
* Algılama, bellek veya CPU kaynaklarını azaltma seçenekleri ile uygulama çalışma zamanları tarafından tükendiği örnekleri.
* Burada collocating kaynakları web uygulamaları ve veritabanları gibi örneklerinin algılama, performans ve düşük maliyetli artırabilir. 

Uygulama Hizmetleri öneriler hakkında daha fazla bilgi için bkz: [Azure App Service için en iyi uygulamaları](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Uygulama Hizmetleri önerileri](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Performans önerileri Danışmanı erişme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede **daha fazla hizmet**.

3. Hizmet menü bölmesinde altında **izleme ve Yönetim**, tıklatın **Azure Danışmanı**.  
 Advisor Panosu görüntülenir.

4. Advisor Panoda tıklatın **performans** sekmesi.

5. Önerileri almak ve ardından istediğiniz aboneliği seçin **alma önerileri**.

> [!NOTE]
> Advisor önerileri erişmek için öncelikle *aboneliğinizi kaydetmek* Danışmanı ile. Bir abonelik kayıtlı olduğunda bir *abonelik sahibi* Danışmanı Pano başlatır ve tıkladığında **alma önerileri** düğmesi. Bu bir *tek seferlik işlem*. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik, bir kaynak grubu ya da belirli bir kaynak için.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:

* [Advisor giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)

