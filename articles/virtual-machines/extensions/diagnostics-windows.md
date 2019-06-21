---
title: Bir Windows VM'de tanılama etkinleştirmek için Azure PowerShell'i kullanma | Microsoft Docs
services: virtual-machines-windows
documentationcenter: ''
description: Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için PowerShell kullanma hakkında bilgi edinin
author: sbtron
manager: jeconnoc
editor: ''
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: bd2bcc9284c24f9fa6a02556d7101c1b788ee71e
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154993"
---
# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Windows çalıştıran bir sanal makinede PowerShell kullanarak Azure Tanılama’yı etkinleştirme

Azure Tanılama, azure'da dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlayan özelliktir. Bir Azure sanal Windows çalıştıran makineden (VM) uygulama günlükleri ya da performans sayaçları gibi tanılama verilerini toplamak için tanılama uzantısı'nı kullanabilirsiniz. 

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Resource Manager dağıtım modeli kullandığınız tanılama uzantısını etkinleştirme
Azure Resource Manager dağıtım modeliyle bir Windows VM uzantısı yapılandırma için Resource Manager şablonu ekleyerek oluştururken tanılama uzantısını etkinleştirebilirsiniz. Bkz: [Azure Resource Manager şablonu kullanarak izleme ve tanılama özellikli bir Windows sanal makine oluşturma](diagnostics-template.md).

Resource Manager dağıtım modeliyle oluşturulmuş olan mevcut VM'de tanılama uzantısını etkinleştirmek için kullanabileceğiniz [kümesi AzVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiagnosticsextension) aşağıda gösterildiği gibi PowerShell cmdlet'i.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* açıklandığı gibi XML'de tanılama yapılandırması içeren dosyanın yolu olan [örnek](#sample-diagnostics-configuration) aşağıda.  

Tanılama yapılandırma dosyası belirtiyorsa bir **StorageAccount** öğesi bir depolama hesabı adı ile sonra *kümesi AzVMDiagnosticsExtension* betik otomatik olarak Tanılama'yı ayarlama Bu depolama hesabına Tanılama verileri gönder uzantısı. Bunun işe yaraması için depolama hesabı VM ile aynı abonelikte olması gerekiyor.

Hayır ise **StorageAccount** olarak geçirmenize gerek sonra tanılama Yapılandırması'nda belirtilen *StorageAccountName* cmdlet'e parametre. Varsa *StorageAccountName* parametresi belirtilmediyse, ardından cmdlet her zaman bir tanılama yapılandırma dosyasında belirtilen ve bir parametre içinde belirtilen depolama hesabı kullanır.

Tanılama depolama hesabı, sanal makineden farklı bir abonelikte olduğundan sonra açıkça geçirin gerek *StorageAccountName* ve *StorageAccountKey* cmdlet parametreleri. *StorageAccountKey* cmdlet, otomatik olarak sorgulamak ve tanılama uzantısı etkinleştirilirken anahtar değeri ayarlamak gibi tanılama depolama hesabı aynı abonelikte değilse parametresi gerekli değildir. Ancak, anahtar aracılığıyla tanılama depolama hesabı farklı bir abonelikte olduğundan sonra cmdlet anahtarını otomatik olarak almak mümkün olmayabilir ve açıkça ihtiyacınız varsa belirtmeniz *StorageAccountKey* parametresi.  

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Bir VM'de tanılama uzantısını etkinleştirildikten sonra geçerli ayarları kullanarak alabilirsiniz [Get-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiagnosticsextension) cmdlet'i.

    Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Cmdlet döndürür *PublicSettings*, tanılama yapılandırması içerir. Desteklenen yapılandırma, WadCfg ve xmlCfg iki tür vardır. WadCfg JSON yapılandırmadır ve xmlCfg Base64 ile kodlanmış bir biçimde XML yapılandırması. XML okuma için çözülmesi gerekir.

    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

[Remove-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdiagnosticsextension) cmdlet'i, VM tanılama uzantısını kaldırmak için kullanılabilir.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Klasik dağıtım modeli kullandığınız tanılama uzantısını etkinleştirme
Kullanabileceğiniz [kümesi AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet'ini bir Klasik dağıtım modeliyle oluşturulan bir VM'de tanılama uzantısını etkinleştirme. Aşağıdaki örnek, yeni bir sanal makine Klasik dağıtım modeliyle tanılama uzantısı ile oluşturma işlemi gösterilmektedir.

    $VM = New-AzVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzVM -Location $Location -ServiceName $Service_Name -VM $VM

Klasik dağıtım modeliyle oluşturulmuş olan mevcut VM'de tanılama uzantısını etkinleştirmek için önce kullanın [Get-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurevm) cmdlet'i, VM yapılandırması alınamıyor. Sonra tanılama uzantısını kullanarak eklemek için sanal makine yapılandırmasını güncelleştirme [kümesi AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet'i. Son olarak, güncelleştirilmiş yapılandırmayı kullanarak sanal Makineye uygulayın [güncelleştirme-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/update-azurevm).

    $VM = Get-AzVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Örnek tanılama yapılandırması
Aşağıdaki XML, yukarıdaki betiklerle ortak tanılama yapılandırması için kullanılabilir. Bu örnek yapılandırmanın çeşitli performans sayaçlarını yanı sıra uygulama, güvenlik ve sistem kanalları Windows olay günlüklerindeki hatalarını ve her türlü hata Tanılama Altyapısı günlükleri tanılama depolama hesabına aktarın.

Yapılandırma şunlar için güncelleştirilmesi gerekiyor:

* *ResourceId* özniteliği **ölçümleri** öğesi kaynak Kimliğine sahip sanal makine için güncelleştirilmesi gerekir.
  
  * Kaynak Kimliği şu deseni kullanılarak oluşturulabilir: "/ subscriptions / {*VM ile abonelik kimliği için abonelik*} /resourceGroups/ {*VM için kaynak grubu adı*} / providers/Microsoft.Compute/virtualMachines/ {*VM adı*} ".
  * Örneğin, sanal Makinenin çalıştığı abonelik için abonelik kimliği ise **11111111-1111-1111-1111-111111111111**, kaynak grubu için kaynak grubu adı **MyResourceGroup**ve VM adı **MyWindowsVM**, ardından değeri *ResourceId* olacaktır:
    
      ```xml
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Hakkında daha fazla bilgi için ölçümler performans sayaçlarını ve ölçümleri yapılandırmasını temel alarak oluşturulur, bkz: [Azure tanılama ölçümleri tablo depolamada](diagnostics-template.md#wadmetrics-tables-in-storage).
* **StorageAccount** öğesi tanılama depolama hesabı adı ile güncelleştirilmesi gerekiyor.
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Sonraki adımlar
* Sorunlarını gidermek için Azure tanılama özelliği ve diğer teknikleri kullanma ile ilgili ek kılavuzlar için bkz: [Azure bulut Hizmetleri ve sanal Makineler'de tanılamayı etkinleştirme](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Tanılama yapılandırmalarını şeması](https://msdn.microsoft.com/library/azure/mt634524.aspx) tanılama uzantısı için çeşitli XML yapılandırma seçeneklerini açıklar.

