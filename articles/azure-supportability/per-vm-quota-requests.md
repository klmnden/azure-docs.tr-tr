---
title: Azure VM başına vCPU Kotayı artırmak istekleri | Microsoft Docs
description: VM vCPU kotası istekleri artırın
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: f921b4a95c1b0cfb29d84c0bacc17d268af6e6c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082826"
---
# <a name="vm-series-vcpu-limit-increase"></a>VM serisi vCPU sınır artışı

Şimdi aracılığıyla bir artış istemek **Yardım + Destek** dikey veya **kullanımları + kota** portaldaki dikey pencere. 

## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>İstek başına VM vCPU kota artışı kullanarak abonelik düzeyinde **Yardım + Destek** dikey penceresi

Azure'nın 'Yardım + Destek' dikey penceresinde Azure portalında kullanılabilir üzerinden destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. Gelen https://portal.azure.com seçin **Yardım + Destek**.

![Yardım + destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  **Yeni destek isteği**’ni seçin. 

![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Sorun türü açılır listesinde seçin **hizmet ve abonelik sınırlarını (kotalar)** .

![Sorun türü açılan kutusu](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik newSR seçin](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seçin **- VM (çekirdek Vcpu) abonelik sınırını artırır işlem** içinde **kota türü** açılır. 

![Kota türü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. İçinde **sorun ayrıntıları**, tıklayarak isteğiniz işlem yardımcı olacak ek bilgiler sağlayın **ayrıntıları sağlayın**.

![Ayrıntıları sağlayın](./media/resource-manager-core-quotas-request/provide-details.png)

7. İçinde **kota ayrıntıları** paneline dağıtım modeli seçin ve bir konum seçin.

![Kota ayrıntıları DM](./media/resource-manager-core-quotas-request/quota-details.png)

8. Seçin **SKU aileleri** artışı gerektirir. 

![SKU Ailesi](./media/resource-manager-core-quotas-request/sku-family.png)

9. İstediğiniz yeni sınırları, abonelik üzerinde girin. Bir satırı kaldırmak için SKU ailesi açılır listeden SKU'seçeneğinin işaretini kaldırın veya atma "x" simgesini tıklatın. Her SKU ailesi için istenen kotayı girdikten sonra tıklayın **Kaydet ve devam et** ile destek isteği oluşturma devam etmek için kota Ayrıntılar paneli üzerinde.

![Yeni sınırlar](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Her VM vCPU kota artışı kullanarak abonelik düzeyinde istek **kullanımları + kota** dikey penceresi

Azure'nın 'kullanımı + kota' aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin dikey penceresinde Azure portalında kullanılabilir. 

1. Gelen https://portal.azure.com seçin **abonelikleri**.

![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seçin **kullanım ve kotalar**

![Kullanım ve kotalar seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **istek artışı**.

![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seçin **işlem VM (çekirdek Vcpu) abonelik sınırını artırır** Teklif türü. 

![Formu doldurun](./media/resource-manager-core-quotas-request/forms.png)
   
6. İçinde **kota ayrıntıları** paneline dağıtım modeli seçin ve bir konum seçin.

![Kota sorun dikey penceresi](./media/resource-manager-core-quotas-request/problemstep.png)

7. Seçin **SKU aileleri** artışı gerektirir.

![SKU serileri](./media/resource-manager-core-quotas-request/sku-family.png)

8. İstediğiniz yeni sınırları, abonelik üzerinde girin. Bir satırı kaldırmak için SKU ailesi açılır listeden SKU'seçeneğinin işaretini kaldırın veya atma "x" simgesini tıklatın. Her SKU ailesi için istenen kotayı girdikten sonra tıklayın **Kaydet ve devam et** ile destek isteği oluşturma devam etmek için sorun adım sayfasında.

![SKU yeni kota isteği](./media/resource-manager-core-quotas-request/new-limits.png)

