---
title: Ölçek genişletme uygulama hizmetleri - Azure Stack çalışan rolleri | Microsoft Docs
description: Azure Stack App Services ölçeklendirmeye yönelik ayrıntılı kılavuz
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: abfc75e40e146b1cf7cb237f18a1ff08626e5be1
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092916"
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Azure Stack üzerinde App Service'te: daha fazla altyapı veya çalışan rolü ekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*  

Bu belge, Azure Stack altyapısını ve çalışan rolleri App Service'ı ölçeklendirme hakkında yönergeler sağlar. Bu, her boyuttaki uygulamaları desteklemek için ek çalışan rolleri oluşturmak için adımları içerir.

> [!NOTE]
> Azure Stack ortamınıza en fazla 96 GB RAM yoksa, ek kapasite ekleme sorunlar olabilir.

Varsayılan olarak, Azure Stack üzerinde App Service, ücretsiz ve paylaşılan çalışan katmanları destekler. Diğer çalışan katmanları eklemek için daha fazla çalışan rolü eklemeniz gerekir.

Hangi Azure Stack yükleme varsayılan App Service ile dağıtılan emin değilseniz, ek bilgileri gözden geçirebilirsiniz [genel Azure Stack üzerinde App Service'te](azure-stack-app-service-overview.md).

Azure Stack'te Azure App Service, sanal makine ölçek kümeleri kullanarak tüm rolleri dağıtır ve bu nedenle bu iş yükünü ölçeklendirme özelliklerinden yararlanır. Bu nedenle, tüm çalışan katmanlarını ölçeklendirme uygulama Hizmet Yöneticisi yapılır

> [!IMPORTANT]
> Şu anda Azure Stack sürüm notlarındaki tanımlanan portalında sanal makine ölçek kümeleri ölçeklendirme, ölçeği genişletmek için bu nedenle PowerShell örneğini kullanın mümkün değildir.
>
>

## <a name="add-additional-workers-with-powershell"></a>Ek çalışan PowerShell ile ekleme

1. [PowerShell'de Azure Stack yönetici ortamını ayarlama](azure-stack-powershell-configure-admin.md)

2. Bu örnek, Ölçek kümesini ölçeklendirme kullanın:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRmAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > Bu adım, birkaç rol türü ve örneği sayısını bağlı olarak saat sürebilir.
   >
   >

3. App Service yönetim yeni rol örneklerini durumunu izleme, tek tek rol örneği durumunu denetlemek için rol türü listesinde tıklayın.

## <a name="add-additional-workers-directly-within-the-app-service-resource-provider-admin"></a>App Service kaynak sağlayıcısı yönetici doğrudan içinde ek çalışanları ekleme

1. Azure Stack yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Gözat **uygulama hizmetleri**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Tıklayın **rolleri**. Burada, dağıtılan tüm App Service rolleri dökümünü görürsünüz.

4. Ölçeklendirme ve ardından istediğiniz tür satırına tıklayın sağ **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Tıklayın **ölçeklendirme**, ölçeklendirme ve ardından istediğiniz örnek sayısını seçin **Kaydet**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Azure Stack üzerinde App Service'te şimdi ek VM'ler eklemek, bunları yapılandırmak, tüm gerekli yazılımları yüklemek ve bu işlem tamamlandığında, bunları hazır işaretleyin. Bu işlem yaklaşık 80 dakika sürebilir.

7. Çalışanlar görüntüleyerek yeni roller hazır olma ilerlemesini izleyebilirsiniz **rolleri** dikey penceresi.

## <a name="result"></a>Sonuç

Tam olarak dağıtıldıktan ve kullanıma hazır olduktan sonra çalışan kullanıcıların bunlara iş yükünü dağıtmak kullanılabilir hale gelir. Aşağıda, varsayılan olarak kullanılabilir birden fazla fiyatlandırma katmanında bir örnek gösterilmektedir. Belirli bir çalışan katmanı için kullanılabilir çalışanlar varsa, karşılık gelen bir fiyatlandırma katmanı seçme seçeneği kullanılamaz.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Management ölçeklendirmek için karşılık gelen rol türünün Ölçeklendirmesi gerekir ön uç veya yayımcı rolü ekleyin. 
>
>

Yönetim, ön uç veya yayımcı rollerini ölçeklendirmek için uygun rol türü seçme aynı adımları izleyin. Denetleyicileri ölçek kümeleri dağıtılmaz ve bu nedenle iki tüm üretim dağıtımları için yükleme zamanında dağıtılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar

[Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)
