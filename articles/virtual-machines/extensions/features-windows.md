---
title: Azure VM uzantıları ve özellikleri Windows için | Microsoft Docs
description: Hangi uzantıların hangi kullanıcılar sağlar veya geliştirmek göre gruplanır, Azure sanal makineler için kullanılabilir olduğunu öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ce13f053c2adee6a9a347a4162b60cc6d6b40eda
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849758"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Sanal makine uzantıları ve özellikleri Windows için

Azure sanal makinesi (VM), Azure Vm'leri üzerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sunan küçük uygulamalar uzantılarıdır. VM uzantısı, örneğin, bir sanal makineye yazılım yükleme, virüsten koruma gerektiriyorsa veya içindeki bir betik çalıştırmak için kullanılabilir. Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portalı ile Azure VM uzantıları çalıştırılabilir. Uzantıları ile yeni bir VM dağıtımını toplanmış veya mevcut bir sistemle bağlantılı çalıştırın.

Bu makalede VM uzantıları, Azure VM uzantıları kullanma önkoşulları genel bir bakış sağlar ve algılamak hakkında yönergeler yönetmek ve VM uzantılarını kaldırın. Birçok VM uzantıları bulunduğundan, bu makale, genelleştirilmiş bilgi sağlar. Her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantı özel ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="use-cases-and-samples"></a>Kullanım ve örnekleri

Birkaç farklı Azure VM uzantıları kullanılabilir, her biri belirli bir kullanım örneği. Bazı örnekler:

- PowerShell istenen durum yapılandırmaları DSC uzantısı ile sanal makineye Windows için geçerlidir. Daha fazla bilgi için [Azure Desired State configuration uzantısı](dsc-overview.md).
- Microsoft İzleme Aracısı VM uzantısı ile sanal makine izlemeyi yapılandırın. Daha fazla bilgi için [Azure Vm'lerine Azure İzleyici günlüklerine](../../log-analytics/log-analytics-azure-vm-extension.md).
- Bir Azure sanal makinesi, Chef kullanarak yapılandırın. Daha fazla bilgi için [Chef ile otomatikleştirme Azure VM dağıtımını](../windows/chef-automation.md).
- Azure altyapınızı Datadog uzantısı ile izlemeyi yapılandırma. Daha fazla bilgi için [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).


İşleme özel uzantılar ek olarak, bir özel betik uzantısı, hem Windows hem de Linux sanal makineler için kullanılabilir. Windows için özel betik uzantısı, bir sanal makine üzerinde çalıştırılacak PowerShell komut dosyaları sağlar. Özel komut dosyaları, yerel hangi Azure Araçları sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için kullanışlıdır. Daha fazla bilgi için [Windows VM özel betik uzantısı](custom-script-windows.md).

## <a name="prerequisites"></a>Önkoşullar

Sanal makine uzantısını işlemek için Azure Windows Aracısı gerekir. Bazı uzantıları ayrı ayrı kaynaklar veya bağımlılıkları erişim gibi önkoşulları sahip.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM Aracısı, Azure VM ve Azure yapı denetleyicisi arasındaki etkileşimler yönetir. VM Aracısı dağıtma ve yönetme Azure Vm'leri, VM uzantılarını çalıştırmak dahil çok sayıda işlevsel görünüşlere için sorumludur. Azure VM Aracısı, Azure Marketi görüntülerinde önceden yüklenmiş olarak ve el ile desteklenen işletim sistemlerine yüklenebilir. Windows için Azure VM Aracısı, Windows Konuk aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](agent-windows.md).

#### <a name="supported-agent-versions"></a>Desteklenen Aracı sürümleri

Mümkün olan en iyi deneyimi sağlamak için en düşük aracı sürümü vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın.

#### <a name="supported-oses"></a>Desteklenen işletim sistemleri

Windows Konuk Aracısı, uzantıları framework limiti işletim sistemleri için bu uzantılar vardır ancak birden çok Oses'te çalıştırır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems
) bakın.

