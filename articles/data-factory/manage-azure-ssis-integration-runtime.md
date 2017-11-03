---
title: "Azure SSIS tümleştirmesi çalışma zamanı yönetme | Microsoft Docs"
description: "Zaten sağlanmış sonra Azure Data Factory Azure SSIS tümleştirmesi çalışma zamanı yeniden öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: spelluru
ms.openlocfilehash: cc0ed958a9e1018ed9f06fdcc94873ae5420ba95
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="manage-an-azure-ssis-integration-runtime"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı yönetme
[Azure SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md) makale Azure Data Factory kullanarak bir Azure SSIS tümleştirmesi çalışma zamanı oluşturulacağını gösterir. Bu makalede durdurun, başlatın, yeniden yapılandırın veya bir Azure SSIS tümleştirmesi çalışma zamanı kaldırma konusunda bilgi sağlayarak tamamlar.  

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

## <a name="stop"></a>Durdur 
Azure SSIS tümleştirmesi çalışma zamanı durdurun. Bu komut tüm düğümlerinin serbest bırakır ve faturalama durdurur.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
```

## <a name="start"></a>Başlatma 
Azure SSIS tümleştirmesi çalışma zamanı başlatın. Bu komut tüm düğümlerinin ayırır ve faturalama başlatır.   

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
```

## <a name="reconfigure"></a>Yeniden yapılandırın
Sağlamak ve bir örneğini Azure SSIS Integration zamanının başlatmak sonra onu bir dizi çalıştırarak yeniden yapılandırabilirsiniz `Stop`  -  `Set`  -  `Start` PowerShell cmdlet öğelerini ardışık. Örneğin, aşağıdaki PowerShell betiğini 5 Azure SSIS tümleştirmesi çalışma zamanı örneği için ayrılan düğüm sayısını değiştirir.

1. İlk olarak, aşağıdaki komutu çalıştırarak Azure SSIS tümleştirmesi çalışma zamanı durdurun:

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Şimdi, Ölçek beş düğümü, Azure SSIS tümleştirme çalışma zamanında genişletme.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Ardından, Azure SSIS tümleştirmesi çalışma zamanı başlatın. Bu tüm düğümlerinin SSIS paketleri çalıştırmak için ayırır.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

## <a name="remove"></a>Kaldır
Var olan bir Azure SSIS tümleştirmesi çalışma zamanı kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
```

## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-deploy-ssis-packages-azure.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (özel önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına (VNet) ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımları sunar. 
