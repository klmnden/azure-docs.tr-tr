---
title: "Kaynak ilkeleri için Azure portalı | Microsoft Docs"
description: "Azure portal oluşturmak ve Resource Manager ilkelerini yönetmek için nasıl kullanılacağını açıklar. Abonelik veya kaynak gruplarının ilkeleri uygulanabilir."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a>Atama ve kaynak ilkelerini yönetmek için Azure portalını kullanma
Azure portalı kaynak grupları ve abonelikler için kaynak ilkeleri atamanızı sağlar. Kullanıcı arabirimi atayın ve ilke ayarlarını özelleştirmek bu ilkeye parametre değerlerini belirtin istediğiniz ilkeyi seçin kolay hale getirir. 

Portal üzerinden ilke atamak için ilke tanımı önceden aboneliğinizde var olmalıdır. Aboneliğiniz hazır, kaynak grupları veya abonelikleri atamak birkaç yerleşik ilke tanımları sahip. Bu yerleşik ilkeleri ve ilkeleri atamak için portal kullanırken, tanımladığınız tüm özel ilkeler bakın. İlkeleri ve özelleştirilmiş ilkesinin nasıl tanımlanacağını bir giriş için bkz: [kaynak ilkesine genel bakış](resource-manager-policy.md).

İlkeler tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklar için geçerlidir. Bu makalede, terimi **kapsam** kaynak grubu veya ilke atanan abonelik başvuruyor. 

İlkeleri oluşturma ve (PUT ve işlemleri düzeltme eki) kaynakları güncelleştirme değerlendirilir.

## <a name="assign-a-policy"></a>Bir ilke atama

1. Kaynak grubu veya abonelik için bir ilke atamak için bu kaynak grubuna veya aboneliğe seçin. Ayarları'nda seçin **ilkeleri**.

   ![İlkeleri'ni seçin](./media/resource-manager-policy-portal/select-policies.png)

2. Bu kapsam için bir ilke ataması oluşturmak için seçin **atama Ekle**.

   ![atama Ekle](./media/resource-manager-policy-portal/add-assignment.png)

3. Atamak istediğiniz ilkeyi seçin. Aboneliğiniz için tüm ilke tanımları eklemediniz olsa bile atama için uygun olan yerleşik ilkelerine bakın. Bu yerleşik ilkeleri birçok yaygın senaryolar kapsar.

   ![tanımı seçin](./media/resource-manager-policy-portal/select-definition.png)

4. Bir ilke seçildikten sonra ilke ve bu ilkeye herhangi bir parametre açıklaması görürsünüz. Örneğin, aşağıdaki gösterir görüntü **konumları izin** kullanılabilir konumlarını kısıtlayan ilkesi için gerekli olan parametre.

   ![Parametreleri Göster](./media/resource-manager-policy-portal/show-parameters.png)

5. Kullanıcı arabirimi aracılığıyla İlkesi parametreler (örneğin, dağıtım için kullanılan konumları) belirtmek için değerleri seçin.

   ![parametre değerlerini seçin](./media/resource-manager-policy-portal/select-parameters.png)

6. Diğer parametreler için değerler sağlayın. Kapsam ilke ataması başlatırken seçili dikey penceresinde otomatik olarak atanır. Tamamladığınızda **Tamam**’ı seçin.

   ![parametrelerini tanımlayın](./media/resource-manager-policy-portal/define-parameters.png)

  Belirtilen kapsam için ilke atadınız.

## <a name="view-policy-assignments"></a>İlke atamalarını görüntülemek

Bir ilke atadıktan sonra bu kapsama ilkelerin listesini konusuna bakın. **Ayrıntıları** sekmesi ilke ataması özetini gösterir.

![Ayrıntıları Göster](./media/resource-manager-policy-portal/show-details.png)

**Atama kuralı** sekmesi ilke tanımı için JSON gösterir.

![atama kuralı Göster](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Var olan bir ilke atamasını değiştirin

Bir ilkeyi değiştirmek için seçin **Düzenle atama** veya **Sil**

![düzenleme veya silme atama](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Özel ilkeler atayın

Özel ilkeler, aboneliğinizde tanımladıysanız, bu ilkeleri portal aracılığıyla kullanılabilir. Bu ilkeleri ile başlayan **[özel]**

![özel ilkeler](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Sonraki adımlar
* İlkeleri tanımlamak için JSON söz dizimi hakkında bilgi edinmek için [kaynak ilkesine genel bakış](resource-manager-policy.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
* İlke şema yayımlanma [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

