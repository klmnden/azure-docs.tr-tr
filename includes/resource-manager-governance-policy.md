---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c04514218c7ed8dfd72b94345d2deb88e663fda1
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
ms.locfileid: "29529981"
---
[Azure ilkeleri](/azure/azure-policy/) emin olmanız tüm kaynakları abonelikte Yardım Kurumsal standartları karşılamak. Yalnızca bu kaynak türleri ve onaylanan SKU'ları için dağıtım seçenekleri kısıtlayarak, maliyetleri azaltmak için ilkeleri kullanın. Kaynaklarınız için kurallar ve Eylemler tanımlayabilir ve bu kurallar, dağıtım sırasında otomatik olarak uygulanır. Örneğin, dağıtılan kaynak türleri kontrol edebilirsiniz. Veya, kaynaklar için onaylanan konumları kısıtlayabilirsiniz. Bazı ilkeler bir eylem Reddet ve eylem denetim bazı ilkeler ayarlama.

Rol tabanlı erişim denetimi (RBAC) tamamlayıcı ilkesidir. RBAC kullanıcı erişimini odaklanır ve bir varsayılan reddetme olduğu ve açık sistem izin verin. İlke sırasında ve dağıtımdan sonra kaynak özellikleri odaklanır. Bir varsayılan izin ve açık sistem engelle.

İlkeleriyle - anlamak için iki kavram vardır *ilke tanımları* ve *ilke atamalarını*. Bir ilke tanımı uygulamak istediğinize yönetim koşulları açıklar. Bir ilke ataması bir ilke tanımı belirli bir kapsam için eyleme yerleştirir.

![İlke ataması](./media/resource-manager-governance-policy/policy-concepts.png)

Azure hiçbir değişiklik yapmadan kullanabileceğiniz birkaç yerleşik ilke tanımları sağlar. Kapsam içinde izin verilen değerleri belirtmek için parametre değerlerini geçirin. Yerleşik ilke tanımı gereksinimlerinizi karşılamak mu yoksa, şunları yapabilirsiniz [özel ilke tanımları oluşturma](../articles/azure-policy/create-manage-policy.md).
