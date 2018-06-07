---
title: Azure VM uzantıları ve özellikleri Windows | Microsoft Docs
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
ms.date: 03/30/2018
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e9e147e2cbe5ff42562d6fcfab62460df48f3d65
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809735"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Sanal makine uzantıları ve özellikleri Windows için

Azure sanal makine (VM), dağıtım sonrası yapılandırma ve Otomasyon görevlerini Azure Vm'lerinde sağlayan küçük uygulamalar uzantılarıdır. Örneğin, bir sanal makineye yazılım yükleme, virüsten koruma gerektiriyorsa veya bir komut dosyası, içinde çalıştırmak için bir VM uzantısı kullanılabilir. Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portal ile Azure VM uzantıları çalıştırılabilir. Uzantıları yeni VM Dağıtım ile birlikte veya varolan bir sistemle bağlantılı çalıştırın.

Bu makalede VM uzantıları, Azure VM uzantıları kullanma önkoşulları genel bir bakış sağlar ve algılamak nasıl hakkında yönergeler yönetmek ve VM uzantılarını kaldırın. Birçok VM uzantıları bulunduğundan, bu makalede genelleştirilmiş bilgiler sağlanmaktadır her potansiyel olarak benzersiz bir yapılandırmaya sahip. Uzantıya özgü ayrıntıları her belge için ayrı ayrı uzantısı belirli bulunabilir.

## <a name="use-cases-and-samples"></a>Kullanım örnekleri ve örnekler

Birkaç farklı Azure VM uzantıları kullanılabilir, her biri belirli bir kullanım örneği. Bazı örnekler:

