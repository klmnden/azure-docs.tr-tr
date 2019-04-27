---
title: Azure Ağ Performansı İzleyicisi için fiyatlandırma SSS | Microsoft Docs
description: Sık sorulan sorular - Azure Ağ Performansı İzleyicisi
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.openlocfilehash: 77cacd7f94d8ddd92fcd7383d2d0a7929734eaeb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60401417"
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Azure Ağ Performansı İzleyicisi için fiyatlandırma değişiklikleri

Biz Geribildiriminizi ve yakın zamanda sunulan bir [deneyimi yeni fiyatlandırma](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure genelinde çeşitli için izleme hizmetleri. Bu makalede, Azure ile ilgili fiyatlandırma değişiklikleri yakalar [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) kolay okunur soru ve yanıt biçiminde.

Ağ Performansı İzleyicisi üç bileşenden oluşur:
* [Performans İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Hizmet uç noktası İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview)
* [ExpressRoute İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

Aşağıdaki bölümlerde, NPM bileşenleri için fiyatlandırma değişiklikleri açıklanmaktadır.

## <a name="performance-monitor"></a>Performans İzleyicisi

**Eski modelde faturalandırılır Performans İzleyicisi'nin kullanım nasıldı?**

NPM faturalandırması kullanım ve iki bileşenden tüketimini temel:
* **Düğümleri**: Tüm yapay işlemler, kaynaklanan ve düğümler sonlandırın. Düğümleri de aracıları veya Microsoft Yönetim aracıları adlandırılır.
* **Veri**: Çeşitli ağ testlerin sonuçlarını Log Analytics çalışma alanında depolanır.

Eski modelde, fatura düğüm sayısını ve oluşturulan veri hacmine bağlı olarak hesaplandı. 

**Performans İzleyicisi'nin kullanımı, yeni modeli altında nasıl ücretlendirilir?**

NPM Performans İzleyicisi özelliği, artık bir birleşimini göre faturalandırılır: 

* İzlenen alt ağ bağlantıları
* Veri hacmi

**Bir alt ağ bağlantısı nedir?**

Performans İzleyicisi, ağ üzerinde en az iki konum arasında bağlantısını izler. Bir Grup düğümleri veya aracılar üzerinde bir alt ağ ve bir grubu başka bir alt ağdaki düğümler arasındaki bağlantıyı bir alt ağ bağlantı adı verilir.

**(A ve B) iki alt ağa sahibim ve her alt ağda birden fazla aracıları sahibim. Bir alt ağdaki tüm aracıları bağlantısı b alt ağdaki tüm aracıları için Performans İzleyicisi izler Arası alt ağ bağlantıları sayısına göre ücretlendirilirim?**

Hayır. Faturalandırma işlemlerinde, bir alt ağından gelen tüm bağlantıları B alt ağa bir alt ağ bağlantısına birlikte gruplandırılır. Tek bir bağlantı için faturalandırılırsınız. Performans İzleyicisi, her alt ağda bulunan çeşitli aracıları arasındaki bağlantıyı izlemek devam eder.

**Bir alt ağ bağlantısı izleme maliyetlerini nelerdir?**

Tüm ay boyunca tek alt ağ bağlantısını izleme maliyeti için bkz. [Ping ağı](https://azure.microsoft.com/pricing/details/network-watcher/) bölümü.

**Performans İzleyicisi oluşturan veri ücretleri nelerdir?**

Alımı (Log Analytics çalışma alanında işleme ve dizin oluşturma Azure İzleyici, veri yükleme) için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) veri alımı bölümünde Log Analytics için. Veri saklama (diğer bir deyişle, veriler ilk aydan sonra müşterinin seçeneği tutulur) ücreti de kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/), veri saklama bölümünde.


## <a name="expressroute-monitor"></a>ExpressRoute İzleyicisi

**ExpressRoute İzleyicisi kullanımı için ücretler nelerdir?**

ExpressRoute İzleyicisi için ücretleri, izleme sırasında oluşturulan veri hacmine göre faturalandırılır. Daha fazla bilgi için "Performans İzleyicisi oluşturan veri ücretleri nelerdir?" konusuna bakın.

**Birden çok ExpressRoute bağlantı hatlarını izlemek için ExpressRoute İzleyicisi kullanıyorum. Devreleri izleniyor sayısına göre ücretlendirilirim?**

Ücretlendirilmez ya da bağlantı hatları sayısı veya eşleme türüne göre (örneğin, özel eşdüzey hizmet sağlama, Microsoft eşdüzey hizmet sağlama). Veri hacmi üzerinden daha önce açıklandığı gibi üzerinden ücretlendirilirsiniz.

**ExpressRoute, tek bir bağlantı hattı izler, oluşturulan veri hacmi nedir?**

Her zaman ExpressRoute özel eşleme bağlantıları izler, ay, oluşturulan veri hacmi aşağıdaki gibidir:

|Yüzdebirlik      |Veri/ay (MB)|
| :---:          |           ---:|
|50<sup>th</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>th</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>th</sup> |            870|
|95<sup>th</sup> |           1560|


Bu tabloda göre 50. yüzdebirlik müşterilerine 192 MB veri için ödeme yaparsınız. USD $2.30/GB üzerinden ilk ay için bir bağlantı hattı izlemek için tahakkuk ABD Doları maliyetidir $0.43 (192 * 2.30 = / 1024).

**Bazı nedenlerle Veri hacmindeki nelerdir?**

Birim, oluşturulan izleme gibi çeşitli etkenlere bağlıdır:
* Aracı sayısı. Aracı sayısı arasında bir artış ile hata Yalıtımı doğruluğu artırır.
* Ağ üzerinde durak sayısı.
* Kaynak ve hedef arasında yolları sayısı.

(Önceki tabloda) daha yüksek yüzdebirliklerini müşterilerine genellikle kendi devreler kendi şirket içi ağındaki birden fazla görüş noktalarından izleyin. Birden çok aracı ayrıca ağ hizmeti sağlayıcısı uç yönlendiricisinde uzaklaştıkça daha derin yerleştirilir. Aracılar, genellikle birkaç kullanıcı siteleri, dalları ve veri merkezlerinde raflar yerleştirilir.

## <a name="service-endpoint-monitor"></a>Hizmet uç noktası İzleyicisi

**Hizmet uç noktası İzleyicisi kullanımı için ücretler nelerdir?**

Hizmet uç noktası İzleyicisi'nin kullanımları temel alınarak hesaplanır:
* Bağlantı sayısı
* Veri hacmi

**Bağlantı nedir?**

Bağlantı bir erişilebilirlik bir uç nokta (URL veya ağ hizmeti) için tek bir Aracıdan tüm ay boyunca sınamasıdır. Örneğin, bir bağlantı için bing.com üç aracılardan izleme üç bağlantıları oluşturur.

**Hizmet uç noktası İzleyicisi maliyetlerini nelerdir?**

Başvurmak [bağlantı izleme](https://azure.microsoft.com/pricing/details/network-watcher/) tüm ay boyunca bir uç nokta izleme bölümüne maliyeti. Veri için ücret kullanılabilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/log-analytics/) veri alımı bölümünde Log Analytics için.

## <a name="references"></a>Başvurular

[Log Analytics fiyatlandırma SSS](https://azure.microsoft.com/pricing/details/log-analytics/): Ücretsiz katman, her düğüm fiyatlandırma ve diğer fiyatlandırma ayrıntıları hakkında bilgi aşağıdaki SSS bölümüne sahiptir.

