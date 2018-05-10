---
title: Azure istenen durum yapılandırması uzantısı işleyici | Microsoft Docs
description: Karşıya yükleme ve bir Azure VM DSC uzantısı kullanılarak bir PowerShell DSC yapılandırmasını Uygula
services: virtual-machines-windows
documentationcenter: ''
author: eshaparmar
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: esparmar
ms.openlocfilehash: b34314951980f7dbe2269119883dec52a90a0587
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC uzantısı

## <a name="overview"></a>Genel Bakış

Windows PowerShell DSC uzantısı yayımlanan ve Microsoft tarafından desteklenmiyor. Uzantıyı yükler ve Azure VM temelinde bir PowerShell DSC yapılandırması uygular. DSC uzantı VM'de alınan DSC yapılandırma yürürlüğe için PowerShell DSC içine çağırır. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için DSC sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

DSC uzantı aşağıdaki işletim sistemlerine destekler

Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows istemci 7/8.1

### <a name="internet-connectivity"></a>İnternet bağlantısı

DSC uzantı Windows için hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema DSC uzantısı'nın ayarlarını kısmı için bir Azure Resource Manager şablonu gösterir. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.73",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.Powershell.DSC | dize |
| type | DSC | dize |
| typeHandlerVersion | 2,73 | Int |

### <a name="settings-property-values"></a>Ayarlar özellik değerleri

| Ad | Veri Türü | Açıklama
| ---- | ---- | ---- |
| settings.wmfVersion | dize | VM üzerinde yüklenmesi gereken Windows Management Framework'ün sürümünü belirtir. 'Son' Bu özelliği ayarlamak WMF en güncel sürümünü yükleyecektir. Bu özellik için yalnızca geçerli olası değerler '4.0', '5.0' ve 'Son' değeridir. Olası değerler tabi güncelleştirmelerdir. 'Son' varsayılan değerdir. |
| Settings.Configuration.URL | dize | DSC yapılandırması zip dosyasını karşıdan yüklemek URL konumu belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir. Settings.Configuration.Script ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir.
| Settings.Configuration.Script | dize | DSC yapılandırması tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası configuration.url özelliği tarafından belirtilen URL'den indirilen ZIP dosyasının kök dizininde olması gerekir. Settings.Configuration.URL ve/veya settings.configuration.script tanımlanmışsa, bu özellik gereklidir.
| Settings.Configuration.Function | dize | DSC yapılandırması adını belirtir. Adlı yapılandırmasını configuration.script tarafından tanımlanan komut dosyasında yer almalıdır. Settings.Configuration.URL ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir.
| settings.configurationArguments | Koleksiyon | DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez.
| settings.configurationData.url | dize | DSC yapılandırmanız için giriş olarak kullanmak için yapılandırma verileri (.pds1) dosyanızı indirileceği URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationDataUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir.
| settings.privacy.dataEnabled | dize | Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik için yalnızca olası değerler şunlardır: 'Enable, 'Devre dışı',' ", veya $null. Bu özellik boş ya da boş bırakarak telemetri etkinleştirir
| settings.advancedOptions.forcePullAndApply | bool | Güncelleştirme ve yenileme modu çekme olduğunda DSC yapılandırmaları yürürlüğe DSC uzantısı sağlar.
| settings.advancedOptions.downloadMappings | Koleksiyon | WMF ve .NET gibi bağımlılıklarını karşıdan yüklemek için alternatif konumlar tanımlar

### <a name="protected-settings-property-values"></a>Korumalı ayarlarını özellik değerleri

| Ad | Veri Türü | Açıklama
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | dize | DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken | dize | Configuration.URL tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken | dize | ConfigurationData.url tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla dağıtım yapılandırma sonrası gerektiren sanal makineler dağıtırken idealdir. OMS Aracısı VM uzantısı içeren bir örnek Resource Manager şablonunu bulunabilir [Azure hızlı başlangıç Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/052db5feeba11f85d57f170d8202123511f72044/dsc-extension-iis-server-windows-vm). 

Sanal makine uzantısı JSON yapılandırması içinde sanal makine kaynağı iç içe geçmiş veya kök veya Resource Manager JSON şablonu en üst düzeyinde yerleştirilir. JSON yapılandırma yerleşimini kaynak adı ve türü değeri etkiler. 

Uzantı kaynak iç içe geçirme sırasında JSON yerleştirilir `"resources": []` sanal makinenin nesnesi. JSON uzantısı şablon kökünde yerleştirirken, kaynak adı üst sanal makine için referans içeriyor ve iç içe geçmiş yapılandırma türü yansıtır.  


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI OMS Aracısı VM uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. OMS anahtarı ve OMS kimliği bu OMS çalışma alanınız ile değiştirin. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name Microsoft.Powershell.DSC \
  --publisher Microsoft.Powershell \
  --version 2.73 --protected-settings '{}' \
  --settings '{}'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure CLI kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure CLI kullanarak şu komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı paketinin karşıdan yüklenir ve bu konuma Azure VM'de dağıtılan
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Uzantı durumu dosyası alt durumunu ve ayrıntılı hata ve Çalıştır her bir uzantı açıklaması ile birlikte durum başarı/hata kodlarını içerir.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Uzantı Çıktı günlükleri şu dizine kaydedilir:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 1000 | Genel hata | Bu hata için ileti uzantısı günlüklerine özgü özel durum tarafından sağlanan |
| 52 | Uzantısı yükleme hatası | Bu hata için ileti belirli bir özel durum nedeniyle sağlanır |
| 1002 | WMF yükleme hatası | WMF yükleme sırasında hata oluştu. |
| 1004 | Geçersiz Zip paketini | Geçersiz zip; Zip paketi açılırken hata oluştu |
| 1100 | Bağımsız Değişken Hatası | Kullanıcı tarafından sağlanan girdi bir sorunu gösterir. Hata iletisinin belirli bir özel durum nedeniyle sağlanır|


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).