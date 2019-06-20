---
title: Azure PowerShell veya Resource Manager şablonları kullanarak Vm'leri (Önizleme) için Azure İzleyicisi'ni etkinleştirme | Microsoft Docs
description: Bu makalede, nasıl Azure İzleyici VM'ler için bir olanak açıklanmaktadır veya Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak daha fazla Azure sanal makineler veya sanal makine ölçek ayarlar.
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
ms.openlocfilehash: ff284ea0adf6021ace84cd6a41f0a0e4e987a9c8
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144238"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-powershell-or-resource-manager-templates"></a>Azure İzleyicisi'ni (Önizleme) VM'ler için Azure PowerShell veya Resource Manager şablonlarını kullanarak etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak sanal makine ölçek kümeleri ya da bu makalede, Azure sanal makineler için Azure İzleyici (Önizleme) VM'ler için etkinleştirme açıklanmaktadır. Bu işlemin sonunda, başarılı bir şekilde tüm sanal makinelerinizin izleme başlamıştır ve tüm performans veya kullanılabilirlik sorunları yaşıyorsanız öğrenin.

## <a name="set-up-a-log-analytics-workspace"></a>Bir Log Analytics çalışma alanını ayarlama 

Bir Log Analytics çalışma alanınız yoksa, oluşturmanız gerekir. Konusunda önerilen yöntemleri inceleyin [önkoşulları](vminsights-enable-overview.md#log-analytics) yapılandırmak için adımlara devam etmeden önce bu bölümü. Ardından Azure Resource Manager şablonu yöntemini kullanarak sanal makineler için Azure İzleyici dağıtımını tamamlayabilir.

### <a name="enable-performance-counters"></a>Performans sayaçları sağlar

Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>ServiceMap ve InfrastructureInsights çözümleri yüklemesi
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları dağıtma bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI kullanmak için öncelikle CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Yüklemek veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

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

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntüler:

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

## <a name="enable-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile etkinleştirme
Örnek eklenmesi için Azure Resource Manager şablonları, sanal makineleriniz ve sanal makine ölçek kümeleri oluşturduk. Bu şablonlar, mevcut bir kaynak üzerinde izlenmesini ve izleme etkin olan yeni bir kaynak oluşturmak için kullanabileceğiniz senaryolar içerir.

>[!NOTE]
>Şablon karttaki duruma getirilebilmesi için aynı kaynak grubunda kaynak olarak dağıtılması gerekiyor.

Bir şablon kullanarak kaynakları dağıtma bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI kullanmak için öncelikle CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Yüklemek veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="download-templates"></a>Şablonları indirir

Azure Resource Manager şablonları yapabileceğiniz bir arşiv dosyasına (.zip) sağlanan [indirme](https://aka.ms/VmInsightsARMTemplates) bizim GitHub deposundan. Dosyasının içeriğini, her dağıtım senaryosuna temsil eden bir şablonu ve parametre dosyası içerir. Bunları çalıştırmadan önce parametre dosyasını değiştirebilir ve gereken değerleri belirtin. Belirli gereksinimlerinizi desteklemek için özelleştirmek gerekli olmadıkça şablon dosyasını değiştirmeyin. Parametre dosyasını değiştirdikten sonra bu makalenin sonraki bölümlerinde açıklanan aşağıdaki yöntemleri kullanarak dağıtabilirsiniz. 

İndirme dosyasını farklı senaryolar için aşağıdaki şablonlardan içerir:

- **ExistingVmOnboarding** şablon sanal makine zaten varsa bu Azure İzleyici VM'ler için etkinleştirir.
- **NewVmOnboarding** şablonu bir sanal makine oluşturur ve izlemek sanal makineler için Azure İzleyici sağlar.
- **ExistingVmssOnboarding** şablon sanal makine ölçek kümesini zaten varsa, bu Azure İzleyici VM'ler için etkinleştirir.
- **NewVmssOnboarding** şablonu, sanal makine ölçek kümeleri oluşturur ve bunları izlemek sanal makineler için Azure İzleyici sağlar.
- **ConfigureWorksapce** şablonu çözümler ve Linux ve Windows işletim sistemi performans sayaçlarını toplamayı etkinleştirerek VM'ler için Azure İzleyici desteklemek için Log Analytics çalışma alanınızın yapılandırır.

>[!NOTE]
>Sanal makine ölçek kümeleri mevcut zaten ve yükseltme İlkesi ayarlanmışsa **el ile**, VM'ler için Azure İzleyici için etkinleştirilmesi gerekmez çalıştırdıktan sonra varsayılan örnekleri **ExistingVmssOnboarding** Azure Resource Manager şablonu. Örnekler el ile yükseltmeniz gerekir.

### <a name="deploy-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak dağıtma

Aşağıdaki adım, Azure PowerShell kullanarak izlemeyi etkinleştirir.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```
Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntüler:

```powershell
provisioningState       : Succeeded
```
### <a name="deploy-by-using-the-azure-cli"></a>Azure CLI kullanarak dağıtma

Aşağıdaki adım, Azure CLI kullanarak izlemeyi etkinleştirir.

```azurecli
az login
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

Çıktı aşağıdakine benzer:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-with-powershell"></a>PowerShell ile etkinleştirme

Azure İzleyici VM'ler için birden çok sanal makineleri veya sanal makine ölçek kümeleri için etkinleştirmek için PowerShell betiğini kullanın. [yükleme VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0). Azure PowerShell Galerisi'nden kullanılabilir. Bu komut dosyası aracılığıyla yinelenir:

- Her sanal makine ve sanal makine ölçek kümesi aboneliğinizde.
- Tarafından belirtilen kapsamı belirlenmiş bir kaynak grubu *ResourceGroup*. 
- Tarafından belirtilen tek bir sanal makine veya sanal makine ölçek kümesi *adı*.

Her sanal makine veya sanal makine ölçek kümesi için betik VM uzantısı zaten yüklü olup olmadığını doğrular. VM uzantısı yüklü değilse, yeniden yüklemek betik çalışır. VM uzantısı yüklü değilse, betik Log Analytics ve bağımlılık Aracısı VM uzantılarını yükler.

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
    This script installs or reconfigures the following on VMs and virtual machine scale sets:
    - Log Analytics VM extension configured to supplied Log Analytics workspace
    - Dependency agent VM extension

    Can be applied to:
    - Subscription
    - Resource group in a subscription
    - Specific VM or virtual machine scale set
    - Compliance results of a policy for a VM or VM extension

    Script will show you a list of VMs or virtual machine scale sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed, they will not install again.
    Use -ReInstall switch if you need to, for example, update the workspace.

    Use -WhatIf if you want to see what would happen in terms of installs, what workspace configured to, and status of the extension.


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

    Install for all VMs in a resource group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example, to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source and to reinstall (move to a new workspace)
```

Aşağıdaki örnek, VM'ler için Azure İzleyici etkinleştirmek ve beklenen çıktıyı anlamak için klasörde PowerShell komutlarını kullanarak göstermektedir:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or virtual machine scale sets matching criteria specified

VMs or virtual machine scale sets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on the previous two VMs or virtual machine scale sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example, to update workspace.

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

Already onboarded: (0)

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

Sanal makineleriniz için izlenmesi etkin olduğunda, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir.
 
- Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). 
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). 
- Performans sorunlarını ve sanal makinenin performans genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md). 
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).