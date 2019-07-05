---
title: Azure Haritalar’a Genel Bakış | Microsoft Docs
description: Azure Haritalar tanıtımı
author: walsehgal
ms.author: v-musehg
ms.date: 02/04/2019
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5f41263113568cf9f3771119135be8db37119181
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442952"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?

Azure haritalar, doğru web ve mobil uygulamalara coğrafik bağlam sağlamak amacıyla yeni eşleme verileri kullanan Jeo-uzamsal hizmetler koleksiyonudur. Azure haritalar sağlar:

* Birden çok stilleri ve uydu tanımayı eşlemeleri oluşturma için REST API'leri.
* Adresler, yerler ve dünyanın dört bir yanındaki ilgi noktası arar.
* Yönlendirme noktadan noktaya, multipoint, multipoint iyileştirme, isochrone, ticari araç, trafiği etkiler ve matris yönlendirme; trafik akışını ve olayları görüntüleme.
* Mobility hizmetlerini'için genel geçiş ve taşıma (bisiklet paylaşımı, scooter paylaşımı ve araba paylaşımı gibi) alternatif modları isteme ve planlama yolların gerçek zamanlı olarak. 
* Coğrafi konum aracılığıyla kullanıcı konumunu oluşturma ve konum saat dilimlerine dönüştürmenizi. 
* Bölge sınırlaması ve eşleme ile veri depolama alanı, konum bilgilerini Azure üzerinde barındırılan hizmetler. 
* Konum zekası Jeo-uzamsal analiz aracılığıyla. 

REST API'lerine ek olarak, Azure haritalar Hizmetleri, Web SDK'sı veya Android SDK'sı kullanılabilir. Bu araçlar, geliştiricilerin hızlı bir şekilde geliştirin ve konum bilgilerini Azure çözümleriyle tümleştirme çözümleri yardımcı olur. 

