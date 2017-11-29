---
title: "Azure konumu genel bakış temel Hizmetleri | Microsoft Docs"
description: "Azure konum tabanlı Hizmetleri (Önizleme) bir giriş"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: article
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c8ebce06a72bcaf769a11ec954702463d7489aa0
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Azure konum tabanlı Hizmetleri (Önizleme) bir giriş
Temel Azure konum Hizmetleri, haritalar, arama, yönlendirme, trafik ve saat dilimleri için hizmet API'leri içeren Jeo-uzamsal hizmetler yelpazesi ' dir. Azure OneAPI uyumlu Hizmetleri yelpazesi, hızlı bir şekilde geliştirmek için tanıdık geliştirici araçlarını kullanmayı ve konum bilgileri Azure çözümleriyle tümleştirmek ölçek çözümleri sağlar. Azure konum tabanlı Hizmetleri coğrafi bağlam web ve mobil uygulamaları için sağlama için yeni eşleme veri kesinliği ile güçlü Jeo-uzamsal özellikleri paketlenmiş tüm sektörlerde geliştiricilerden sağlar. Azure konum tabanlı Hizmetleri bir Azure bir API uyumlu REST web tabanlı bir JavaScript denetimi ile eşlik geliştirme arasında birden çok ortamlarının Süper kolay, esnek ve taşınabilir hale getirmek için API kümesidir. 

Azure konum tabanlı Hizmetleri coğrafi bağlam gerektiren Azure uygulamalarını destekleyecek beş birincil hizmetinden oluşur. Hizmetlerinin her biri aşağıda ayrıntılı olarak açıklanmıştır.

**Hizmetini işleme** – işleme hizmeti, geliştiricilerin web ve mobil uygulamaları eşleme geçici oluşturmak tasarlanmıştır. Hizmet, yüksek kaliteli ızgara grafik görüntüleri, 19 yakınlaştırma düzeylerinde kullanılabilir ya da tam olarak özelleştirilebilir vektör biçimi harita resimlerini kullanır.

![Hizmetleri Map.png Azure konumuna dayalı](media/about-location-based-services/Introduction_Map.png)

**Hizmet rota** – rota hizmet güçlü gerçek altyapı geometri hesaplamaları ve birden çok taşıma modu yönergeleri ile oluşturulur. Geliştiricilerin arasında seyahat modları araba kamyon, bisiklet veya taramasını gibi çeşitli yönleri hesaplamak hizmet sağlar; aynı zamanda, girdi trafiği koşulları, ağırlık kısıtlamaları veya tehlikeli malzeme aktarım gibi bir sayı.

![Hizmetleri Route.png Azure konumuna dayalı](media/about-location-based-services/Introduction_Route.png)

**Arama hizmeti** – arama hizmeti adresleri, yerler, adı veya kategori iş listelerini ve diğer coğrafi bilgileri aramak geliştiriciler için tasarlanmıştır. Arama hizmeti de yapabilirsiniz [ters geocode](https://en.wikipedia.org/wiki/Reverse_geocoding) adresleri ve çapraz streets göre enlem/boylam üzerinde. 

![Hizmetleri Search.png Azure konumuna dayalı](media/about-location-based-services/Introduction_Search.png)

**Saat dilimi hizmet** – saat dilimi hizmet ya da enlem boylam çiftleri kullanarak sorgu geçerli, geçmiş ve gelecekteki saat dilimi bilgileri verir veya bir [IANA kimliği](http://www.iana.org/). Saat dilimi hizmet da IANA saat dilimleri için Microsoft Windows Saat dilimi kimlikleri dönüştürme, bir saat dilimi farkı UTC'ye getirme ve ilgili bir saat diliminde geçerli saati alma sağlar. Bir sorgu için tipik JSON yanıt saat dilimi hizmetine aşağıdaki gibi görünür:

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

**Hizmet trafiği** – trafik hizmetidir geliştiricilerin web ve trafik gerektiren mobil uygulamaları oluşturmak için tasarlanmış web hizmetleri dizisi. Teklifi aşağıdaki ayrılır:
1. Trafik akışı - gerçek zamanlı gözlemlenen hızları sağlar ve; ağdaki tüm anahtar yollar saatlere seyahat ve 
2. Olaylar trafiği - trafik sıkışması ve yol ağ geçici olaylar hakkında doğru bir görünümünü sağlar.

![Hizmetleri trafiğinin Azure konumuna dayalı](media/about-location-based-services/Introduction_Traffic.png)

Azure konum tabanlı Hizmetleri için mobility oluşturulur ve programlama modeli bağımsızdır ve REST API'leri aracılığıyla JSON çıktısını destekleyen platformlar arası uygulamalar güç. Ayrıca, Azure lb kullanışlı bir JavaScript harita denetiminin hem web hem de mobil uygulamalar, hızlı kolay geliştirilmesi için basit bir programlama modeli sunar. 

Azure konum tabanlı Hizmetleri bir anahtar tabanlı kimlik doğrulama şemasını kullanır, böylece hizmetlerine erişme sağlasa da için gezinme [Azure portal](http://portal.azure.com) ve Azure konum tabanlı Services hesabı oluşturma. Hesabınız, sizin için önceden oluşturulan iki anahtarları ile birlikte gelir. Bu konumu yetenekleri anahtarlarınızı herhangi birini Azure konum tabanlı Hizmetleri hizmet isteklerine kullanarak doğrudan uygulamalarınıza tümleştirmek başlatın.

Kaydolun bir [Azure konum tabanlı hizmetler hesabı Bugün!](http://aka.ms/azurelbsportal)

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure konum tabanlı Hizmetleri (Önizleme) genel bakış var. Sonraki adım olarak, konum tabanlı Hizmetleri birtakım sergileyen bir örnek uygulama deneyin bir uçtan uca senaryoyu web uygulamanızı oluşturmak için ' dir.

> [!div class="nextstepaction"]
> [Azure konum tabanlı Hizmetleri (Önizleme) kullanarak gösteri etkileşimli harita arama başlatma](quick-demo-map-app.md)
> [ilgi çekici Azure konum tabanlı Hizmetleri kullanarak yakındaki arama](tutorial-search-location.md)