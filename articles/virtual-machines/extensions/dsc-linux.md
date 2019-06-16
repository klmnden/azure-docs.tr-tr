---
title: Linux için Azure DSC uzantısı
description: Desired State Configuration kullanılarak yapılandırılması bir Azure Linux VM izin vermek için OMI ve DSC paketleri yükler.
services: virtual-machines-linux
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: robreed
ms.openlocfilehash: 4b0cd88cbb3729a3e81aeb5d6f43f417c8cb2f17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64682759"
---
# <a name="dsc-extension-for-linux-microsoftostcextensionsdscforlinux"></a>Linux (Microsoft.OSTCExtensions.DSCForLinux) için DSC uzantısı

Desired State Configuration ' nı (DSC), BT yönetmenize olanak sağlayan bir yönetim platformudur ve yapılandırmayı kod olarak ile geliştirme altyapısı.

DSCForLinux uzantısı yayımlandı ve Microsoft tarafından desteklenmiyor. Uzantı, Azure sanal makinelerinde OMI ve DSC aracı yükler. DSC uzantısı, aşağıdaki eylemleri de yapabilirsiniz


- Azure Otomasyonu hizmetine (ExtensionAction kaydetme) yapılandırmalardan çekmek için Azure Otomasyon hesabı Linux VM kaydetme
- Linux VM (anında iletme ExtensionAction) anında iletme MOF yapılandırmaları
- Düğüm yapılandırması (çekme ExtensionAction) çekmek için çekme sunucusunu yapılandırmak için Linux VM meta MOF yapılandırmasını Uygula
- Linux VM (yükleme ExtensionAction) özel DSC modülleri yükleme
- Linux VM (ExtensionAction kaldırmak) için özel DSC modülleri kaldırma

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Tüm Linux DSC uzantı destekler [Azure'da desteklenen Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) hariç:

| Dağıtım | Version |
|---|---|
| Debian | Tüm sürümler |
| Ubuntu| 18.04 |
 
### <a name="internet-connectivity"></a>İnternet bağlantısı

Hedef sanal makineyi internet'e bağlı olduğundan emin DSCForLinux uzantısı gerektirir. Örneğin, kayıt uzantısı Otomasyon hizmeti bağlantısı gerektirir. İstek, istek, gibi diğer eylemleri için azure depolama/github bağlantısı yükleme gerektirir. Bunu, müşteri tarafından sağlanan ayarlarına bağlıdır.

## <a name="extension-schema"></a>Uzantı şeması

### <a name="11-public-configuration"></a>1.1 genel yapılandırma

Tüm desteklenen ortak yapılandırma parametreleri şunlardır:

* `FileUri`: (isteğe bağlı, dize) MOF dosyası/Meta MOF dosyası/özel kaynak ZIP dosyası URI'si.
* `ResourceName`: (isteğe bağlı, dize) özel kaynak modülünün adı
* `ExtensionAction`: (isteğe bağlı, dize) bir uzantı ne yapacağını belirtir. Geçerli değerler: Kayıt, anında iletme, çekme, yükleme, kaldırma. Belirtilmezse, varsayılan olarak anında iletme eylem olarak kabul edilir.
* `NodeConfigurationName`: (isteğe bağlı, dize) uygulamak için bir düğüm yapılandırmasının adı.
* `RefreshFrequencyMins`: (isteğe bağlı, int) DSC çekme sunucusundan yapılandırmalara girişimlerini ne sıklıkla (dakika cinsinden) belirtir. 
       Çekme sunucusu yapılandırmasını hedef düğümde bulunan geçerli hesaptan farklıysa, bekleyen deposuna kopyalanır ve uygulanır.
* `ConfigurationMode`: (isteğe bağlı, dize) DSC yapılandırmasını nasıl uygulanacağını belirtir. Geçerli değerler şunlardır: ApplyOnly, ApplyAndMonitor, ApplyAndAutoCorrect.
* `ConfigurationModeFrequencyMins`: (isteğe bağlı, int) DSC yapılandırmasını istenen durumda olmasını sağlar ne sıklıkla (dakika cinsinden) belirtir.

> [!NOTE]
> Sürüm < 2.3 kullanıyorsanız, mode parametresi ExtensionAction aynıdır. Aşırı yüklenmiş bir terim olacak şekilde modu gibi görünüyor. Bu nedenle, Karışıklığı önlemek için ExtensionAction ve sonraki sürümlerde 2.3 sürümü olarak kullanılıyor. Geriye dönük uyumluluk için uzantı modu hem de ExtensionAction destekler. 
>

### <a name="12-protected-configuration"></a>1.2 korumalı yapılandırma

Tüm desteklenen korumalı yapılandırma parametreleri şunlardır:

* `StorageAccountName`: (isteğe bağlı, dize) dosyasını içeren depolama hesabı adı
* `StorageAccountKey`: (isteğe bağlı, dize) dosyasını içeren depolama hesabı anahtarı
* `RegistrationUrl`: (isteğe bağlı, dize) Azure Otomasyonu hesap URL'si
* `RegistrationKey`: (isteğe bağlı, dize) Azure Otomasyon hesabının erişim anahtarı


## <a name="scenarios"></a>Senaryolar

### <a name="register-to-azure-automation-account"></a>Azure Otomasyon hesabı için kaydolun
protected.json
```json
{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}
```
public.json
```json
{
  "ExtensionAction" : "Register",
  "NodeConfigurationName" : "<node-configuration-name>",
  "RefreshFrequencyMins" : "<value>",
  "ConfigurationMode" : "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins" : "<value>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}'

$publicConfig = '{
  "ExtensionAction" : "Register",
  "NodeConfigurationName": "<node-configuration-name>",
  "RefreshFrequencyMins": "<value>",
  "ConfigurationMode": "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins": "<value>"
}'
```

### <a name="apply-a-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>VM için bir MOF yapılandırma dosyasında (Azure depolama hesabı) uygulama

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}'
```


### <a name="apply-a-mof-configuration-file-in-public-storage-to-the-vm"></a>VM için bir MOF yapılandırma dosyasında (genel depolama alanı) uygulama

public.json
```json
{
  "FileUri": "<mof-file-uri>"
}
```

PowerShell biçimi
```powershell
$publicConfig = '{
  "FileUri": "<mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>Bir meta MOF yapılandırma dosyasında (Azure depolama hesabı) VM'lere uygulanır.

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-public-storage-to-the-vm"></a>Bir meta MOF yapılandırma dosyasında (genel depolama alanı) VM'lere uygulanır.
public.json
```json
{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-azure-storage-account-to-the-vm"></a>Özel kaynak Modülü (ZIP dosyası Azure depolama hesabındaki) VM'ye yükleyin
protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```

