---
title: Azure ağı Performans İzleyicisi için fiyatlandırma hakkında SSS | Microsoft Docs
description: Sık sorulan sorular - Azure Ağ Performansı İzleyicisi
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.openlocfilehash: 1e7e43dc2e7ed386f8f77fd1ab186d2ff34af405
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Azure Ağ Performansı İzleyicisi için fiyatlandırma değişiklikleri

Biz geri bildirimlerinize kulak ve en yeni bir [yeni deneyimi fiyatlandırma](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure çeşitli için izleme hizmetleri. Bu makalede Azure için ilgili fiyatlandırma değişiklikleri yakalar [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) kolay okunur soru ve yanıt biçiminde.

Ağ Performansı İzleyicisi üç bileşenden oluşur:
* [Performans İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Hizmet uç noktası İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor)
* [ExpressRoute İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

Aşağıdaki bölümlerde NPM bileşenleri için fiyatlandırma değişiklikleri açıklamaktadır.

## <a name="performance-monitor"></a>Performans İzleyicisi

**Performans İzleyicisi'ni eski modelinde faturalandırılır kullanımını nasıl oldu mu?**

NPM için fatura kullanım ve iki bileşenden tüketim dayalı:
* **Düğümleri**: tüm yapay işlemler kaynaklanan ve düğümler sonlandırılacak. Düğümler aynı zamanda aracıları veya Microsoft Yönetim aracıları olarak adlandırılır.
* **Veri**: çeşitli ağ test sonuçlarını Azure günlük analizi deposunda saklanır.

Eski modelinde, fatura düğüm sayısı ile oluşturulan veri hacmine göre hesaplanmıştır. 

**Kullanım Performans İzleyicisi'nin altında yeni model nasıl ücretlendirilir?**

NPM Performans İzleyicisi'ni özelliği şimdi bir birleşimini faturalandırılır: 

* İzlenen alt ağ bağlantıları
* Veri hacmi

**Bir alt ağ bağlantı nedir?**

Performans İzleyicisi ağda iki veya daha fazla konum arasında bağlantısını izler. Bir Grup düğümleri veya bir alt ağdaki aracıları ve bir grubu başka bir alt ağdaki düğümler arasındaki bağlantı, bir alt ağ bağlantı adı verilir.

**(A ve B) iki alt ağa sahip ve her alt ağda birkaç aracıları sahibim. Performans İzleyicisi B. alt ağdaki tüm aracıların bir alt ağdaki tüm aracıların bağlantısını izler I arası alt ağ bağlantılarının sayısı göre ücretlendirilir?**

Hayır. Faturalandırma amacıyla, alt ağa B alt ağı gelen tüm bağlantıları bir alt ağ bağlantısına birlikte gruplandırılır. Tek bir bağlantı için fatura. Performans İzleyicisi'ni her alt ağda çeşitli aracılar arasındaki bağlantıyı izlemeye devam eder.

**Bir alt ağ bağlantı izleme maliyetleri nelerdir?**

Maliyet tüm ay için tek bir alt ağ bağlantısını izleme Bkz [Ping kafes](https://azure.microsoft.com/pricing/details/network-watcher/) bölümü.

**Performans İzleyicisi'ni oluşturur veri ücretleri nelerdir?**

Alım (günlük işleme ve dizin oluşturma analizi, karşıya yükleme veri) için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) veri alımı bölümündeki günlük analizi için. Veri saklama (diğer bir deyişle, ilk ayın ötesinde müşterinin seçeneği korunur veri) için ücretsiz olarak da kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/), veri saklama bölümünde.


## <a name="expressroute-monitor"></a>ExpressRoute İzleyicisi

**ExpressRoute İzleyicisi'nin kullanım ücretleri nelerdir?**

ExpressRoute İzleyici için ücret izleme sırasında oluşturulan veri hacmine göre faturalandırılır. Daha fazla bilgi için "Performans İzleyicisi'ni oluşturur veri ücretleri nelerdir?" konusuna bakın.

**I birden çok ExpressRoute bağlantı hatları izlemek için ExpressRoute İzleyicisi'ni kullanın. I izlenmekte olan devreler sayısına göre ücret 'M?**

Değil Ücretli tabanlı ya da sayısını devreler veya eşliği türünü (örneğin, özel eşleme, Microsoft eşlemesi). Veri hacmine göre daha önce açıklandığı gibi sizden ücret kesilir.

**Tek bir hattı ExpressRoute izler zaman oluşturulan veri hacmini nedir?**

Her zaman ExpressRoute özel bir eşleme bağlantı izler, ay, oluşturulan veri hacmini aşağıdaki gibidir:

|Yüzdebirlik      |Veri/ay (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Bu tablo göre 50. Yüzdeliğini müşterilerine 192 MB veri için ücret ödersiniz. ABD Doları $2.30/GB olarak ilk ayı için bir bağlantı hattı izleme için ücrete ABD Doları maliyetidir $0.43 (192 * 2.30 = / 1024).

**Bazı nedenlerle Veri hacmi Çeşitlemeler nelerdir?**

Birimin oluşturulan veri izleme gibi çeşitli etkenlere bağlıdır:
* Aracı sayısı. Aracı sayısı artan hataya yalıtım doğruluğunu artırır.
* Ağ üzerinde durak sayısı.
* Kaynak ve hedef arasındaki yolları sayısı.

Daha yüksek bir yüzdebirlik değeri (önceki tabloda) müşterilerine genellikle kendi devreler kendi şirket içi ağındaki birkaç vantage noktalarından izleyin. Birden çok aracı ayrıca ağ hizmeti sağlayıcısı sınır yönlendiricisi uzağa daha derin yerleştirilir. Aracılar, genellikle birkaç kullanıcı sitelerde, dalları ve veri merkezleri rafları yerleştirilir.

## <a name="service-endpoint-monitor"></a>Hizmet uç noktası İzleyicisi

**Hizmet uç noktası İzleyicisi'nin kullanım ücretleri nelerdir?**

İçin hizmet uç noktası İzleyicisi kullanım ücretleri temel alınarak hesaplanır:
* bağlantı sayısı
* veri birimi

**Bir bağlantı nedir?**

Bir bağlantı ulaşılabilirlik sınamasını bir uç nokta (URL veya ağ hizmeti) için tüm ay için tek bir Aracıdan olur. Örneğin, bir bağlantı için aratıp üç aracılardan izleme üç bağlantıları meydana gelir.

**Hizmet uç noktası İzleyicisi maliyetlerini nelerdir?**

Başvurmak [bağlantı izleme](https://azure.microsoft.com/pricing/details/network-watcher/) tüm ay için bir uç nokta izleme bölümü maliyeti. Veri için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) veri alımı bölümündeki günlük analizi için.

## <a name="references"></a>Başvurular

[Günlük analizi fiyatlandırma SSS](https://azure.microsoft.com/pricing/details/log-analytics/): SSS bölümüne sahip düğüm fiyatlandırma başına ücretsiz katmanı ve diğer fiyatlandırma ayrıntıları hakkında bilgi.

