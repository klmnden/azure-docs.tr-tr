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
ms.openlocfilehash: 680cb70777574d0ed88c5f83fb0a6fa20263b951
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
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
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > Bu adım, rolün türü ve örnek sayısına bağlı olarak tamamlamak için saat sayısını alabilir.
   >
   >

3. Yeni Uygulama Hizmeti Yönetimi rol örneklerinin durumunu izlemek, tek rol örneğini durumunu denetlemek için listede rol türü'ı tıklatın.

## <a name="add-additional-workers-directly-within-the-app-service-resource-provider-admin"></a>Uygulama hizmeti kaynak sağlayıcısı yönetici doğrudan içinde ek çalışanları ekleme

1. Azure yığın yönetim portalına Hizmet Yöneticisi olarak oturum açın.

2. Gözat **uygulama hizmetleri**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Tıklatın **rolleri**. Burada, dağıtılan tüm uygulama hizmeti rolleri dökümünü görürsünüz.

4. Ölçek ve ardından istediğiniz türü satırındaki sağ tıklayın **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Tıklatın **ölçeklendirme**, üzere ölçek ve ardından istediğiniz örneklerinin sayısını seçin **kaydetmek**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Azure yığın uygulama hizmeti şimdi ilave VM'ler eklemek, bunları yapılandırmanız, tüm gerekli yazılımları yüklemek ve bu işlem tamamlandıktan sonra bunları hazır olarak işaretlemek. Bu işlem yaklaşık 80 dakika sürebilir.

7. Çalışanlar görüntüleyerek yeni rolleri hazırlık ilerlemesini izleyebilirsiniz **rolleri** dikey.

## <a name="result"></a>Sonuç

Tam olarak dağıtılan ve hazır olduktan sonra çalışan kullanıcılar bunlara kendi iş yükü dağıtmak için kullanılabilir hale gelir. Aşağıdaki varsayılan olarak kullanılabilir birden çok fiyatlandırma katmanlarına örneği gösterir. Kullanılabilir hiçbir çalışanları belirli çalışan katmanı için varsa, karşılık gelen fiyatlandırma katmanını seçmesine izin seçeneği kullanılamaz.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Management'ı ölçeklendirmek için karşılık gelen rol türü ölçeklendirme ön uç veya yayımcı rolü ekleyin. 
>
>

Yönetim, ön uç veya yayımcı rollerini ölçeklendirmek için uygun rol türü seçme aynı adımları izleyin. Denetleyicileri ölçek kümeleri olarak dağıtılmaz ve bu nedenle iki tüm üretim dağıtımları için yükleme zamanında dağıtılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar

[Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)
