---
title: API etkenleri | Microsoft Docs
description: Market API'leri kullanırken sürüm oluşturma, hata işleme ve yetkilendirme sorunları.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: c6cfb41cb6254145821ab3fef662e9a5e54f6298
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60625066"
---
<a name="api-considerations"></a>API etkenleri
=================

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
