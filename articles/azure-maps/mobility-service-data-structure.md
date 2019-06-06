---
title: Mobility hizmeti veri yapılarını Azure haritalar | Microsoft Docs
description: Azure haritalar Mobility hizmeti veri yapılarını
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 29e8a9d7555ca836b6266879f3b3c1e32ffd3980
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735564"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmeti veri yapılarını

Bu makalede Metro alanı kavramını sunar [Azure haritalar Mobility hizmetini](https://aka.ms/AzureMapsMobilityService) ve Hizmetler üzerinden döndürülen bazı ortak alanlar, sorgulandığında için ortak bir geçiş durdurur ve satırlar. Mobility hizmeti API'lerini kullanmaya başlamadan önce bu makalede giderek öneririz. Bu ortak alanlar aşağıda ele alır.

## <a name="metro-area"></a>Metro alanı

Mobility hizmeti verileri desteklenen metro bölüme ayrılır. Metro alanları Şehir sınırları izlemeyin, metro alan birden fazla şehir, örneğin, densely doldurulmuş Şehir ve kendi çevreleyen şehirler içerebilir; ve ülke/bölge metro bir alan olabilir. 

`metroID` Çağırmak için kullanılan alanın metro kimliği [alma Metro alanı bilgisi API](https://aka.ms/AzureMapsMobilityMetroAreaInfo) istek desteklenen geçiş türleri ve metro alanının geçiş kurumları ve etkin uyarıları gibi ek ayrıntılar için. Azure haritalar alma Metro API'si kullanarak desteklenen metro alanlarını ve metroIDs istemek için kullanabilirsiniz. Metro alan, değiştirilebilir Kimlikleridir.

**metroID:** 522 **adı:** Seattle Ankara Bellevue

![Seattle metro alanı](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Kimlikleri Durdur

Geçiş durakları başvurulabilir kimlikleri, iki tür tarafından [genel aktarım akışı belirtimi (GFTS)](https://gtfs.org/) kimliği (stopKey adlandırılır) ve Azure haritalar kimliği (stopId adlandırılır) durdurun. Zaman içinde durakları söz konusu olduğunda, bu kimliği daha kararlı ve fiziksel Durdur mevcut olduğu sürece büyük olasılıkla değiştirmez, Azure haritalar durdurma kimliği kullanmak için önerilir. Fiziksel Durdur herhangi bir değişiklik olmasına rağmen GTFS durdurma kimliği daha sık örneğin GTFS sağlayıcısı değiştirmeniz gerekiyor veya yeni GTFS sürüm yayımlandıktan, durumunda güncelleştirilir.

Başlamak için yakında geçiş durdurur kullanarak isteyebilir [alma yakında aktarım API](https://aka.ms/AzureMapsMobilityNearbyTransit).

## <a name="line-groups-and-lines"></a>Satır grupları ve satırlar

Mobility hizmeti, satırları bir paralel veri modeli kullanır ve daha iyi değişikliklerle başa çıkmak için satır grupları öğesinden devralınan [GTFS](https://gtfs.org/) yollar ve gelişlerin veri modeli.


### <a name="line-groups"></a>Satır grupları

Grup haline getirdiği mantıksal olarak aynı grubunun parçası olan tüm satırları varlığın satır grubudur. Genellikle iki satır, bir giderek noktası a, B, diğer noktası B'den A'ya, her iki ait aynı kamu Ulaşımını Ajans döndüren ve aynı satır numarasına sahip bir satır grubu içerir. Ancak, satır grubu iki satırdan veya yalnızca tek bir satır içindeki olduğu durumlar olabilir.


### <a name="lines"></a>satırları

Yukarıda açıklandığı gibi her bir satır grubu satır kümesinden oluşur. Genellikle bir yön ve her satırın her satırını açıklayan iki satır grubuna oluşur. Ancak, bazen belirli bir Komşuları ve bazen saptıran satırı daha fazla hangi satırda bir satır grubu örneğin var. oluşturan olduğu vakaları desteklemez ve her iki durumda da aynı satır numarası altında işletilen ve diğer durumlarda vardır bir satır g tek bir satır, örneğin döngüsel bir satır tek bir yön ile ruplama oluşur.

Başlamak için satır grupları kullanarak isteyebilirsiniz [alma geçiş satırı API](https://aka.ms/AzureMapsMobilityTransitLine) ve sonraki satırları Veriler'i.


## <a name="next-steps"></a>Sonraki adımlar

İstek Mobility Hizmeti'ni kullanarak geçiş verilerini öğrenin:

> [!div class="nextstepaction"]
> [İstek aktarım sırasında verileri nasıl](how-to-request-transit-data.md)

Mobility hizmetini kullanarak gerçek zamanlı veri isteği işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veri isteği nasıl](how-to-request-real-time-data.md)

Azure haritalar Mobility hizmeti API belgelerini keşfedin

> [!div class="nextstepaction"]
> [Mobility hizmeti API'si belgeleri](https://aka.ms/AzureMapsMobilityService)
