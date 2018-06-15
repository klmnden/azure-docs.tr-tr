---
title: Sanal makine uzantıları ve özellikleri Windows Azure için | Microsoft Docs
description: Hangi uzantıları ne bunlar sağlayın veya geliştirmek tarafından gruplandırılmış Azure sanal makineler için kullanılabilir olduğunu öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09eb2e723be80fd623ad53c9bda8271ef8846a0f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32192443"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Sanal makine uzantıları ve özellikleri Windows için

Azure sanal makine uzantıları Azure sanal makinelerde dağıtım sonrası yapılandırma ve Otomasyon görevlerini sağlayan küçük uygulamalardır. Örneğin, bir sanal makineye yazılım yükleme, virüsten koruma veya Docker yapılandırma gerektiriyorsa, bu görevleri tamamlamak için bir VM uzantısı kullanılabilir. Azure VM uzantıları, Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portalını kullanarak çalıştırabilirsiniz. Uzantıları ile yeni bir sanal makine dağıtımı paketlenebilir veya varolan bir sistemle bağlantılı çalıştırın.

Bu belge, sanal makine uzantıları, sanal makine uzantıları ve Kılavuzu algılamak, yönetmek ve sanal makine uzantıları kaldırmak nasıl kullanma önkoşulları genel bir bakış sağlar. Birçok VM uzantıları bulunduğundan, bu belgede genelleştirilmiş bilgiler sağlanmaktadır her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantıya özgü ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

## <a name="use-cases-and-samples"></a>Kullanım örnekleri ve örnekler

Birçok farklı Azure VM uzantıları vardır, her biri belirli bir kullanım örneği. Bazı örnek kullanım örnekleri şunlardır:

- PowerShell istenen durum yapılandırmalar için Windows DSC uzantısı kullanılarak bir sanal makine için geçerlidir. Daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı](extensions-dsc-overview.md).
- Microsoft İzleme Aracısı VM uzantısı kullanarak sanal makine izlemeyi yapılandırın. Daha fazla bilgi için bkz: [bağlanmak Azure sanal makineleri için günlük analizi](../../log-analytics/log-analytics-azure-vm-extension.md).
- Azure altyapınızın Datadog uzantılı izlemeyi yapılandırın. Daha fazla bilgi için bkz: [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Bir Azure sanal makinesi Chef kullanarak yapılandırın. Daha fazla bilgi için bkz: [otomatikleştirme Azure sanal makine dağıtımı Chef ile](chef-automation.md).

İşleme özgü uzantılar ek olarak, bir özel betik uzantısı, Windows ve Linux sanal makineleri için kullanılabilir. Windows için özel betik uzantısı, bir sanal makine üzerinde çalıştırılacak PowerShell komut dosyaları sağlar. Yerel hangi Azure araçlar sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlarken kullanışlıdır. Daha fazla bilgi için bkz: [Windows VM özel betik uzantısı](extensions-customscript.md).


## <a name="prerequisites"></a>Önkoşullar

Her sanal makine uzantısı önkoşulları kendi kümesine sahip. Örneğin, Docker VM uzantısı desteklenen Linux dağıtım önkoşul vardır. Tek tek uzantıların gereksinimlerini uzantıya özgü belgelerinde açıklanmıştır.

### <a name="azure-vm-agent"></a>Azure VM aracısı
Azure VM Aracısı bir Azure sanal makinesi ve Azure yapı denetleyicisi arasındaki etkileşimi yönetir. VM Aracısı dağıtma ve yönetme Azure sanal makineler, çalışan VM uzantıları dahil olmak üzere birçok işlevsel görünüşlere için sorumludur. Azure VM Aracısı Azure Marketi görüntülerinde önceden yüklenmiş ve desteklenen işletim sistemlerine yüklenebilir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](agent-user-guide.md).

## <a name="discover-vm-extensions"></a>VM uzantıları Bul
Birçok farklı VM uzantıları, Azure sanal makineler ile kullanmak için kullanılabilir. Tam listesini görmek için Azure Resource Manager PowerShell modülü ile aşağıdaki komutu çalıştırın. Bu komutu çalıştırdığınızda istenen konumu belirttiğinizden emin olun.

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM uzantıları çalıştırın

Azure sanal makine uzantıları yapılandırma değişikliklerini yapın veya zaten dağıtılmış bir VM'de bağlantısı kurtarmak gerektiğinde faydalı olan mevcut sanal makinelerde çalıştırılabilir. VM uzantıları, Azure Resource Manager şablonu dağıtımlarında da gönderilebilir. Resource Manager şablonları ile uzantıları kullanarak, dağıtılması ve dağıtım sonrası müdahalesi gerektirmeden yapılandırılmış için Azure sanal makineleri etkinleştirebilirsiniz.

Aşağıdaki yöntemlerden bir uzantısı olan bir sanal makineyi karşı çalıştırmak için kullanılabilir.

### <a name="powershell"></a>PowerShell

Birkaç PowerShell komutları tek tek uzantıların çalıştırmak için mevcut. Bir listesini görmek için aşağıdaki PowerShell komutlarını çalıştırın.

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Bu, aşağıdakine benzer bir çıktı sağlar:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

