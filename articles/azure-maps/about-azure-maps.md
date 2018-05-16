---
title: Azure Haritalar’a Genel Bakış | Microsoft Docs
description: Azure Haritalar tanıtımı
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: overview
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: e84580c4023be87ebfc1988c631af0b76e213987
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="an-introduction-to-azure-maps"></a>Azure Haritalar tanıtımı
Azure Haritalar; Haritalar, Arama, Yönlendirme, Trafik ve Saat dilimlerine yönelik hizmet API’lerini içeren bir jeo-uzamsal hizmetler portföyüdür. Azure OneAPI uyumlu hizmet portföyü bilindik araçları kullanarak hızlıca konum bilgilerini Azure çözümlerinizle tümleştiren çözümler geliştirmenizi ve bu çözümleri ölçeklendirmenizi sağlar. Azure Haritalar, tüm sektörlerdeki geliştiricilere, web ve mobil uygulamalara yönelik coğrafi bağlam sağlamak amacıyla yeni eşleme verileriyle donatılmış güçlü jeo-uzamsal özellikler sunar. Azure Haritalar, gelişimi kolay, esnek ve birden fazla ortam arasında taşınabilir hale getirmek amacıyla web tabanlı bir JavaScript denetimiyle birlikte sunulan REST API kümesidir. 

Aşağıdaki videoda Azure Haritalar tanıtılmaktadır:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Azure Haritalar, coğrafi bağlama ihtiyaç duyan Azure uygulamalarını güçlendiren beş temel hizmetten oluşur. Hizmetlerin her biri ayrıntılı şekilde açıklanmıştır.

**İşleme hizmeti**, geliştiricilerin harita verileri kullanan web ve mobil platform uygulamaları oluşturması amacıyla tasarlanmıştır. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

**Yönlendirme hizmeti**, sağlam gerçek dünya altyapı geometrisi hesaplamaları ve çoklu taşıma modu yol tarifleri içerir. Hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi çeşitli seyahat modları arasında yol tariflerini hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma gibi girdileri de dikkate alabilir.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

**Arama hizmeti**, geliştiricilerin ad veya kategori veya diğer coğrafi bilgileri kullanarak adres, yer veya işletme aramasını sağlayacak şekilde tasarlanmıştır. Arama Hizmeti adresleri ve caddeleri enlem/boylam değerine göre [tersine coğrafi koda dönüştürebilir](https://en.wikipedia.org/wiki/Reverse_geocoding). 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

**Saat Dilimi hizmeti**, güncel, geçmiş ve gelecek saat dilimi bilgilerini enlem-boylam çifti veya [IANA ID](http://www.iana.org/) kullanarak sorgulamanızı sağlar. Saat Dilimi Hizmeti ayrıca Microsoft Windows saat dilimi kimliklerini IANA saat dilimlerine dönüştürmenizi, UTC ile saat dilimi farkını öğrenmenizi ve belirli bir saat dilimindeki güncel saati almanızı sağlar. Saat Dilimi Hizmetine gönderilen bir sorguya verilen tipik JSON yanıtı aşağıdaki örnek gibi olacaktır:

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

**Trafik hizmeti**, geliştiricilerin trafik verilerine ihtiyaç duyan web ve mobil platform uygulamaları geliştirmeleri için tasarlanmış bir web hizmeti paketidir. Hizmet, iki tür veri sağlar:
* Trafik akışı: Ağ üzerindeki tüm ana yollar için gerçek zamanlı hız ve seyahat süresi bilgileri. 
* Trafik olayları: Yol ağı üzerindeki trafik sıkışıklıklarının ve olaylarının doğru bir görünümü.

![Azure Haritalar Trafiği](media/about-azure-maps/Introduction_Traffic.png)

Azure Haritalar, hareketlilik için oluşturulmuştur ve programlama modeli belirsiz olduğundan ve REST API’leri üzerinden JSON çıkışını desteklediğinden platformlar arası uygulamalarda kullanılabilir. Azure Haritalar ayrıca hem web hem de mobil platform uygulamalarının hızlı ve kolay bir şekilde geliştirilmesi için basit bir programlama modeliyle kullanışlı bir JavaScript harita denetimi sunar. 

Azure Haritalar, anahtar tabanlı kimlik doğrulaması düzeni kullandığından, hizmetlere erişmek için [Azure portalına](http://portal.azure.com) gidip Azure Haritalar hesabı oluşturulması gerekir. Hesabınızda önceden oluşturulmuş iki anahtar mevcuttur. Bu anahtarlardan birini Azure Haritalar hizmetine yapılan isteklerde kullanarak bu konum özelliklerini doğrudan uygulamalarınızla tümleştirebilirsiniz.

## <a name="unsupported-regions"></a>Desteklenmeyen bölgeler
Azure Haritalar API’si şu anda bazı ülkelerde kullanılamamaktadır. Geçerli IP adresinizi kontrol edin ve IP adresinizin konumunun aşağıdaki desteklenmeyen ülkelerden birinde olmadığını doğrulayın:

* Arjantin
* Çin
* Hindistan
* Fas
* Pakistan
* Güney Kore

## <a name="relationship-with-bing-maps"></a>Bing Haritalar ile İlişki
Bu belgede açıklanan haritalar, Bing Haritalar tarafından sağlanan hizmetlerden farklıdır. Birçok ortak işlevi olmasına rağmen bu iki hizmet birbirinden farklıdır ve birbiriyle ilişkili değildir. Bu Azure hizmeti, Bing Haritalar ürün teklifini veya yol haritasını etkilemez.

Microsoft'un amacı, geliştirici topluluğuna konum hizmeti teklifleri bakımından seçenek sunmaktır. Aşağıdaki tabloda, hangi hizmetin kullanılacağına karar veren geliştiriciler için yönergeler yer alır: 

| Senaryo | Azure Haritalar’ın kullanılacağı durumlar… | Bing Haritalar’ın kullanılacağı durumlar… |
| ------------- | ------------- | ------------- |
| Geliştirme ortamı | Diğer Azure hizmetleri üzerinde derleme veya diğer Azure hizmetleriyle koordinasyon sağlama | Üçüncü taraf bulut veya diğer geliştirici ortamını kullanma |
| Geliştirme aşaması  | Azure Haritalar şu anda Genel Önizleme aşamasında olduğundan, erken aşama testleri ve Kavram Kanıtı geliştirmesi için iyileştirilmiştir | Üretim ortamı için kurumsal düzeyde bir SLA gerekir |
| Fiyatlandırma seçenekleri | Ön geliştirici fiyatlandırma seçenekleri yeterlidir | Özelleştirilmiş kurumsal düzeyde fiyatlandırma gereklidir |
| Kullanım örneği ortamı | Araç içi kullanım gereklidir | Araç içi kullanım gerekli değildir |
| Coğrafi kapsam | Hindistan, Çin, Japonya ve Güney Kore gerekli değildir | Hindistan, Çin, Japonya ve Güney Kore harita kapsamı gereklidir |
| Eşleme içeriği | Standart yüzey eşlemeleri yeterlidir | Uydu, havadan ve sokak yan görüntüleri gereklidir |
| Temel eşleme kaynağı | TomTom eşleme verileri tercih edilir | HERE eşleme verileri tercih edilir |

[Hemen Azure Haritalar hesabına](http://aka.ms/azurelbsportal) kaydolun.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Haritalar’ın genel bakış bilgilerine sahipsiniz. Sonraki adım olarak, hizmeti gösteren bir örnek uygulama denenecektir.

> [!div class="nextstepaction"]
> [Bir demo etkileşimli arama haritası başlatma](quick-demo-map-app.md)

