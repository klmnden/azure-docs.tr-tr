---
title: "Uygulama Hizmetleri - Azure yığın çalışan rollerinde genişletme | Microsoft Docs"
description: "Azure yığın uygulama hizmetleri ölçeklendirme için ayrıntılı kılavuz"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: d6471796863a80e69fdaf740b68fb27d59503453
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Azure yığın uygulama hizmeti: daha fazla altyapı veya çalışan rolleri Ekle

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*  

Bu belge, Azure yığın altyapı ve çalışan rolleri uygulama hizmeti ölçeklendirme hakkında yönergeler sağlar. Herhangi bir boyuttaki uygulamaları desteklemek için ek çalışan rolleri oluşturmak için adımları içerir.

> [!NOTE]
> Azure yığın ortamınıza en fazla 96 GB RAM yoksa, ek kapasite ekleme ilgili sorunlar olabilir.

Varsayılan olarak, Azure yığın uygulama hizmeti ücretsiz ve paylaşılan çalışan katmanları destekler. Diğer çalışan katmanı eklemek için daha fazla çalışan rolleri eklemeniz gerekir.

Hangi Azure yığın yüklemede varsayılan uygulama hizmeti ile dağıtılan emin değilseniz, ek bilgileri gözden geçirebilirsiniz [genel bakış Azure yığın uygulama hizmeti](azure-stack-app-service-overview.md).

Azure uygulama hizmeti Azure yığında sanal makine ölçekleme kümeleri kullanarak tüm rolleri dağıtır ve bu nedenle bu iş yükünün ölçeklendirme özelliklerinden yararlanır. Bu nedenle, tüm ölçeklendirme çalışan katmanı uygulama Hizmet Yöneticisi yapılır

> [!IMPORTANT]
> Şu anda Azure yığın Sürüm Notları'nda tanımlanan portalında sanal makine ölçek kümeleri ölçeklendirme, bu nedenle ölçeği genişletme PowerShell örneği kullanmak mümkün değil.
>
>

## <a name="add-additional-workers-with-powershell"></a>PowerShell ile ek çalışanları ekleme

1. [PowerShell'de Azure yığın yönetim ortamı Kurulumu](azure-stack-powershell-configure-admin.md)
2. Ölçek kümesi ölçeklendirmek için bu örneği kullanın:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRMAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if you VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
  
    '''

> [!NOTE]
> This step can take a number of hours to complete depending on the type of role and the number of instances.
>
>

3. Monitor the status of the new role instances in the App Service Administration, to check the status of an individual role instance click the role type in the list.

## Add additional workers directly within the App Service Resource Provider Admin.

1. Log in to the Azure Stack administration portal as the service administrator.

2. Browse to **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Click **Roles**. Here you see the breakdown of all App Service roles deployed.

4. Right click on the row of the type you want to scale and then click **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Click **Scaling**, select the number of instances you want to scale to, and then click **Save**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App Service on Azure Stack will now add the additional VMs, configure them, install all the required software, and mark them as ready when this process is complete. This process can take approximately 80 minutes.

7. You can monitor the progress of the readiness of the new roles by viewing the workers in the **Roles** blade.

## Result

After they are fully deployed and ready, the workers become available for users to deploy their workload onto them. The following shows an example of the multiple pricing tiers available by default. If there are no available workers for a particular worker tier, the option to choose the corresponding pricing tier is unavailable.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> To scale out Management, Front End or Publisher roles add you must scale out the corresponding role type. 
>
>

To scale out Management, Front End, or Publisher roles, follow the same steps selecting the appropriate role type. Controllers are not deployed as Scale Sets and therefore two should be deployed at Installation time for all production deployments.

### Next steps

[Configure deployment sources](azure-stack-app-service-configure-deployment-sources.md)