Bazı uzantılar tüm işletim sistemlerinde desteklenmez ve yayabilir *hata kodu 51, 'Desteklenmeyen işletim sistemi'*. Desteklenebilirlik için ayrı bir uzantı belgelerine bakın.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure depolama uzantısı deposundan yüklenir ve uzantı durumu karşıya Azure Depolama'ya gönderilen değerler. Kullanırsanız [desteklenen](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) aracıların sürümünü, gereksinim gibi aracı iletişimi Azure yapı denetleyicisi için aracı iletişimlerini yeniden yönlendirmek için kullanabilirsiniz VM bölgesindeki Azure Depolama'da erişime izin vermek. Bir desteklenmeyen Aracı sürümünde varsa, Azure depolama bu bölgede VM'den giden erişime izin gerekir.

> [!IMPORTANT]
> Erişim engellenirse *168.63.129.16* Konuk Güvenlik Duvarı'nı kullanarak, daha sonra uzantıları yukarıdaki bağımsız olarak başarısız.

Aracıları yalnızca uzantı paketleri ve raporlama durumu indirmek için kullanılabilir. Örneğin, bir uzantı yükleme (özel betik) Github'dan bir betik indirmeniz gerekiyor veya Azure depolama (Azure Backup) sonra erişim ek gerekirse Güvenlik Duvarı/güvenlik ağ Grup bağlantı noktaları açılması gerekir. Uygulamaları kendi sağ olduğundan farklı uzantılarına farklı gereksinimlere sahiptir. Azure depolama erişimi gerektiren uzantılar için Azure NSG hizmet etiketleri kullanarak erişim izni verebilirsiniz [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Windows Konuk Aracısı, proxy sunucusu, aracı trafiğini isteklerini yeniden yönlendirmek destek yok.

## <a name="discover-vm-extensions"></a>VM uzantıları bulma

Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Tam listesini görmek için [Get-AzVMExtensionImage](https://docs.microsoft.com/powershell/module/az.compute/get-azvmextensionimage). Aşağıdaki örnekte tüm kullanılabilir uzantıları listeler *WestUS* konumu:

```powershell
Get-AzVmImagePublisher -Location "WestUS" | `
Get-AzVMExtensionImageType | `
Get-AzVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM Uzantıları'nı çalıştırın

Azure VM uzantıları mevcut Vm'lerde çalıştıracağınız başka yapılandırma değişiklikleri yapmak veya zaten dağıtılmış bir sanal makine bağlantısı kurtarmak gerektiğinde bu faydalıdır. VM uzantıları Azure Resource Manager şablon dağıtımları ile toplanmış. Resource Manager şablonları ile uzantıları kullanarak, Azure Vm'leri dağıtılabilir ve dağıtım sonrası müdahalesi olmadan yapılandırılmış.

Aşağıdaki yöntemlerden bir uzantı mevcut bir VM'ye karşı çalıştırmak için kullanılabilir.

### <a name="powershell"></a>PowerShell

Çeşitli PowerShell komutlarını tek tek uzantıların çalıştırmak için mevcut. Listesini görmek için [Get-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-command) ve filtre *uzantısı*:

```powershell
Get-Command Set-Az*Extension* -Module Az.Compute
```

Bu, aşağıdakine benzer bir çıktı sağlar:

```powershell
CommandType     Name                                          Version    Source
-----------     ----                                          -------    ------
Cmdlet          Set-AzVMAccessExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMADDomainExtension                     4.5.0      Az.Compute
Cmdlet          Set-AzVMAEMExtension                          4.5.0      Az.Compute
Cmdlet          Set-AzVMBackupExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMBginfoExtension                       4.5.0      Az.Compute
Cmdlet          Set-AzVMChefExtension                         4.5.0      Az.Compute
Cmdlet          Set-AzVMCustomScriptExtension                 4.5.0      Az.Compute
Cmdlet          Set-AzVMDiagnosticsExtension                  4.5.0      Az.Compute
Cmdlet          Set-AzVMDiskEncryptionExtension               4.5.0      Az.Compute
Cmdlet          Set-AzVMDscExtension                          4.5.0      Az.Compute
Cmdlet          Set-AzVMExtension                             4.5.0      Az.Compute
Cmdlet          Set-AzVMSqlServerExtension                    4.5.0      Az.Compute
Cmdlet          Set-AzVmssDiskEncryptionExtension             4.5.0      Az.Compute
```

Aşağıdaki örnek, hedef sanal makineye bir GitHub deposundan bir betik indirin ve ardından komut dosyasını çalıştırmak için özel betik uzantısı kullanır. Özel betik uzantısı hakkında daha fazla bilgi için bkz. [özel betik uzantısına genel bakış](custom-script-windows.md).

```powershell
Set-AzVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

Aşağıdaki örnekte, VM erişimi uzantısı, geçici bir parola için Windows VM'nin yönetici parolasını sıfırlamak için kullanılır. VM erişimi uzantısı hakkında daha fazla bilgi için bkz. [sıfırlama Uzak Masaüstü hizmetini bir Windows VM'de](../windows/reset-rdp.md). Bu işlemi gerçekleştirdikten sonra ilk oturum açmada parola sıfırlama:

