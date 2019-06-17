---
title: VM'ler için (Önizleme), Azure PowerShell veya Resource Manager şablonu kullanarak Azure İzleyicisi'ni etkinleştirme | Microsoft Docs
description: Bu makalede, bir veya daha fazla Azure sanal makineler veya Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak sanal makine ölçek kümeleri için Azure İzleyici Vm'leri için nasıl olanak açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: a22bc88fb066d9b845f7fdf1592e2194a03915bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65524138"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-powershell-or-resource-manager-template"></a>Azure İzleyicisi'ni (Önizleme) VM'ler için Azure PowerShell veya Resource Manager şablonu kullanarak etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure İzleyici (Önizleme) VM'ler için Azure sanal makineler veya Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak sanal makine ölçek kümeleri için etkinleştirmek açıklanmaktadır. Bu işlem, başarıyla başladı sonunda tüm sanal makinelerinizin izleme ve tüm performans veya kullanılabilirlik sorunları yaşıyorsanız öğrenin. 

## <a name="set-up-a-log-analytics-workspace"></a>Bir Log Analytics çalışma alanını ayarlama 

Bir Log Analytics çalışma alanınız yoksa, oluşturun, önerilen yöntemleri inceleyerek [önkoşulları](vminsights-enable-overview.md#log-analytics) bölümü sürdürebilmeniz için Azure İzleyici dağıtımını tamamlamak için yapılandırma adımları Azure Resource Manager şablonu yöntemi kullanarak Vm'leri.

### <a name="enable-performance-counters"></a>Performans sayaçları sağlar

Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>ServiceMap ve InfrastructureInsights çözümleri yüklemesi
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları dağıtma ile bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Bu dosyayı farklı Kaydet *installsolutionsforvminsights.json* yerel bir klasöre.

1. Değerleri yakalama *WorkspaceName*, *ResourceGroupName*, ve *WorkspaceLocation*. Değeri *WorkspaceName* Log Analytics çalışma alanınızın adıdır. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.

1. Bu şablonu dağıtmaya hazırsınız.
 
    * Şablonu içeren klasörde aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Azure CLI kullanarak aşağıdaki komutu çalıştırmak için:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-with-azure-resource-manager-template"></a>Azure Resource Manager şablonu ile etkinleştirme
Örnek eklenmesi için Azure Resource Manager şablonları, sanal makineleriniz ve sanal makine ölçek kümeleri oluşturduk. Bu şablonlar, mevcut bir kaynak üzerinde izlemeyi etkinleştirme ve izleme etkin olan yeni bir kaynak oluşturmak için senaryolar içerir.

>[!NOTE]
>Şablon yapılacak aynı kaynak grubunda kaynak olarak dağıtılması gerekiyor.

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="download-templates"></a>Şablonları indirir

Azure Resource Manager şablonları yapabileceğiniz bir arşiv dosyasına (.zip) sağlanan [indirme](https://aka.ms/VmInsightsARMTemplates) bizim GitHub deposundan. Dosyanın içeriği her dağıtım senaryosuna temsil eden bir şablonu ve parametre dosyasıyla klasörleri içerir. Çalıştırmadan önce parametre dosyasını değiştirebilir ve gereken değerleri belirtin. Belirli gereksinimlerinizi desteklemek için özelleştirmek gerekli olmadıkça şablon dosyasını değiştirmeyin. Parametre dosyasını değiştirdikten sonra bu makalenin sonraki bölümlerinde açıklanan aşağıdaki yöntemleri kullanarak dağıtabilirsiniz. 

İndirme dosyasını farklı senaryolar için aşağıdaki şablonlardan içerir:

- **ExistingVmOnboarding** şablon sanal makineler zaten varsa bu Azure İzleyici VM'ler için etkinleştirir.
- **NewVmOnboarding** şablonu bir sanal makine oluşturur ve izlemek sanal makineler için Azure İzleyici sağlar.
- **ExistingVmssOnboarding** şablon sanal makine ölçek kümesini zaten varsa, bu Azure İzleyici VM'ler için etkinleştirir.
- **NewVmssOnboarding** şablonu bir sanal makine ölçek kümesi oluşturur ve bunları izlemek sanal makineler için Azure İzleyici sağlar.
- **ConfigureWorksapce*** şablonu çözümler ve Linux ve Windows işletim sistemi performans sayaçlarını toplamayı etkinleştirerek VM'ler için Azure İzleyici desteklemek için Log Analytics çalışma alanınızın yapılandırır.

>[!NOTE]
>Sanal makine ölçek kümeleri mevcut zaten ve yükseltme İlkesi ayarlanmışsa **el ile**, VM'ler için Azure İzleyici etkin olmayabilir için örnekleri çalıştırdıktan sonra varsayılan olarak **ExistingVmssOnboarding** Azure Resource Manager şablonu. Örnekler el ile yükseltmeniz gerekir.

### <a name="deploy-using-azure-powershell"></a>Azure PowerShell’i kullanarak dağıtma

Aşağıdaki adım, Azure PowerShell kullanarak izlemeyi etkinleştirir.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```
Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

```powershell
provisioningState       : Succeeded
```
### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtma

Aşağıdaki adım, Azure CLI kullanarak izlemeyi etkinleştirir.   

```azurecli
az login
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

Çıktı şuna benzer:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-with-powershell"></a>PowerShell ile etkinleştirme

Azure İzleyici VM'ler için birden çok sanal makineleri veya sanal makine ölçek kümeleri için etkinleştirmek için PowerShell betiğini kullanabilirsiniz [yükleme VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), Azure PowerShell Galerisi kullanılabilir. Bu betik, aboneliğinizdeki tarafından belirtilen kapsamı belirlenmiş bir kaynak grubundaki her sanal makine ve sanal makine ölçek kümesi gezinir *ResourceGroup*, ya da tarafından belirtilen tek bir sanal makine veya sanal makine ölçek kümesi *Adı*. Her sanal makine veya sanal makine ölçek kümesi için betik VM uzantısı zaten yüklü olup olmadığını doğrular. VM uzantısı yüklü değilse, yeniden yüklemek betik çalışır. VM uzantısı yüklü değilse, betik Log Analytics ve bağımlılık Aracısı VM uzantılarını yükler.

Bu betik, Azure PowerShell modülü Az sürüm 1.0.0 gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için.

Betik bağımsız değişkeni ayrıntıları ve örnek kullanım listesini almak için çalıştırın `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

Aşağıdaki örnek, VM'ler için Azure İzleyici etkinleştirmek ve beklenen çıktıyı anlamak için klasörde PowerShell komutlarını kullanarak göstermektedir:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izlenmesi etkin olduğunda, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). Performans sorunlarını ve Vm'leri performansınızı ile genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).