- PowerShell istenen durum yapılandırmalar VM DSC uzantısı ile Windows için geçerlidir. Daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı](dsc-overview.md).
- Microsoft İzleme Aracısı VM uzantısı olan bir VM izlemeyi yapılandırın. Daha fazla bilgi için bkz: [günlük analizi Azure Vm'lerine bağlanmak](../../log-analytics/log-analytics-azure-vm-extension.md).
- Bir Azure VM Chef kullanarak yapılandırın. Daha fazla bilgi için bkz: [otomatikleştirme Azure VM dağıtımı Chef ile](../windows/chef-automation.md).
- Azure altyapınızın Datadog uzantılı izlemeyi yapılandırın. Daha fazla bilgi için bkz: [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).


İşleme özgü uzantılar ek olarak, bir özel betik uzantısı, Windows ve Linux sanal makineleri için kullanılabilir. Windows için özel betik uzantısı, bir VM üzerinde çalıştırılacak PowerShell komut dosyaları sağlar. Özel komut dosyaları, yerel hangi Azure araçlar sağlayabilir ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için faydalıdır. Daha fazla bilgi için bkz: [Windows VM özel betik uzantısı](custom-script-windows.md).

## <a name="prerequisites"></a>Önkoşullar

VM uzantısı işlemek için Azure Linux aracısı yüklü gerekir. Bazı tek tek uzantıların kaynaklarına erişim veya bağımlılıkları gibi önkoşulları vardır.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM Aracısı bir Azure VM ve Azure yapı denetleyicisi arasındaki etkileşimler yönetir. VM Aracısı dağıtma ve yönetme Azure VM'ler, VM Uzantıları'nı çalıştırarak dahil olmak üzere birçok işlevsel görünüşlere için sorumludur. Azure VM Aracısı Azure Marketi görüntülerinde önceden yüklenmiş ve desteklenen işletim sistemlerinde el ile yüklenebilir. Windows için Azure VM Aracısı Windows Konuk aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için bkz: [Azure sanal makine Aracısı](agent-windows.md).

#### <a name="supported-agent-versions"></a>Desteklenen Aracı sürümleri

Olası en iyi deneyimi sağlamak için aracı en düşük sürümü vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın.

#### <a name="supported-oses"></a>Desteklenen işletim sistemleri

Uzantıları framework bir sınır işletim sistemleri için bu uzantıları sahiptir ancak Windows Konuk Aracısı birden çok işletim sistemleri üzerinde çalışır. [Bu makalede] daha fazla bilgi için bkz: (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Bazı uzantılar tüm işletim sistemlerinde desteklenmez ve yayabilir *hata kodu 51, 'Desteklenmeyen işletim sistemi'*. Desteklenebilirlik için tek tek uzantısı belgelerine bakın.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure Storage uzantısı deposundan yüklenir ve uzantı durumu yüklemeleri için Azure Storage gönderilen. Kullanırsanız [desteklenen](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) aracıların sürümünü, gereksinim aracı iletişimi aracı iletişimi için Azure yapı denetleyicisi yeniden yönlendirmek için kullanabileceğiniz gibi Azure Storage VM bölgede erişmesine izin vermek. Bir desteklenmeyen Aracı sürümünde varsa, Azure depolama bu bölgede sanal makineden giden erişime izin vermeniz gerekiyor.

> [!IMPORTANT]
> Erişimi engellemişse *168.63.129.1* Konuk Güvenlik Duvarı'nı kullanarak, ardından uzantıları yukarıdaki yedeklemiş başarısız.

Aracıları yalnızca uzantısı paketleri ve raporlama durumu indirmek için kullanılabilir. Örneğin, bir uzantı yükleme komut dosyası (özel komut dosyası) Github'dan karşıdan yüklemek gereken veya Azure Storage (Azure yedekleme) sonra erişim ek gerekiyorsa Güvenlik Duvarı/güvenlik ağ grubu bağlantı noktalarının açılması gerekir. Uygulamaları kendi sağ olduğundan farklı uzantıları farklı gereksinimleri vardır. Azure depolamaya erişim gerektiren uzantılar, Azure NSG hizmet etiketleri kullanarak erişim izni verebilirsiniz [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Windows Konuk Aracısı proxy sunucusu, aracı trafiği isteklerini yeniden yönlendirmek destek yok.

## <a name="discover-vm-extensions"></a>VM uzantıları Bul

Birçok farklı VM uzantıları, Azure sanal makineler ile kullanmak için kullanılabilir. Tam listesini görmek için [Get-AzureRmVMExtensionImage](/powershell/module/azurerm.compute/get-azurermvmextensionimage). Aşağıdaki örnekte tüm kullanılabilir uzantıları listeler *WestUS* konumu:

```powershell
Get-AzureRmVmImagePublisher -Location "WestUS" | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>VM uzantıları çalıştırın

Azure VM uzantıları, yapılandırma değişikliklerini yapın veya zaten dağıtılmış bir VM'de bağlantısı kurtarmak gerektiğinde faydalı olan mevcut Vm'lerinde çalıştırın. VM uzantıları, Azure Resource Manager şablonu dağıtımlarında da gönderilebilir. Resource Manager şablonları ile uzantılarını kullanarak Azure VM'ler dağıtılabilir ve dağıtım sonrası müdahalesi olmadan yapılandırılmış.

Aşağıdaki yöntemlerden bir uzantı karşı mevcut bir VM'yi çalıştırmak için kullanılabilir.

### <a name="powershell"></a>PowerShell

Birkaç PowerShell komutları tek tek uzantıların çalıştırmak için mevcut. Listesini görmek için [Get-Command](/powershell/module/microsoft.powershell.core/get-command) ve filtre *uzantısı*:

```powershell
Get-Command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Bu, aşağıdakine benzer bir çıktı sağlar:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVmssDiskEncryptionExtension             4.5.0      AzureRM.Compute
```

Aşağıdaki örnek, bir komut dosyası hedef sanal makine üzerine GitHub deposunu indirin ve komut dosyasını çalıştırmak için özel betik uzantısı kullanır. Özel betik uzantısı hakkında daha fazla bilgi için bkz: [özel betik uzantısı genel bakış](custom-script-windows.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

Aşağıdaki örnekte, bir Windows VM yönetici parolasını geçici bir parola sıfırlama için VM erişim uzantısı kullanılır. VM erişim uzantısı ile ilgili daha fazla bilgi için bkz: [Windows VM Uzak Masaüstü'nü Sıfırla Hizmeti'nde](../windows/reset-rdp.md). Bu çalıştırdıktan sonra ilk oturum açmada parola sıfırlama:

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

`Set-AzureRmVMExtension` Komutu, tüm VM uzantısı başlatmak için kullanılabilir. Daha fazla bilgi için bkz: [kümesi AzureRmVMExtension başvuru](https://msdn.microsoft.com/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure portalına

VM uzantıları, mevcut bir VM'yi Azure Portalı aracılığıyla uygulanabilir. Portalda VM seçin, **uzantıları**seçeneğini belirleyip **Ekle**. Kullanılabilir uzantılar listeden istediğiniz ve Sihirbazı'ndaki yönergeleri izleyin uzantı seçin.

Aşağıdaki örnek, Azure portalından Microsoft Antimalware uzantının yüklenmesi gösterir:

![Kötü amaçlı yazılımdan koruma uzantısını yükleyin](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları, bir Azure Resource Manager şablonu eklenir ve şablon dağıtımı ile yürütüldü. Bir şablon uzantılı dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturamazsınız. Örneğin, aşağıdaki JSON alınmış bir Kaynak Yöneticisi'nden şablonu bir dizi yük dengeli sanal makineleri ve Azure SQL Veritabanını dağıtır ve ardından her VM .NET Core uygulamayı yükler. VM uzantısı yazılım yüklemesi mvc'deki.

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

Resource Manager şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma Windows VM uzantıları ile](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>VM uzantısı verileri güvenli

VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarlarını gibi hassas bilgileri içerecek şekilde gerekli olabilir. Birçok VM uzantıları verileri şifreler ve yalnızca hedef VM içinde şifresini çözer korumalı bir yapılandırmayı içerir. Belirli korumalı yapılandırma şeması her uzantısına sahip ve her uzantıya özgü belgelerinde ayrıntılı olarak gösterilmiştir.

Aşağıdaki örnek, Windows için özel betik uzantısı örneğini gösterir. Komutun yürütülmesi için kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifreli değil:

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

Taşıma **yürütülecek komut** özelliğine **korumalı** yapılandırma aşağıdaki örnekte gösterildiği gibi yürütme dize güvenliğini sağlar:

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

### <a name="how-do-agents-and-extensions-get-updated"></a>Aracılar ve uzantıları güncelleştirilme?

Uzantıları ve aracıları aynı güncelleştirme mekanizması paylaşır. Bazı güncelleştirmeler, ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanıma hazır, bir değişiklik uzantılarını ve diğer VM Model değişiklikleri gibi olduğunda yalnızca VM yüklenir:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama kapsayıcısı
- Konuk işletim sistemi gizli
- VM boyutu
- Ağ profili

Olası farklı sürümleri üzerinde farklı bölgelerdeki VM'ye sahip olacak şekilde yayımcılar güncelleştirmeleri farklı zamanlarda bölgelere kullanıma sunma.

#### <a name="listing-extensions-deployed-to-a-vm"></a>Bir VM'ye dağıtılan uzantılarını listeleme

```powershell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Windows Konuk Aracısı'nı yalnızca içeren *uzantısı işleme kod*, *Windows sağlama kod* ayrıdır. Windows Konuk aracısını kaldırabilirsiniz. Pencere Konuk Aracısı otomatik güncelleştirilmesini devre dışı bırakılamıyor.

*Uzantısı işleme kod* Azure doku ile iletişim kurmasını ve VM uzantıları işlemleri gibi işleme, durum bildirimi, tek tek uzantıların güncelleştirme ve bunları kaldırma yüklemeler için sorumludur. Güncelleştirmeleri, güvenlik düzeltmeleri, hata düzeltmeleri ve geliştirmeler içerir *uzantısı işleme kod*.

Çalıştırdığınız hangi sürümü denetlemek için bkz: [algılama Windows Konuk aracısı yüklü](agent-windows.md#detect-the-vm-agent).

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirme kullanılabilir olduğunda, Windows Konuk Aracısı'nı yükler ve uzantı yükseltir. Otomatik uzantısı güncelleştirmelerin ya da *küçük* veya *düzeltme*. Kabul ya da uzantıları dışında opt *küçük* uzantısı sağladığınızda güncelleştirir. Aşağıdaki örnek Resource Manager şablonu ile ikincil sürümlerinde otomatik olarak Yükselt gösterilmektedir *autoUpgradeMinorVersion ": true,'*:

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

En son alt sürüm hata düzeltmeleri almak için otomatik güncelleştirme her zaman uzantısı dağıtımlarınızı seçin kullanmamanız önerilir. Güvenlik veya anahtar hata düzeltmeleri taşımak düzeltme güncelleştirmelerini geri çevrildi olamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri belirleme

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Uzantı üzerinde bir VM ile aynı autoUpgradeMinorVersion ayarlarsanız tanımlama

Uzantı 'autoUpgradeMinorVersion' ile sağlanan durumunda VM modelden görebilirsiniz. Denetlemek için kullanın [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) ve VM ve kaynak grubu adı aşağıdaki gibi belirtin:

```powerShell
 $vm = Get-AzureRmVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

Aşağıdaki örnek çıkış gösterir *autoUpgradeMinorVersion* ayarlanır *true*:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Bir autoUpgradeMinorVersion oluştuğunda tanımlama

Aracısı'nı gözden geçirin, uzantı için bir güncelleştirme gerçekleştiği bakın oturum açtığında VM *C:\WindowsAzure\Logs\WaAppAgent.log*

Aşağıdaki örnekte, VM vardı *Microsoft.Compute.CustomScriptExtension 1.8* yüklü. Bir düzeltme sürümü için kullanılabilir *1.9*:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Aracı izinleri

Görevleri gerçekleştirmek için aracı olarak çalıştırmak gerek duyduğu *yerel sistem*.

## <a name="troubleshoot-vm-extensions"></a>VM uzantıları sorun giderme

Her VM uzantısı, sorun giderme adımları belirli uzantısına sahip olabilir. Örneğin, özel betik uzantısı kullandığınızda, komut dosyası yürütme ayrıntılarını yerel olarak uzantısı çalıştırdığı VM üzerinde bulunabilir. Uzantı özel sorun giderme işlemleri uzantıya özgü belgelerinde açıklanmıştır.

Tüm VM uzantıları için aşağıdaki sorun giderme adımlarını uygulayın.

1. Windows Konuk Aracısı günlüğünü denetlemek için uzantınızı içinde hazırlandığında faaliyeti Ara *C:\WindowsAzure\Logs\WaAppAgent.txt*

2. Daha ayrıntılı bilgi için gerçek uzantı günlükleri denetleyin *C:\WindowsAzure\Logs\Plugins\<UzantıAdı >*

3. Hata kodları, bilinen sorunlar vb. için uzantı belirli belgeleri sorun giderme bölümleri denetleyin.

4. Sistem günlüklerine bakın. Özel Paket Yöneticisi erişim gerektiren başka bir uygulamanın uzun süren bir yükleme gibi uzantıya sahip uğratmıştır diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hataları yaygın nedenler

1. Uzantılara sahip çalıştırmak için 20 dakika (özel durumlar: CustomScript uzantıları, Chef ve 90 dakika sahip DSC). Bu süre, dağıtımınızı aşarsa, bir zaman aşımı olarak işaretlenir. Bunun nedeni düşük kaynak VM'ler, diğer VM yapılandırmaları/başlangıç uzantısı sağlamak okunurken yüksek miktarda kaynak kullanan görevler nedeniyle olabilir.

2. Minimum Önkoşullar karşılanmadı. Bazı uzantılar HPC görüntüleri gibi VM SKU'larında bağımlılıkları vardır. Uzantılar, Azure Storage veya kamu hizmetleri için iletişim gibi belirli ağ erişim gereksinimleri gerektirebilir. Diğer örnekler paket depoları, disk alanı veya güvenlik kısıtlamaları dışında çalışan erişimi olabilir.

3. Özel Paket Yöneticisi erişim. Bazı durumlarda, bir uzun süre çalışan VM yapılandırma ve genişletme yüklemesinin çakışan, burada her ikisi de özel Paket Yöneticisi erişmesi karşılaşabilirsiniz.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüle

VM uzantısı bir VM'ye karşı çalıştırdıktan sonra kullanmak [Get-AzureRmVM ](/powershell/module/azurerm.compute/get-azurermvm) uzantısı durumuna döndürmek için. *Alt durumlar [0]* uzantısı sağlama, yani VM, dağıtılmış, BT'nin başarılı başarılı oldu ancak VM uzantısı yürütülmesi başarısız oldu, gösterir *alt durumlar [1]*.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM" -Status
```

Çıktı aşağıdaki örnek çıkış benzer:

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

Uzantı yürütme durumu de Azure portalında bulunabilir. Uzantı durumunu görüntülemek için VM seçin, **uzantıları**, istenen uzantıyı seçin.

### <a name="rerun-vm-extensions"></a>VM uzantıları yeniden çalıştırın

VM uzantısı yeniden çalıştırılması gereken durumlar olabilir. Uzantı, kaldırarak ve tercih ettiğiniz yürütme yöntemiyle uzantısı yeniden çalıştırma çalıştırabilirsiniz. Bir uzantıyı kaldırmak için kullanın [Kaldır AzureRmVMExtension](/powershell/module/AzureRM.Compute/Remove-AzureRmVMExtension) gibi:

```powershell
Remove-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

Ayrıca bir uzantı Azure portalında şu şekilde kaldırabilirsiniz:

1. Bir VM'yi seçin.
2. Seçin **uzantıları**.
3. İstenen uzantıyı seçin.
4. Seçin **kaldırma**.

## <a name="common-vm-extensions-reference"></a>Ortak VM uzantıları başvurusu
| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Windows için özel betik uzantısı |Bir Azure sanal makinesi karşı komut dosyalarını çalıştır |[Windows için özel betik uzantısı](custom-script-windows.md) |
| Windows için DSC uzantısı |PowerShell DSC (İstenen durum Yapılandırması ') uzantısı |[Windows için DSC uzantısı](dsc-overview.md) |
| Azure Tanılama Uzantısı |Azure tanılama yönetme |[Azure Tanılama Uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM erişim uzantısı |Kullanıcılar ve kimlik bilgilerini yönetme |[Linux VM erişim uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz: [Azure sanal makine uzantıları ve özellikleri genel bakış](overview.md).