---
title: Azure Konum Tabanlı Hizmetler'e Genel Bakış | Microsoft Docs
description: Azure Konum Tabanlı Hizmetler'e (önizleme) giriş
services: location-based-services
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 02/05/2018
ms.topic: overview
ms.service: location-based-services
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 91df49aa3f522ea765698a1d64ed2686606f71f3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Azure Konum Tabanlı Hizmetler'e (önizleme) giriş
Azure Konum Tabanlı Hizmetler; Haritalar, Arama, Yönlendirme, Trafik ve Saat dilimlerine yönelik hizmet API'lerini içeren bir jeo-uzamsal hizmetler portföyüdür. Azure OneAPI uyumlu hizmet portföyü bilindik geliştirici araçlarını kullanarak hızlıca konum bilgilerini Azure çözümlerinizle tümleştiren çözümler geliştirmenizi ve bu çözümleri ölçeklendirmenizi sağlar. Azure Konum Tabanlı Hizmetler tüm sektörlerden geliştiriciler ve güçlü jeo-uzamsal özellikler sunar. Ayrıca web ve mobil uygulamalara coğrafi bağlam sağlamak için gerekli olan yeni eşleme verileriyle donatılmıştır. Azure Konum Tabanlı Hizmetler, REST API'lerin Azure One API’siyle uyumlu bir kümesidir ve geliştirmeyi çok kolay, esnek ve birden fazla ortam arasında taşınabilir hale getirmek amacıyla web tabanlı bir JavaScript denetimiyle birlikte sunulur. 

Aşağıdaki video Azure Konum Tabanlı Hizmetleri tanıtır.

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

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

## <a name="unsupported-regions"></a>Desteklenmeyen Bölgeler
API’miz şu anda aşağıdaki ülkelerde kullanılamamaktadır. Lütfen geçerli IP adresinizi kontrol edin ve IP adresinizin konumunun aşağıdaki desteklenmeyen ülkelerden birinde olmadığını doğrulayın.

* Arjantin
* Çin
* Hindistan
* Fas
* Pakistan
* Güney Kore

## <a name="relationship-with-bing-maps"></a>Bing Haritalar ile İlişki
Bu belgede açıklanan Azure Konum Tabanlı Hizmetler, Bing Haritalar tarafından sağlanan hizmetlerden farklıdır.  Birçok ortak işlevi olmasına rağmen bu iki hizmet birbirinden farklıdır ve birbiriyle ilişkili değildir.  Bing Haritalar ürün teklifi veya yol haritası, Azure’da ayrıca yönetilecek bu yeni hizmetin kullanılabilir olmasından etkilenmez.

Microsoft'un amacı, geliştirici topluluğuna konum hizmeti teklifleri bakımından seçenek sunmaktır.  Çeşitli kullanım örnekleri ve müşteri durumları için kullanılacak hizmeti belirleme konusunda geliştiriciler için bazı hızlı yönergeler aşağıda verilmiştir.  Bu rehber Genel Önizleme aşamasında olduğundan şu anda yalnızca Azure LBS için geçerlidir ve 2018 yılı içerisinde Genel Kullanıma sunulduktan sonra güncelleştirilecektir.

| Müşteri Ölçütleri | Azure Konum Tabanlı Hizmetler’in kullanılacağı durumlar… | Bing Haritalar’ın kullanılacağı durumlar… |
| ------------- | ------------- | ------------- |
| Geliştirme Ortamı | Diğer Azure hizmetlerini derleme veya kullanma | Üçüncü taraf bulut veya diğer geliştirici ortamlarını kullanma |
| Geliştirme Aşaması  | Azure LBS şu anda Genel Önizleme aşamasında olduğundan, erken aşama testleri ve Kavram Kanıtı geliştirmesi için iyileştirilmiştir | Üretim ortamı için kurumsal düzeyde bir SLA gerekir |
| Fiyatlandırma Seçenekleri | Ön geliştirici fiyatlandırma seçenekleri yeterlidir | Özelleştirilmiş kurumsal düzeyde fiyatlandırma gereklidir |
| Kullanım Örneği Ortamı | Araç içi kullanım gereklidir | Araç içi kullanım gerekli değildir |
| Coğrafi Kapsam | Hindistan, Çin, Japonya ve Güney Kore gerekli değildir | Hindistan, Çin, Japonya ve Güney Kore harika kapsamı gereklidir |
| Eşleme İçeriği | Standart yüzey eşlemeleri yeterlidir | Uydu, havadan ve sokak yan görüntüleri gereklidir |
| Temel Eşleme Kaynağı | TomTom eşleme verileri tercih edilir | HERE eşleme verileri tercih edilir |

[Azure Konum Tabanlı Hizmetler hesabınızı hemen](http://aka.ms/azurelbsportal) oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Konum Tabanlı Hizmetler (önizleme) genel bakış bölümünü tamamladınız. Bir sonraki adımda Konum Tabanlı Hizmetleri kullanan örnek uygulamayı inceleyin ve web uygulamanızda uçtan uca senaryo oluşturun.

> [!div class="nextstepaction"]
> [Azure Konum Tabanlı Hizmetler (önizleme) kullanarak bir demo etkileşimli harita araması başlatma](quick-demo-map-app.md)
> [Azure Konum Tabanlı Hizmetler kullanarak yakın ilgi noktalarını arama](tutorial-search-location.md)
