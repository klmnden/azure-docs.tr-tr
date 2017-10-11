---
title: "Azure API Management ilkeleri özelliklerini kullanma"
description: "Azure API Management ilkeleri özelliklerini kullanmayı öğrenin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3b0fe2a300038e13cc488bdb4f50f8be270ea8f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Azure API Management ilkeleri özelliklerini kullanma
API yönetimi ilkelerini yapılandırma yoluyla API'nin davranışını değiştirmek yayımcının sisteminin güçlü bir özellik var. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. İlke deyimleri metin değerleri, ilke ifadelerini ve özellikleri kullanarak oluşturulabilir. 

Her API Management hizmet örneği için hizmet örneği genel anahtar/değer çiftleri özellikleri koleksiyonu vardır. Bu özellikler, sabit dize değerlerini tüm API yapılandırması ve ilkeleri yönetmek için kullanılabilir. Her bir özellik aşağıdaki özniteliklere sahiptir.

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| Ad |Dize |Özelliğin adı. İçeren yalnızca harf, rakam, nokta, tire ve alt çizgi karakterleri. |
| Değer |Dize |Özelliğin değeri. Bunu boş olamaz veya yalnızca boşluktan oluşamaz. |
| Gizli dizi |Boole değeri |Değer bir parolası ve veya şifrelenmelidir olup olmadığını belirler. |
| Etiketler |Dize dizisi |İsteğe bağlı etiketleri, sağlandığında özellik listesini filtrelemek için kullanılabilir. |

Özellikleri yapılandırılır yayımcı portalında **özellikleri** sekmesi. Aşağıdaki örnekte, üç özellikleri yapılandırılır.

![Özellikler][api-management-properties]

