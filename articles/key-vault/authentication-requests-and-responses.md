---
title: Kimlik doğrulaması, istekler ve yanıtlar
description: AD için Key Vault'u kullanarak kimlik doğrulaması
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 4c321939-8a5b-42ca-83c4-2f5f647ca13e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: caa2d74ecafe0b0e2508bd97eb4dc21a18e58f51
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626429"
---
# <a name="authentication-requests-and-responses"></a>Kimlik doğrulaması, istekler ve yanıtlar

Azure Key Vault, JSON biçimli isteklerini ve yanıtlarını destekler. Azure Key vault'a istekleri bir geçerli kullanarak Azure Key Vault URL'si için bazı URL parametreleri ile HTTPS ve JSON istek ve yanıt gövdeleri kodlanmış yönlendirilir.

Bu konu, Azure Key Vault hizmeti için özellikleri kapsar. Azure REST arabirimleri, kimlik doğrulama/yetkilendirme ve bir erişim belirteci almak nasıl dahil olmak üzere kullanma hakkında genel bilgi için bkz. [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/azure).

## <a name="request-url"></a>İstek URL'si  
 Anahtar yönetimi işlemlerini HTTP DELETE, GET, düzeltme eki, PUT ve HTTP POST ve HTTP POST mevcut anahtar nesnelere karşı şifreleme işlemleri kullanın. Belirli HTTP fiillerine destekleyemiyorsa istemciler de hedeflenen fiili belirtmek için X-HTTP-istek üstbilgisi kullanarak HTTP POST kullanabilirsiniz; Normalde bir gövde gerektirmeyen istekleri, HTTP POST, POST, DELETE yerine kullanırken örneğin kullanırken boş gövdesi içermelidir.  

 Azure anahtar Kasası'nda nesnelerle çalışmaya örnek URL'leri aşağıda verilmiştir:  

-   Bir Key Vault kullanımda TESTKEY adlı bir anahtar oluşturmak için- `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

-   Bir Key Vault kullanımı IMPORTEDKEY adlı bir anahtar almak için- `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

-   Bir Key Vault kullanımda ETTİYSENİZ adlı bir gizli diziyi almak için- `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

-   Bir anahtar kullanarak bir Özet İMZALAMAK için TESTKEY bir Key Vault kullanımda olarak çağrıldı- `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

 Yetkili bir Key Vault için bir istek için her zaman gibidir,  `https://{keyvault-name}.vault.azure.net/`  

 Anahtarlar, her zaman /keys yolun altında depolanır, gizli dizileri her zaman /secrets yolun altında depolanır.  

## <a name="api-version"></a>API Sürümü  
 Bu istemciler için tüm özellikleri kullanılabilse Protokolü sürüm oluşturma, alt düzey istemciler, uyum sağlamak için Azure Key Vault hizmetine destekler. İstemciler kullanmalıdır `api-version` sorgu dizesi parametresi için varsayılan olarak destekledikleri protokol sürümünü belirtin.  

 Azure Key Vault protokol sürümleri numaralandırma şemasını {YYYY} kullanarak bir tarih izleyin. {MM}. {DD} biçimi.  

## <a name="request-body"></a>İstek Gövdesi  
 HTTP belirtimine göre GET işlemleri istek gövdesi olmaması gerekir ve POST ve PUT işlemleri istek gövdesi olmalıdır. SİLME işlemleri gövdesinde HTTP isteğe bağlıdır.  

 İşlem açıklamasında aksi belirtilmediği sürece, istek gövdesi içerik türü application/json olmalıdır ve serileştirilmiş bir JSON nesnesi uyumlu içerik türü içermelidir.  

 İşlemi açıklamasında aksi belirtilmediği sürece, uygulama/json medya türü Accept istek üst bilgisi içermelidir.  

## <a name="response-body"></a>Yanıt Gövdesi  
 İşlem açıklamasında aksi belirtilmediği sürece, başarılı ve başarısız işlemlerin yanıt gövdesi içerik türü application/json olacaktır ve ayrıntılı hata bilgileri içerir.  

## <a name="using-http-post"></a>HTTP POST kullanılmış  
 Bazı istemciler, PATCH veya DELETE gibi belirli HTTP fiillerine kullanmanız mümkün olmayabilir. İstemci özel özgün HTTP fiili "X-HTTP-METHOD" başlığına da içerir. sağlanan azure Key Vault HTTP POST alternatif olarak bu istemciler için destekler. HTTP POST için destek, bu belgede tanımlanan API'sinin her biri için Not edilir.  

## <a name="error-responses"></a>Hata yanıtları  
 Hata işleme HTTP durum kodları kullanır. Tipik sonuçlar şu şekildedir:  

-   2xx – başarılı: normal işlem için kullanılmaz. Yanıt gövdesi beklenen sonuç içerir  

-   3xx – yeniden yönlendirme: 304 "değişiklik" şartlı alma karşılamak için döndürülebilir. Diğer 3xx kodları gelecekte DNS ve yolu değişiklikleri göstermek için kullanılabilir.  

-   4xx – istemci hatası: hatalı istekler, eksik anahtar, sözdizimi hatası, geçersiz parametreler, kimlik doğrulama hataları için kullanıldığında, vs. Yanıt gövdesi ayrıntılı hata açıklamasını içerir.  

-   5XX – sunucu hatası: İç sunucu hataları için kullanılır. Yanıt gövdesi özetlenen hata bilgilerini içerir.  

 Sistem, bir proxy veya güvenlik duvarı çalışacak şekilde tasarlanmıştır. Bu nedenle, bir istemci diğer hata kodları alabilirsiniz.  

 Bir sorun ortaya çıktığında azure Key Vault ayrıca yanıt gövdesi içinde hata bilgilerini döndürür. Yanıt gövdesi JSON ile biçimlendirilmemiş ve biçimi alır:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Kimlik Doğrulaması  
 Tüm istekler için Azure anahtar kasası gerekir. kimlik doğrulaması. Azure Key Vault, oauth2'yi kullanarak elde edilebilir Azure Active Directory erişim belirteçleri destekleyen [[RFC6749](http://tools.ietf.org/html/rfc6749)]. 
 
 Uygulamanızı kaydetmek ve Azure anahtar kasası kullanılacak kimlik doğrulaması hakkında daha fazla bilgi için bkz. [istemci uygulamanızı Azure AD'ye kaydetme](https://docs.microsoft.com/rest/api/azure/index#register-your-client-application-with-azure-ad).
 
 Erişim belirteçleri, HTTP yetkilendirme üst bilgisi kullanarak hizmete gönderilen gerekir:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Bir erişim belirteci sağlanmadı veya bir belirteç hizmeti tarafından kabul edilmiyor bir HTTP 401 hatası istemciye döndürülen ve WWW-Authenticate üstbilgisi, örneğin içerir:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 WWW-Authenticate üstbilgisi Parametreler şunlardır:  

-   Yetkilendirme: istek için bir erişim belirteci almak için kullanılabilir OAuth2 yetkilendirme hizmeti adresi.  

-   Kaynak: Yetkilendirme isteğine kullanmak için bir kaynak adı.  

## <a name="see-also"></a>Ayrıca Bkz.  
 [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
