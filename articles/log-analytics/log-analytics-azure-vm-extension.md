---
title: "Azure sanal makineler için günlük analizi bağlanma | Microsoft Docs"
description: "Windows ve Linux sanal Azure'da çalışan makineler için önerilen toplanan günlüklerini ve ölçümleri günlük analizi Azure VM uzantısı yükleyerek yoludur. Azure VM'ler üzerine günlük analizi sanal makine uzantısı yüklemek için Azure portal veya PowerShell kullanın."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cdae291b546fef4d7fdb8b067c8e4f4c9708d43f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a>Azure sanal makineler için günlük analizi günlük analizi aracı ile bağlanma

Windows ve Linux bilgisayarlar için günlükleri ve ölçümleri toplamak için önerilen günlük analizi Aracısı'nı yükleyerek yöntemdir.

Günlük analizi VM uzantısı aracılığıyla Azure sanal makinelerde günlük analizi Aracısı'nı yüklemek için en kolay yoludur.  Uzantısını kullanarak yükleme işlemini basitleştirir ve belirttiğiniz için günlük analizi çalışma alanına veri göndermek için aracı otomatik olarak yapılandırır. Aracı ayrıca otomatik olarak en son özellikleri ve düzeltmeleri sahip olduktan yükseltilir.

Windows sanal makineler için etkinleştirmeniz *Microsoft İzleme Aracısı* sanal makine uzantısı.
Linux sanal makineleri için etkinleştirmeniz *için OMS Aracısı Linux* sanal makine uzantısı.

