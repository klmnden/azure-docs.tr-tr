---
title: Azure API Management ilkeleri | Microsoft Docs
description: Oluşturun, düzenleyin ve API Yönetimi'nde ilkelerini yapılandırma hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: 99f756b5415811b3d4c2ee0167f98b31c905df1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657698"
---
# <a name="policies-in-azure-api-management"></a>Azure API Management ilkeleri

Azure API Management (APIM), ilkeler yayımcının API configuration aracılığıyla davranışını değiştirmek sistemin güçlü özellikleridir. İstekte sırayla yürütülen deyimlerin bir koleksiyon veya bir API yanıtı ilkelerdir. Sık kullanılan deyimler, biçim dönüştürme, XML'den json'a içerir ve bir geliştiriciden gelen çağrıların miktarını sınırlamak için hız sınırı çağırın. Kullanıma hazır çok daha fazla ilke kullanılabilir.

İlkeler, yönetilen API API tüketici arasında yer alan ağ geçidi içinde uygulanır. Ağ geçidi, tüm istekleri alan ve bunları temel alınan API için değiştirilmeden genellikle iletir. Ancak bir ilke gelen talep ve giden yanıt için değişiklikleri uygulayabilirsiniz.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. Gibi bazı ilkeler [denetim akışı] [ Control flow] ve [değişken Ayarla] [ Set variable] ilkeler ilke ifadelerini temel alarak. Daha fazla bilgi için [Gelişmiş İlkeler] [ Advanced policies] ve [ilke ifadeleri][Policy expressions].

## <a name="sections"> </a>İlke Yapılandırması anlama

İlke tanımı, gelen ve giden ifadeler açıklanmaktadır basit bir XML dosyasıdır. XML tanım penceresinden doğrudan düzenleyebilirsiniz. Deyimlerin listesini sağa sağlanır ve ifadeleri geçerli kapsam için geçerli etkin ve vurgulanır.

Etkin bir deyim tıklayarak uygun XML tanımı görünümünde imlecin bulunduğu konumda ekler. 

> [!NOTE]
> Eklemek istediğiniz ilke etkin değilse, bu ilkeye doğru kapsamında olduğundan emin olun. Her ilke bildirimi, belirli kapsamları ve ilke bölümler kullanılmak üzere tasarlanmıştır. İlke bölüm ve bir ilke için kapsamları gözden geçirmek için kontrol **kullanım** bölümü söz konusu ilkenin [ilke başvurusu][Policy Reference].
> 
> 

Yapılandırma bölünmüştür `inbound`, `backend`, `outbound`, ve `on-error`. Belirtilen ilke ifadelerini dizi olan bir istek ve yanıt için sırayla yürütür.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Bir isteğin işlenmesi sırasında bir hata varsa, tüm kalan adımları `inbound`, `backend`, veya `outbound` bölümleri atlanır ve yürütme atlar ifadeler için `on-error` bölümü. İlke deyimlerinde yerleştirerek `on-error` gözden geçirebileceğiniz hata kullanarak bölümü `context.LastError` özelliği inceleyin ve hata yanıtı kullanarak özelleştirme `set-body` İlkesi ve bir hata oluşması durumunda ne olacağını yapılandırabilirsiniz. Hata kodları ve ilke bildirimlerine işlenmesi sırasında oluşabilecek hatalar için yerleşik adımlar vardır. Daha fazla bilgi için [hata işleme API Management ilkeleri](/azure/api-management/api-management-error-handling-policies).

## <a name="scopes"> </a>İlkeleri yapılandırma

İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [ayarlayın veya ilkeleri düzenleme](set-edit-policies.md).

## <a name="policy-reference"></a>İlke başvurusu

Bkz: [ilke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için.

## <a name="policy-samples"></a>İlke örnekleri

Bkz: [ilkesi örnekleri](policy-samples.md) daha fazla kod örnekleri için.

## <a name="examples"></a>Örnekler

### <a name="apply-policies-specified-at-different-scopes"></a>Farklı kapsamların belirtilen ilkelerini uygula

Genel düzeyinde ve bir API için yapılandırılmış bir ilke bir ilke varsa, her iki ilkeyi API'nin kullanıldığında uygulanır. API yönetimi, belirlenimci birleşik ilke bildirimlerine base öğesi aracılığıyla sıralama için sağlar. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

Yukarıdaki örnekte ilke tanımında `cross-domain` gerekir, sırayla daha yüksek herhangi ilkeleri arkasından önce deyimi yürütün `find-and-replace` ilkesi. 

### <a name="restrict-incoming-requests"></a>Gelen istekleri kısıtlayın

Gelen istekler için belirtilen IP adreslerini kısıtlamak için yeni bir bildirimi eklemek için içeriği yalnızca içinde imleci `inbound` XML öğesi tıklatıp **kısıtlama arayan IP'ler** deyimi.

![Kısıtlama ilkeleri][policies-restrict]

Bu için XML kod parçacığı ekler `inbound` deyim yapılandırma hakkında rehberlik sağlayan bir öğe.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Gelen istekleri sınırlandırmak ve kabul etmek için yalnızca bu IP adresini 1.2.3.4 ve XML gibi değiştirin:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
