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
ms.openlocfilehash: 5b2335ee2584af07ed23ce87be92a869f3a07ba1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Azure ağı Performans İzleyicisi için fiyatlandırma değişiklikleri

Geri bildiriminiz için dinleme ve yakın zamanda sunulan bir [yeni deneyimi fiyatlandırma](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), Azure üzerinde çeşitli için izleme hizmetleri.

Bu belge için Azure ilgili fiyatlandırma değişiklikleri yakalar [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) herhangi bir soru ve yanıt biçimi okunması kolaydır.

Ağ Performansı İzleyicisi üç bileşenden oluşur:
* [Performans İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Hizmet uç noktası İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor) ve
* [ExpressRoute Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

Aşağıdaki bölümde yukarıdaki bileşenleri için fiyatlandırma değişiklikleri açıklanmaktadır.

## <a name="performance-monitor-pm"></a>Performans İzleyicisi'ni (PM)

**Performans İzleyicisi'ni eski modelinde faturalandırılır kullanımını nasıl oldu mu?**

NPM için fatura kullanım/tüketimi iki bileşenden dayanır:
* Düğüm: Tüm yapay işlemler kaynaklanan ve düğümler sonlandırılacak. Düğümler aynı zamanda aracıları veya MMA (Microsoft Yönetim Aracısı) adlandırılır.
* Veri: Çeşitli ağ test sonuçlarını günlük analizi deposunda saklanır.

Eski modelinde, fatura düğüm sayısı ile oluşturulan veri hacmine göre hesaplanmıştır. 

**Performans İzleyicisi, yeni modeli altında ücret kullanımını nasıl mi?**

NPM Performans İzleyicisi'ni özelliği şimdi bir birleşimini faturalandırılır: 

* izlenen alt ağ bağlantıları ve
* Veri birimi

**Bir alt ağ bağlantı nedir?**

Performans İzleyicisi ağda iki veya daha fazla konum arasında bağlantısını izler.  Düğümleri/aracıları bir alt ağdaki bir grup ve bir grubu başka bir alt ağdaki düğümler arasındaki bağlantı adlandırılır alt ağ bağlantı olarak.

**I iki alt ağ (alt ağ A ve B) ve her alt ağda birkaç aracıları sahiptir.  Performans İzleyicisi B. alt ağdaki tüm aracıların bir alt ağdaki tüm aracıların bağlantısını izler  I arası alt ağ bağlantılarının sayısı göre ücretlendirilir?**

Hayır. Faturalandırma amacıyla alt ağına B alt ağı gelen tüm bağlantıları bir alt ağ bağlantısına birlikte gruplanır ve tek bir bağlantı için faturalandırılır.  Performans İzleyicisi, her alt ağda çeşitli aracılar arasındaki bağlantıyı izlemek devam eder.

**Bir alt ağ bağlantı izleme maliyetleri nelerdir?**

Başlıklı bölüme bakın [Ping kafes](https://azure.microsoft.com/pricing/details/network-watcher/) tüm ay için tek bir alt ağ bağlantısını izleme maliyeti.

**Performans İzleyicisi tarafından oluşturulan veri ücretleri nelerdir?**

Alım (günlük işleme ve dizin oluşturma analizi, karşıya yükleme veri) için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) günlük analizi için.  (Bölüm: veri alımı).

Veri saklama (diğer bir deyişle, ilk ayın ötesinde müşterinin seçeneği korunur veri) için ücretsiz olarak da kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/).  (Bölüm: veri saklama).


## <a name="expressroute-monitor-erm"></a>ExpressRoute İzleyicisi'ni (ERM)

**ExpressRoute İzleyicisi'nin kullanım ücretleri nelerdir?**

ExpressRoute İzleyici için ücret izleme sırasında oluşturulan veri hacmine göre faturalandırılır.   "Performans İzleyicisi tarafından oluşturulan veri ücretleri nelerdir?" soruya bakın Daha fazla bilgi için.

**I ERM birden çok ExpressRoute bağlantı hatlarını izlemek için kullanın. I izlenmekte olan devreler sayısına göre ücret 'M?**

Değil Ücretli tabanlı ya da sayısını devreler veya eşliği türünü (örneğin, özel eşleme, Microsoft eşlemesi).  Yukarıda açıklandığı şekilde, bir veri biriminde sizden ücret kesilir.

**Tek bir hattı izlerken, oluşturulan veri hacmini nedir?**

Özel bir eşleme bağlantı izleme gibi olduğunda ayda oluşturulan veri hacmi:

|Yüzdebirlik      |Veri/ay (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Yukarıdaki tabloda 50. Yüzdeliğini müşterilerine 192 MB veri için ücret ödersiniz. ABD Doları $2.30/GB olarak ilk ayı için bir bağlantı hattı izleme için ücrete ABD Doları maliyetidir 0.43 (192 * 2.30 = / 1024) ilk ayı için.

**Veri birimi Çeşitlemeler nedenlerinden bazıları nelerdir?**

Birimin oluşturulan veri izleme gibi çeşitli etkenlere bağlıdır:
* Aracı sayısı artan aracılar - hata Yalıtımı doğruluğunu sayısını artırır
* ağ üzerinde durak sayısı
* Kaynak ve hedef arasındaki yolları sayısı

Daha yüksek bir yüzdebirlik değeri (Yukarıdaki tablodaki), müşterilerinin genellikle kendi devreler kendi şirket içi ağındaki birkaç vantage noktalarından izleyin.  Birden çok aracı ayrıca ağ hizmeti sağlayıcısı sınır yönlendiricisi uzağa daha derin yerleştirilir. Aracıları genellikle birkaç kullanıcı siteleri, dal ve veri merkezleri rafları yerleştirilmiş.

## <a name="service-endpoint-monitor-sepm"></a>Hizmet uç noktası İzleyicisi (SEPM)

**Hizmet uç noktası İzleyicisi'nin kullanım ücretleri nelerdir?**

İçin hizmet uç noktası İzleyicisi kullanım ücretleri temel alınarak hesaplanır:
* bağlantı sayısı
* veri birimi

**Bir bağlantı nedir?**

Bir bağlantı ulaşılabilirlik sınamasını bir uç nokta (URL veya ağ hizmeti) için tüm ay için tek bir Aracıdan olur. Örneğin, bir bağlantı için aratıp üç aracılardan izleme üç bağlantıları meydana gelir.

**Hizmet uç noktası İzleyicisi maliyetlerini nelerdir?**

- Başvurmak [bağlantı izleme](https://azure.microsoft.com/pricing/details/network-watcher/) tüm ay için bir uç nokta izleme maliyeti için bölüm.
- Veri için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) günlük analizi için.  (Bölüm: veri alımı).

## <a name="references"></a>Başvurular

- [Günlük analizi fiyatlandırma SSS](https://azure.microsoft.com/pricing/details/log-analytics/) -SSS bölümüne sahip düğüm fiyatlandırma, vb. başına ücretsiz katmanı hakkında bilgi.

