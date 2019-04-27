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
ms.openlocfilehash: 944a111ac0c31e755b3991f9ea101ba652c676d3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799301"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?

Azure haritalar, doğru web ve mobil uygulamalara coğrafi bağlam kullanılabilir en yeni eşleme verileri tarafından Temizle, Jeo-uzamsal hizmetler koleksiyonudur. Azure haritalar, işleme için REST API'lerini oluşur **haritalar** birden çok stilleri ve uydu tanımayı **arama** adresler, yerler ve ilgi; dünyanın dört bir yanındaki noktaları için **Yönlendirme** noktadan noktaya, multipoint, multipoint iyileştirme, isochrone, ticari araç, trafiği etkileyen ve yönlendirme, trafik akışını sektör lideri ve olayları görüntüleme; oluşturma kullanıcı konumu Matrisi aracılığıyla **coğrafi konum**; ve konuma dönüştürme **saat dilimlerini**, yanı, zamanı bir konumda getiriliyor. Ayrıca, Azure haritalar hizmetleri sunar **bölge sınırlaması**, harita **veri** barındırma konum bilgilerini Azure; depolama - ve **uzamsal Operations** konum Jeo-uzamsal analiz aracılığıyla gösterimi. Azure haritalar Hizmetleri doğrudan REST API veya ya da aracılığıyla kullanılabilir bizim sağlam **Web SDK'sı** veya **Android SDK'sı**. Bu araçlar, geliştiricilerin hızlı bir şekilde geliştirin ve konum bilgilerini Azure bulutunun içinden Azure çözümlerinden tümleştirin ölçek çözümleri sağlar. Kaydolmak için ücretsiz [Azure haritalar hesabı](https://azure.microsoft.com/services/azure-maps/) bugüne, başlangıç geliştirme!

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Harita Denetimleri

### <a name="web-sdk"></a>Web SDK

Azure haritalar Web SDK'sı kendi içerik ve tanımayı görüntülenmek üzere web veya mobil uygulamalarınızla etkileşimli Eşlemleriyle özelleştirmenizi sağlar. Bu denetimde büyük veri kümelerini yüksek performansla oluşturmanızı sağlayan WebGL bileşeni kullanılmaktadır. JavaScript veya TypeScript SDK'sı ile geliştirin.

