---
title: Adlandırılmış değerler Azure API Management ilkelerini kullanma
description: Adlandırılmış değerler Azure API Management ilkelerini kullanmayı öğrenin.
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
ms.openlocfilehash: 9e1b1953520c5502668fbbae70a37a140253b035
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241684"
---
# <a name="how-to-use-named-values-in-azure-api-management-policies"></a>Adlandırılmış değerler Azure API Management ilkelerini kullanma
API Management ilkeleri güçlü bir API configuration aracılığıyla davranışını değiştirmek Azure portalın sistem özellikleridir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. İlke ifadeleri, metin değerleri, ilke ifadeleri kullanarak ve adlandırılmış değerler oluşturulabilir. 

Her API Management hizmet örneği adlı hizmet örneği için genel kabul edilen değerler, adlı bir anahtar/değer çiftleri özellikleri koleksiyonu vardır. Bu adlandırılmış değerler, dize sabit değerleri, tüm API yapılandırması ve ilkelerini yönetmek için kullanılabilir. Her bir özellik aşağıdaki özniteliklere sahip olabilir:

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| `Display name` |string |İlkeleri özelliğinde başvurmak için kullanılan alfasayısal dize. |
| `Value`        |string |Özelliğin değeri. Değil boş olamaz veya yalnızca boşluktan oluşamaz olabilir. |
| `Secret`       |boole|Değer bir gizli dizidir ve veya şifrelenmesi gerektiğini belirler.|
| `Tags`         |dize dizisi |İsteğe bağlı etiketleri sağlandığında özellik listesini filtrelemek için kullanılabilir. |

![Adlandırılmış değerler](./media/api-management-howto-properties/named-values.png)

Özellik değerleri, değişmez değer dizeleri içerebilir ve [ilke ifadeleri](/azure/api-management/api-management-policy-expressions). Örneğin, değeri `ExpressionProperty` geçerli tarih ve saat içeren bir dize döndüren bir ilke ifadesidir. Özellik `ContosoHeaderValue` değeri görüntülenmez, böylece bir gizli dizi işaretlenir.

| Ad | Value | Secret | Tags |
| --- | --- | --- | --- |
| ContosoHeader |Trackingıd |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-add-and-edit-a-property"></a>Ekleme ve bir özelliği düzenlemek için

![Özellik ekleme](./media/api-management-howto-properties/add-property.png)

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. Seçin **değerleri adlı**.
3. Tuşuna **+ Ekle**.

   Ad ve değer gerekli değerler. Bu özellik değeri bir gizli dizi ise, bu, gizli bir onay kutusu denetleyin. Girin veya adlandırılmış değerlerinizi düzenleme ile yardımcı ve daha fazla isteğe bağlı etiket kaydedin.
4. **Oluştur**’a tıklayın.

Özellik oluşturulduktan sonra özellikte tıklayarak düzenleyebilirsiniz. Özellik adını değiştirirseniz, bu özelliğe başvuran tüm ilkeler yeni adı kullanacak şekilde otomatik olarak güncelleştirilir.

REST API kullanarak bir özellik düzenleme hakkında daha fazla bilgi için bkz. [REST API kullanarak bir özelliği düzenlemeyi](/rest/api/apimanagement/2019-01-01/property?patch).

## <a name="to-delete-a-property"></a>Bir özelliği silmek için

Bir özelliği silmek için tıklayın **Sil** silme özelliği yanında.

> [!IMPORTANT]
> Özellik ilkelerle başvuruyorsa, onu kullanan tüm ilkelerden özelliği kaldırana kadar başarılı bir şekilde silmek mümkün olmayacaktır.
> 
> 

REST API kullanarak bir özelliği silme hakkında daha fazla bilgi için bkz: [REST API kullanarak bir özelliği silmeye](/rest/api/apimanagement/2019-01-01/property/delete).

## <a name="to-search-and-filter-named-values"></a>Arama ve filtre adlı değerleri

**Değerleri adlı** arama ve filtreleme özellikleri, adlandırılmış değerler yönetmenize yardımcı olmak için sekmesinde bulunur. Özellik listesi özellik adına göre filtre uygulamak için bir arama terimi girin. **arama özelliği** metin. Adlandırılmış tüm değerleri görüntülemek için Temizle **arama özelliği** metin kutusunu ve ENTER'a basın.

Etiket değerlerine göre özellik listesini filtrelemek için bir veya daha fazla etiketi ekleyerek girin **etiketlere göre filtre** metin. Adlandırılmış tüm değerleri görüntülemek için Temizle **etiketlere göre filtre** metin kutusunu ve ENTER'a basın.

## <a name="to-use-a-property"></a>Bir özelliği kullanmak için

İlke bir özelliği kullanmak için özellik adı bir çift çifti gibi küme ayraçlarının içine yerleştirin `{{ContosoHeader}}`, aşağıdaki örnekte gösterildiği gibi:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Bu örnekte, `ContosoHeader` üst bilgisinde adı olarak kullanılan bir `set-header` İlkesi ve `ContosoHeaderValue` bu üst bilgi değeri olarak kullanılır. Bu ilke, bir istek veya yanıt olarak API Management ağ geçidinde sırasında değerlendirilirken `{{ContosoHeader}}` ve `{{ContosoHeaderValue}}` ilgili özellik değerlerini ile değiştirilir.

Adlandırılmış değerler tam özniteliği veya öğenin değerlerini önceki örnekte gösterildiği gibi olarak kullanılabilir, ancak bunlar ayrıca eklenen veya aşağıdaki örnekte gösterildiği gibi bir metin ifadesinin bir parçası birleştirilmiş: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Adlandırılmış değerler, ilke ifadeleri de içerebilir. Aşağıdaki örnekte, `ExpressionProperty` kullanılır.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Bu ilke değerlendirildiğinde `{{ExpressionProperty}}` değeriyle değiştirilir: `@(DateTime.Now.ToString())`. Değer bir ilke ifadesi olduğundan, ifade değerlendirilir ve ilke ile yürütme devam eder.

Bu Geliştirici Portalı'nda adlandırılmış değerler ile bir ilke kapsamında olan bir işlemine çağrı yaparak test edebilirsiniz. Aşağıdaki örnekte, bir işlem önceki iki örnekle çağrılır `set-header` ilkeleriyle adlandırılmış değerler. Yanıt, ilkelerle adlandırılmış değerler ile yapılandırılmış iki özel üst içerdiğine dikkat edin.

![Geliştirici portalı][api-management-send-results]

Bakarsanız [API denetçisi izleme](api-management-howto-api-inspector.md) iki önceki örnek ilkeleriyle adlandırılmış değerler içeren bir çağrı için iki gördüğünüz `set-header` ilkeleriyle yanı sıra ilke ifadesi eklenmiş özellik değerleri ilke ifadesi içeren bir özellik için değerlendirme.

![API denetçisi izleme][api-management-api-inspector-trace]

Özellik değerleri, özellik değerlerini ilke ifadeleri içerebilir ancak diğer adlandırılmış değerler içeremez. Bir özelliği başvuru içeren metin için bir özellik değeri gibi kullanılıyorsa `Property value text {{MyProperty}}`, bu özellik başvurusu değiştirilmesi gerekmez ve özellik değerinin bir parçası olarak dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar
* İlkeleriyle çalışma hakkında daha fazla bilgi edinin
  * [API Management ilkeleri](api-management-howto-policies.md)
  * [İlke başvurusu](/azure/api-management/api-management-policies)
  * [İlke ifadeleri](/azure/api-management/api-management-policy-expressions)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