```powershell
$cred=Get-Credential

Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

`Set-AzVMExtension` Komutu, herhangi bir VM uzantısı'nı başlatmak için kullanılabilir. Daha fazla bilgi için [kümesi AzVMExtension başvuru](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension).


### <a name="azure-portal"></a>Azure portal

VM uzantıları, var olan bir sanal makineye Azure Portalı aracılığıyla uygulanabilir. Portalda VM seçin, **uzantıları**, ardından **Ekle**. Sihirbazdaki yönergeleri izleyin ve kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin.

Aşağıdaki örnek, Azure portalından Microsoft Antimalware uzantının yüklenmesi gösterir:

![Kötü amaçlı yazılımdan koruma uzantısını yükle](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları bir Azure Resource Manager şablonuna eklenebilir ve şablon dağıtımı ile yürütüldü. Uzantı bir şablon ile dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturamazsınız. Örneğin, aşağıdaki JSON alınmış bir Kaynak Yöneticisi'nden şablonu yük dengeli VM'ler ile Azure SQL veritabanı kümesi dağıtır ve ardından her bir VM üzerinde .NET Core uygulamasını yükler. VM uzantısı yazılım yüklemesi üstlenir.

Daha fazla bilgi için [tam Resource Manager şablonu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

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
    "typeHandlerVersion": "1.9",
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

Resource Manager şablonları oluşturmaya daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma Windows VM uzantıları içeren](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Güvenli VM uzantısı verileri

VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarları gibi hassas bilgileri içerecek şekilde gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef sanal makine içinde şifresini çözer ve korumalı bir yapılandırma içerir. Belirli bir korumalı yapılandırma şeması her uzantısına sahiptir ve her uzantı özgü belgelerinde ayrıntılı.

Aşağıdaki örnek, Windows için özel betik uzantısı'nın bir örneğini gösterir. Yürütülecek komut, kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifreli değil:

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
    "typeHandlerVersion": "1.9",
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

Taşıma **yürütmek için komut** özelliğini **korumalı** yapılandırma aşağıdaki örnekte gösterildiği gibi yürütme dize güvenliğini sağlar:

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
    "typeHandlerVersion": "1.9",
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

### <a name="how-do-agents-and-extensions-get-updated"></a>Nasıl aracısı ve uzantıları güncelleştirilmesi?

Uzantıları ve aracıları aynı güncelleştirme mekanizmasını paylaşın. Bazı güncelleştirmeler, ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanılabilir olduğunda, bir değişikliği uzantılarını ve diğer VM Model değişiklikleri gibi olduğunda yalnızca sanal makinede yüklü:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama kapsayıcı
- Konuk işletim sistemi gizli dizileri
- VM boyutu
- Ağ profili

Yayımcılar olası farklı sürümleri üzerinde farklı bölgelerde Vm'niz olabilir, bu nedenle güncelleştirmeler farklı zamanlarda bölgeler kullanılabilir hale getirmek.

#### <a name="listing-extensions-deployed-to-a-vm"></a>Bir VM'ye dağıttınız uzantılarını listeleme

```powershell
$vm = Get-AzVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Windows Konuk Aracısı yalnızca içeren *kod uzantısı işleme*, *Windows sağlama kod* ayrıdır. Windows Konuk Aracısı kaldırabilirsiniz. Pencere Konuk Aracısı'nın otomatik güncelleştirme devre dışı bırakılamıyor.

