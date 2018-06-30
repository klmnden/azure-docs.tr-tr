---
title: 'Azure yığın uygulama hizmeti: hata etki alanı güncelleştirme | Microsoft Docs'
description: Azure uygulama hizmeti Azure yığında hata etki alanlarında yeniden dağıtmak nasıl
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: anwestg
ms.openlocfilehash: ce57e153dcab6a386150ebefe1ecb4a018514247
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130379"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Azure uygulama hizmeti Azure yığında hata etki alanlarında yeniden dağıtmak nasıl

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

1802 güncelleştirmeyle Azure yığını şimdi iş yüklerinin dağıtımını hata etki alanlarında, yüksek kullanılabilirlik için kritik bir özelliğini destekler.

>[!IMPORTANT]
>Hata etki alanı desteği yararlanmak için 1802 için Azure tümleşik yığını sisteminizi güncelleştirmeniz gerekir. Bu belgede yalnızca 1802 güncelleştirmeden önce tamamlandı uygulama hizmeti kaynak sağlayıcısı dağıtımları için geçerlidir. Azure yığınına 1802 güncelleştirme uygulandıktan sonra Azure yığın uygulama hizmeti dağıtılmışsa, kaynak sağlayıcısı zaten hata etki alanları arasında dağıtılır.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Bir uygulama hizmeti kaynak sağlayıcısı hata etki alanları arasında yeniden dengelemeniz

Uygulama hizmeti kaynak sağlayıcısı için dağıtılan ölçek kümeleri yeniden dağıtmak için her ölçek kümesi için bu makaledeki adımları uygulamanız gerekir. Varsayılan olarak, scaleset adları şunlardır:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Çalışan katmanı ölçek kümeleri bazıları dağıtılan örnekleri yoksa, bu ölçek kümeleri yeniden dengelemeniz gerek yoktur. Bunları gelecekte ölçeklendirme zaman ölçek kümeleri doğru dengelenir.

Ölçek kümeleri ölçeklendirmek için şu adımları izleyin:

1. Azure yığın Yönetici portalında oturum açın.
2. Seçin **daha fazla hizmet**.
3. İŞLEM altında seçin **sanal makine ölçek kümeleri**. Uygulama hizmeti dağıtımının bir parçası dağıtılan mevcut ölçek kümesi örnek sayısı bilgilerle listelenir. Aşağıdaki ekran görüntüsünde ölçek kümesi örneği gösterilmektedir.

      ![Sanal makine ölçek kümeleri UX içinde listelenen Azure App Service ölçek kümeleri][1]

4. Her küme ölçeklendirin. Ölçek kümesindeki var olan üç örneklerini varsa üç yeni örnekleri hata etki alanları arasında dağıtılan şekilde Örneğin, 6 ölçeğini gerekir. Aşağıdaki PowerShell örnek ölçek kümesini ölçeklendirmek için kullanıma gösterir.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Bu adım birkaç, rolün türü ve örnek sayısına bağlı olarak tamamlamak için saat sürebilir.

5. İçinde **uygulama hizmeti yönetim rolleri**, yeni rol örneklerinin durumunu izleyebilirsiniz. Rol örneği durumunu denetlemek için listeden rol türü seçin

    ![Azure yığın rolleri Azure uygulama hizmeti][2]

6. Yeni rol örneklerinin durumunu olduğunda **hazır**, geri dönerek **sanal makine ölçek kümesi** ve **silmek** eski rol örnekleri.

7. İçin bu adımları yineleyin **her** sanal makine ölçek kümesi.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