Daha fazla bilgi edinmek [Azure sanal makine uzantıları](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux Aracısı](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aracı tabanlı koleksiyon için günlük verileri kullandığınızda, yapılandırmalısınız [günlük analizi veri kaynaklarında](log-analytics-data-sources.md) günlüklerini ve toplamak istediğiniz ölçümleri belirtmek için.

> [!IMPORTANT]
> Kullanarak günlük analizi günlük veri dizini oluşturmak için yapılandırırsanız [Azure tanılama](log-analytics-azure-storage.md)ve aynı günlükleri toplamak için aracısını yapılandırın, ardından Günlükler iki kez toplanır. Her iki veri kaynakları için ücretlendirilirsiniz. Yüklü aracı varsa, yalnızca aracı kullanarak günlük verileri toplamak - Azure tanılama günlük verilerini toplamak için günlük analizi yapılandırmayın.
>
>

Günlük analizi sanal makine uzantısını etkinleştirmek için üç kolay yol vardır:

* Azure Portalı'nı kullanarak
* Azure PowerShell kullanarak
* Bir Azure Resource Manager şablonu kullanarak

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Azure portalında VM uzantısı etkinleştir
Günlük analizi için aracıyı yükleyin ve üzerinde çalıştığı kullanarak Azure sanal makineyi bağlayın [Azure portal](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Günlük analizi Aracısı'nı yükleme ve sanal makine günlük analizi çalışma alanına bağlayın
1. [Azure portal](http://portal.azure.com) oturum açın.
2. Seçin **Gözat** portal ve sonra Git sol tarafındaki **günlük analizi (OMS)** ve seçin.
3. Azure VM ile kullanmak istediğiniz bir günlük analizi çalışma alanları, listeden seçin.  
   ![OMS çalışma alanları](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. Altında **oturum analytics Yönetimi**seçin **sanal makineleri**.  
   ![Sanal makineler](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. Listesinde **sanal makineleri**, aracıyı yüklemek istediğiniz sanal makineyi seçin. **OMS bağlantı durumu** VM, olup olmadığını gösterir **bağlı**.  
   ![VM bağlı değil](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. Sanal makineniz için Ayrıntılar seçin **Bağlan**. Aracıyı otomatik olarak yüklenir ve günlük analizi çalışma alanınız için yapılandırılır. Bu işlem hangi sırada OMS bağlantısı durumudur birkaç dakika sürer *bağlanıyor...*  
   ![VM Bağlan](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Yükleyip aracısına bağlanmak sonra **OMS bağlantısı** durumunu göstermek için güncelleştirilmeyecek **bu çalışma**.  
   ![Bağlı](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-the-vm-extension-using-powershell"></a>PowerShell kullanarak VM uzantısı etkinleştir
PowerShell kullanarak sanal makineniz yapılandırdığınızda sağlamanız gereken **Workspaceıd** ve **workspaceKey**. Json yapılandırmanızda özellik adları **büyük küçük harfe duyarlı**.

Kimliğini bulun ve üzerinde anahtar **ayarları** sayfasında OMS portalı veya önceki örnekte gösterildiği gibi PowerShell kullanarak.

![Çalışma alanı kimliği ve birincil anahtar](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Azure Klasik sanal makineleri ve Resource Manager sanal makineleri için farklı komutlar vardır. Aşağıda, Klasik ve Resource Manager sanal makineleri için örnekler verilmiştir.

Klasik sanal makineler için aşağıdaki PowerShell örneği kullanın:

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Resource Manager Linux VM'ler için aşağıdaki CLI kullanma
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Resource Manager sanal makineler için aşağıdaki PowerShell örneği kullanın:

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-the-vm-extension-using-a-template"></a>Bir şablon kullanarak VM uzantısı dağıtma
Azure Kaynak Yöneticisi'ni kullanarak, dağıtım ve uygulamanızın yapılandırmasını tanımlayan bir şablon (JSON biçiminde) oluşturabilirsiniz. Bu şablon Resource Manager şablonu olarak bilinir ve dağıtımı tanımlamanın bildirim temelli bir yolunu sunar. Bir şablon kullanarak, sürekli olarak uygulama yaşam döngüsü boyunca uygulamanızı dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtılması emin olabilirsiniz.

Günlük analizi aracı, Resource Manager şablonu bir parçası olarak dahil ederek, her bir sanal makine için günlük analizi çalışma alanınız bildirmek için önceden yapılandırılmış olduğundan emin olabilirsiniz.

Resource Manager şablonları hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Microsoft Monitoring Agent uzantı yüklü Windows çalıştıran bir sanal makine dağıtmak için kullanılan bir Resource Manager şablonu örneği verilmiştir. Bu şablon tipik sanal makine şablonu, aşağıdaki eklemelerle şöyledir:

* Workspaceıd ve workspaceName parametreleri
* Microsoft.EnterpriseCloud.Monitoring kaynak uzantısı bölümü
* Workspaceıd ve workspaceSharedKey yedekleme aramak için çıkışları

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Aşağıdaki PowerShell komutunu kullanarak şablonu dağıtabilirsiniz:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-the-log-analytics-vm-extension"></a>Günlük analizi VM uzantısı sorunlarını giderme
Genellikle Azure portalında veya Azure powershell şeyler çalışmıyor zaman bir ileti alırsınız.

1. [Azure portal](http://portal.azure.com) oturum açın.
2. VM bulun ve VM Ayrıntıları'nı açın.
3. Tıklatın **uzantıları** için OMS uzantısı veya etkin olup olmadığını denetleyin.

   ![VM uzantısı görünümü](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Tıklatın *MicrosoftMonitoringAgent*(Windows) veya *OmsAgentForLinux*(Linux) uzantısı ve görünüm ayrıntıları. 

   ![VM uzantısı ayrıntıları](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Sorun giderme Windows sanal makineler
Varsa *Microsoft İzleme Aracısı* VM Aracısı uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Azure VM aracısı yüklü olup olmadığını denetleyin ve doğru adımları kullanarak çalışma [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * VM Aracısı günlük dosyasını gözden geçirebilirsiniz`C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük mevcut değilse, VM aracısı yüklü değil.
     * [Klasik sanal makinelerin Azure VM Aracısı'nı yükleme](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Microsoft Monitoring Agent uzantısı sinyal görev aşağıdaki adımları kullanarak çalışıyor onaylayın:
   * Sanal makinede oturum açın
   * Görev Zamanlayıcısı'nı açın ve Bul `update_azureoperationalinsight_agent_heartbeat` görevi
   * Görev etkin ve bir dakikada çalıştığını onaylayın
   * Sinyal günlük dosyası iade etme`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Microsoft İzleme Aracısı VM uzantısı günlük dosyalarını inceleyin`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Sanal makine PowerShell betikleri çalıştırabilirsiniz emin olun
5. C:\Windows\temp izinlerini değiştirmezsiniz emin olun
6. Sanal makine üzerinde yükseltilmiş bir PowerShell penceresinde aşağıdaki komutu yazarak Microsoft Monitoring Agent durumunu görüntüleme`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Microsoft Monitoring Agent Kurulum günlük dosyalarını inceleyin`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Daha fazla bilgi için bkz: [Windows uzantıları sorun giderme](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Linux sanal makineleri sorunlarını giderme
Varsa *Linux için OMS aracısının* VM Aracısı uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Uzantı durumu ise *bilinmeyen* Azure VM aracısı yüklü olup olmadığını denetleyin ve doğru VM aracı günlük dosyasını gözden geçirerek çalışma`/var/log/waagent.log`
   * Günlük mevcut değilse, VM aracısı yüklü değil.
   * [Linux VM'ler üzerinde Azure VM Aracısı yükleme](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Linux VM uzantısı günlük dosyaları için sağlıksız diğer durumlar için OMS Aracısı'nı gözden `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` ve`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Uzantı durumu iyi değil, ancak veri değil yükleniyor Linux günlük dosyalarında OMS Aracısı gözden geçirin.`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma [günlük analizi veri kaynaklarında](log-analytics-data-sources.md) günlüklerini ve ölçümleri toplamak için belirtmek için.
* Sanal makinelerden veri toplamak üzere [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
* [Azure Tanılama'yı kullanarak verileri toplamak](log-analytics-azure-storage.md) Azure'da çalışan diğer kaynaklar için.

Azure'da olmayan bilgisayarlar için günlük analizi aracısı aşağıdaki makalelerde açıklanan yöntemlerle yükleyebilirsiniz:

* [Windows bilgisayarları günlük Analizi'ne bağlayın](log-analytics-windows-agents.md)
* [Linux bilgisayarları günlük Analizi'ne bağlayın](log-analytics-linux-agents.md)
