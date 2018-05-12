---
title: Kimlik doğrulama, istekleri ve yanıtları
description: Anahtar kasası kullanmak için AD kimlik doğrulaması
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
ms.openlocfilehash: 94080fb124478a4b8e196e341c335ca32321ecdf
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="authentication-requests-and-responses"></a>Kimlik doğrulama, istekleri ve yanıtları

Azure anahtar kasası biçimlendirilmiş JSON isteklerini ve yanıtlarını destekler. Azure anahtar kasası isteklerine geçerli Azure anahtar kasası URL'si kullanarak bir HTTPS bazı URL parametrelerle ve JSON istek ve yanıt gövdesi kodlanmış yönlendirilir.

Bu konu Azure anahtar kasası hizmeti için özellikleri kapsar. Kimlik doğrulama/yetkilendirme ve bir erişim belirteci almak üzere nasıl dahil olmak üzere, Azure REST arabirimleri kullanma hakkında genel bilgi için bkz: [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/).

## <a name="request-url"></a>İstek URL'si  
 Anahtar yönetim işlemlerini HTTP DELETE, GET, düzeltme eki, PUT ve HTTP POST ve HTTP POST varolan anahtar nesneleri karşı şifreleme işlemlerini kullanın. Belirli HTTP fiillerini desteklemiyor istemcileri de hedeflenen fiili belirtmek için X HTTP istek üstbilgisi kullanarak HTTP POST kullanabilirsiniz; Normalde bir gövde gerektirmeyen istekleri yerine DELETE POST kullanırken, örneğin HTTP POST kullanırken, boş bir gövde içermesi gerekir.  

 Azure anahtar kasası nesneleri ile çalışmak için örnek URL'leri aşağıda verilmiştir:  

-   Bir anahtar kasası kullanımda TESTKEY adlı bir anahtar oluşturmak için- `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

-   Bir anahtar kasası kullanımı IMPORTEDKEY adlı bir anahtar almak için- `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

-   Bir anahtar kasası kullanımda ETTİYSENİZ adlı bir gizli anahtarı almak için- `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

-   Bir Özet İMZALAMAK için bir anahtar kullanarak TESTKEY bir anahtar kasası kullanımda olarak adlandırılan- `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

 Bir anahtar kasası isteğine yetki her zaman gibidir,  `https://{keyvault-name}.vault.azure.net/`  

 Anahtarları her zaman /keys yolu altında depolanır, gizli her zaman /secrets yolu altında depolanır.  

## <a name="api-version"></a>API Sürümü  
 Bu istemciler için tüm özellikleri kullanılabilse Azure anahtar kasası hizmeti alt düzey istemciler, uyum sağlamak için iletişim kuralı sürüm destekler. İstemcileri kullanmalıdır `api-version` sorgu dizesi parametresi varsayılan olarak, destekledikleri protokol sürümünü belirtin.  

 Azure anahtar kasası protokol sürümleri {YYYY} kullanarak düzeni numaralandırma tarih izleyin. {MM}. {GG} biçimi.  

## <a name="request-body"></a>İstek Gövdesi  
 HTTP belirtimine göre GET işlemleri bir istek gövdesi olmaması gerekir ve bir istek gövdesi POST ve PUT işlemlerine olması gerekir. SİLME işlemleri gövdesinde HTTP isteğe bağlıdır.  

 İşlemi açıklamasında aksi belirtilmedikçe, istek gövdesi içerik türü uygulama/json olmalıdır ve içerik türü için serileştirilmiş bir JSON nesnesi uyumluluğunu içermesi gerekir.  

 İşlemi açıklamasında aksi belirtilmedikçe, kabul etme isteği üstbilgisi uygulama/json ortam türünü içermesi gerekir.  

## <a name="response-body"></a>Yanıt Gövdesi  
 İşlemi açıklamasında aksi belirtilmedikçe, yanıt gövdesi içerik türünü başarılı ve başarısız işlemler uygulama/json olacaktır ve ayrıntılı hata bilgileri içerir.  

## <a name="using-http-post"></a>HTTP POST kullanılmış  
 Bazı istemciler düzeltme eki veya silme gibi belirli HTTP fiillerini kullanan mümkün olmayabilir. İstemci ayrıca özel özgün HTTP fiili "X-HTTP-METHOD" üstbilgiye ekler sağlanan azure anahtar kasası HTTP POST alternatif olarak bu istemciler için destekler. HTTP POST için destek, bu belgede tanımlanan API her biri için Not edilir.  

## <a name="error-responses"></a>Hata yanıtları  
 Hata işleme HTTP durum kodları kullanır. Tipik sonuçlar şu şekildedir:  

-   2xx – başarı: normal işlem için kullanılır. Yanıt gövdesi beklenen sonucu içerir  

-   3xx – yeniden yönlendirme: 304 "değiştirilmeyen" şartlı alma karşılamak için döndürülebilir. Diğer 3xx kodları gelecekte DNS ve yol değişiklikleri göstermek için kullanılabilir.  

-   4xx – istemci hatası: hatalı istekler, eksik anahtar, sözdizimi hatası, geçersiz parametreler, kimlik doğrulama hataları için kullanıldığında, vb. Yanıt gövdesi ayrıntılı hata açıklama içerir.  

-   5XX – sunucu hatası: İç sunucu hataları için kullanılır. Yanıt gövdesi özetlenen hata bilgilerini içerir.  

 Sistem, bir proxy veya güvenlik duvarı arkasında çalışmak üzere tasarlanmıştır. Bu nedenle, bir istemci, diğer hata kodları alabilirsiniz.  

 Bir sorun ortaya çıktığında azure anahtar kasası yanıt gövdesinde hata bilgilerini de döndürür. Yanıt gövdesi biçimlendirilmiş JSON ve biçimdedir:  

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
 Azure anahtar kasası gerekir. tüm istekler kimliği. Azure anahtar kasası OAuth2 kullanarak alınabilir Azure Active Directory'ye erişim belirteçleri destekler [[RFC6749](http://tools.ietf.org/html/rfc6749)]. 
 
 Uygulamanızı kaydetme ve Azure anahtar kasası kullanmak için kimlik doğrulaması hakkında daha fazla bilgi için bkz: [istemci uygulamanızı Azure AD'ye kaydolacak](https://docs.microsoft.com/rest/api/index#register-your-client-application-with-azure-ad).
 
 Erişim belirteçleri HTTP Authorization Üstbilgisi kullanarak hizmete gönderilen gerekir:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Bir erişim belirteci sağlanmadı veya bir belirteç hizmeti tarafından kabul edilmedi bir HTTP 401 hata istemciye döndürülen ve WWW-Authenticate üstbilgisi örneğin içerir:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 WWW-Authenticate üstbilgisi Parametreler şunlardır:  

-   Yetkilendirme: isteği için bir erişim belirteci almak için kullanılabilir OAuth2 yetkilendirme hizmeti adresidir.  

-   Kaynak: Yetkilendirme isteğinde kullanılacak kaynağın adı.  

## <a name="see-also"></a>Ayrıca Bkz.  
 [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
