---
title: Azure Resource Manager vCPU Kotayı artırmak istekleri | Microsoft Docs
description: Azure Resource Manager vCPU kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 05/09/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9d19d1a993d574777aa630b9c58f2472b94716cd
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479320"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Kaynak Yöneticisi'ni vCPU kota artışı istekleri

Sanal makineler ve sanal makine ölçek kümeleri için Kaynak Yöneticisi'ni vCPU kotaları, her bölgede her abonelik için iki katmanda uygulanır. 

İlk katman toplam bölgesel Vcpu sınırına (tüm VM serisi) ve ikinci katman VM serisi Vcpu sınırına (örneğin, D serisi Vcpu). Dağıtılması için yeni bir VM olan herhangi bir zamanda bu belirli sanal makine serisi başka için onaylanmış vCPU kotası VM serisi için yeni ve mevcut Vcpu kullanım toplamını aşmamalıdır, tüm VM serisi dağıtılmış toplam vCPU sayısı, toplam bölgesel Vcpu kotası aşmamalıdır  Abonelik için onaylandı. Kotalarda biri aşılırsa, VM dağıtımı izin verilmiyor.
Azure Portalı'ndan VM serisi Vcpu kotası sınırının artışı isteyebilir. VM serisi kota artış, toplam bölgesel Vcpu sınırına tutarında otomatik olarak artırır. 

Yeni bir abonelik oluşturulduğunda, varsayılan toplam bölgesel tüm bireysel VM serisi bu bir abonelikte yeterli miktarda kota ile dağıtmak istediğiniz tek tek her VM serisi için sonuçlanabilir için Vcpu varsayılan vCPU kotaları toplamına eşit olamaz , ancak abonelik için tüm dağıtımlar için toplam bölgesel Vcpu yetecek kadar kotanız yok. Bu durumda, açıkça toplam kota artışı için bölge sınırı artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vcpu sayısı sınırı, bölge için tüm VM serisi arasında onaylı kota toplamını aşamaz.

Kotaları hakkında daha fazla bilgi [sanal makine vCPU Kotası sayfası](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet sınırlamaları](https://aka.ms/quotalimits) sayfası. 

Şimdi aracılığıyla bir artış istemek **Yardım + Destek** dikey veya **kullanımları + kota** portaldaki dikey pencere. 

## <a name="request-quota-increase-using-the-help--support-blade"></a>Kullanarak kota artışı isteği **Yardım + Destek** dikey penceresi

Azure'nın 'Yardım + Destek' dikey penceresinde Azure portalında kullanılabilir üzerinden destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. Gelen https://portal.azure.comseçin **Yardım + Destek**.

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


## <a name="request-quota-increase-at-subscription-level-using-usages--quota"></a>Abonelik düzeyinde kullanımları + kota kullanarak kota artışı isteği

Azure'nın 'kullanımı + kota' aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin dikey penceresinde Azure portalında kullanılabilir. 

1. Gelen https://portal.azure.comseçin **abonelikleri**.

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