Ücretsiz kaydolabilirsiniz [Azure haritalar hesabı](https://azure.microsoft.com/services/azure-maps/) ve geliştirmeye başlayın.

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Eşleme denetimleri

### <a name="web-sdk"></a>Web SDK

Azure haritalar Web SDK'sı kendi içerik ve tanımayı görüntülenmek üzere web veya mobil uygulamalarınızla etkileşimli Eşlemleriyle özelleştirmenizi sağlar. Yüksek performanslı, büyük veri kümelerini işleyebilir WebGL, kullandığından bu denetimi yapar. SDK'sı ile JavaScript veya TypeScript kullanarak geliştirin.

![Örnek eşleme popülasyonun değiştirme](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Android SDK

Azure haritalar Android SDK'sı mobil eşleme uygulamaları oluşturmak için kullanın. 

![Bir mobil cihazda harita örnekleri](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure haritalar, Azure uygulamalarınız coğrafik bağlam sağlamak aşağıdaki dokuz Hizmetleri oluşur.

### <a name="data-service"></a>Data Service

Veri eşlemeleri için bir zorunluluktur. Karşıya yükleme ve uzamsal işlemleri veya görüntü oluşturma ile kullanmak için Jeo-uzamsal verileri depolamak için veri hizmetini kullanın.  Azure haritalar hizmetine yakın müşteri verileri getirme gecikme süresini azaltın, verimlilik ve yeni senaryolar uygulamalarınızda oluşturun. Bu hizmet hakkında ayrıntılı bilgi için bkz. [veri hizmeti API'si belgeleri](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobility hizmeti

Azure haritalar Mobility hizmeti, gerçek zamanlı seyahat planlama sağlar. Bu en iyi olası rota seçenekleri döndürür ve birçok farklı seyahat modlarında sağlar. Metro (şehir) alanları için bu modlardan walking Bisiklete ve genel aktarım içerebilir. Geliştiriciler, geçiş rehberi ayrıntıları satır geometri, durdurur, zamanlanmış bir listesi ve gerçek zamanlı varış ve hizmet uyarılarını gibi isteyebilir.

Hizmet, bir konuma paylaşılan bisiklet, scooters veya arabalar gibi belirli nesne türlerini arar de sağlar. Kullanıcılar, kaç tane kullanılabilir paylaşılan bisiklet en yakın dock'ta bırakılır isteyebilir. Kullanılabilir bir araba paylaşımı taşıtlardan aramak ve gelecekteki kullanılabilirlik ve geçerli yakıt düzeyi gibi ayrıntıları bulabilirsiniz.

Hizmet hakkında daha fazla bilgi için bkz: [Mobility API belgeleri](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>İşleme hizmeti

İşleme hizmeti, geliştiricilerin web ve mobil uygulamaları oluşturma yardımcı olur. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Bir harita işleme hizmeti örneği](media/about-azure-maps/Introduction_Map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Daha fazla bilgi edinmek için [Render API'si belgeleri](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Yönlendirme hizmeti

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi çeşitli seyahat modları arasında yol tariflerini hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma gibi girdileri de dikkate alabilir.

![Örnek eşleme yönlendirme hizmeti](media/about-azure-maps/Introduction_Route.png)

Yönlendirme hizmeti gibi gelişmiş özelliklerin önizlemesini sunar: 

* Birden çok yol istekleri toplu işlenmesi.
* Matrisler, zaman ve uzaklık hedefler ve kaynaklar kümesi arasında seyahat.
* Bulma yollar veya zaman ya da yakıt gereksinimlerine göre kullanıcıların ilerleyebilir uzaklıkları gösterir. 

Yönlendirme özellikleri hakkında daha fazla bilgi için okuma [rota API belgeleri](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Arama hizmeti

Arama hizmeti geliştiricilerin arama adresler, yerler, ad veya kategori tarafından işletme listeleme için yardımcı olur ve diğer coğrafi bilgileri. Arama hizmeti [tersine coğrafi](https://en.wikipedia.org/wiki/Reverse_geocoding) adresleri ve caddeleri latitudes ve longitudes tabanlı.

![Bir haritadaki bir arama örneği](media/about-azure-maps/Introduction_Search.png)

Arama hizmeti gibi gelişmiş özellikler de sağlar:

* Yol boyunca arayın.
* Daha geniş bir alanı içinde arayın.
* Bir grup arama istekleri toplu olarak.
* Bir konumu nokta yerine daha büyük alan arayın. 

Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkında daha fazla ayrıntı için okuma [arama API'si belgeleri](https://docs.microsoft.com/rest/api/maps/search).

### <a name="spatial-operations-service"></a>Uzamsal Operations hizmeti

Azure haritalar, uzamsal Operations hizmet konum bilgileri alır ve süre ve alanı gerçekleşen Süren etkinlikler müşteriye yardımcı olmak için hareket halindeyken analiz eder. Ancak, gerçek zamanlı analiz ve Tahmine dayalı modelleme olayları etkinleştirir. 

Hizmet, müşterilerin kendi konum zekası en yakın noktası, harika daire uzaklık ve arabellek gibi ortak Jeo-uzamsal matematiksel hesaplamalar kitaplığı ile geliştirmek sağlar. Çeşitli özelliklerini ve hizmet hakkında daha fazla bilgi edinmek için [uzamsal Operations API'si belgeleri](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat dilimi hizmeti güncel, geçmiş ve gelecek saat dilimi bilgileri sorgulamak için ya da enlem/boylam çiftleri kullanarak sağlar veya bir [IANA ID](https://www.iana.org/). Saat dilimi Hizmeti ayrıca sağlar:

* Microsoft Windows Saat dilimi kimliklerini IANA saat dilimlerine dönüştürmenizi.
* Bir saat dilimi uzaklığı UTC'ye getiriliyor.
* Geçerli saati bir saat diliminde alınıyor. 

Saat dilimi hizmeti için tipik JSON yanıtı bir sorgu için aşağıdaki örneğe benzer şekilde görünür:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Bu hizmet hakkında daha fazla bilgi için okuma [saat dilimi API belgeleri](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Trafik hizmeti

Trafik hizmeti geliştiricilerin web ve akış bilgisi gerektiren mobil uygulamalar oluşturmak için kullanabileceğiniz web hizmetleri paketidir. Hizmet, iki tür veri sağlar:

* Trafik akışı: Gerçek zamanlı, yüksek hız ve seyahat planlarını ağdaki tüm anahtar yollar için gözlemledik.
* Trafik olayları: Trafik sıkışıklıklarının ve olaylarının yol ağı, güncel bir görünüm.

![Akış bilgileri ile bir harita örneği](media/about-azure-maps/Introduction_Traffic.png)

Daha fazla bilgi için [trafiği API belgeleri](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location-service"></a>Konum hizmeti için IP

Bir IP adresi için alınan iki harfli ülke kodu önizlemek için konum hizmeti IP kullanın. Bu hizmet, uyarlama ve özelleştirilmiş uygulama içerik coğrafi konum temelinde sağlayarak kullanıcı deneyimini geliştirmenize yardımcı olabilir.

Konum hizmeti IP için REST API'leri hakkında daha fazla ayrıntı için okuma [Azure haritalar coğrafi konum API belgeleri](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Programlama modeli

Azure haritalar hareketlilik için oluşturulmuştur ve platformlar arası uygulamalar geliştirmenize yardımcı olabilir. Dil belirsiz olduğundan ve üzerinden JSON çıkışını destekleyen bir programlama modeli kullanan [REST API'leri](https://docs.microsoft.com/rest/api/maps/).

Azure Haritalar ayrıca hem web hem de mobil platform uygulamalarının hızlı ve kolay bir şekilde geliştirilmesi için basit bir programlama modeliyle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-control) sunar.

## <a name="usage"></a>Kullanım

Azure haritalar hizmetlerine erişimi olan sağlasa da, gidip [Azure portalında](https://portal.azure.com) ve bir Azure haritalar hesabı oluşturuluyor.

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınız, sizin için önceden oluşturulmuş iki anahtar ile birlikte gelir. Bu anahtarlardan birini kullanıp Azure Haritalar hizmetine istekte bulunarak bu konum özelliklerini uygulamanızla tümleştirmeye başlayabilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure haritalar API bunlar dışında tüm ülkeler/bölgeler şu anda kullanılabilir:

* Arjantin
* Çin
* Hindistan
* Fas
* Pakistan
* Güney Kore

Geçerli IP adresinizi konumunu desteklenmeyen ülkeler/bölgelerden birinde olmadığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar'ı gösteren bir örnek uygulama deneyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir web uygulaması oluşturma](quick-demo-map-app.md)

Azure haritalar hakkında güncel kalır: 

> [!div class="nextstepaction"]
> [Azure haritalar blogu](https://azure.microsoft.com/blog/topics/azure-maps/)
