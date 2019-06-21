---
title: Sınır artışı ağ | Microsoft Docs
description: Ağ sınırını artırın
author: anavinahar
ms.author: anavin
ms.date: 06/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 48d7e9cc4a3034e149901931f2addbc7df78e2bc
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297302"
---
# <a name="networking-limit-increase"></a>Ağ sınırını artırın

Geçerli ağ kullanımınızı ve kota görüntülemek için ziyaret edebilirsiniz **kullanımları + kota** Azure portalındaki dikey penceresinde. Kullanım kullanabilirsiniz [CLI](https://docs.microsoft.com//cli/azure/network?view=azure-cli-latest#az-network-list-usages), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkusage?view=azurermps-6.13.0) veya [ağ kullanım API'si](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/listusage) sınırları ve ağ kullanımını görüntülemek için.

Aracılığıyla artışı isteyebilir **Yardım + Destek** dikey veya **kullanımları + kota** portaldaki dikey pencere.

## <a name="request-networking-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Abonelik düzeyi kullanarak ağ kota artışı isteği **Yardım + Destek** dikey penceresi

Azure'nın 'Yardım + Destek' dikey penceresinde Azure portalında kullanılabilir üzerinden destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. Gelen https://portal.azure.com seçin **Yardım + Destek**.

    ![Yardım + destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  **Yeni destek isteği**’ni seçin. 

    ![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Sorun türü açılır listesinde seçin **hizmet ve abonelik sınırlarını (kotalar)** .

    ![Sorun türü açılan kutusu](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Kotasını artırmanız gereken aboneliği seçin.

    ![Abonelik newSR seçin](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seçin **ağ** içinde **kota türü** açılır. 

    ![Kota türü seçin](./media/networking-quota-request/select-quota-type-network.png)

6. İçinde **sorun ayrıntıları**, tıklayarak isteğiniz işlem yardımcı olacak ek bilgiler sağlayın **ayrıntıları sağlayın**.

    ![Ayrıntıları sağlayın](./media/resource-manager-core-quotas-request/provide-details.png)

7. İçinde **kota ayrıntıları** panelinde, dağıtım modeli, bir konum ve artış istemek için istediğiniz kaynakları seçin.

    ![Kota ayrıntıları DM](./media/networking-quota-request/quota-details-network.png)

8.  İstediğiniz yeni sınırları, abonelik üzerinde girin. Bir satırı kaldırmak için kaynak grubundan kaynak açılan seçeneğinin işaretini kaldırın veya atma "x" simgesini tıklatın. Her kaynak için istenen kotayı girdikten sonra tıklayın **Kaydet ve devam et** ile destek isteği oluşturma devam etmek için kota Ayrıntılar paneli üzerinde.

    ![Yeni sınırlar](./media/networking-quota-request/network-new-limits.png)


## <a name="request-networking-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Abonelik düzeyi kullanarak ağ kota artışı isteği **kullanımları + kota** dikey penceresi

Azure'nın 'kullanımı + kota' aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin dikey penceresinde Azure portalında kullanılabilir. 

1. Gelen https://portal.azure.com seçin **abonelikleri**.

    ![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

    ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seçin **kullanım ve kotalar**

    ![Kullanım ve kotalar seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **istek artışı**.

    ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. 3\. adımından başlayarak adımları *abonelik düzeyinde kota artışı isteği ağ* kullanma bölümünde **Yardım + Destek** dikey bölümde

## <a name="about-networking-limits"></a>Ağ limitleri hakkında

Ağ limitleri hakkında daha fazla bilgi için bkz: [ağ bölümü](../azure-subscription-service-limits.md#networking-limits) sınırları sayfasının veya ağ sınırları SSS bölümümüzü