![Azure haritalar Web SDK'sı](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Android SDK

Azure haritalar Android SDK'sı, güçlü mobil eşleme uygulamaları oluşturmanızı sağlar. 

![Azure haritalar Android SDK'sı](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure Haritalar, Azure uygulamalarınıza coğrafi bağlam sağlayabilecek şu altı hizmetten oluşur.

### <a name="render-service"></a>İşleme hizmeti

İşleme hizmeti, geliştiricilerin harita verileri kullanan web ve mobil platform uygulamaları oluşturması amacıyla tasarlanmıştır. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Diğer ayrıntılar için [Azure Haritalar İşleme API'leri](https://docs.microsoft.com/rest/api/maps/render) konusunu okuyun.

### <a name="route-service"></a>Yönlendirme hizmeti

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi çeşitli seyahat modları arasında yol tariflerini hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma gibi girdileri de dikkate alabilir.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Yönlendirme hizmeti şimdi birden çok yol isteğini toplu işleme, bir dizi başlangıç noktasıyla varış noktası arasındaki seyahat süresi ve mesafe matrisleri ve zamanınıza veya benzin gereksinimlerinize göre seyahat edebileceğiniz yolları veya mesafeleri bulma gibi gelişmiş özelliklerin önizlemesini sunar. Yönlendirme özelliklerinin ayrıntıları için, [Azure Haritalar Yönlendirme API'leri](https://docs.microsoft.com/rest/api/maps/route) konusunu okuyun.

### <a name="search-service"></a>Arama hizmeti

Arama hizmeti, geliştiricilerin ad veya kategori veya diğer coğrafi bilgileri kullanarak adres, yer veya işletme aramasını sağlayacak şekilde tasarlanmıştır. Arama Hizmeti adresleri ve caddeleri enlemlere ve boylamlara göre [tersine coğrafi koda dönüştürebilir](https://en.wikipedia.org/wiki/Reverse_geocoding).

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Arama hizmeti, ayrıca yol boyunca arama, daha geniş bir alanda arama, bir grup arama isteğini toplu işleme ve bir konum noktası yerine daha geniş bir alan için arama yapma gibi gelişmiş özellikler de sağlar. Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkındaki diğer ayrıntıları için, [Azure Haritalar Arama API'leri](https://docs.microsoft.com/rest/api/maps/search) sayfasını okuyun.

### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat Dilimi hizmeti, güncel, geçmiş ve gelecek saat dilimi bilgilerini enlem-boylam çifti veya [IANA ID](https://www.iana.org/) kullanarak sorgulamanızı sağlar. Saat Dilimi Hizmeti ayrıca Microsoft Windows saat dilimi kimliklerini IANA saat dilimlerine dönüştürmenizi, UTC ile saat dilimi farkını öğrenmenizi ve belirli bir saat dilimindeki güncel saati almanızı sağlar. Saat Dilimi Hizmetine gönderilen bir sorguya verilen tipik JSON yanıtı aşağıdaki örnek gibi olacaktır:

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

Bu hizmetle ilgili ayrıntılar için [Azure Haritalar Saat Dilimi API'leri](https://docs.microsoft.com/rest/api/maps/timezone) sayfasını ziyaret edin.

### <a name="traffic-service"></a>Trafik hizmeti

Trafik hizmeti, geliştiricilerin trafik verilerine ihtiyaç duyan web ve mobil platform uygulamaları geliştirmeleri için tasarlanmış bir web hizmeti paketidir. Hizmet, iki tür veri sağlar:

* Trafik akışı: Ağ üzerindeki tüm ana yollar için gerçek zamanlı hız ve seyahat süresi bilgileri.
* Trafik olayları - trafik sıkışıklıklarının ve olaylarının yol ağı hakkında güncel bir görünüm.

![Azure Haritalar Trafiği](media/about-azure-maps/Introduction_Traffic.png)

Diğer ayrıntılar için [Azure Haritalar Trafik API'leri](https://docs.microsoft.com/rest/api/maps/traffic) sayfasını ziyaret edin.

### <a name="ip-to-location"></a>IP Aracılığıyla Konum

IP Aracılığıyla Konum hizmeti, belirli bir IP adresi için alınan iki harfli ülke kodunun önizlemesini görüntülemenize olanak sağlar. Bu hizmet, coğrafi konuma göre uyarlanmış uygulama içeriğini destekleyerek kullanıcı deneyimini uyarlamanıza ve geliştirmenize yardımcı olabilir.

IP Aracılığıyla Konum için REST API'lerini kullanma hakkında bilgi almak için [Azure Haritalar Coğrafi Konum API'leri](https://docs.microsoft.com/rest/api/maps/geolocation) sayfasına bakın.

## <a name="programming-model"></a>Programlama modeli

Azure Haritalar mobil kullanım için oluşturulmuştur ve platformlar arası uygulamaları destekleyebilir. Dilden bağımsız bir programlama modeli kullanır ve [REST API'ler](https://docs.microsoft.com/rest/api/maps/) üzerinde JSON çıkışını destekler.

Azure Haritalar ayrıca hem web hem de mobil platform uygulamalarının hızlı ve kolay bir şekilde geliştirilmesi için basit bir programlama modeliyle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-control) sunar.

## <a name="usage"></a>Kullanım

Haritalar hizmetlerine erişmek için [Azure portalına](https://portal.azure.com) gidip Azure Haritalar hesabı oluşturulması gerekir.

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınızda önceden oluşturulmuş iki anahtar mevcuttur. Bu anahtarlardan birini kullanıp Azure Haritalar hizmetine istekte bulunarak bu konum özelliklerini uygulamanızla tümleştirmeye başlayabilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure haritalar API'si şu anda aşağıdaki alt bölgeler dışındaki tüm ülkelerde kullanılabilir:

* Arjantin
* Çin
* Hindistan
* Fas
* Pakistan
* Güney Kore

Geçerli IP adresinizi konumunu yukarıdaki desteklenmeyen ülkelerden birinde olmadığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar'ın yeni özellikleriyle ilgili daha fazla bilgi için:

> [!div class="nextstepaction"]
> [Yol Matrisi, İzokronlar, IP araması ve daha fazlası](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/)

Azure haritalar kullanan örnek uygulamayı deneyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir web uygulaması oluşturma](quick-demo-map-app.md)
