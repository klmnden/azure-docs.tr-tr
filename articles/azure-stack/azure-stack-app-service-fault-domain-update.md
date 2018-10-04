---
title: "Azure Stack üzerinde App Service'te: hata etki alanı güncelleştirme | Microsoft Docs"
description: Azure Stack'te Azure App Service, hata etki alanlarında yeniden dağıtma
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
ms.date: 09/05/2018
ms.author: anwestg
ms.openlocfilehash: d361b4165c1fbbf79321e3f6d2ade711f9173c56
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48267224"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Azure Stack'te Azure App Service, hata etki alanlarında yeniden dağıtma

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

1802 güncelleştirme ile Azure Stack artık dağıtım iş yüklerinin hata etki alanları arasında yüksek kullanılabilirlik için kritik olan bir özelliği destekler.

>[!IMPORTANT]
>Hata etki alanı desteği avantajlarından yararlanmak için Azure Stack tümleşik sistemi 1802 için güncelleştirmeniz gerekir. Bu belgede yalnızca 1802 güncelleştirmeden önce tamamlanmış App Service kaynak sağlayıcısı dağıtımları için geçerlidir. Azure Stack'e 1802 güncelleştirme uygulandıktan sonra Azure Stack üzerinde App Service'te dağıttıysanız, kaynak sağlayıcısı zaten hata etki alanlarına dağıtılır.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Bir App Service kaynak sağlayıcısı, hata etki alanları arasında yeniden dengelemeniz

Ölçek kümeleri için App Service kaynak sağlayıcısı dağıtılan yeniden dağıtmak için her bir ölçek kümesi için bu makaledeki adımları gerçekleştirmeniz gerekir. Varsayılan olarak, Ölçek kümesi adları şunlardır:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Çalışan katmanı ölçek kümeleri bazı durumlarda dağıtılan örneğe sahip değilseniz, bu ölçek kümeleri yeniden dengelemek gerekmez. Bunları gelecekte ölçeğini daralttığınızda ölçek kümeleri doğru dengelenir.

Ölçek kümeleri ölçeklendirmek için aşağıdaki adımları izleyin:

1. Azure Stack Yönetici portalında oturum açın.
1. **Tüm Hizmetler**’i seçin.
2. İçinde **işlem** kategorisi, select **sanal makine ölçek kümeleri**. Örnek sayısı bilgilerle uygulama hizmeti dağıtımının bir parçası olarak dağıtılan var olan ölçek kümeleri listelenir. Aşağıdaki ekran görüntüsü yakalamayı ölçek kümesi örneği gösterilmektedir.

      ![Sanal makine ölçek kümeleri UX içinde listelenen Azure App Service ölçek kümeleri][1]

1. Her kümesini ölçeklendirin. Ölçek kümesinde üç mevcut örneği varsa, hata etki alanları arasında dağıtılan üç yeni örnekleri için örneğin, 6'ölçeği gerekir. Ölçek kümesini ölçeklendirme çıkış aşağıdaki PowerShell örneği gösterilmektedir.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Bu adım birkaç rol türü ve örneği sayısını bağlı olarak, son bir saat sürebilir.

1. İçinde **App Service yönetim rolleri**, yeni alan rol örneklerinin durumunu izleyin. Bir rol örneğinin durumunu denetlemek için listeden rol türü seçin

    ![Azure Stack rolleri bir Azure uygulama hizmeti][2]

1. Yeni rol örneklerini durumunu olduğunda **hazır**dönün **sanal makine ölçek kümesi** ve **Sil** eski rol örnekleri.

1. Bu adımı yineleyin **her** sanal makine ölçek kümesi.

## <a name="next-steps"></a>Sonraki adımlar

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md).

* [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md)
* [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