Aşağıdaki örnek, bir komut dosyası hedef sanal makine üzerine GitHub deposunu indirin ve komut dosyasını çalıştırmak için özel betik uzantısı kullanır. Özel betik uzantısı hakkında daha fazla bilgi için bkz: [özel betik uzantısı genel bakış](extensions-customscript.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

Bu örnekte, VM erişim uzantısı, Windows sanal makine yönetici parolasını sıfırlamak için kullanılır. VM erişim uzantısı ile ilgili daha fazla bilgi için bkz: [Windows VM Uzak Masaüstü'nü Sıfırla Hizmeti'nde](reset-rdp.md).

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

`Set-AzureRmVMExtension` Komutu, tüm VM uzantısı başlatmak için kullanılabilir. Daha fazla bilgi için bkz: [kümesi AzureRmVMExtension başvuru](https://msdn.microsoft.com/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure portalına

VM uzantısı olan bir sanal makineyi Azure Portalı aracılığıyla uygulanabilir. Bunu yapmak için kullanmak, seçmek istediğiniz sanal makineyi seçin **uzantıları**, tıklatıp **Ekle**. Bu, kullanılabilir uzantıları listesini sağlar. İstediğiniz ve sihirbazdaki adımları izleyin birini seçin.

Aşağıdaki resim Azure portalından Microsoft Antimalware uzantının yüklenmesi gösterir.

![Kötü amaçlı yazılımdan koruma uzantısını yükleyin](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları, bir Azure Resource Manager şablonu eklenir ve şablon dağıtımı ile yürütüldü. Bir şablonla uzantıları tam olarak yapılandırılmış Azure dağıtımları oluşturmak için kullanışlıdır. Örneğin, aşağıdaki JSON yük dengeli sanal makineler kümesi ve Azure SQL Veritabanını dağıtır ve ardından her VM .NET Core uygulama yükleyen bir Resource Manager şablonu alınır. VM uzantısı yazılım yüklemesi mvc'deki.

Daha fazla bilgi için bkz: [tam Resource Manager şablonu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma Windows VM uzantıları ile](template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>VM uzantısı verileri güvenli

VM uzantısı çalıştırırken kimlik bilgilerini, depolama hesabı adları ve depolama hesabı erişim anahtarlarını gibi hassas bilgiler dahil etmek gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef sanal makine içinde şifresini çözer korumalı bir yapılandırmayı içerir. Her bir uzantı uzantıya özgü belgelerinde ayrıntılı bir belirli korumalı yapılandırma şeması vardır.

Aşağıdaki örnek, Windows için özel betik uzantısı örneğini gösterir. Komutun yürütülmesi için kimlik bilgileri kümesini içerdiğine dikkat edin. Bu örnekte, yürütülecek komut şifrelenmez.


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Yürütme dize taşıyarak güvenli **yürütülecek komut** özelliğine **korumalı** yapılandırma.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a>VM uzantıları sorun giderme

Her VM uzantısı özel sorun giderme adımları olabilir. Örneğin, özel betik uzantısı kullanırken, komut dosyası yürütme ayrıntılarını yerel olarak sanal makinede uzantı çalıştırıldı bulunabilir. Uzantı özel sorun giderme işlemleri uzantıya özgü belgelerinde açıklanmıştır.

Tüm sanal makine uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüle

Bir sanal makine uzantısı bir sanal makine çalıştırdıktan sonra uzantı durumunu döndürmek için aşağıdaki PowerShell komutunu kullanın. Örnek parametre adları kendi değerlerinizle değiştirin. `Name` Parametresi, yürütme esnasında uzantısını verilen ad alır.

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Çıktı aşağıdaki gibi görünür:

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

Uzantı yürütme durumu de Azure portalında bulunabilir. Uzantı durumunu görüntülemek için sanal makineyi seçin, **uzantıları**ve istenen uzantı seçin.

### <a name="rerun-vm-extensions"></a>VM uzantıları yeniden çalıştırın

Bir sanal makine uzantısı yeniden çalıştırılması gereken durumlar olabilir. Uzantı kaldırarak ve tercih ettiğiniz yürütme yöntemiyle uzantısı yeniden çalıştırma bunu yapabilirsiniz. Bir uzantıyı kaldırmak için Azure PowerShell modülü ile aşağıdaki komutu çalıştırın. Örnek parametre adları kendi değerlerinizle değiştirin.

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı, Azure portalını kullanarak da kaldırılabilir. Bunu yapmak için:

1. Bir sanal makineyi seçin.
2. Seçin **uzantıları**.
3. İstenen uzantı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extensions-reference"></a>Ortak VM uzantıları başvurusu
| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Windows için özel betik uzantısı |Bir Azure sanal makinesi karşı komut dosyalarını çalıştır |[Windows için özel betik uzantısı](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Windows için DSC uzantısı |PowerShell DSC (İstenen durum Yapılandırması ') uzantısı |[Windows için DSC uzantısı](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure Tanılama Uzantısı |Azure tanılama yönetme |[Azure Tanılama Uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişim uzantısı |Kullanıcılar ve kimlik bilgilerini yönetme |[Linux VM erişim uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