PowerShell biçimi
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-public-storage-to-the-vm"></a>Özel kaynak Modülü (genel depolama alanı ZIP dosyasında) VM'ye yükleyin
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="remove-a-custom-resource-module-from-the-vm"></a>Özel kaynak modülü sanal makineden kaldırın.
public.json
```json
{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}
```
PowerShell biçimi
```powershell
$publicConfig = '{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}'
```

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla Azure Otomasyonu'na ekleme gibi dağıtım sonrası yapılandırma gerektiren sanal makineler dağıtırken idealdir. 

Örnek Resource Manager şablonu [201-dsc-linux-azure-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-azure-storage-on-ubuntu) ve [201-dsc-linux-public-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-public-storage-on-ubuntu).

Azure Resource Manager şablonu hakkında daha fazla ayrıntı için ziyaret [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md).


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

### <a name="21-using-azure-cliazure-cli"></a>2.1. Kullanma [**Azure CLI**] [azure-cli]
DSCForLinux uzantısını dağıtmadan önce yapılandırmanız gereken, `public.json` ve `protected.json`, Bölüm 3 farklı senaryolarda göre.

#### <a name="211-classic"></a>2.1.1. Klasik
Klasik mod, Azure hizmet yönetimi modu olarak da adlandırılır. Kendisine çalıştırarak geçebilirsiniz:
```
$ azure config mode asm
```

DSCForLinux uzantısı çalıştırarak dağıtabilirsiniz:
```
$ azure vm extension set <vm-name> DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```

En son uzantısı sürümü öğrenmek için şunu çalıştırın:
```
$ azure vm extension list
```

#### <a name="212-resource-manager"></a>2.1.2. Resource Manager
Çalıştırarak Azure Resource Manager moduna geçebilirsiniz:
```
$ azure config mode arm
```

DSCForLinux uzantısı çalıştırarak dağıtabilirsiniz:
```
$ azure vm extension set <resource-group> <vm-name> \
DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```
> [!NOTE]
> Azure Resource Manager modunda `azure vm extension list` şu an için kullanılamıyor.
>

### <a name="22-using-azure-powershellazure-powershell"></a>2.2. Kullanma [**Azure PowerShell**] [azure powershell]

#### <a name="221-classic"></a>2.2.1 Klasik

Azure hesabınıza (Azure Hizmet Yönetimi modunda) çalıştırarak oturum:

```powershell>
Add-AzureAccount
```

Ve çalıştırarak DSCForLinux uzantısı dağıtın:

```powershell>
$vmname = '<vm-name>'
$vm = Get-AzureVM -ServiceName $vmname -Name $vmname
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Yukarıdaki bölümde farklı senaryolarda göre $publicConfig ve $privateConfig içeriği değiştirmeniz gerekir 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzureVMExtension -ExtensionName $extensionName -VM $vm -Publisher $publisher `
  -Version $version -PrivateConfiguration $privateConfig `
  -PublicConfiguration $publicConfig | Update-AzureVM
```

#### <a name="222resource-manager"></a>2.2.2.Resource Manager

Azure hesabınıza (Azure Resource Manager moduna) çalıştırarak oturum:

```powershell>
Login-AzAccount
```

Tıklayın [ **burada** ](../../azure-resource-manager/manage-resources-powershell.md) Azure PowerShell'i Azure Resource Manager ile kullanma hakkında daha fazla bilgi için.

DSCForLinux uzantısı çalıştırarak dağıtabilirsiniz:

```powershell>
$rgName = '<resource-group-name>'
$vmName = '<vm-name>'
$location = '< location>'
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Yukarıdaki bölümde farklı senaryolarda göre $publicConfig ve $privateConfig içeriği değiştirmeniz gerekir 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzVMExtension -ResourceGroupName $rgName -VMName $vmName -Location $location `
  -Name $extensionName -Publisher $publisher -ExtensionType $extensionName `
  -TypeHandlerVersion $version -SettingString $publicConfig -ProtectedSettingString $privateConfig
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Hata kodu: 51 desteklenmeyen distro ya da desteklenmeyen bir uzantı eylemi temsil eder.
Bazı durumlarda, DSC uzantı OMI daha yüksek bir sürümü zaten olduğunda OMI yüklenmesi başarısız Linux makine var. [hata yanıtı: (000003) İndirgeme izin yok]



### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantıları hakkında daha fazla bilgi için bkz. [Linux yönelik sanal makine uzantılarına ve özelliklerine](features-linux.md).
