---
title: "Azure Konum Tabanlı Hizmetler'e Genel Bakış | Microsoft Docs"
description: "Azure Konum Tabanlı Hizmetler'e (önizleme) giriş"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: overview
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9b4b54c3a4cf0ed4350f570259f6997e4398682b
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Azure Konum Tabanlı Hizmetler'e (önizleme) giriş
Azure Konum Tabanlı Hizmetler; Haritalar, Arama, Yönlendirme, Trafik ve Saat dilimlerine yönelik hizmet API'lerini içeren bir jeo-uzamsal hizmetler portföyüdür. Azure OneAPI uyumlu hizmet portföyü bilindik geliştirici araçlarını kullanarak hızlıca konum bilgilerini Azure çözümlerinizle tümleştiren çözümler geliştirmenizi ve bu çözümleri ölçeklendirmenizi sağlar. Azure Konum Tabanlı Hizmetler tüm sektörlerden geliştiriciler ve güçlü jeo-uzamsal özellikler sunar. Ayrıca web ve mobil uygulamalara coğrafi bağlam sağlamak için gerekli olan yeni eşleme verileriyle donatılmıştır. Azure Konum Tabanlı Hizmetler, REST API'lerin Azure One API’siyle uyumlu bir kümesidir ve geliştirmeyi çok kolay, esnek ve birden fazla ortam arasında taşınabilir hale getirmek amacıyla web tabanlı bir JavaScript denetimiyle birlikte sunulur. 

Azure Konum Tabanlı Hizmetler, coğrafi bağlama ihtiyaç duyan Azure uygulamalarını güçlendiren beş temel hizmetten oluşur. Hizmetlerin her biri aşağıda ayrıntılı bir şekilde açıklanmıştır.

**İşleme Hizmeti**: İşleme Hizmeti, geliştiricilerin harita verileri kullanan web ve mobil platform uygulamaları oluşturması amacıyla tasarlanmıştır. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Azure Location Based Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Yönlendirme Hizmeti**: Yönlendirme Hizmeti sağlam gerçek dünya altyapı geometrisi hesaplamalarıyla ve çoklu taşıma modu yönleriyle oluşturulmuştur. Bu hizmet geliştiricilerin araba, kamyon, bisiklet veya yürüme gibi farklı seyahat modlarında yön hesaplamasının yanı sıra trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma özellikleri gibi girişlerden faydalanmasını sağlar.

![Azure Location Based Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Arama Hizmeti**: Arama Hizmeti geliştiricilerin ad veya kategori veya diğer coğrafi bilgileri kullanarak adres, yer veya işletme aramasını sağlayacak şekilde tasarlanmıştır. Arama Hizmeti adresleri ve caddeleri enlem/boylam değerine göre [tersine coğrafi koda dönüştürebilir](https://en.wikipedia.org/wiki/Reverse_geocoding). 

![Azure Location Based Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Saat Dilimi Hizmeti**: Saat Dilimi Hizmeti güncel, geçmiş ve gelecek saat dilimi bilgilerini enlem-boylam çifti veya [IANA ID](http://www.iana.org/) kullanarak sorgulamanızı sağlar. Saat Dilimi Hizmeti ayrıca Microsoft Windows saat dilimi kimliklerini IANA saat dilimlerine dönüştürmenizi, UTC ile saat dilimi farkını öğrenmenizi ve belirli bir saat dilimindeki güncel saati almanızı sağlar. Saat Dilimi Hizmetine gönderilen bir sorguya verilen tipik JSON yanıtı aşağıdaki gibi olacaktır:

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

**Trafik Hizmeti**: Trafik Hizmeti geliştiricilerin trafik verilerine ihtiyaç duyan web ve mobil platform uygulamaları geliştirmeleri için tasarlanmış bir web hizmeti paketidir. Teklif aşağıdakilere ayrılmıştır:
1. Trafik Akışı: Ağ üzerindeki tüm ana yollar için gerçek zamanlı hız ve seyahat süresi bilgilerini sağlar ve 
2. Trafik Olayları: Yol ağı üzerindeki trafik sıkışıklıklarının ve olaylarının gerçek görünümünü sağlar.

![Azure Konum Tabanlı Hizmetler Trafik](media/about-location-based-services/Introduction_Traffic.png)

Azure Konum Tabanlı Hizmetler hareketlilik için oluşturulmuştur ve programlama modeli belirsiz olduğundan ve REST API'leri üzerinden JSON çıkışını desteklediğinden platformlar arası uygulamalarda kullanılabilir. Azure LBS ayrıca hem web hem de mobil platform uygulamalarının hızlı ve kolay bir şekilde geliştirilmesi için basit bir programlama modeliyle kullanışlı bir JavaScript Harita Denetimi sunar. 

Azure Konum Tabanlı Hizmetler anahtar tabanlı kimlik doğrulaması düzeni kullandığı için hizmetlere erişmek için [Azure portalına](http://portal.azure.com) gidip Azure Konum Tabanlı Hizmetler hesabı oluşturmak gerekir. Hesabınızda önceden oluşturulmuş iki anahtar mevcuttur. Bu anahtarlardan birini Azure Konum Tabanlı Hizmetler hizmeti isteklerinde kullanarak uygulamalarınıza doğrudan konum özelliklerini tümleştirebilirsiniz.

[Azure Konum Tabanlı Hizmetler hesabınızı](http://aka.ms/azurelbsportal) hemen oluşturun!

## <a name="next-steps"></a>Sonraki adımlar

Azure Konum Tabanlı Hizmetler (önizleme) genel bakış bölümünü tamamladınız. Bir sonraki adımda Konum Tabanlı Hizmetleri kullanan örnek uygulamayı inceleyin ve web uygulamanızda uçtan uca senaryo oluşturun.

> [!div class="nextstepaction"]
> [Azure Konum Tabanlı Hizmetler (önizleme) kullanarak bir demo etkileşimli harita araması başlatma](quick-demo-map-app.md)
> [Azure Konum Tabanlı Hizmetler kullanarak yakın ilgi noktalarını arama](tutorial-search-location.md)