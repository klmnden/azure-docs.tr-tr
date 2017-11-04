---
title: Azure API Management ilkeleri | Microsoft Docs
description: "Oluşturma, düzenleme ve API yönetimi ilkelerini yapılandırma öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ef4cb447430a613dd519d96dd7732a9349ebba39
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="policies-in-azure-api-management"></a>Azure API Management ilkeleri
Azure API Management'te yapılandırma yoluyla API'nin davranışını değiştirmek yayımcının sisteminin güçlü bir özellik ilkelerdir. İstek üzerinde sırayla yürütülen deyimlerin bir koleksiyon veya bir API yanıtını ilkelerdir. Sık kullanılan deyimler, XML'den JSON biçimi dönüştürme içerir ve bir geliştiriciden gelen çağrıların miktarını sınırlamak için hız sınırı çağırın. Kutudan çıktığında çok daha fazla ilke kullanılabilir.

Bkz: [İlkesi başvurusu] [ Policy Reference] ilke deyimleri ve ayarlarının tam listesi için.

İlkeler, yönetilen API API tüketici arasında bulunur geçidi içinde uygulanır. Ağ geçidi, tüm istekleri alır ve bunları temel API değiştirilmemiş genellikle iletir. Ancak bir ilke gelen talep ve giden yanıt için değişiklikleri uygulayabilirsiniz.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. Gibi bazı ilkeler [kontrol akışı] [ Control flow] ve [değişken Ayarla] [ Set variable] ilkeler ilke ifadelerini temel alarak. Daha fazla bilgi için bkz: [ilkeleri Gelişmiş] [ Advanced policies] ve [ilke ifadelerini][Policy expressions].

## <a name="scopes"></a>İlkelerinin nasıl yapılandırılacağını
Genel veya kapsamını ilkeleri yapılandırılabilir bir [ürün][Product], [API] [ API] veya [işlemi] [Operation]. Bir ilke yapılandırmak için İlke Düzenleyicisi'nde yayımcı portalına gidin.

![İlkeleri menüsü][policies-menu]

İlke Düzenleyicisi'ni üç ana bölümden oluşur: ilke kapsamı (üst), burada ilkeleri düzenlenmesi (soldaki) ve deyimleri (sağdaki) listesinde ilke tanımı:

![İlke Düzenleyicisi][policies-editor]

Bir ilke yapılandırmaya başlamak için ilkenin uygulanacağı kapsamı seçin. Aşağıdaki ekran görüntüsünde **Starter** ürün seçilidir. İlke adı yanındaki kare simgesini bir ilke zaten bu düzeyde uygulanan olduğuna dikkat edin.

![Kapsam][policies-scope]

Bir ilke uygulanmış olduğundan, yapılandırma tanımı görünümünde gösterilir.

![Yapılandırma][policies-configure]

İlke salt okunur görüntülenen ilk. Tanımı öğesini düzenlemek için **ilkesini yapılandırma** eylem.

![Düzenle][policies-edit]

İlke tanımı gelen ve giden ifadeler tanımlayan basit bir XML dosyasıdır. XML tanımı penceresinden doğrudan düzenlenebilir. Deyimleri listesini sağa sağlanır ve deyimleri geçerli kapsam için geçerli etkin ve vurgulanmış; tarafından gösterildiği gibi **çağrı hızını sınırla** deyimi yukarıdaki ekran görüntüsü.

Etkin bir deyimi tıklatarak uygun XML tanım görünümü imlecin konumda ekler. 

> [!NOTE]
> Eklemek istediğiniz ilke etkin değilse, bu ilkeye doğru kapsamında olduğundan emin olun. Her ilke bildirimi, bazı kapsamlar ve ilke bölümler kullanılmak üzere tasarlanmıştır. İlke bölüm ve bir ilke kapsamları gözden geçirmek için kontrol **kullanım** bölüm söz konusu ilkenin [İlkesi başvurusu][Policy Reference].
> 
> 

İlke deyimleri tam listesi ve bunların ayarları kullanılabilir olan [İlkesi başvurusu][Policy Reference].

Örneğin, gelen istekleri belirtilen IP adreslerini kısıtlamak için yeni bir sistem eklemek için imleci hemen içeriğini iç koyun `inbound` XML öğesi tıklatıp **sınırla çağıran IP'leri** deyimi.

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

![Kaydet][policies-save]

Tamamlandığında deyimleri ilke için yapılandırma, tıklayın **kaydetmek** ve değişiklikleri API Yönetimi ağ geçidi hemen yayılır.

## <a name="sections"></a>Anlama ilkesi yapılandırma
Bir ilke için bir istek ve yanıt sırayla yürütmek deyimleri dizisidir. Yapılandırma uygun şekilde bölünür `inbound`, `backend`, `outbound`, ve `on-error` bölümler aşağıdaki yapılandırmada gösterildiği gibi.

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

Yapılandırma ilkeleri farklı düzeylerde (Genel, ürün, API ve işlem) belirtilebilir bu yana ilke tanımının deyimleri göre üst ilkesi yürütme sırasını belirlemek bir yol sağlar. 

İlke kapsamları aşağıdaki sırayla değerlendirilir.

1. Genel kapsamlı
2. Ürün kapsamı
3. API kapsamı
4. İşlem kapsamı

Bunların içindeki deyimleri yerleşimini göre değerlendirilir `base` varsa, öğesi. Genel ilke üst öğeye sahip İlkesi ve kullanarak `<base>` öğesi içindeki etkisi yoktur.

Genel düzeyinde ve bir API için yapılandırılmış bir ilke bir ilke varsa, API'nin kullanıldığı her Örneğin, ardından her iki ilke uygulanır. API Management belirleyici temel öğe aracılığıyla birleşik İlkesi deyimlerinin sıralama için sağlar. 

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

İlkeler ilke düzenleyicisinde geçerli kapsamdaki görmek için tıklatın **yeniden hesapla seçili kapsam için etkin ilke**.

## <a name="next-steps"></a>Sonraki adımlar
İlke ifadelerini temel video aşağıdaki çıkışı denetleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