*Uzantısı işleme kodu* Azure yapısı ile iletişim kurmasını ve sanal makine uzantıları işlem gibi işleme, bunları kaldırma durumu raporlama ve uzantıları ayrı ayrı güncelleştirme yüklemeler için sorumludur. Güncelleştirmeleri, güvenlik düzeltmeleri, hata düzeltmeleri ve geliştirmeleri içerir *uzantısı işleme kodu*.

Çalıştırdığınız hangi sürümünü denetlemek için bkz: [algılama Windows Konuk aracısı yüklü](agent-windows.md#detect-the-vm-agent).

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirme kullanılabilir olduğunda, Windows Konuk Aracısı indirir ve uzantısını yükseltir. Uzantı otomatik güncelleştirmelerin ya da *küçük* veya *düzeltme*. Kabul et veya uzantıları dışında iyileştirilmiş *küçük* uzantı sağladığınızda güncelleştirir. Aşağıdaki örnek bir Resource Manager şablonu ile küçük sürümlerde otomatik olarak yükseltme gösterir *autoUpgradeMinorVersion ": true,'*:

```json
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
```

En son alt sürüm hata düzeltmeleri almak için otomatik güncelleştirme her zaman uzantısı dağıtımlarınızda seçin önerilir. Güvenlik veya anahtar hata düzeltmeleri taşıyan düzeltme güncelleştirmelerini vazgeçti olamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri belirleme

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Uzantı bir VM ile aynı autoUpgradeMinorVersion ayarlarsanız tanımlama

Uzantı 'ile aynı autoUpgradeMinorVersion' sağladıysanız VM modelden görebilirsiniz. Denetlemek için kullanmak [Get-AzVm](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) ve VM ve kaynak grubu adı şu şekilde sağlayın:

```powerShell
 $vm = Get-AzVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

Aşağıdaki örnek çıktı gösterilmektedir *autoUpgradeMinorVersion* ayarlanır *true*:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Bir autoUpgradeMinorVersion oluştuğunda tanımlama

Uzantısı için bir güncelleştirme gerçekleştiği görmek için gözden geçirme aracı VM açtığında *C:\WindowsAzure\Logs\WaAppAgent.log*

Aşağıdaki örnekte, sanal makine vardı *Microsoft.Compute.CustomScriptExtension 1.8* yüklü. Bir düzeltme sürümü için kullanılabilir *1.9*:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Aracısı izinleri

Görevleri gerçekleştirmek için aracının Çalıştır gerekir *yerel sistem*.

## <a name="troubleshoot-vm-extensions"></a>VM uzantı sorunlarını giderme

Her VM uzantısı, sorun giderme adımları belirli uzantısına sahip olabilir. Örneğin, özel betik uzantısı kullandığınızda, betik yürütme ayrıntıları yerel olarak VM uzantısı çalıştırdığı bulunabilir. Uzantı özel sorun giderme işlemleri uzantısı özgü belgelerinde açıklanmıştır.

Tüm VM uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

1. Windows Konuk Aracısı günlüğünü denetlemek için uzantınız içinde sağlanırken faaliyeti Ara *C:\WindowsAzure\Logs\WaAppAgent.txt*

2. Daha fazla bilgi için gerçek uzantı günlükleri denetleyin *C:\WindowsAzure\Logs\Plugins\<extensionName >*

3. Hata kodları, bilinen sorunlar vb. için uzantı özgü belgelere yönlendirir sorun giderme bölümleri denetleyin.

4. Sistem günlüklerine bakın. Uzantılı bir uzun süre çalışan özel Paket Yöneticisi erişim gerektiren başka bir uygulama yüklemesini gibi zorlayıcı nedenleriniz diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hatalarının sık karşılaşılan nedenleri

1. Uzantılı çalıştırmak için 20 dakika (özel durumlar: CustomScript uzantıları, Chef ve 90 dakika olan DSC). Dağıtımınız bu defa aşıyorsa, bir zaman aşımı işaretlenir. Bunun nedeni düşük kaynak VM, diğer sanal makine yapılandırmaları/başlangıç uzantı için sağlama okunurken yüksek miktarda kaynak tüketen görevlerini nedeniyle olabilir.

2. En düşük Önkoşullar karşılanmadı. Bazı uzantılar, HPC görüntüleri gibi VM SKU'ları, bağımlılıkları vardır. Uzantılar, Azure depolama veya genel hizmetlerle iletişim kurma gibi belirli ağ erişim gereksinimleri gerektirebilir. Diğer örnekler paket depolarına, disk alanı veya güvenlik kısıtlamaları dışında çalışan erişimi olabilir.

3. Özel Paket Yöneticisi erişim. Bazı durumlarda, uzun süre çalışan bir VM yapılandırması ve uzantı yüklemesi çakışan, burada her ikisi de Paket Yöneticisi özel erişmesi gereken karşılaşabilirsiniz.

### <a name="view-extension-status"></a>Uzantı durumu görüntüle

Bir VM'ye karşı VM uzantısı çalıştırıldıktan sonra kullanmak [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) uzantı Durumu döndürülecek. *Alt durumlar [0]* uzantı sağlama başarılı, VM'ye dağıttınız BT'nin başarılı anlamına gelir, ancak uzantı VM içindeki yürütülemedi, gösterir *alt durumlar [1]*.

```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -VMName "myVM" -Status
```

Aşağıdaki örnek çıktıya benzer bir çıkış:

```powershell
Extensions[0]           :
  Name                  : CustomScriptExtension
  Type                  : Microsoft.Compute.CustomScriptExtension
  TypeHandlerVersion    : 1.9
  Substatuses[0]        :
    Code                : ComponentStatus/StdOut/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : Windows PowerShell \nCopyright (C) Microsoft Corporation. All rights reserved.\n
  Substatuses[1]        :
    Code                : ComponentStatus/StdErr/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : The argument 'cseTest%20Scriptparam1.ps1' to the -File parameter does not exist. Provide the path to an existing '.ps1' file as an argument to the

-File parameter.
  Statuses[0]           :
    Code                : ProvisioningState/failed/-196608
    Level               : Error
    DisplayStatus       : Provisioning failed
    Message             : Finished executing command
```

Uzantı yürütme durumu, ayrıca Azure portalında bulunabilir. Bir uzantı durumunu görüntülemek için VM seçin, **uzantıları**, ardından istediğiniz uzantıyı seçin.

### <a name="rerun-vm-extensions"></a>VM uzantılarını yeniden çalıştırın

VM uzantısı çalıştırılması gereken durumlar olabilir. Uzantı, kaldırma ve uzantı ile kendi tercih ettiğiniz bir yürütme yöntemi daha sonra yeniden çalıştırabilirsiniz. Bir uzantıyı kaldırmak için [Remove-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/Remove-AzVMExtension) gibi:

```powershell
Remove-AzVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

Ayrıca uzantı Azure portalında şu şekilde kaldırabilirsiniz:

1. Bir VM'yi seçin.
2. Seçin **uzantıları**.
3. İstediğiniz uzantıyı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extensions-reference"></a>Ortak VM uzantıları başvurusu
| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Windows için özel betik uzantısı |Bir Azure sanal makinesi karşı betikleri çalıştırma |[Windows için özel betik uzantısı](custom-script-windows.md) |
| Windows için DSC uzantısı |PowerShell DSC (Desired State Configuration) uzantısı |[Windows için DSC uzantısı](dsc-overview.md) |
| Azure Tanılama Uzantısı |Azure Tanılama'yı yönetme |[Azure Tanılama Uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişimi uzantısı |Kullanıcı ve kimlik bilgilerini yönetme |[Linux için VM erişimi uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantılarına ve özelliklerine genel bakış](overview.md).
