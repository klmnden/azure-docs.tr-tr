---
title: Azure İlkesi ile uyumlu olmayan kaynakları Düzelt
description: Bu yöntem Azure İlkesi'nde ilkelerine uyumlu olmayan kaynakları düzeltme size kılavuzluk eder.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 747650bc47644cdca07f705f42d063c995ebe9bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980262"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Azure İlkesi ile uyumlu olmayan kaynakları Düzelt

İçin uyumlu olmayan kaynakları bir **Deployıfnotexists** İlkesi koyabilir ile uyumlu bir duruma **düzeltme**. Düzeltme çalıştırmak için ilke yönlendirerek gerçekleştirilir **Deployıfnotexists** atanan ilke mevcut kaynaklarınız üzerindeki etkisi. Bu nasıl yapılır bunun yerine getirmek için gereken adımlarda size yol gösterir.

## <a name="how-remediation-security-works"></a>Düzeltme güvenliği nasıl çalışır

İlke çalıştığında şablonu **Deployıfnotexists** ilke tanımı, mevcut bunu kullanarak bir [yönetilen kimliği](../../../active-directory/managed-identities-azure-resources/overview.md).
İlke, her bir atama için yönetilen bir kimlik oluşturur, ancak yönetilen kimlik vermek üzere hangi rolleri hakkındaki ayrıntıları sağlanmalıdır. Yönetilen kimlik rolleri bulunmuyorsa, bu ilkeyi veya ilkeyi içeren bir girişim ataması sırasında görüntülenir. Atama başlatıldıktan sonra portalı kullanırken, ilke otomatik olarak yönetilen kimlik listelenen rollere izin vermiş olursunuz.

![Yönetilen kimlik - eksik rol](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Bir kaynak tarafından değiştirilirse **Deployıfnotexists** atamanın yönetilen kimlik ilke ataması kapsamı olmalıdır program aracılığıyla dış erişim izni verilir veya düzeltme dağıtımı başarısız olur.

## <a name="configure-policy-definition"></a>İlke tanımı'nı yapılandırma

Rol tanımlamak için ilk adımıdır, **Deployıfnotexists** , dahil şablon içeriği başarıyla dağıtmak için ilke tanımında gerekir. Altında **ayrıntıları** özelliği eklemek bir **roleDefinitionIds** özelliği. Bu, ortamınızdaki eşleşen dizeler dizisidir.
Tam bir örnek için bkz: [Deployıfnotexists örnek](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** tam kaynak kimliğini kullanır ve kısa verdiği hızlı tepkilerden faydalanamamış **roleName** rolü. Ortamınızı 'Katkıda bulunan' rolü için kimliği almak için aşağıdaki kodu kullanın:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Contributor'
```

## <a name="create-a-remediation-task"></a>Düzeltme görev oluşturma

Değerlendirme, ilke atamasıyla sırasında **Deployıfnotexists** etkisi, uyumlu olmayan kaynakları olup olmadığını belirler. Uyumlu olmayan kaynakları bulunduğunda ayrıntıları sağlanır **düzeltme** sayfası. Uyumlu olmayan kaynakları olan ilkeleri listesinde birlikte tetiklemeye yönelik seçeneği olan bir **düzeltme görev**. Bu bir dağıtımın ne oluşturur, **Deployıfnotexists** şablonu.

Oluşturmak için bir **düzeltme görev**, şu adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![İlke arama](../media/remediate-resources/search-policy.png)

1. Seçin **düzeltme** Azure İlkesi sayfasının sol tarafındaki.

   ![Düzeltme seçin](../media/remediate-resources/select-remediation.png)

1. Tüm **Deployıfnotexists** uyumlu olmayan kaynakları olan ilke atamaları eklenir **düzeltmeye yönelik ilkeler** sekmesi ve veri tablosu. Bir ilkeyle uyumlu olmayan kaynakları tıklayın. **Yeni bir düzeltme görev** sayfası açılır.

   > [!NOTE]
   > Açmak için alternatif bir yolu **düzeltme görev** sayfasıdır bulup ilkeden tıklayarak **Uyumluluk** sayfasında'a tıklayın **düzeltme Görevi Oluştur** düğmesi.

1. Üzerinde **yeni bir düzeltme görev** sayfasında, kaynakları kullanarak düzeltmek için filtre **kapsam** alt kaynakları burada İlke atandı seçmek için üç nokta simgesini (aşağı ayrı kaynak dahil nesneler). Ayrıca, **konumları** daha da fazla filtrelemek için kaynakları açılır. Yalnızca kaynak tabloda listelenen düzeltilebilir.

   ![Düzelt - kaynakları seçin](../media/remediate-resources/select-resources.png)

1. Düzeltme görevini kaynakları tıklayarak filtrelendi sonra başlatmak **düzelt**. İlke uyumluluk sayfası açılacak **düzeltme görevleri** görevleri ilerleme durumunu göstermek için sekmesinde.

   ![Düzelt - görev ilerleme durumu](../media/remediate-resources/task-progress.png)

1. Tıklayarak **düzeltme görev** İlkesi uyumluluk sayfasından ilerleme durumu hakkında ayrıntılı bilgi edinmek için. Görev için kullanılan filtreleme gösterilen düzeltilen kaynakların listesini yanı sıra.

1. Gelen **remedation görev** sayfasında, ya da düzeltme görevin dağıtım görüntülemek için bir kaynak veya kaynak üzerinde sağ tıklayın. Satırın sonunda tıklayarak **ilgili olaylar** gibi bir hata iletisi ayrıntılarını görmek için.

   ![Düzeltme - kaynak görev bağlam menüsü](../media/remediate-resources/resource-task-context-menu.png)

Dağıtılan kaynakları aracılığıyla bir **düzeltme görev** eklenir **dağıtılan kaynakların** uyumluluk İlkesi sayfasında kısa bir gecikmeden sonra sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](../concepts/definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](../concepts/effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](getting-compliance-data.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz