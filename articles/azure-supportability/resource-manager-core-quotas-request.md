---
title: Azure Resource Manager vCPU Kotayı artırmak istekleri | Microsoft Docs
description: Azure Resource Manager vCPU kota artışı istekleri
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 204deaf3a67984c0dd5eca5352686719e7767885
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60649358"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Kaynak Yöneticisi'ni vCPU kota artışı istekleri

Kaynak Yöneticisi'ni vCPU kotaları bölge düzeyinde ve SKU ailesi düzeyinde uygulanır.
Nasıl kotalar üzerinde zorlanır hakkında daha fazla bilgi [Azure aboneliği ve hizmet sınırlamaları](https://aka.ms/quotalimits) sayfası.
SKU aileleri hakkında daha fazla bilgi için maliyet ve performans üzerinde karşılaştırmak [sanal makineler fiyatlandırma](https://aka.ms/pricingcompute) sayfası.

Bir artış istemek için Azure'nın 'kullanımı + kota' aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. dikey penceresinde Azure Portalı'nda kullanılabilir. 

## <a name="request-quota-increase-at-subscription-level"></a>Abonelik düzeyinde kota artışı isteği

1. Gelen https://portal.azure.comseçin **abonelikleri**.

   ![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seçin **kullanım ve kotalar**

   ![Kullanım ve kotalar seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **istek artışı**.

   ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. : 1. adım - Select **çekirdek** Teklif türü. 

   ![Formu doldurun](./media/resource-manager-core-quotas-request/forms.png)
   
6. : 2. adım - Select Deployment model "Kaynak Yöneticisi" ve bir konum seçin.

    ![Kota sorun dikey penceresi](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Bir artış gerektiren SKU aileleri seçin.

    ![SKU serileri](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. İstediğiniz yeni sınırları, abonelik üzerinde girin.

    ![SKU yeni kota isteği](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Bir satırı kaldırmak için SKU ailesi açılır listeden SKU'seçeneğinin işaretini kaldırın veya atma "x" simgesini tıklatın.
Her SKU ailesi için istenen kotayı girdikten sonra sorun adım sayfasında ile destek isteği oluşturma devam etmek için "İleri" ye tıklayın.

