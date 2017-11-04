---
title: "Bir Windows VM üzerinde tanılamayı etkinleştirmek için Azure PowerShell kullanın | Microsoft Docs"
services: virtual-machines-windows
documentationcenter: 
description: "Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için PowerShell kullanma hakkında bilgi edinin"
author: sbtron
manager: timlt
editor: 
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: d0be4a712657edfc516c5f32e66519f5d9486728
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Windows çalıştıran bir sanal makinede PowerShell kullanarak Azure Tanılama’yı etkinleştirme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure tanılama dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Bir Azure sanal Windows çalıştıran makineden (VM) uygulama günlüklerini veya performans sayaçları gibi tanılama verilerini toplamak için tanılama uzantısını kullanabilirsiniz. Bu makalede, bir VM için tanılama uzantısını etkinleştirmek için Windows PowerShell kullanmayı açıklar. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) Bu makale için gereken önkoşulları için.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Resource Manager dağıtım modeli kullanırsanız tanılama uzantısını etkinleştirme
Azure Resource Manager dağıtım modeli üzerinden Windows VM uzantısı yapılandırma Resource Manager şablonu ekleyerek oluştururken tanılama uzantısını etkinleştirebilirsiniz. Bkz: [Azure Resource Manager şablonunu kullanarak izleme ve Tanılama ile Windows sanal makine oluşturma](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Resource Manager dağıtım modeli oluşturulmuş mevcut bir VM'yi tanılama uzantısını etkinleştirmek için kullanabileceğiniz [kümesi AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell cmdlet'ini aşağıda gösterildiği gibi.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* açıklandığı gibi XML'de tanılama yapılandırması içeren dosyanın yolu olan [örnek](#sample-diagnostics-configuration) aşağıda.  

Tanılama yapılandırma dosyası belirtiyorsa bir **StorageAccount** bir depolama hesabı adı bir öğesiyle sonra *kümesi AzureRMVMDiagnosticsExtension* komut dosyası otomatik olarak Tanılama'yı ayarlama Bu depolama hesabına Tanılama verileri gönder uzantısı. Bunun çalışması için depolama hesabı VM ile aynı abonelikte olması gerekiyor.

Öyle değilse **StorageAccount** olarak geçirmenize gerek sonra tanılama yapılandırmasında belirtilen *StorageAccountName* cmdlet parametresi. Varsa *StorageAccountName* parametresi belirtilirse, ardından cmdlet her zaman parametre ile bir tanılama yapılandırma dosyasında belirtilen belirtilen depolama hesabı kullanır.

Sanal makineden farklı bir abonelik tanılama depolama hesabı bulunduğu sonra içinde açıkça geçirmenize gerek *StorageAccountName* ve *StorageAccountKey* cmdlet parametreleri. *StorageAccountKey* cmdlet, otomatik olarak sorgulamak ve tanılama uzantısını etkinleştirirken anahtar değeri ayarlamak gibi tanılama depolama hesabı aynı abonelikte olduğunda parametresi gerekli değildir. Ancak, tanılama depolama hesabı farklı bir abonelikte cmdlet anahtarını otomatik olarak almak mümkün olmayabilir açıkça gereken ise ve tuşuyla belirtmeniz *StorageAccountKey* parametresi.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Tanılama uzantısını VM üzerinde etkinleştirildikten sonra geçerli ayarları kullanarak alabileceğiniz [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet'i.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Cmdlet döndürür *PublicSettings*, tanılama yapılandırması içerir. Desteklenen yapılandırma, WadCfg ve xmlCfg iki tür vardır. WadCfg JSON yapılandırması ve xmlCfg XML yapılandırması Base64 ile kodlanmış bir biçimde değil. XML okumak için çözülmesi gerekir.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

[Kaldır AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) cmdlet, VM'den tanılama uzantısını kaldırmak için kullanılabilir.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanırsanız tanılama uzantısını etkinleştirme
Kullanabileceğiniz [kümesi AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet Klasik dağıtım modeli aracılığıyla oluşturduğunuz bir VM'de tanılama uzantısını etkinleştirme. Aşağıdaki örnek tanılama uzantısı ile klasik dağıtım modeli aracılığıyla yeni bir VM oluşturulacağını gösterir.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Klasik dağıtım modeli aracılığıyla oluşturulmuş mevcut bir VM'yi tanılama uzantısını etkinleştirmek için önce kullanın [Get-AzureVM](/powershell/module/azure/get-azurevm) VM yapılandırmasını almak için cmdlet. Tanılama uzantısını kullanarak eklemek için VM yapılandırması güncelleştirme [kümesi AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet'i. Son olarak, güncelleştirilmiş yapılandırmayı kullanarak VM uygulamak [güncelleştirme-AzureVM](/powershell/module/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Örnek tanılama yapılandırması
Aşağıdaki XML, yukarıdaki komut dosyalarıyla tanılama genel yapılandırması için kullanılabilir. Bu örnek yapılandırmanın çeşitli performans sayaçları tanılama depolama hesabı, uygulama, güvenlik ve Windows olay günlüklerini kanallarında sistem hatalarını ve tanılama altyapı günlüklerinden hataları birlikte aktarın.

Yapılandırma aşağıdaki içerecek şekilde güncelleştirilmesi gerekir:

* *ResourceId* özniteliği **ölçümleri** öğesi kaynak Kimliğiyle VM için güncelleştirilmesi gerekiyor.
  
  * Kaynak Kimliği şu biçimi kullanarak oluşturulabilir: "/ subscriptions / {*VM ile abonelik kimliği için abonelik*} /resourceGroups/ {*VM için kaynak grubu adı*} / providers/Microsoft.Compute/virtualMachines/ {*VM adını*} ".
  * Örneğin, VM çalıştığı abonelik için abonelik kimliği ise **11111111-1111-1111-1111-111111111111**, kaynak grubu için kaynak grubu adı **MyResourceGroup**ve VM adı **MyWindowsVM**, ardından değeri *ResourceId* olacaktır:
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Hakkında daha fazla bilgi için ölçümleri performans sayaçları ve ölçümleri yapılandırmasını temel alarak oluşturulur, bkz: [depolama Azure tanılama ölçümleri tablosuna](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).
* **StorageAccount** öğesi tanılama depolama hesabı adı ile güncelleştirilmesi gerekiyor.
  
    ```
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
* Sorunları gidermek için Azure tanılama yetenek ve başka teknikler kullanarak ek yönergeler için bkz: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Tanılama yapılandırmaları şema](https://msdn.microsoft.com/library/azure/mt634524.aspx) tanılama uzantısını için çeşitli XML yapılandırma seçenekleri açıklanmaktadır.

