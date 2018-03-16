---
title: "Azure yığın uygulama hizmeti: hata etki alanı güncelleştirme | Microsoft Docs"
description: "Azure uygulama hizmeti Azure yığında hata etki alanlarında yeniden dağıtmak nasıl"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 851747263879aa89fabe8b168876238a004ea8b2
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Azure uygulama hizmeti Azure yığında hata etki alanlarında yeniden dağıtmak nasıl

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

1802 güncelleştirmeyle Azure yığını şimdi iş yüklerinin dağıtımını hata etki alanlarında, yüksek kullanılabilirlik için kritik bir özelliği destekler.

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi hata etki alanı desteği yararlanmak için 1802 için güncelleştirilmiş gerekir.  Bu belgede yalnızca önce 1802 güncelleştirme tamamlandı uygulama hizmeti kaynak sağlayıcısı dağıtımları için geçerlidir.  Azure yığınına 1802 güncelleştirme uygulandıktan sonra Azure yığın uygulama hizmeti dağıttıysanız, kaynak sağlayıcısı zaten hata etki alanları arasında dağıtılır.
>
>

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Bir uygulama hizmeti kaynak sağlayıcısı hata etki alanları arasında yeniden dengelemeniz

Uygulama hizmeti kaynak sağlayıcısı için dağıtılan ölçek kümeleri yeniden dağıtmak için her bir ölçek kümesi için aşağıdaki adımları gerçekleştirmeniz gerekir.  Varsayılan olarak scaleset adları şunlardır:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

> [!NOTE]
> Çalışan katmanı ölçek kümeleri bazıları dağıtılan hiçbir örneği varsa, bu ölçek kümeleri yeniden dengelemeniz gerekmez.  Bunları gelecekte ölçeklendirme zaman ölçek kümeleri doğru dengelenir.
>
>

1. Sanal makine ölçek kümeleri Azure yığın Yönetici portalında göz atın.  Uygulama hizmeti dağıtımının bir parçası dağıtılan mevcut ölçek kümesi örnek sayısı bilgilerle listelenir.

    ![Sanal makine ölçek kümeleri UX içinde listelenen Azure App Service ölçek kümeleri][1]

2. Her kümedeki sonraki genişletme.  Ölçek kümesindeki var olan üç örneklerini varsa, böylece üç yeni örnekleri hata etki alanlarında sağlanacak Örneğin, 6'ölçeğini gerekir.
    a. [PowerShell'de Azure yığın yönetim ortamı Kurulumu](azure-stack-powershell-configure-admin.md) b. Ölçek kümesi ölçeklendirmek için bu örneği kullanın:
        ```powershell
                Login-AzureRMAccount -EnvironmentName AzureStackAdmin 

                # Get current scale set
                $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

                # Set and update the capacity of your scale set
                $vmss.sku.capacity = 6
                Update-AzureRmVmss -ResourceGroupName "AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
        '''
> [!NOTE]
> Bu adım, rolün türü ve örnek sayısına bağlı olarak tamamlamak için saat sayısını alabilir.
>
>

3. Yeni uygulama hizmeti yönetim rolleri dikey penceresinde rol örneklerinin durumunu izleyin.  Rol türü listesinde tıklayarak tek rol örneğini durumunu denetleme

    ![Azure yığın rolleri Azure uygulama hizmeti][2]

4. Bir yeni örnekleri olan bir **hazır** durumu, sanal makine ölçek kümesi dikey geri gidin ve **silmek** eski örnekleri.

5. İçin bu adımları yineleyin **her** sanal makine ölçek kümesi.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
