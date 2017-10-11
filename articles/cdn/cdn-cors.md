---
title: CORS'yi Azure CDN kullanarak | Microsoft Docs
description: "Azure içerik teslim ağı (CDN) için çıkış noktaları arası kaynak paylaşımı (CORS) ile kullanmayı öğrenin."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7070397f6e69b21add75bad8220f0b8ebe36d266
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-cdn-with-cors"></a>CORS'yi Azure CDN kullanma
## <a name="what-is-cors"></a>CORS nedir?
CORS (arası kaynak kaynak paylaşımı) başka bir etki alanındaki kaynaklara erişmek bir etki alanında çalışan bir web uygulaması sağlayan bir HTTP özelliğidir. Siteler arası komut dosyası saldırıları olasılığını azaltmak için tüm modern web tarayıcıları olarak bilinen bir güvenlik kısıtlama uygulamak [kaynak aynı ilke](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Bu arama API'ları farklı bir etki alanındaki bir web sayfasından önler.  CORS içinde başka bir kaynak API'leri çağırmak bir kaynak (kaynak etki alanı) izin vermek için güvenli bir yol sağlar.

## <a name="how-it-works"></a>Nasıl çalışır?
CORS isteklerini, iki tür vardır *basit istekleri* ve *karmaşık istekleri.*

### <a name="for-simple-requests"></a>Basit istekleri için:

1. CORS istekle ek bir tarayıcı gönderir **kaynak** HTTP isteği üstbilgisi. Bu üst bileşimi tanımlanır ana sayfa hizmet başlangıç değeri *protokolü,* *etki alanı,* ve *bağlantı noktası.*  Https://www.contoso.com sayfasından fabrikam.com kaynak kullanıcının verileri erişmeye çalıştığında, aşağıdaki istek üstbilgisi fabrikam.com olarak gönderilir:

   `Origin: https://www.contoso.com`

2. Sunucusu aşağıdakilerden herhangi biri ile yanıt verebilir:

   * Bir **Access-Control-Allow-Origin** hangi kaynak site izin verilen gösteren yanıt üstbilgisi. Örneğin:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Sunucu çıkış noktaları arası istek kaynak üst bilgisini denetledikten sonra izin vermiyorsa 403 gibi bir HTTP hata kodu

   * Bir **Access-Control-Allow-Origin** tüm kaynaklara izin veren bir joker karakter üstbilgiyle:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Karmaşık istekleri için:

Karmaşık bir CORS istek göndermek için tarayıcı burada gereken istektir bir *denetim öncesi isteği* (yani ön bir araştırma) fiili CORS isteğini göndermeden önce. Denetim öncesi isteği özgün CORS devam edebilirsiniz ve olduğundan isteği sunucu izni soran bir `OPTIONS` aynı URL'ye isteği.

> [!TIP]
> CORS akışları ve ortak Tuzaklar hakkında daha fazla ayrıntı için görüntülemek [REST API'leri için CORS kılavuza](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Joker karakter ya da tek kaynak senaryoları
CORS'yi Azure CDN üzerinde ek bir yapılandırma olmadan otomatik olarak çalışır, **Access-Control-Allow-Origin** üstbilgi joker karakter (*) veya tek bir kaynak olarak ayarlanmış.  CDN ilk yanıtı önbelleğe alır ve sonraki istekleri aynı başlığını kullanır.

İstek zaten ile CDN üzerinde ayarlanan CORS önce yapılmışsa içerik ile yeniden yüklemek için uç nokta içeriğinizi içeriği temizlenecek gerekir, kaynak **Access-Control-Allow-Origin** üstbilgi.

## <a name="multiple-origin-scenarios"></a>Birden çok kaynak senaryoları
Belirli bir listesi için CORS izin verilecek çıkış noktaları yer izin vermeniz gerekiyorsa, biraz daha karmaşık işlerinizi. CDN önbelleğe sorun oluşur **Access-Control-Allow-Origin** ilk CORS çıkış üstbilgisi.  Farklı bir CORS kaynaktan bir sonraki istekte bulunduğunda CDN önbelleğe alınan hizmet **Access-Control-Allow-Origin** eşleşmeyecektir üstbilgi.  Bu sorunu gidermek için birkaç yolu vardır.

### <a name="azure-cdn-premium-from-verizon"></a>Verizon’dan Azure CDN Premium
Bu ayarı etkinleştirmek için en iyi yolu kullanmaktır **verizon'dan Azure CDN Premium**, gelişmiş işlevselliği, bazı kullanıma sunar. 

Gerekir [bir kural oluşturmak](cdn-rules-engine.md) denetlemek için **kaynak** isteği üstbilgisi.  Geçerli bir kaynak varsa, kural ayarlar **Access-Control-Allow-Origin** üstbilgi isteğinde sağlanan kaynağına sahip.  Kaynak olarak belirtilmişse **kaynak** üstbilgi izin verilmez, kuralınız atlayın **Access-Control-Allow-Origin** isteğini reddetmek tarayıcı neden olacak üstbilgi. 

Bu kurallar altyapısı ile yapmak için iki yolu vardır.  Her iki durumda da **Access-Control-Allow-Origin** dosyanın kaynak sunucu başlığından tamamen göz ardı, CDN'ın kurallar altyapısı tamamen izin verilen CORS çıkış noktası yönetir.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Tüm geçerli kaynaklara sahip bir normal ifade
Bu durumda, tüm izin vermek istediğiniz kaynakları içerir normal bir ifade oluşturmanız: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Verizon'dan Azure CDN** kullanan [Perl uyumlu normal ifadeler](http://pcre.org/) normal ifadeler için kendi altyapısı olarak bulunabilir.  Gibi bir araç kullanabilirsiniz [normal ifadeler 101](https://regex101.com/) normal ifade doğrulanacak.  "/" Karakterini normal ifadelerde geçerli olduğundan ve kaçış gerekmez, ancak bu karakteri kaçış en iyi yöntem olarak kabul edilir ve bazı regex doğrulayıcıları tarafından beklenen olduğunu unutmayın.
> 
> 

Normal ifadeyle eşleşen, kuralınız değiştirir **Access-Control-Allow-Origin** başlığından (varsa) kaynağa isteği gönderen kaynağına sahip.  Ek CORS üstbilgilerini gibi ekleyebilirsiniz **erişim-denetim-Allow-Methods**.

![Normal ifade ile kurallar örneği](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Her kaynak için istek üstbilgisi kuralı.
Normal ifadeler yerine, bunun yerine kullanarak izin vermek istediğiniz her kaynak için ayrı bir kural oluşturabileceğiniz **istek üstbilgisi joker** [eşleşen koşulu](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Normal ifade yöntemiyle olduğu gibi tek başına kurallar altyapısı CORS üstbilgilerini ayarlar. 

![Normal ifade olmadan kuralları örneği](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> Joker karakter kullanımı yukarıdaki örnekte * hem HTTP hem de HTTPS eşleştirmek için kurallar altyapısı söyler.
> 
> 

### <a name="azure-cdn-standard"></a>Azure CDN standart
Azure CDN standart profilleri, birden çok çıkış joker kaynak kullanımı için izin vermek için tek mekanizması kullanmaktır [sorgu dizesi önbelleğe alma](cdn-query-string.md).  CDN uç noktası için sorgu dizesi ayarı etkinleştirmek ve izin verilen her etki alanı gelen istekleri için bir benzersiz sorgu dizesi kullanmak gerekir. Bunun yapılması her benzersiz sorgu dizesi için ayrı bir nesne önbelleği CDN neden olur. Bu yaklaşım ancak ideal, değil gibi birden çok kopyası üzerinde CDN önbelleğe aynı dosya sonuçlanır.  

