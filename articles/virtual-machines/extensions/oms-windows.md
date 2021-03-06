---
title: Windows için Azure İzleyici sanal makine uzantısı | Microsoft Docs
description: Log Analytics aracısını sanal makine uzantısı'nı kullanarak Windows sanal makine dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/29/2019
ms.author: roiyz
ms.openlocfilehash: b9d0e582b77dc06e1655a7bdb57ee232c603bc86
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706677"
---
# <a name="azure-monitor-virtual-machine-extension-for-windows"></a>Windows için Azure İzleyici sanal makine uzantısı

Azure İzleyici günlüklerine, Bulut ve şirket içi varlıkları arasında izleme özelliklerini sağlar. Windows için Log Analytics aracısını sanal makine uzantısı yayımlandı ve Microsoft tarafından desteklenmiyor. Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve sanal makinelerin mevcut bir Log Analytics çalışma alanına kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için Azure İzleyici sanal makine uzantısı için dağıtım seçenekleri açıklanmaktadır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Windows için Log Analytics Aracısı uzantısı, Windows işletim sistemi sürümlerini destekler:

- Windows Server 2019
- Windows Server 2008 R2, 2012, 2012 R2'de, 2016, sürüm 1709 ve 1803

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM uzantısı sürümü
Aşağıdaki tabloda, her sürüm için Log Analytics aracısını paketini ve Windows Azure İzleyicisi VM uzantısı sürümünü bir eşleme sağlar. 

| Log Analytics Windows aracı Paket sürümü | Azure İzleyici Windows VM uzantısı sürümü | Yayınlanma Tarihi | Sürüm Notları |
|--------------------------------|--------------------------|--------------------------|--------------------------|
| 10.20.18001 | 1.0.18001 | Haziran 2019 | <ul><li> Küçük hata düzeltmeleri ve geliştirmeleri sabitleme </li><li> Proxy bağlantısı (WINHTTP_AUTOLOGON_SECURITY_LEVEL_HIGH desteği) yapılırken varsayılan kimlik bilgileri devre dışı bırakmak için özelliği eklendi </li></ul>|
| 10.19.13515 | 1.0.13515 | Mart 2019 | <ul><li>Küçük sabitleme düzeltmeleri </li></ul> |
| 10.19.10006 | yok | Aralık 2018 | <ul><li> Küçük sabitleme düzeltmeleri </li></ul> | 
| 8.0.11136 | yok | Eylül 2018 |  <ul><li> Kaynak Kimliği değişikliğini VM hareket algılama için destek eklendi </li><li> Kaynak uzantısı olmayan kullanırken kimliği yükleme raporlaması için destek eklendi </li></ul>| 
| 8.0.11103 | yok |  Nisan 2018 | |
| 8.0.11081 | 1.0.11081 | Kasım 2017 | | 
| 8.0.11072 | 1.0.11072 | Eylül 2017 | |
| 8.0.11049 | 1.0.11049 | Şubat 2017 | |

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, otomatik olarak Log Analytics aracısını sağlar ve Azure aboneliğinin varsayılan Log Analytics çalışma alanıyla bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması yapılandırılmış çalışma alanı ve bağlantıyı kesme Azure Güvenlik Merkezi ile üzerine yazar.

### <a name="internet-connectivity"></a>İnternet bağlantısı
Windows için Log Analytics Aracısı uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Log Analytics aracısını Uzantı Şeması aşağıdaki JSON'u göstermektedir. Uzantı, çalışma alanı kimliği ve hedef Log Analytics çalışma alanından bir çalışma alanı anahtarı gerektirir. Bu, Azure portalında çalışma alanı ayarlarında bulunabilir. Çalışma alanı anahtarı hassas verileri olarak değerlendirilip olduğundan, bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi. Unutmayın **Workspaceıd** ve **workspaceKey** büyük küçük harfe duyarlıdır.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
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
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| workspaceId (örn.)* | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örn.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |

\* Çalışma alanı kimliği, günlük analizi API'SİNDE ConsumerID çağrılır.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında Log Analytics Aracısı uzantısı çalıştırmak için kullanılabilir. VM uzantısı Log Analytics aracısını içeren bir örnek şablonu bulunabilir [Azure hızlı başlangıç Galerisine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

>[!NOTE]
>Şablon, birden fazla çalışma alanı Kimliğiniz ve çalışma alanı anahtarı bir aracıyı birden çok çalışma alanına raporlama yapacak yapılandırmak istediğinizde belirtmeyi desteklemiyor. Bir aracıyı birden çok çalışma alanına raporlama yapacak yapılandırmak için bkz [ekleme veya bir çalışma alanı kaldırılıyor](../../azure-monitor/platform/agent-manage.md#adding-or-removing-a-workspace).  

Sanal makine uzantısı için JSON içinde sanal makine kaynağı iç içe veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yerleşimini etkiler. Daha fazla bilgi için [ayarlamak için alt kaynakları ad ve tür](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources). 

Aşağıdaki örnek, Azure İzleyici uzantısı sanal makine kaynağı içinde iç içe varsayar. İç içe uzantısı kaynak, JSON yerleştirildi `"resources": []` sanal makinenin nesne.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
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
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

Uzantı JSON şablonu kökünde yerleştirilirken, kaynak adı üst sanal makineye bir başvuru içerir ve iç içe geçmiş yapılandırma türü yansıtır. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
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
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzVMExtension` Komutu, Log Analytics aracısını sanal makine uzantısı varolan bir sanal makineye dağıtmak için kullanılabilir. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanan gerekir. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure PowerShell modülü kullanarak şu komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
