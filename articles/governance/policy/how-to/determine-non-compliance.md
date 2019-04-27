---
title: Uyumsuzluk nedenlerini belirleme
description: Uyumlu olmayan bir kaynak olduğunda, çok sayıda olası nedeni vardır. Uyumsuzluk neyin neden olduğunu bulmak öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/30/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 0af3fd8596bf558f9d5cc97c95be773aa40954cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499362"
---
# <a name="determine-causes-of-non-compliance"></a>Uyumsuzluk nedenlerini belirleme

Bir Azure kaynağı için bir ilke kuralı uyumlu olmadığı belirlendiğinde kaynağı ile uyumlu değil kural hangi kısmının anlamak yararlıdır. Hangi değişiklik uyumlu hale getirmek için daha önce uyumlu bir kaynak değiştirilmiş anlamak kullanışlıdır. Bu bilgileri bulmanın iki yolu vardır:

> [!div class="checklist"]
> - [Uyumluluk ayrıntıları](#compliance-details)
> - [Değişiklik geçmişi (Önizleme)](#change-history-preview)

## <a name="compliance-details"></a>Uyumluluk ayrıntıları

Uyumlu olmayan bir kaynak olduğunda, o kaynak için Uyumluluk ayrıntıları kullanılabilir **ilke uyumluluğunu** sayfası. Uyumluluk ayrıntıları bölmesinde aşağıdaki bilgileri içerir:

- Ad, tür, konum ve kaynak kimliği gibi kaynak ayrıntıları
- Uyumluluk durumu ve geçerli ilke ataması için son değerlendirme zaman damgası
- Listesini _nedeniyle_ kaynak uyumsuzluk

> [!IMPORTANT]
> Uyumluluk ayrıntıları olarak bir _uyumlu_ kaynak gösterir Özellikleri'nin geçerli değeri bu kaynak üzerinde kullanıcı olmalıdır **okuma** işlemi **türü** , Kaynak. Örneğin, varsa _uyumlu_ kaynak **Microsoft.Compute/virtualMachines** kullanıcının olmalıdır sonra **Microsoft.Compute/virtualMachines/read** işlem. Kullanıcının gerekli işlemi yoksa, bir erişim hatası görüntülenir.

Uyumluluk ayrıntıları görüntülemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Üzerinde **genel bakış** veya **Uyumluluk** sayfasında, ilke bir **uyumluluk durumu** diğer bir deyişle _uyumlu_.

1. Altında **kaynak Uyumluluk** sekmesinde **ilke uyumluluğunu** sayfasında sağ tıklayın veya bir kaynağın noktayı bir **uyumluluk durumu** diğer bir deyişle  _Uyumlu olmayan_. Ardından **uyumluluk ayrıntıları görüntüle**.

   ![Görünümü uyumluluğu ayrıntıları seçeneği](../media/determine-non-compliance/view-compliance-details.png)

1. **Uyumluluk ayrıntıları** bölmesi, geçerli bir ilke ataması kaynağın en son değerlendirme bilgileri görüntüler. Bu örnekte, alanın **Microsoft.Sql/servers/version** olduğu tespit edildiğinden _12.0_ while beklenen ilke tanımı _14.0_. Kaynak birden fazla nedenlerle uyumlu ise, her bu bölmesinde listelenir.

   ![Ayrıntılar bölmesinde uyumluluk ve uyumsuzluk nedenleri](../media/determine-non-compliance/compliance-details-pane.png)

   İçin bir **auditIfNotExists** veya **Deployıfnotexists** ilke tanımı ayrıntıları dahil **details.type** özelliği ve diğer isteğe bağlı özellikleri. Bir liste için bkz. [auditIfNotExists özellikleri](../concepts/effects.md#auditifnotexists-properties) ve [Deployıfnotexists özellikleri](../concepts/effects.md#deployifnotexists-properties). **Kaynak son değerlendirme** ilgili bir kaynaktan geliyorsa **ayrıntıları** tanımının bölümü.

   Örnek kısmi **Deployıfnotexists** tanımı:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Uyumluluk ayrıntıları bölmesinde - * ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Bir özellik değeri olduğunda verileri korumak için bir _gizli_ yıldız geçerli değeri görüntüler.

Bu ayrıntıları neden bir kaynak şu anda uyumlu olduğunu açıklayan, ancak değişikliğin uyumlu olmayan bir duruma neden olan bir kaynak için ne zaman yapıldığı gösterme. Bu bilgi için bkz: [değişiklik geçmişini (Önizleme)](#change-history-preview) aşağıda.

### <a name="compliance-reasons"></a>Uyumluluk nedenleriyle

Matris her olası eşler _neden_ sorumlu için [koşul](../concepts/definition-structure.md#conditions) ilke tanımı içinde:

|Neden | Koşul |
|-|-|
|Geçerli değer, anahtar olarak hedef değeri içermelidir. |containsKey veya **değil** notContainsKey |
|Geçerli değer hedef değeri içermelidir. |içerir veya **değil** notContains |
|Geçerli değer hedef değere eşit olmalıdır. |eşittir veya **değil** notEquals |
|Geçerli değer mevcut olmalıdır. |Var. |
|Geçerli değer hedef değerde bulunmalıdır. |içinde veya **değil** notIn |
|Geçerli değer hedef değer gibi olmalıdır. |gibi veya **değil** notLike |
|Geçerli değer, büyük/küçük harfe duyarlı olarak hedef değerle eşleşmelidir. |eşleşen veya **değil** notMatch |
|Geçerli değer, büyük/küçük harfe duyarlı olmayan şekilde hedef değerle eşleşmelidir. |matchInsensitively veya **değil** notMatchInsensitively |
|Geçerli değer, anahtar olarak hedef değeri içermemelidir. |notContainsKey veya **değil** containsKey|
|Geçerli değer hedef değeri içermemelidir. |notContains veya **değil** içerir |
|Geçerli değer hedef değere eşit olmamalıdır. |notEquals veya **değil** eşittir |
|Geçerli değer mevcut olmamalıdır. |**değil** var.  |
|Geçerli değer hedef değerde bulunmamalıdır. |notIn veya **değil** içinde |
|Geçerli değer hedef değer gibi olmamalıdır. |notLike veya **değil** gibi |
|Geçerli değer büyük/küçük harfe duyarlı olarak hedef değerle eşleşmemelidir. |notMatch veya **değil** eşleşmesi |
|Geçerli değer, büyük/küçük harfe duyarlı olmayan bir şekilde hedef değerle eşleşmemelidir. |notMatchInsensitively veya **değil** matchInsensitively |
|İlke tanımındaki etki ayrıntılarıyla eşleşen ilgili kaynak yok. |Bir kaynak türünün tanımlanan **then.details.type** ve ilgili tanımlanan kaynağa **varsa** ilke kuralı bölümü yok. |

## <a name="change-history-preview"></a>Değişiklik geçmişi (Önizleme)

Yeni bir parçası olarak **genel Önizleme**, son 14 gün, değişiklik geçmişini destekleyen tüm Azure kaynakları için kullanılabilir [tamamlama modu silme](../../../azure-resource-manager/complete-mode-deletion.md). Değişiklik geçmişi sağlar ayrıntılar hakkında bir değişiklik algıladığında ve _visual fark_ her değişiklik için. Resource Manager özelliklerini eklendiğinde, kaldırılmış veya değiştirilmiş bir değişiklik algılama tetiklenir.

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Üzerinde **genel bakış** veya **Uyumluluk** sayfasında, herhangi bir ilke seçin **uyumluluk durumu**.

1. Altında **kaynak Uyumluluk** sekmesinde **ilke uyumluluğunu** sayfasında, bir kaynak seçin.

1. Seçin **değişiklik geçmişini (Önizleme)** sekmesinde **kaynak Uyumluluk** sayfası. Tüm mevcut görüntüleniyorsa listesini değişiklikleri algıladı.

   ![Kaynak uyumluluk sayfasında ilke değişiklik geçmişi sekmesi](../media/determine-non-compliance/change-history-tab.png)

1. Algılanan değişiklikler birini seçin. _Visual fark_ kaynak üzerinde sunulan için **değişiklik geçmişini** sayfası.

   ![Değişiklik geçmişi sayfasındaki İlkesi değişiklik geçmişi görsel fark](../media/determine-non-compliance/change-history-visual-diff.png)

_Visual fark_ aides içinde bir kaynak değişikliklerini tanımlama. Algılanan değişiklikler kaynağın geçerli uyumluluk durumu için ilişkili değil.

Değişiklik geçmişi verilerini sağlayan [Azure kaynak Graph](../../resource-graph/overview.md). Azure portalı bu bilgileri sorgulamak için bkz. [alma kaynak değişiklikleri](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](../concepts/definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](../concepts/effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](getting-compliance-data.md)
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları Düzelt](remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz