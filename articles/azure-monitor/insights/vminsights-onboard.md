---
title: Azure İzleyici (Önizleme) VM'ler için dağıtma | Microsoft Docs
description: Bu makalede nasıl dağıtıp dağıtılmış uygulamanızı nasıl performans gösterdiğini anlamak başlatabilmeniz, VM'ler için Azure İzleyici yapılandırın ve hangi durumu sorunları belirlenmiştir açıklar.
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
ms.date: 03/13/2019
ms.author: magoedte
ms.openlocfilehash: 1a4bfae22477e345176971bd40b0afa91c8867fb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885834"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Azure İzleyici (Önizleme) VM'ler için dağıtma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure İzleyici ' VM'ler için ayarlanacak açıklar. Hizmet, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümeleri ve sanal makineleri ortamınızda işletim sistemi durumunu izler. Bu izleme, bulma ve bunlar üzerinde barındırılabilir uygulama bağımlılıkları eşleme içerir. 

VM'ler için Azure İzleyici aşağıdaki yöntemlerden birini kullanarak etkinleştirin:

* Tek bir Azure sanal makine seçerek etkinleştirin **Insights (Önizleme)** doğrudan VM'den.
* İki veya daha fazla Azure sanal makine, Azure İlkesi'ni kullanarak etkinleştirin. Bu yöntem kullanılarak, mevcut ve yeni VM'lerin gerekli bağımlılıkları yüklü ve düzgün yapılandırılmış. Bunları etkinleştirmek ve düzeltmek istiyorsanız nasıl karar verebilmek için uyumlu olmayan Vm'leri raporlanır.
* İki etkinleştirmek veya PowerShell kullanarak belirtilen abonelik veya kaynak grubu üzerinde daha fazla Azure sanal makineleri veya sanal makine ölçek kümeleri.

Her yöntem hakkında ek bilgi makalenin sonraki bölümlerinde sağlanır.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki bölümlerde yer alan bilgiler anladığınızdan emin olun.

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure İzleyici, bir Log Analytics çalışma alanı şu bölgelerde destekler:

- Batı Orta ABD
- Doğu ABD
- Kanada orta<sup>1</sup>
- UK Güney<sup>1</sup>
- Batı Avrupa
- Güneydoğu Asya<sup>1</sup>

<sup>1</sup> VM'ler için bu bölgede şu anda Azure İzleyici'nın sistem durumu özelliğini desteklemiyor.

>[!NOTE]
>Azure sanal makineleri, herhangi bir bölgeden dağıtılabilir ve Log Analytics çalışma alanı için desteklenen bölgelerin sınırlı değildir.
>

