---
title: "MySQL için işlem birimleri Azure veritabanındaki açıklayan | Microsoft Docs"
description: "Azure DB MySQL için: Bu makalede işlem birimleri ve işleminizi iş yükü en fazla işlem birimleri ulaştığında olanlar kavramlarını açıklar."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: 2c4894ae9a4235f9ced4a8d9b991238543646f53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="explaining-compute-units-in-azure-database-for-mysql"></a>Azure veritabanı işlem birimleri için MySQL açıklayan
Bu konu kavramını işlem birimleri ve işleminizi iş yükü en fazla işlem birimleri ulaştığında ne olacağını açıklar.

## <a name="what-are-compute-units"></a>İşlem birimleri nelerdir?
MySQL sunucusu için tek bir Azure veritabanı için kullanılabilir olmasını garanti CPU işleme verimlilik ölçü birimleridir işlem. Bir işlem CPU ve bellek kaynakları oluşan karışık bir ölçüyü birimdir. Genel olarak, 50 işlem birimleri yarısı çekirdek eşitlemek. 100 işlem birimleri için bir çekirdek eşitlemek. Garantili işleme sn'ye sunucunuza kullanılabilir 20 çekirdekleri 2.000 işlem birimleri eşitlemek.

İşlem birim başına bellek miktarını temel ve standart fiyatlandırma katmanları için optimize edilmiştir. Performans düzeyinin artırarak işlem birimleri Katlama, kaynağın tek Azure veritabanına kullanılabilir kümesi MySQL için Katlama için karşılık gelir.

Örneğin, bir standart 100 işlem birimleri yapılandırması daha işlem standart bir 800 birimleri 8 x daha fazla CPU işleme ve bellek sağlar. Ancak, standart 100 işlem birimleri temel 100 birimlerine işlem karşılaştırıldığında aynı CPU verimliliği sağlayın, ancak standart fiyatlandırma katmanında önceden yapılandırılmış bellek miktarı çift temel fiyatlandırma katmanı için yapılandırılmış bellek miktarı ' dir. Bu nedenle, daha iyi iş yükü performansını ve temel fiyatlandırma katmanı aynı işlem seçili birimleriyle normalden daha düşük işlem gecikmesi standart fiyatlandırma katmanı sağlar.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>İşlem my iş yükü için gereken birim sayısını nasıl belirleyebilirim?
Şirket içi çalıştıran mevcut bir MySQL server geçirmek istediğiniz isterseniz bir sanal makinede işlemeyi sn'ye kaç çekirdek İş yükünüzün gerektirdiği tahminlerle işlem birim sayısını belirleyebilirsiniz. 

Mevcut şirket içi veya sanal makine sunucu şu anda 4 çekirdek (CPU hiper iş parçacığı sayımı olmadan) kullanıyorsa, MySQL server için Azure veritabanınızın 400 işlem birimleri yapılandırarak başlatın. İşlem birimleri yukarı veya aşağı dinamik olarak iş yükü gereksinimlerinize bağlı olarak ve neredeyse hiçbir uygulama kapalı kalma süresi ile genişletilebilir. 

Azure portalında ölçümleri grafiği izlemek veya işlem birimleri ölçmek için Azure CLI komutları yazın. İzlemek için ilgili işlem birim yüzdesi ve işlem birim sınırı ölçümleridir.

>[!IMPORTANT]
> En büyük IOPS tam olarak kullanılmaz depolama bulursanız, işlem birimleri kullanımı da izleme göz önünde bulundurun. İşlem birimleri oluşturma sınırlı CPU veya bellek kaynaklanan performans düşüklüğü lessening tarafından daha yüksek g/ç işleme için izin verebilir.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>My en fazla işlem birimleri isabet ne olur?
Performans düzeyleri kalibre ve veritabanının yükünüzü seçilen fiyatlandırma katmanını ve performans düzeyi için en fazla sınırlarına kadar çalıştırmak için kaynaklar sağlamak için kapsamındadır. 

İş yükünüzün ya da işlem birimleri en fazla sınırları veya sağlanan IOPS sınırları ulaşırsa, izin verilen düzeyi en kaynakları kullanmaya devam edebilirsiniz, ancak sorgularınızı daha yüksek gecikme karşılaşabileceğiniz olası. Yavaşlama zaman aşımı sorgular nedenle önemli hale sürece bu sınırları hataları yerine iş yükü yavaşlama sonuçlanır. 

İş yükünüzün en fazla bağlantı sayısı sınırlamaları ulaşırsa, açık hatalar ortaya çıkar. Kaynakları sınırları hakkında daha fazla bilgi için bkz: [MySQL için Azure veritabanındaki sınırlamaları](concepts-limits.md).

## <a name="next-steps"></a>Sonraki adımlar
Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [Azure veritabanı fiyatlandırma katmanlarına MySQL için](./concepts-service-tiers.md).
