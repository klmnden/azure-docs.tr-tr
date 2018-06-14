---
title: Azure API Management ilkeleri özelliklerini kullanma
description: Azure API Management ilkeleri özelliklerini kullanmayı öğrenin.
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
ms.date: 01/25/2018
ms.author: apimpm
ms.openlocfilehash: e0559380f6d686a4e559779c4271ea85106558d6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197121"
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Azure API Management ilkeleri özelliklerini kullanma
API yönetimi ilkelerini yapılandırma yoluyla API'nin davranışını değiştirmek Azure portal izin sistemi güçlü bir özellik var. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. İlke deyimleri metin değerleri, ilke ifadelerini ve özellikleri kullanarak oluşturulabilir. 

Her API Management hizmet örneği için hizmet örneği genel anahtar/değer çiftleri özellikleri koleksiyonu vardır. Bu özellikler, sabit dize değerlerini tüm API yapılandırması ve ilkeleri yönetmek için kullanılabilir. Her bir özellik aşağıdaki özniteliklere sahip olabilir:

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| Görünen ad |dize |İlkelerdeki özelliklere başvurmak için kullanılan alfasayısal dize. |
| Değer |dize |Özelliğin değeri. Bunu boş olamaz veya yalnızca boşluktan oluşamaz. |
|Gizli dizi|boole|Değer bir parolası ve veya şifrelenmelidir olup olmadığını belirler.|
| Etiketler |Dize dizisi |İsteğe bağlı etiketleri, sağlandığında özellik listesini filtrelemek için kullanılabilir. |

![Adlandırılmış değerler](./media/api-management-howto-properties/named-values.png)

Özellik değerlerini değişmez değer dizeleri içerebilir ve [ilke ifadelerini](https://msdn.microsoft.com/library/azure/dn910913.aspx). Örneğin, değeri `ExpressionProperty` geçerli tarih ve saati içeren bir dize döndürür bir ilke ifadesidir. Özellik `ContosoHeaderValue` değerini gösterilmesi için gizlilik olarak işaretlenmiş.

| Ad | Değer | Gizli dizi | Etiketler |
| --- | --- | --- | --- |
| ContosoHeader |TrackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-add-and-edit-a-property"></a>Ekleme ve bir özellik düzenlemek için

![Özellik ekleme](./media/api-management-howto-properties/add-property.png)

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Seçin **değerleri adlı**.
3. Tuşuna **+ Ekle**.

  Ad ve değer gerekli değerlerdir. Bu özellik değeri bir gizli ise, bu gizli bir onay kutusudur denetleyin. Birini girin veya özelliklerinizi düzenleme ile yardımcı olmak ve daha fazla isteğe bağlı etiket kaydedin.
4. **Oluştur**’a tıklayın.

Özellik oluşturulduktan sonra özellikte tıklayarak düzenleyebilirsiniz. Özellik adını değiştirirseniz, bu özellik başvuru ilkeleri yeni bir ad kullanmak için otomatik olarak güncelleştirilir.

REST API kullanarak bir özellik düzenleme hakkında daha fazla bilgi için bkz: [REST API kullanarak bir özelliği Düzenle](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Bir özelliği silmek için

Bir özellik silmek için tıklatın **silmek** silmek için özellik yanında.

> [!IMPORTANT]
> Özellik hiçbir ilke tarafından başvurulduğunda başarıyla özelliğini kullanan tüm ilkelerden kaldırana kadar silemiyor olacaktır.
> 
> 

REST API kullanarak bir özelliği silmeye hakkında daha fazla bilgi için bkz: [REST API kullanarak bir özelliği silmeye](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Arama ve filtreleme için özellikleri

**Değerleri adlı** sekmesi, arama ve filtreleme özelliklerinizi yönetmenize yardımcı olmak için özellikleri içerir. Özellik listesi özellik adına göre filtre uygulamak için bir arama terimi girmeniz **arama özelliği** metin kutusu. Tüm özellikleri görüntülemek için temizleyin **arama özelliği** textbox ve ENTER tuşuna basın.

Özellik listesi etiket değerlerine göre filtre uygulamak için bir veya daha fazla etiket içine girin **filtre etiketleriyle** metin kutusu. Tüm özellikleri görüntülemek için temizleyin **filtre etiketleriyle** textbox ve ENTER tuşuna basın.

## <a name="to-use-a-property"></a>Bir özelliği kullanmak için

Bir ilke özelliği kullanmak için özellik adı gibi küme ayraçları çift çifti içinde yerleştirin `{{ContosoHeader}}`, aşağıdaki örnekte gösterildiği gibi:

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

Özellik değerlerini ilke ifadelerini içerebilir olsa da, özellik değerleri diğer özellikleri içeremez. Bir özellik referansı içeren metin gibi bir özellik değeri için kullanılıyorsa, `Property value text {{MyProperty}}`, bu özellik başvurusu değiştirilmesi olmaz ve özellik değerinin bir parçası olarak dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar
* İlkeleri ile çalışma hakkında daha fazla bilgi edinin
  * [API Management ilkeleri](api-management-howto-policies.md)
  * [İlke başvurusu](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [İlke ifadeleri](https://msdn.microsoft.com/library/azure/dn910913.aspx)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

