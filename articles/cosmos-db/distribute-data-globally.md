---
title: Verileri Azure Cosmos DB ile küresel olarak dağıtma | Microsoft Docs
description: Bir Global olarak dağıtılmış çok modelli veritabanı hizmeti olan Azure Cosmos DB genel veritabanlarından kullanarak çok büyük ölçekli coğrafi çoğaltma, çok yöneticili, yük devretme ve veri kurtarma hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: fef4d14e6f5a9dcba1f7e111f9a54c3e1a9fbfa9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959867"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Azure Cosmos DB ile verileri küresel dağıtım

Azure bulunabilen - 50'den fazla coğrafi bölgeler arasında bir küresel kaplama alanını sahiptir ve sürekli genişliyor. Küresel varlık ve çok yöneticili desteği, Azure, geliştiricilere sunduğu fark yaratan özellikleri oluşturmanızı, dağıtmanızı ve Global olarak dağıtılmış uygulamaları kolayca yönetme olanağı biridir.

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB anahtar teslim küresel dağıtım, sağlar [aktarım hızı ve depolama esnek ölçeklendirme](../cosmos-db/partition-data.md) dünya, tek basamaklı okuma ve yazma 99. yüzdebirlik dilimde milisaniye gecikme süreleri [iyi tanımlanmış tutarlılık modelleri](consistency-levels.md)ve garantili yüksek kullanılabilirlik, olanaklarına [sektör lideri kapsamlı SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [tüm verileriniz otomatik olarak dizinleyen](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) şema veya dizin yönetimiyle ilgilenmenize gerek kalmadan.

## <a name="global-distribution-with-multi-master"></a>Çok yöneticili ile genel dağıtım

Bir yerel olarak born bulut hizmetinde, Azure Cosmos DB dikkatli bir şekilde baştan ayarlama ile çok kiracılı, genel dağıtım ve birden çok ana belge, anahtar-değer, grafik ve sütun ailesi veri modelleri için tasarlanmıştır.

![Bölümlenmiş ve üç bölgeler arasında dağıtılmış azure Cosmos DB kapsayıcısı](./media/distribute-data-globally/global-apps.png)

**Bölümlenmiş ve dağıtılmış Azure bölgelerinde tek bir Azure Cosmos DB kapsayıcısı**

Azure Cosmos DB oluşturulurken edindiğimiz gibi ekleme genel dağıtım akla olamaz. "Cıvatalı"çoklu site"bir veritabanı sistemi açma" olamaz. Global olarak dağıtılmış bir veritabanı tarafından sunulan "tek sitede" veritabanı tarafından sunulan geleneksel coğrafi olağanüstü durum kurtarma (coğrafi-DR) tarafından sunulan özelliklerin ötesine yayılır. Tek site veritabanları GEO-DR özelliği sunan, Global olarak dağıtılmış veritabanları özelliklerinin katı bir alt değildir.

Azure Cosmos DB anahtar teslim küresel dağıtım ile geliştiriciler kendi çoğaltma yapı iskelesi veritabanı günlüğü üzerinden Lambda deseni kullanan ya da birden çok bölgede "çift yazmalar" gerçekleştirerek yapı gerekmez. Yaptığımız *değil* , bu tür bir yaklaşım doğruluğunu sağlamak ve ses SLA'lar sağlamak mümkün olduğundan bu yaklaşım önerilir.

## <a id="Next Steps"></a>Sonraki adımlar

* [Azure Cosmos DB anahtar teslim küresel dağıtım nasıl sağlar?](distribute-data-globally-turnkey.md)

* [Azure Cosmos DB genel dağıtımını önemli avantajları](distribute-data-globally-benefits.md)

* [Azure Cosmos DB genel veritabanı çoğaltması yapılandırma](tutorial-global-distribution-sql-api.md)

* [Azure Cosmos DB hesapları için çok yöneticili etkinleştirme](enable-multi-master.md)

* [Otomatik ve el ile yük devretme, Azure Cosmos DB'de nasıl çalışır?](regional-failover.md)

* [Azure Cosmos DB'de çakışma çözümü anlama](multi-master-conflict-resolution.md)

* [çok yöneticili açık kaynak NoSQL veritabanları ile kullanma](multi-master-oss-nosql.md)
