---
title: Azure API Management ilkeleri | Microsoft Docs
description: "Oluşturma, düzenleme ve API yönetimi ilkelerini yapılandırma öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: 315e4bd7372416800373f98ecb5d8b1eb440e134
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="policies-in-azure-api-management"></a>Azure API Management ilkeleri

Azure API Management (APIM) yapılandırma yoluyla API'nin davranışını değiştirmek yayımcının sisteminin güçlü bir özellik ilkelerdir. İstek üzerinde sırayla yürütülen deyimlerin bir koleksiyon veya bir API yanıtını ilkelerdir. Sık kullanılan deyimler, XML'den JSON biçimi dönüştürme içerir ve bir geliştiriciden gelen çağrıların miktarını sınırlamak için hız sınırı çağırın. Kutudan çıktığında çok daha fazla ilke kullanılabilir.

İlkeler, yönetilen API API tüketici arasında bulunur geçidi içinde uygulanır. Ağ geçidi, tüm istekleri alır ve bunları temel API değiştirilmemiş genellikle iletir. Ancak bir ilke gelen talep ve giden yanıt için değişiklikleri uygulayabilirsiniz.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. Gibi bazı ilkeler [kontrol akışı] [ Control flow] ve [değişken Ayarla] [ Set variable] ilkeler ilke ifadelerini temel alarak. Daha fazla bilgi için bkz: [ilkeleri Gelişmiş] [ Advanced policies] ve [ilke ifadelerini][Policy expressions].

## <a name="sections"></a>Anlama ilkesi yapılandırma

İlke tanımı gelen ve giden ifadeler tanımlayan basit bir XML dosyasıdır. XML tanımı penceresinden doğrudan düzenlenebilir. Deyimleri listesini sağa sağlanır ve deyimleri geçerli kapsam için geçerli etkin ve vurgulanır.

Etkin bir deyimi tıklatarak uygun XML tanım görünümü imlecin konumda ekler. 

> [!NOTE]
> Eklemek istediğiniz ilke etkin değilse, bu ilkeye doğru kapsamında olduğundan emin olun. Her ilke bildirimi, bazı kapsamlar ve ilke bölümler kullanılmak üzere tasarlanmıştır. İlke bölüm ve bir ilke kapsamları gözden geçirmek için kontrol **kullanım** bölüm söz konusu ilkenin [İlkesi başvurusu][Policy Reference].
> 
> 

Yapılandırma bölünmüştür `inbound`, `backend`, `outbound`, ve `on-error`. Belirtilen ilke deyimleri dizisidir bir istek ve yanıt için sırayla yürütür.

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

Bir isteğin işlenmesi sırasında bir hata varsa, tüm kalan adımları `inbound`, `backend`, veya `outbound` bölümleri atlanır ve yürütme atlar ifadeler için `on-error` bölümü. İlke deyimlerinde yerleştirerek `on-error` gözden geçirebileceğiniz hata kullanarak bölüm `context.LastError` özelliği inceleyebilir ve hata yanıtı kullanarak özelleştirin `set-body` İlkesi ve bir hata oluşursa ne olacağını yapılandırın. Hata kodları ve ilke deyimleri işleme sırasında oluşabilecek hatalar için yerleşik adımları vardır. Daha fazla bilgi için bkz: [hata API Management ilkeleri işleme](https://msdn.microsoft.com/library/azure/mt629506.aspx).

## <a name="scopes"></a>İlkelerinin nasıl yapılandırılacağını

İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [ayarlayın veya ilkeleri düzenleme](set-edit-policies.md).

## <a name="policy-reference"></a>Grup İlkesi başvurusu

Bkz: [İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için.

## <a name="policy-samples"></a>İlke örnekleri

Bkz: [ilkesi örnekleri](policy-samples.md) daha fazla kod örnekleri için.

## <a name="examples"></a>Örnekler

### <a name="appliy-policies-specified-at-different-scopes"></a>Farklı kapsamların belirtilen Appliy ilkeleri

Genel düzeyinde ve bir API için yapılandırılmış bir ilke bir ilke varsa, her iki ilkeyi API'nin kullanıldığında uygulanır. API Management belirleyici temel öğe aracılığıyla birleşik İlkesi deyimlerinin sıralama için sağlar. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

Yukarıdaki örnek ilke tanımı'ndaki `cross-domain` hangi sırayla, misiniz yüksek ilkeleri tarafından uyulması önce deyimi yürütün `find-and-replace` ilkesi. 

### <a name="restrict-incoming-requests"></a>Gelen istekleri kısıtlayın

Gelen istekleri belirtilen IP adreslerini kısıtlamak için yeni bir sistem eklemek için imleci hemen içeriğini iç koyun `inbound` XML öğesi tıklatıp **sınırla çağıran IP'leri** deyimi.

![Kısıtlama ilkeleri][policies-restrict]

Bu bir XML parçacığını ekler `inbound` öğesi deyim yapılandırma hakkında yönergeler sağlar.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Gelen istekleri sınırlamak ve kabul etmek için yalnızca bir IP adresinden, 1.2.3.4 XML aşağıdaki gibi değiştirin:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
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