Bir çalışma alanınız yoksa, aşağıdaki yöntemlerden biriyle oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalı](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Azure portalında tek bir Azure VM için izlemeyi etkinleştirme, bu işlem sırasında bir çalışma alanı oluşturabilirsiniz.

Ölçekli senaryosu için çözümü etkinleştirmek için Log Analytics çalışma alanınızda önce aşağıdakileri yapılandırın:

* ServiceMap ve InfrastructureInsights çözümlerini yükleyin. Bu yükleme, yalnızca bu makalede sağlanan bir Azure Resource Manager şablonu kullanarak tamamlayabilirsiniz.
* Performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırın.

Çalışma alanınız ölçekli senaryo için yapılandırmak için bkz ölçekli dağıtımı için Log Analytics çalışma alanını ayarlama.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, VM'ler için Azure İzleyici ile desteklenen Windows ve Linux işletim sistemleri listelenmiştir. Büyük ve küçük Linux işletim sistemi sürüm ayrıntıları ve çekirdek sürümleriyle desteklenen tam listesi daha sonra bu bölümde sağlanır.

|İşletim sistemi sürümü |Performans |Haritalar |Durum |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14.04, 16.04, 18.04 | X | X | X |
|CentOS Linux 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11, 12 | X | X | X |
|Debian 8, 9.4 sürümünden | X<sup>1</sup> | | X |

<sup>1</sup> VM'ler için Azure İzleyici performans özelliği yalnızca Azure İzleyici'deki kullanılabilir. Doğrudan Azure VM'nin sol bölmeden eriştiğinizde kullanılamaz.

>[!NOTE]
>Aşağıdaki bilgiler, Linux işletim sistemini desteklemek için geçerlidir:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel Adres Uzantısı (PAE) ve Xen, desteklenmeyen bir Linux dağıtımı için gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle *2.6.16.21-0.8-xen* desteklenmiyor.
> - Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez.
> - CentOSPlus çekirdek desteklenir.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus
| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| Ubuntu 18.04 | Çekirdek 4.15. * |
| Ubuntu 16.04.3 | Çekirdek 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|11 SP4 | 3.0.* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 kuruluş sunucusu

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency aracı
Vm'leri Haritası özelliği için Azure İzleyici verilerini Microsoft Dependency Aracıdan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Bu nedenle, sisteminizde yüklü ve yapılandırılmış bağımlılık aracısını Log Analytics aracısını olması gerekir.

Azure İzleyici, VM'ler için tek bir Azure VM için etkinleştirmek ister ölçekli dağıtım yöntemini kullanmak, deneyiminin bir parçası aracıyı yüklemek için Azure VM bağımlılık Aracısı uzantısı kullanmanız gerekir.

Karma bir ortamda, indirin ve iki yoldan biriyle bağımlılık aracısını yükleme: El ile veya sanal makineler için bir otomatik dağıtım yöntemi kullanarak, Azure'un dışında barındırılır.

Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | Ek olarak [Windows için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Windows aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Ek olarak [Linux için Log Analytics aracısını](../../azure-monitor/platform/log-analytics-agent.md), Linux aracıları Microsoft Dependency Aracısı gerektirir. İşletim sistemlerinin tam listesi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık Aracısı'nı aşağıdaki konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Etkinleştirmek ve VM'ler için Azure İzleyici'nın özelliklerine erişmek için aşağıdaki erişim rolleri atanmış olması gerekir:

- Çözümü etkinleştirmek için olmalıdır *Log Analytics katkıda bulunan* rol.

- Performans, sistem durumu, görüntüleme ve harita verileri için olmalıdır *izleme okuyucusu* Azure VM rolü. Log Analytics çalışma alanı için Azure İzleyici VM'ler için yapılandırılmış olması gerekir.

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi etkinleştir
Azure portalında Azure sanal makinenizin izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **sanal makineler**.

1. Listeden bir VM seçin.

1. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)**.

1. Üzerinde **Insights (Önizleme)** sayfasında **şimdi deneyin**.

    ![Bir VM için sanal makineler için Azure İzleyici etkinleştir](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. Üzerinde **Azure İzleyici İçgörüler ekleme** sayfasında mevcut bir Log Analytics varsa, aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Listenin varsayılan çalışma alanı ve sanal makine abonelikte dağıtılmış konumunu belirler. 

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md) daha önce desteklenen bölgelerden birinde listelenir.

İzleme etkinleştirdikten sonra sanal makine için sistem durumu ölçümleri görmeden önce yaklaşık 10 dakika sürebilir.

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Uygun ölçekte dağıtın
Bu bölümde, Azure İzleyici VM'ler için uygun ölçekte Azure İlkesi veya Azure PowerShell kullanarak dağıtırsınız.

Sanal makinelerinizi dağıtmadan önce aşağıdakileri yaparak Log Analytics çalışma alanınızın önceden yapılandırın:

1. Bir çalışma alanı yoksa sanal makineler için Azure İzleyicisi'ni destekleyen bir tane oluşturun.  
    Devam etmeden önce bkz [çalışma alanlarını yönetme](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json) maliyeti, yönetim ve uyumluluk için yapılacak değerlendirmeleri anlamaktır.

1. Yeni bir tane zaten, mevcut değilse çalışma alanı, VM'ler için Azure İzleyici desteklemek için kullanılabilir. Gözden geçirme [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) devam etmeden önce maliyeti, yönetim ve uyumluluk konuları anlamak için yeni bir çalışma alanı oluşturmadan önce.

1. Çalışma alanı koleksiyonu Linux ve Windows Vm'leri için performans sayaçları sağlar.

1. Yükleme ve çalışma alanınızdaki ServiceMap ve InfrastructureInsights çözümü etkinleştirin.

### <a name="set-up-a-log-analytics-workspace"></a>Bir Log Analytics çalışma alanını ayarlama
Bir Log Analytics çalışma alanınız yoksa, oluşturun, önerilen yöntemleri inceleyerek ["Önkoşullar"](#log-analytics) bölümü.

#### <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>ServiceMap ve InfrastructureInsights çözümleri yüklemesi
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

### Enable by using Azure Policy
To enable Azure Monitor for VMs at scale in a way that helps ensure consistent compliance and the automatic enabling of the newly provisioned VMs, we recommend [Azure Policy](../../governance/policy/overview.md). These policies:

* Deploy the Log Analytics agent and the Dependency agent.
* Report on compliance results.
* Remediate for non-compliant VMs.

To enable Azure Monitor for VMs by using Azure Policy in your tenant:

- Assign the initiative to a scope: management group, subscription, or resource group
- Review and remediate compliance results

For more information about assigning Azure Policy, see [Azure Policy overview](../../governance/policy/overview.md#policy-assignment) and review the [overview of management groups](../../governance/management-groups/index.md) before you continue.

The policy definitions are listed in the following table:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Enable Azure Monitor for VMs |Enable Azure Monitor for the Virtual Machines (VMs) in the specified scope (management group, subscription, or resource group). Takes Log Analytics workspace as a parameter. |Initiative |
|[Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Linux VMs |Deploy Dependency Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Windows VMs |Deploy Dependency Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Linux VMs |Deploy Log Analytics Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Windows VMs |Deploy Log Analytics Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |

Standalone policy (not included with the initiative) is described here:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Audit Log Analytics Workspace for VM - Report Mismatch |Report VMs as non-compliant if they aren't logging to the Log Analytics workspace specified in the policy/initiative assignment. |Policy |

#### Assign the Azure Monitor initiative
With this initial release, you can create the policy assignment only in the Azure portal. To understand how to complete these steps, see [Create a policy assignment from the Azure portal](../../governance/policy/assign-policy-portal.md).

1. To launch the Azure Policy service in the Azure portal, select **All services**, and then search for and select **Policy**.

1. In the left pane of the Azure Policy page, select **Assignments**.  
    An assignment is a policy that has been assigned to take place within a specific scope.
    
1. At the top of the **Policy - Assignments** page, select **Assign Initiative**.

1. On the **Assign Initiative** page, select the **Scope** by clicking the ellipsis (...), and select a management group or subscription.  
    In our example, a scope limits the policy assignment to a grouping of virtual machines for enforcement.
    
1. At the bottom of the **Scope** page, save your changes by selecting **Select**.

1. (Optional) To remove one or more resources from the scope, select **Exclusions**.

1. Select the **Initiative definition** ellipsis (...) to display the list of available definitions, select **[Preview] Enable Azure Monitor for VMs**, and then select **Select**.  
    The **Assignment name** box is automatically populated with the initiative name you selected, but you can change it. You can also add an optional description. The **Assigned by** box is automatically populated based on who is logged in, and this value is optional.
    
1. In the **Log Analytics workspace** drop-down list for the supported region, select a workspace.

   > [!NOTE]
   > If the workspace is beyond the scope of the assignment, grant *Log Analytics Contributor* permissions to the policy assignment's Principal ID. If you don't do this, you might see a deployment failure such as: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...`
   > To grant access, review [how to manually configure the managed identity](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  The **Managed Identity** check box is selected, because the initiative being assigned includes a policy with the *deployIfNotExists* effect.
    
1. In the **Manage Identity location** drop-down list, select the appropriate region.

1. Select **Assign**.

#### Review and remediate the compliance results

You can learn how to review compliance results by reading [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). In the left pane, select **Compliance**, and then locate the **[Preview] Enable Azure Monitor for VMs** initiative for VMs that aren't compliant according to the assignment you created.

![Policy compliance for Azure VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Based on the results of the policies included with the initiative, VMs are reported as non-compliant in the following scenarios:

* Log Analytics or the Dependency agent isn't deployed.  
    This scenario is typical for a scope with existing VMs. To mitigate it, deploy the required agents by [creating remediation tasks](../../governance/policy/how-to/remediate-resources.md) on a non-compliant policy.  
    - [Preview]: Deploy Dependency Agent for Linux VMs
    - [Preview]: Deploy Dependency Agent for Windows VMs
    - [Preview]: Deploy Log Analytics Agent for Linux VMs
    - [Preview]: Deploy Log Analytics Agent for Windows VMs

* VM Image (OS) isn't identified in the policy definition.  
    The criteria of the deployment policy include only VMs that are deployed from well-known Azure VM images. Check the documentation to see whether the VM OS is supported. If it isn't supported, duplicate the deployment policy and update or modify it to make the image compliant.  
    - [Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted
    - [Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted

* VMs aren't logging in to the specified Log Analytics workspace.  
    It's possible that some VMs in the initiative scope are logging in to a Log Analytics workspace other than the one that's specified in the policy assignment. This policy is a tool to identify which VMs are reporting to a non-compliant workspace.  
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch

### Enable with PowerShell
To enable Azure Monitor for VMs for multiple VMs or virtual machine scale sets, you can use the PowerShell script [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), available from the Azure PowerShell Gallery. This script iterates through every virtual machine and virtual machine scale set in your subscription, in the scoped resource group that's specified by *ResourceGroup*, or to a single VM or virtual machine scale set that's specified by *Name*. For each VM or virtual machine scale set, the script verifies whether the VM extension is already installed. If the VM extension is not installed, the script tries to reinstall it. If the VM extension is installed, the script installs the Log Analytics and Dependency agent VM extensions.

This script requires Azure PowerShell module Az version 1.0.0 or later. Run `Get-Module -ListAvailable Az` to find the version. If you need to upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps). If you're running PowerShell locally, you also need to run `Connect-AzAccount` to create a connection with Azure.

To get a list of the script's argument details and example usage, run `Get-Help`.

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

## <a name="enable-for-a-hybrid-environment"></a>Karma bir ortamınız için etkinleştir
Bu bölümde, sanal makine veya veri merkezinizde veya diğer bulut ortamları, izleme, VM'ler için Azure İzleyici tarafından barındırılan fiziksel bilgisayarları dağıtmak açıklanmaktadır.

Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](../../azure-monitor/platform/gateway.md) BT güvenlik ilkeleriniz bilgisayarların internet'e bağlanmak için ağ üzerinde izin verme durumunda.

Dağıtım yöntemleri ve gereksinimleri gözden [Log Analytics Linux ve Windows Aracısı](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Adımları aşağıda özetlenmiştir:

1. Windows veya Linux için log Analytics aracısını yükleyin.

1. Vm'leri harita bağımlılık aracısı için Azure İzleyicisi'ni yükleyip [Windows](https://aka.ms/dependencyagentwindows) veya [Linux](https://aka.ms/dependencyagentlinux).

1. Performans sayaçlarını toplamayı etkinleştirin.

1. VM'ler için Azure İzleyici dağıtın.

### <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık aracısını yükleme
Bağımlılık aracısını el ile Windows bilgisayarlarda çalıştırarak yükleyebilirsiniz `InstallDependencyAgent-Windows.exe`. Bu yürütülebilir dosya hiçbir seçenek olmadan çalıştırırsanız, aracıyı etkileşimli olarak yüklemek için izlemeniz gereken bir Kurulum Sihirbazı başlar.

>[!NOTE]
>*Yönetici* ayrıcalıkları yükleyin veya aracıyı kaldırmak için gereklidir.

Aşağıdaki tabloda kurulum tarafından desteklenen komut satırı aracı için parametreleri vurgular.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini döndürür. |
| /S | Kullanıcı etkileşimi olmadan Sessiz bir yükleme gerçekleştirir. |

Örneğin, ile yükleme programını çalıştırmak için `/?` parametresi, türü **InstallDependencyAgent Windows.exe /?**.

Windows bağımlılık aracısını için dosyalar yüklenir *C:\Program Files\Microsoft bağımlılık aracısını* varsayılan olarak. Kurulum tamamlandıktan sonra başlatmak bağımlılık Aracısı'nı başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Günlük dizini *%Programfiles%\Microsoft bağımlılık Agent\logs*.

### <a name="install-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık aracısını yükleme
Bağımlılık Aracısı'nı Linux sunuculardan yüklü *InstallDependencyAgent Linux64.bin*, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği. Kullanarak dosyayı çalıştırabilirsiniz `sh` veya yürütme izinleri dosya için.

>[!NOTE]
> Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.
>

| Parametre | Açıklama |
|:--|:--|
| -Yardım | Komut satırı seçeneklerinin listesini alır. |
| -s | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| --denetleyin | İzinler ve işletim sistemini denetleyin, ancak aracıyı yüklemeyin. |

Örneğin, ile yükleme programını çalıştırmak için `-help` parametresi, türü **InstallDependencyAgent Linux64.bin-yardımcı**.

Aşağıdaki komutu çalıştırarak kök olarak Linux bağımlılık aracısını yükleme `sh InstallDependencyAgent-Linux64.bin`.

Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, günlük dizindir */var/opt/microsoft/dependency-agent/log*.

Bağımlılık Aracısı'nı dosyaları aşağıdaki dizinlerde yerleştirilir:

| Dosyalar | Konum |
|:--|:--|
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Performans sayaçları sağlar
Çözüm tarafından başvurulan Log Analytics çalışma alanı zaten çözüm için gerekli performans sayaçları toplamak için yapılandırılmamışsa, bunları etkinleştirmeniz gerekir. Bunu iki yoldan biriyle yapabilirsiniz:
* İçinde açıklandığı şekilde el ile [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md)
* İndiriliyor ve kullanılabilir bir PowerShell betiğini çalıştırarak [Azure PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="deploy-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici dağıtma
Bu yöntem, Log Analytics çalışma alanınızda çözüm bileşenlerini etkinleştirmek için yapılandırmasını belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları dağıtma ile bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

#### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

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

1. Değerlerini düzenleyin *WorkspaceName*, *ResourceGroupName*, ve *WorkspaceLocation*. Değeri *WorkspaceName* çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. Değeri *WorkspaceLocation* çalışma alanı içinde tanımlanan bölgedir.

1. Aşağıdaki PowerShell komutunu kullanarak bu şablonu dağıtmaya hazırsınız:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

    ```powershell
    provisioningState       : Succeeded
    ```
   İzleme etkinleştirdikten sonra sistem durumunu ve karma bilgisayar için ölçümleri görmeden önce yaklaşık 10 dakika sürebilir.

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi
VM'ler için Azure İzleyici, çözüm tarafından kullanılan performans sayaçları toplamak için bir Log Analytics çalışma alanı yapılandırır. Aşağıdaki tabloda, 60 saniyede toplanan çözüm tarafından yapılandırılan sayaçlarını ve nesneleri listeler.

### <a name="windows-performance-counters"></a>Windows performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|MantıksalDisk |% Boş alan |
|MantıksalDisk |Ort. Disk sn/Okuma |
|MantıksalDisk |Ort. Disk sn/Aktarım |
|MantıksalDisk |Ort. Disk sn/yazma |
|MantıksalDisk |Disk Bayt/sn |
|MantıksalDisk |Disk Okuma Bayt/sn |
|MantıksalDisk |Disk Okuma/sn |
|MantıksalDisk |Disk aktarımı/sn |
|MantıksalDisk |Disk Yazma Bayt/sn |
|MantıksalDisk |Disk Yazma/sn |
|MantıksalDisk |Boş megabayt |
|Bellek |Kullanılabilir MBayt |
|Ağ bağdaştırıcısı |Alınan Bayt/sn |
|Ağ bağdaştırıcısı |Gönderilen bayt/sn |
|İşlemci |% İşlemci zamanı |

### <a name="linux-performance-counters"></a>Linux performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|Mantıksal Disk |% Kullanılan alan |
|Mantıksal Disk |Disk Okuma Bayt/sn |
|Mantıksal Disk |Disk Okuma/sn |
|Mantıksal Disk |Disk aktarımı/sn |
|Mantıksal Disk |Disk Yazma Bayt/sn |
|Mantıksal Disk |Disk Yazma/sn |
|Mantıksal Disk |Boş megabayt |
|Mantıksal Disk |Mantıksal Disk Bayt/sn |
|Bellek |Kullanılabilir MBayt belleği |
|Ağ |Alınan toplam bayt sayısı |
|Ağ |Aktarılan toplam bayt |
|İşlemci |% İşlemci zamanı |

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri
Microsoft, Azure İzleyici hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve bütünlüğünü hizmeti geliştirmek için bu verileri kullanır. Doğru ve etkili sorun giderme özellikleri sağlamak üzere eşleme özelliğini verilerden işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Sonraki adımlar

İzleme sanal makineniz için etkinleştirilir, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