Özellik değerlerini değişmez değer dizeleri içerebilir ve [ilke ifadelerini](https://msdn.microsoft.com/library/azure/dn910913.aspx). Aşağıdaki tabloda, önceki üç örnek özelliklerini ve onların öznitelikleri gösterir. Değeri `ExpressionProperty` geçerli tarih ve saati içeren bir dize döndürür bir ilke ifadesidir. Özellik `ContosoHeaderValue` değerini gösterilmesi için gizlilik olarak işaretlenmiş.

| Ad | Değer | Gizli dizi | Etiketler |
| --- | --- | --- | --- |
| ContosoHeader |İzleme kodu |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-use-a-property"></a>Bir özelliği kullanmak için
Bir ilke özelliği kullanmak için özellik adı gibi küme ayraçları çift çifti içinde yerleştirin `{{ContosoHeader}}`, aşağıdaki örnekte gösterildiği gibi.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Bu örnekte, `ContosoHeader` bir üstbilgi adı olarak kullanılan bir `set-header` İlkesi ve `ContosoHeaderValue` Bu üstbilgi değeri olarak kullanılır. Bu ilke bir istek veya yanıt API Yönetimi ağ geçidi olarak sırasında değerlendirildiğinde `{{ContosoHeader}}` ve `{{ContosoHeaderValue}}` ilgili özellik değerlerine ile değiştirilir.

Özellikler tam özniteliği ya da önceki örnekte gösterildiği gibi öğesi değerleri olarak kullanılabilir, ancak bunlar eklenen ya da aşağıdaki örnekte gösterildiği gibi bir metin ifadenin parçası birlikte:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Özellikler de ilke ifadelerini içerebilir. Aşağıdaki örnekte, `ExpressionProperty` kullanılır.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Bu ilke değerlendirildiğinde `{{ExpressionProperty}}` değeriyle değiştirilir: `@(DateTime.Now.ToString())`. Değer bir ilke ifadesi olduğundan ifade değerlendirilir ve ilke yürütülmesinin ile devam eder.

Özelliklere sahip bir ilke kapsamında olan bir işlem çağırarak bu çıkış Geliştirici Portalı'nda test edebilirsiniz. Aşağıdaki örnekte, bir işlem iki önceki örnekle çağrılır `set-header` ilkeleri özelliklere sahip. Yanıt özellikleri ile ilkelerini kullanarak yapılandırılmış iki özel üstbilgi içerdiğine dikkat edin.

![Geliştirici portalı][api-management-send-results]

Bakarsanız [API denetçisi izleme](api-management-howto-api-inspector.md) iki önceki örnek ilkeleriyle özellikleri içeren bir arama iki görebilirsiniz `set-header` ilke ifadesi bulunan bir özellik için ilke ifade değerlendirmesi yanı sıra eklenen özellik değerlerini ilkeleriyle.

![API denetçisi izleme][api-management-api-inspector-trace]

Özellik değerlerini ilke ifadelerini içerebilir olsa da, özellik değerleri diğer özellikleri içeremez unutmayın. Bir özellik referansı içeren metin gibi bir özellik değeri için kullanılıyorsa, `Property value text {{MyProperty}}`, bu özellik başvurusu değiştirilmesi olmaz ve özellik değerinin bir parçası olarak dahil edilir.

## <a name="to-create-a-property"></a>Bir özellik oluşturmak için
Bir özellik oluşturmak için tıklatın **özellik ekleme** üzerinde **özellikleri** sekmesi.

![Özellik ekleme][api-management-properties-add-property-menu]

**Ad** ve **değeri** gerekli değerlerdir. Bu özellik değeri bir gizli anahtarı ise denetleyin **bu gizli olmadığından** onay kutusu. Özelliklerinizi düzenleme ile yardımcı olmak için bir veya daha fazla isteğe bağlı etiketleri girin ve **kaydetmek**.

![Özellik ekleme][api-management-properties-add-property]

Yeni bir özellik kaydedildiğinde **arama özelliği** metin kutusuna yeni özellik adı ile doldurulur ve yeni özellik görüntülenir. Tüm özellikleri görüntülemek için temizleyin **arama özelliği** textbox ve ENTER tuşuna basın.

![Özellikler][api-management-properties-property-saved]

REST API kullanarak bir özelliği oluşturma hakkında daha fazla bilgi için bkz: [REST API kullanarak özellik oluşturma](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Bir özellik düzenlemek için
Bir özellik düzenlemek için tıklatın **Düzenle** düzenlemek için özellik yanında.

![Özelliği Düzenle][api-management-properties-edit]

İstediğiniz değişiklikleri yapın ve tıklatın **kaydetmek**. Özellik adını değiştirirseniz, bu özellik başvuru ilkeleri yeni bir ad kullanmak için otomatik olarak güncelleştirilir.

![Özelliği Düzenle][api-management-properties-edit-property]

REST API kullanarak bir özellik düzenleme hakkında daha fazla bilgi için bkz: [REST API kullanarak bir özelliği Düzenle](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Bir özelliği silmek için
Bir özellik silmek için tıklatın **silmek** silmek için özellik yanında.

![Özellik Sil][api-management-properties-delete]

Tıklatın **Evet, silmeden** onaylamak için.

![Silmeyi onayla][api-management-delete-confirm]

> [!IMPORTANT]
> Özellik hiçbir ilke tarafından başvurulduğunda başarıyla özelliğini kullanan tüm ilkelerden kaldırana kadar silemiyor olacaktır.
> 
> 

REST API kullanarak bir özelliği silmeye hakkında daha fazla bilgi için bkz: [REST API kullanarak bir özelliği silmeye](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Arama ve filtreleme için özellikleri
**Özellikleri** sekmesi, arama ve filtreleme özelliklerinizi yönetmenize yardımcı olmak için özellikleri içerir. Özellik listesi özellik adına göre filtre uygulamak için bir arama terimi girmeniz **arama özelliği** metin kutusu. Tüm özellikleri görüntülemek için temizleyin **arama özelliği** textbox ve ENTER tuşuna basın.

![Arama][api-management-properties-search]

Özellik listesi etiket değerlerine göre filtre uygulamak için bir veya daha fazla etiket içine girin **filtre etiketleriyle** metin kutusu. Tüm özellikleri görüntülemek için temizleyin **filtre etiketleriyle** textbox ve ENTER tuşuna basın.

![Filtre][api-management-properties-filter]

## <a name="next-steps"></a>Sonraki adımlar
* İlkeleri ile çalışma hakkında daha fazla bilgi edinin
  * [API Management ilkeleri](api-management-howto-policies.md)
  * [İlke başvurusu](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [İlke ifadeleri](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Video genel izleyin
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

