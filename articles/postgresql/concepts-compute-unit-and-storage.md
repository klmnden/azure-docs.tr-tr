---
title: "PostgreSQL için işlem birimleri Azure veritabanındaki açıklayan | Microsoft Docs"
description: "Azure DB PostgreSQL için: Bu makalede işlem birimleri ve işleminizi iş yükü en fazla işlem birimleri ulaştığında olanlar kavramlarını açıklar."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: dbb9f733455fa0492358b24b178c8c637ff08c71
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="explaining-compute-units-in-azure-database-for-postgresql"></a>Azure veritabanı işlem birimleri için PostgreSQL açıklayan
Bu konu kavramını işlem birimleri ve İş yükünüzün işlem birimleri düzeyi üst sınırına ulaştığında ne olacağını açıklar.

## <a name="what-are-compute-units"></a>İşlem birimleri nelerdir?
Tek bir Azure veritabanı PostgreSQL sunucu için kullanılabilir olmasını garanti CPU işleme verimlilik ölçü birimleridir işlem. Bir işlem CPU ve bellek kaynakları oluşan karışık bir ölçüyü birimdir. Genel olarak, 50 işlem birimleri yarısı çekirdek eşitlemek. 100 işlem birimleri için bir çekirdek eşitlemek. Garantili işleme sn'ye sunucunuza kullanılabilir 20 çekirdekleri 2000 işlem birimleri eşitlemek.

İşlem birim başına bellek miktarını temel ve standart fiyatlandırma katmanları için optimize edilmiştir. Performans düzeyinin artırarak işlem birimleri Katlama CPU ve bellek tek Azure veritabanına kullanılabilir miktarı PostgreSQL için Katlama için karşılık gelir.

Örneğin, bir standart 800 işlem birimleri 8 x daha fazla CPU işleme ve standart 100 işlem birimleri yapılandırma daha bellek sağlar. Ancak, temel 100 işlem birimler gibi standart 100 işlem birimleri aynı CPU verimliliği sağlayın, ancak standart fiyatlandırma katmanında önceden yapılandırılmış bellek miktarı çift temel fiyatlandırma katmanı için yapılandırılmış bellek miktarı ' dir. Bu nedenle, daha iyi iş yükü performansını ve temel fiyatlandırma katmanı aynı işlem seçili birimleriyle normalden daha düşük işlem gecikmesi standart fiyatlandırma katmanı sağlar.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>İşlem my iş yükü için gereken birim sayısını nasıl belirleyebilirim?
Şirket içi çalıştıran varolan bir PostgreSQL sunucusu geçirmek için aradığınız isterseniz bir sanal makinede, iş yükü işleme sn'ye kaç çekirdek gerekiyor tahminlerle işlem birim sayısını belirleyebilirsiniz. 

Mevcut şirket içi veya sanal makine sunucu şu anda 4 çekirdek (CPU hiper iş parçacığı sayımı olmadan) kullanılarak olduğundan, PostgreSQL server için Azure veritabanınızın 400 işlem birimleri yapılandırarak başlatın. İşlem birimleri dinamik olarak yukarı veya aşağı neredeyse hiçbir uygulama kapalı kalma süresi ile iş yükü gereksinimlerinize bağlı olarak genişletilebilir. 

Azure portalında ölçümleri grafiği izlemek veya işlem birimleri ölçmek için Azure CLI komutları yazın. İzlemek için ilgili işlem birim yüzdesi ve işlem birim sınırı ölçümleridir.

>[!IMPORTANT]
> En büyük IOPS tam olarak kullanılmaz depolama bulursanız, işlem birimleri kullanımı da izleme göz önünde bulundurun. İşlem birimleri oluşturma sınırlı CPU veya bellek nedeniyle performans düşüklüğü lessening tarafından daha yüksek g/ç işleme için izin verebilir.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>My en fazla işlem birimleri isabet ne olur?
Performans düzeyleri kalibre ve veritabanının yükünüzü seçilen fiyatlandırma katmanını ve performans düzeyi için en fazla sınırlarına kadar çalıştırmak için kaynaklar sağlamak için kapsamındadır. 

İş yükünüzün işlem birimleri ya da sağlanan IOPS maksimum sınırları ulaşırsa, izin verilen düzeyi en kaynakları kullanmaya devam edebilirsiniz, ancak sorgularınızı daha yüksek gecikme görmeniz olasıdır. Yavaşlama zaman aşımı sorgular nedenle önemli hale sürece bu sınırları hataları, ancak bunun yerine iş yavaşlama neden değil. 

İş yükünüzün en fazla bağlantı sayısı sınırlamaları ulaşırsa, açık hatalar ortaya çıkar. Kaynakları sınırları hakkında daha fazla bilgi için bkz: [PostgreSQL Azure veritabanındaki sınırlamalar](concepts-limits.md).

## <a name="next-steps"></a>Sonraki adımlar
Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [Azure veritabanı fiyatlandırma katmanlarına PostgreSQL için](./concepts-service-tiers.md).
