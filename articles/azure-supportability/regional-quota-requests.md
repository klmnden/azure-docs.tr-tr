---
title: Azure bölgesel Kotayı artırmak istekleri | Microsoft Docs
description: Bölgesel kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7ac6dce9aec1c363fa9772caabad9ce542d43888
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310659"
---
# <a name="total-regional-vcpu-limit-increase"></a>Toplam bölgesel vCPU sınır artışı 

Sanal makineler ve sanal makine ölçek kümeleri için Kaynak Yöneticisi'ni vCPU kotaları, her bölgede her abonelik için iki katmanda uygulanır. 

İlk katmandır **toplam bölgesel Vcpu sınırına** (genelinde tüm VM serisi), ve ikinci katman **Vcpu VM serisi sınırlamak** (örneğin, D serisi Vcpu). Dilediğiniz zaman dağıtılması için yeni bir sanal makine olduğu belirli sanal makine serisi için onaylanmış vCPU kotası VM serisi için yeni ve mevcut Vcpu kullanım toplamını aşmamalıdır. Ayrıca, tüm VM serisi dağıtılmış toplam yeni ve mevcut vCPU sayısı abonelik için onaylanmış toplam bölgesel Vcpu kotası aşmamalıdır. Kotalarda biri aşılırsa, VM dağıtımı izin verilmiyor.
Azure Portalı'ndan VM serisi Vcpu kotası sınırının artışı isteyebilir. VM serisi kota artış, toplam bölgesel Vcpu sınırına tutarında otomatik olarak artırır. 

Yeni bir abonelik oluşturulurken varsayılan toplam bölgesel vcpu sayısı tek tek tüm VM serisi için varsayılan vCPU kotaları toplamına eşit olmayabilir. Bu, bir abonelikte dağıtmak istediğiniz tek tek her VM serisi için yeterli miktarda kota, ancak tüm dağıtımlar için toplam bölgesel Vcpu için yeterli kotası ile sonuçlanabilir. Bu durumda, açıkça toplam bölgesel Vcpu sınırı artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vcpu sayısı sınırı, bölge için tüm VM serisi arasında onaylı kota toplamını aşamaz.

Kotaları hakkında daha fazla bilgi [sanal makine vCPU Kotası sayfası](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet sınırlamaları](https://aka.ms/quotalimits) sayfası. 

Şimdi aracılığıyla bir artış istemek **Yardım + Destek** dikey veya **kullanımları + kota** portaldaki dikey pencere. 

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Abonelik kullanarak düzeyinde toplam bölgesel Vcpu kota artışı isteği **Yardım + Destek** dikey penceresi

Azure'nın 'Yardım + Destek' dikey penceresinde Azure portalında kullanılabilir üzerinden destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. Gelen https://portal.azure.com seçin **Yardım + Destek**.

![Yardım + destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  **Yeni destek isteği**’ni seçin. 

![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Sorun türü açılır listesinde seçin **hizmet ve abonelik sınırlarını (kotalar)** .

![Sorun türü açılan kutusu](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik newSR seçin](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seçin **diğer istekler** içinde **kota türü** açılır.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. İçinde **ayrıntıları** bölmesinde isteğiniz işlem yardımcı olmak için aşağıdaki örnekte ek bilgileri sağlayın ve durum oluşturma işlemine devam edin. 
    1.  **Dağıtım modeli** – ' Resource Manager' belirtin
    2.  **İstenen bölge** – gerekli bölgenizi örn: Doğu ABD 2 belirtin
    3.  **Yeni sınır değeri** – yeni bölge sınırı belirtin. Bu abonelik için tek tek SKU aileleri için onaylanan kota toplamını aşmamalıdır

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>Abonelik kullanarak düzeyinde toplam bölgesel Vcpu kota artışı isteği **kullanımları + kota** dikey penceresi

Azure'nın 'kullanımı + kota' aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin dikey penceresinde Azure portalında kullanılabilir. 

1. Gelen https://portal.azure.com seçin **abonelikleri**.

![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seçin **kullanım ve kotalar**

![Kullanım ve kotalar seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **istek artışı**.

![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seçin **diğer istekler** içinde **kota türü** açılır.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. İçinde **ayrıntıları** bölmesinde isteğiniz işlem yardımcı olmak için aşağıdaki örnekte ek bilgileri sağlayın ve durum oluşturma işlemine devam edin. 
    1.  **Dağıtım modeli** – ' Resource Manager' belirtin
    2.  **İstenen bölge** – gerekli bölgenizi örn: Doğu ABD 2 belirtin
    3.  **Yeni sınır değeri** – yeni bölge sınırı belirtin. Bu abonelik için tek tek SKU aileleri için onaylanan kota toplamını aşmamalıdır

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)



