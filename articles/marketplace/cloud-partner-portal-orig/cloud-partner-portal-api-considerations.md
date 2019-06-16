---
title: API etkenleri | Azure Market
description: Market API'leri kullanırken sürüm oluşturma, hata işleme ve yetkilendirme sorunları.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 6bf27db27daee50f78552344ae1b2b116d48a5c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935570"
---
# <a name="api-considerations"></a>API etkenleri


<a name="api-versioning"></a>API sürümü oluşturma
--------------

Aynı anda kullanılabilir olan birden çok API sürümünü olabilir. İstemciler, istedikleri sağlayarak kullanım çağırmak için hangi sürümün belirtmelisiniz `api-version` kapsamında sorgu dizesi parametresi.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

Bilinmeyen veya geçersiz bir API sürümünü içeren bir istek yanıtı bir HTTP 400 kodudur. Bu hata, yanıt gövdesi içinde bilinen API sürümlerini koleksiyonunu döndürür.

``` json
    {
        "error”: { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Hatalar
------

API, ek bilgiler JSON olarak serileştirilen yanıt hataları karşılık gelen HTTP durum kodları ve isteğe bağlı olarak yanıt verir.
Bir hata, özellikle bir sınıf 400 hatası aldığınızda istek altında yatan sebebi düzeltmeden önce yeniden denemeyin. Örneğin, yukarıdaki örnek yanıtta API Sürüm parametresi isteğini yeniden göndermeden önce düzeltin.

<a name="authorization-header"></a>Yetkilendirme üst bilgisi
--------------------

Tüm API'ler için bu başvurusu, Azure Active Directory'den (Azure AD) alınan taşıyıcı belirteç birlikte yetkilendirme üst bilgisi geçmesi gerekir. Bu üst bilgisi, geçerli bir yanıt almak için gereklidir; yoksa, bir `401 Unauthorized` hata döndürülür. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
