---
title: "PowerShell kullanarak Azure Cloud Services tanılamada etkinleştirme | Microsoft Docs"
description: "PowerShell kullanarak bulut Hizmetleri için tanılamayı etkinleştirin öğrenin"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: adegeo
ms.openlocfilehash: 8dd9724981860c9cd4ccc443cc2bfdc465811e7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>PowerShell kullanarak Azure Cloud Services tanılamada etkinleştir
Uygulama günlükleri gibi tanılama verilerini toplama performans sayaçları vb. Azure tanılama uzantısını kullanarak bir bulut hizmetinden. Bu makalede, PowerShell kullanarak bir bulut hizmeti için Azure tanılama uzantısını etkinleştirme açıklar.  Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) Bu makale için gereken önkoşulları için.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Bulut Hizmeti dağıtımının bir parçası olarak tanılama uzantısını etkinleştirme
Bu yaklaşım bulut hizmetine dağıtma bir parçası olarak tanılama uzantısını burada etkinleştirilebilir senaryoları, sürekli tümleştirme türü için geçerlidir. Yeni bir bulut hizmeti dağıtımı oluştururken geçirerek tanılama uzantısını etkinleştirebilirsiniz *ExtensionConfiguration* parametresi [yeni AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet'i. *ExtensionConfiguration* parametresi alan bir dizi kullanılarak oluşturulan tanılama Yapılandırması [yeni AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet'i.

Aşağıdaki örnek, bir WebRole ve her farklı tanılama yapılandırması sahip örn, bir bulut hizmeti için tanılama nasıl etkinleştirebilirsiniz gösterir.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Tanılama yapılandırma dosyası belirtiyorsa bir `StorageAccount` bir depolama hesabı adı bir öğesiyle sonra `New-AzureServiceDiagnosticsExtensionConfig` cmdlet'i otomatik olarak bu depolama hesabı kullanın. Bunun çalışması için depolama hesabı dağıtılan bulut hizmeti ile aynı abonelikte olması gerekiyor.

Azure SDK MSBuild tarafından oluşturulan uzantısı yapılandırma dosyalarını yayımlama 2.6 ileriye doğru hedef çıkış hizmet yapılandırma dosyasında (.cscfg) belirtilen tanılama yapılandırma dizesinde temel depolama hesabı adı içerir. Aşağıdaki komut dosyası Yayımla hedef çıktısından uzantısı yapılandırma dosyalarını ayrıştırma ve bulut hizmeti dağıtırken tanılama uzantısını her rol için yapılandırma gösterilmektedir.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online, bulut hizmetlerinin tanılama uzantısını otomatik dağıtımlar için benzer bir yaklaşım kullanır. Bkz: [Yayımla AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) tam bir örnek için.

Öyle değilse `StorageAccount` olarak geçirmenize gerek sonra tanılama yapılandırmasında belirtilen *StorageAccountName* cmdlet parametresi. Varsa *StorageAccountName* parametresi belirtilirse, ardından cmdlet her zaman parametre ile bir tanılama yapılandırma dosyasında belirtilen belirtilen depolama hesabı kullanır.

Tanılama depolama hesabı bulut hizmetinden farklı bir abonelikte olduğundan sonra içinde açıkça geçirmenize gerek *StorageAccountName* ve *StorageAccountKey* cmdlet parametreleri. *StorageAccountKey* cmdlet, otomatik olarak sorgulamak ve tanılama uzantısını etkinleştirirken anahtar değeri ayarlamak gibi tanılama depolama hesabı aynı abonelikte olduğunda parametresi gerekli değildir. Ancak, tanılama depolama hesabı farklı bir abonelikte cmdlet anahtarını otomatik olarak almak mümkün olmayabilir açıkça gereken ise ve tuşuyla belirtmeniz *StorageAccountKey* parametresi.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Mevcut bir Bulut Hizmetinde tanılama uzantısını etkinleştirme
Kullanabileceğiniz [kümesi AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'ini etkinleştirin veya zaten çalışmakta olan bir bulut hizmeti Tanılama yapılandırmasını güncelleştirin.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Güncel tanılama uzantı yapılandırmasını alma
Kullanım [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) bir bulut hizmeti geçerli Tanılama yapılandırmasını almak için cmdlet.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Tanılama uzantısını kaldırma
Tanılama kullanabileceğiniz bir bulut hizmetini devre dışı bırakmak üzere [Kaldır AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'i.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Kullanarak tanılama uzantısını etkinleştirilirse *kümesi AzureServiceDiagnosticsExtension* veya *yeni AzureServiceDiagnosticsExtensionConfig* olmadan *rol* parametresi sonra kaldırabilir uzantısını kullanarak *Kaldır AzureServiceDiagnosticsExtension* olmadan *rol* parametresi. Varsa *rol* parametresi uzantısı sonra etkinleştirme ayrıca uzantı kaldırılırken kullanılmalıdır kullanıldı.

Tanılama uzantısını her bir rolden kaldırmak için:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Sonraki Adımlar
* Sorunları gidermek için Azure tanılama ve başka teknikler kullanarak ek yönergeler için bkz: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](cloud-services-dotnet-diagnostics.md).
* [Tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) tanılama uzantısını için çeşitli xml yapılandırma seçenekleri açıklanmaktadır.
* Sanal makineler için tanılama uzantısını etkinleştirme hakkında bilgi edinmek için [izleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makine oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md)
