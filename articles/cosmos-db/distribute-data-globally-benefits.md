---
title: Azure Cosmos DB genel dağıtımını önemli avantajları
description: Coğrafi çoğaltma, çok yöneticili ve kullanım durumları yararlı olduğu tarafından sunulan Azure Cosmos DB çok yöneticili, anahtar avantajları hakkında bilgi edinin.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 11d70a648bfc1882753688e5352835b04cee6b9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968363"
---
# <a name="distribute-data-globally-with-azure-cosmos-db"></a>Verileri Azure Cosmos DB ile küresel olarak dağıtma

Bu makalede, verileri genel olarak dağıtma ve genel veri dağıtım gerekmesi halinde bazı gerçek zamanlı senaryoları öne çıkan avantajları anlatılmaktadır.

## <a name="key-benefits"></a>Önemli avantajlar

Azure Cosmos DB genel veri dağıtım özelliklerinden yararlanmak için hesapları kullanılabilir başlıca yararları şunlardır:

* **Tek basamaklı gecikme süresi** – Azure Cosmos DB sunar < 10 ms okuma ve yazma 99. yüzdebirlik dilimde düşük gecikme süresi garanti edilir tarafından [SLA'mali olarak desteklenen](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **5-9 kullanılabilirlik** – Azure Cosmos DB, % 99,999 okuma ve yazma kullanılabilirliğini, tarafından garanti sunar [SLA'mali olarak desteklenen](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **Esnek çakışma çözümü** – genel veri bütünlüğü ve tutarlılığı sağlamak için çakışmalarını işleme için üç moddan çok yöneticili özellikleri, Azure Cosmos DB yararlanan müşteriler sağlar.

* **Ayarlanabilir tutarlılık** – Azure Cosmos DB destekleyen 5 farklı [tutarlılık düzeyleri](consistency-levels.md) ile genel dağıtım birden çok yönetim özelliğine sahip hesaplar için tüm güçlü tutarlılık desteğiyle unutmayın.

* **Örtük hata toleransı** -birden çok bölgede çoğaltılan verilerle uygulamalar bölgesel kesintiler karşı yüksek dayanıklılık tadını çıkarın.

## <a name="use-cases"></a>Uygulama alanları

Yararlanabilir bu senaryolarda, yalnızca küçük bir örnek aşağıdadır dağıtım özellikleri Azure Cosmos DB genel olarak.

* **Sosyal medya uygulamaları** : sosyal medya uygulamalarını gerektiren düşük gecikmeli, yüksek kullanılabilirlik ve ölçeklenebilirlik kullanıcılar için harika bir deneyim sağlamak için dünyanın dört bir yanında bulunan.

* **IOT** -coğrafi olarak dağıtılmış uç dağıtımları birçok konumdan zaman serisi verilerini izlemek genellikle gerekir. Her cihaz, en yakın bir bölgeye bağlantılı. Cihazlar farklı konumlara taşıdığınızda, kullanılabilir en yakın bölgeyi yazmak için bu cihazları barındırılması.

* **E-ticaret** -E-ticaret, çok düşük gecikme süreli yanı sıra yüksek kullanılabilirlik gerektirir. Veri okuma ve yazma işlemleri ile coğrafi dağıtım, uygulamaların yanıt verme hızını artırma kullanıcılara en yakın veri koyar. Hem okuma hem de yazma işlemleri için bir kullanılabilirlik birden çok bölgede daha yüksek kullanılabilirlik sağlar.

* **Sahtekarlık/Anomali algılama** -genellikle kullanıcı etkinliğinde veya hesap etkinliği izleme uygulamaları küresel olarak dağıtılan ve aynı anda risk ölçümleri satır içi tutmak puanları güncelleştirmek için çeşitli olayları izler gerekir.

* **Kullanım ölçümü** - sayım ve kullanım düzenlenmesi (API çağrıları gibi işlemler/saniye, dakika kullanılır) kullanarak Azure Cosmos DB çok yöneticili kolaylığıyla genel olarak uygulanabilir. Her iki doğruluğu sayıları ve gerçek zamanlı düzenleme, yerleşik bir çakışma çözümü sağlar.

## <a name="next-steps"></a>Sonraki adımlar  

Bu makalede, temel avantajları ve Azure Cosmos DB'nin genel veri dağıtım özellikleri için kullanım örnekleri hakkında bilgi edindiniz. Ardından, aşağıdaki kaynakları arayın:

* [Azure Cosmos DB anahtar teslim küresel dağıtım nasıl sağlar?](distribute-data-globally-turnkey.md)

* [Azure Cosmos DB genel veritabanı çoğaltması yapılandırma](tutorial-global-distribution-sql-api.md)

* [Azure Cosmos DB hesapları için çok yöneticili etkinleştirme](enable-multi-master.md)

* [Otomatik ve el ile yük devretme, Azure Cosmos DB'de nasıl çalışır?](regional-failover.md)

* [Azure Cosmos DB'de çakışma çözümü anlama](multi-master-conflict-resolution.md)

* [çok yöneticili açık kaynak NoSQL veritabanları ile kullanma](multi-master-oss-nosql.md)
