---
title: CORS'yi Azure CDN kullanarak | Microsoft Docs
description: Azure içerik teslim ağı (CDN) için çıkış noktaları arası kaynak paylaşımı (CORS) ile kullanmayı öğrenin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f9429e88525e27c0b6bad29d1927d53d05dfbcc8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765373"
---
# <a name="using-azure-cdn-with-cors"></a>CORS'yi Azure CDN kullanma
## <a name="what-is-cors"></a>CORS nedir?
CORS (arası kaynak kaynak paylaşımı) başka bir etki alanındaki kaynaklara erişmek bir etki alanında çalışan bir web uygulaması sağlayan bir HTTP özelliğidir. Siteler arası komut dosyası saldırıları olasılığını azaltmak için tüm modern web tarayıcıları olarak bilinen bir güvenlik kısıtlama uygulamak [kaynak aynı ilke](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Bu arama API'ları farklı bir etki alanındaki bir web sayfasından önler.  CORS içinde başka bir kaynak API'leri çağırmak bir kaynak (kaynak etki alanı) izin vermek için güvenli bir yol sağlar.

## <a name="how-it-works"></a>Nasıl çalışır?
CORS isteklerini, iki tür vardır *basit istekleri* ve *karmaşık istekleri.*

### <a name="for-simple-requests"></a>Basit istekleri için:

1. CORS istekle ek bir tarayıcı gönderir **kaynak** HTTP isteği üstbilgisi. Bu üst bileşimi tanımlanır ana sayfa hizmet başlangıç değeri *protokolü,* *etki alanı,* ve *bağlantı noktası.*  Sayfadan zaman https://www.contoso.com fabrikam.com kaynağı, aşağıdaki istek üstbilgisi kullanıcının verileri erişmeye çalışır fabrikam.com olarak gönderilen:

   `Origin: https://www.contoso.com`

2. Sunucusu aşağıdakilerden herhangi biri ile yanıt verebilir:

   * Bir **Access-Control-Allow-Origin** hangi kaynak site izin verilen gösteren yanıt üstbilgisi. Örneğin:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Sunucu çıkış noktaları arası istek kaynak üst bilgisini denetledikten sonra izin vermiyorsa 403 gibi bir HTTP hata kodu

   * Bir **Access-Control-Allow-Origin** tüm kaynaklara izin veren bir joker karakter üstbilgiyle:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Karmaşık istekleri için:

Karmaşık bir CORS istek göndermek için tarayıcı burada gereken istektir bir *denetim öncesi isteği* (diğer bir deyişle, ön bir araştırma) fiili CORS isteğini göndermeden önce. Denetim öncesi isteği özgün CORS devam edebilirsiniz ve olduğundan isteği sunucu izni soran bir `OPTIONS` aynı URL'ye isteği.

> [!TIP]
> CORS akışları ve ortak Tuzaklar hakkında daha fazla ayrıntı için görüntülemek [REST API'leri için CORS kılavuza](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Joker karakter ya da tek kaynak senaryoları
CORS'yi Azure CDN üzerinde ek bir yapılandırma olmadan otomatik olarak çalışır, **Access-Control-Allow-Origin** üstbilgi joker karakter (*) veya tek bir kaynak olarak ayarlanmış.  CDN ilk yanıtı önbelleğe alır ve sonraki istekleri aynı başlığını kullanır.

İstek zaten CDN kaynağınıza üzerinde ayarlanan CORS önce yapıldı, içerikle yeniden yüklemek için uç nokta içeriğinizi içeriği temizlenecek gerekir **Access-Control-Allow-Origin** üstbilgi.

## <a name="multiple-origin-scenarios"></a>Birden çok kaynak senaryoları
Belirli bir listesi için CORS izin verilecek çıkış noktaları yer izin vermeniz gerekiyorsa, biraz daha karmaşık işlerinizi. CDN önbelleğe sorun oluşur **Access-Control-Allow-Origin** ilk CORS çıkış üstbilgisi.  Farklı bir CORS kaynaktan bir sonraki istekte bulunduğunda CDN önbelleğe alınan hizmet **Access-Control-Allow-Origin** eşleşmeyecektir üstbilgi.  Bu sorunu gidermek için birkaç yolu vardır.

### <a name="azure-cdn-premium-from-verizon"></a>Verizon’dan Azure CDN Premium
Bu ayarı etkinleştirmek için en iyi yolu kullanmaktır **verizon'dan Azure CDN Premium**, gelişmiş işlevselliği, bazı kullanıma sunar. 

Gerekir [bir kural oluşturmak](cdn-rules-engine.md) denetlemek için **kaynak** isteği üstbilgisi.  Geçerli bir kaynak varsa, kural ayarlar **Access-Control-Allow-Origin** üstbilgi isteğinde sağlanan kaynağına sahip.  Kaynağı olarak belirtilmişse **kaynak** üstbilgi izin verilmez, kuralınız atlayın **Access-Control-Allow-Origin** üst bilgisi isteği reddetmek için tarayıcıyı neden olur. 

Bu kurallar altyapısı ile yapmak için iki yolu vardır. Her iki durumda da **Access-Control-Allow-Origin** dosyanın kaynak sunucu başlığından göz ardı edilir ve CDN'ın kurallar altyapısı tamamen izin verilen CORS çıkış noktası yönetir.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Tüm geçerli kaynaklara sahip bir normal ifade
Bu durumda, tüm izin vermek istediğiniz kaynakları içerir normal bir ifade oluşturmanız: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Verizon'dan Azure CDN Premium** kullanan [Perl uyumlu normal ifadeler](http://pcre.org/) normal ifadeler için kendi altyapısı olarak bulunabilir.  Gibi bir araç kullanabilirsiniz [normal ifadeler 101](https://regex101.com/) normal ifade doğrulanacak.  "/" Karakterini normal ifadelerde geçerli olduğundan ve kaçış gerekmez, ancak bu karakteri kaçış en iyi yöntem olarak kabul edilir ve bazı regex doğrulayıcıları tarafından beklenen olduğunu unutmayın.
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

### <a name="azure-cdn-standard-profiles"></a>Azure CDN Standart profiller
Azure CDN standart profilleri (**Azure CDN standart Microsoft**, **akamai'den Azure CDN standart**, ve **verizon'dan Azure CDN standart**), yalnızca mekanizması joker karakter kaynak kullanımını kullanmaktır olmadan için birden çok kaynaklara izin [sorgu dizesi önbelleğe alma](cdn-query-string.md). CDN uç noktası için sorgu dizesi ayarını etkinleştirin ve izin verilen her etki alanı gelen istekleri için bir benzersiz sorgu dizesini kullanın. Bunun yapılması her benzersiz sorgu dizesi için ayrı bir nesne önbelleği CDN neden olur. Bu yaklaşım ancak ideal, değil gibi birden çok kopyası üzerinde CDN önbelleğe aynı dosya sonuçlanır.  

