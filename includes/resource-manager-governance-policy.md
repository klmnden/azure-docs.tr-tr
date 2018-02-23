---
title: "include dosyası"
description: "include dosyası"
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c1e490aacdc9f712c6e186e56fe35fd5872fb575
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
[Azure ilkeleri](/azure/azure-policy/) emin olmanız tüm kaynakları abonelikte Yardım Kurumsal standartları karşılamak. Yalnızca bu kaynak türleri ve onaylanan SKU'ları için dağıtım seçenekleri kısıtlayarak, maliyetleri azaltmak için ilkeleri kullanın. Kaynaklarınız için kurallar ve Eylemler tanımlayabilir ve bu kurallar, dağıtım sırasında otomatik olarak uygulanır. Örneğin, dağıtılan kaynak türleri kontrol edebilirsiniz. Veya, kaynaklar için onaylanan konumları kısıtlayabilirsiniz. Bazı ilkeler bir eylem Reddet ve eylem denetim bazı ilkeler ayarlama.

Rol tabanlı erişim denetimi (RBAC) tamamlayıcı ilkesidir. RBAC kullanıcı erişimini odaklanır ve bir varsayılan reddetme olduğu ve açık sistem izin verin. İlke sırasında ve dağıtımdan sonra kaynak özellikleri odaklanır. Bir varsayılan izin ve açık sistem engelle.

İlkeleriyle - ilke tanımları ve ilke atamalarını anlamak için iki kavram vardır. Bir ilke tanımı uygulamak istediğinize yönetim koşulları açıklar. Bir ilke ataması bir ilke tanımı belirli bir kapsam için eyleme yerleştirir.

![İlke ataması](./media/resource-manager-governance-policy/policy-concepts.png)

Azure hiçbir değişiklik yapmadan kullanabileceğiniz birkaç yerleşik ilke tanımları sağlar. Kapsam içinde izin verilen değerleri belirtmek için parametre değerlerini geçirin. Yerleşik ilke tanımı gereksinimlerinizi karşılamak mu yoksa, şunları yapabilirsiniz [özel ilke tanımları oluşturma](../articles/azure-policy/create-manage-policy.md).

### <a name="who-can-create-and-assign-policies"></a>Kimin oluşturabilir ve ilke ataması

İlkeleri kullanmak için RBAC aracılığıyla kimliğinizi doğrulamış olmanız gerekir. Hesabınız özellikle şunlara ihtiyaç duyar:

* İlke tanımlamak için `Microsoft.Authorization/policydefinitions/write` izni.
* İlke atamak için `Microsoft.Authorization/policyassignments/write` izni.

Bu izinleri dahil olmayan **katkıda bulunan** rol.
