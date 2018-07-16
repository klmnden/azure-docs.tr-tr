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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740589"
---
[Azure ilkeleri](/azure/azure-policy/) abonelikteki tüm kaynakların şirket standartlarına uyduğundan emin olmanıza yardımcı olur. Dağıtım seçeneklerini yalnızca onaylanan kaynak türleri ve SKU’larına kısıtlayarak maliyetlerinizi azaltmak için ilkeleri kullanın. Kaynaklarınız için kuralları ve eylemleri siz belirlersiniz ve bu kurallar da dağıtım sırasında otomatik olarak uygulanır. Örneğin, dağıtılan kaynakları türlerini denetleyebilirsiniz. Alternatif olarak, kaynaklar için onaylanan konumları kısıtlayabilirsiniz. Bazı ilkeler bir eylemi reddeder ve bazı ilkeler ise bir eylemin denetimini ayarlar.

İlkeler, rol tabanlı erişim denetimi (RBAC) konusunda tamamlayıcı bir etkendir. RBAC, kullanıcı erişimine odaklanır ve bir varsayılan reddetme ve açık izin verme sistemidir. İlke, dağıtım sırasında ve sonrasında kaynak özelliklerine odaklanır. İlke, varsayılan bir izin ve açık reddetme sistemidir.

İlkelerle ilgili anlaşılması gereken iki kavram vardır - *ilke tanımları* ve *ilke atamaları*. İlke tanımı, uygulamak istediğiniz yönetim koşullarını açıklar. İlke ataması, bir ilke tanımını belirli bir kapsam için uygulamaya koyar.

![İlke atama](./media/resource-manager-governance-policy/policy-concepts.png)

Azure, herhangi bir değişiklik yapmadan kullanabileceğiniz birkaç yerleşik ilke tanımı sağlar. Kapsamınızda izin verilen değerleri belirtmek için parametre değerleri geçirirsiniz. Yerleşik ilke tanımı gereksinimlerinizi karşılamıyorsa, [özel ilke tanımları oluşturabilirsiniz](../articles/azure-policy/create-manage-policy.md).
