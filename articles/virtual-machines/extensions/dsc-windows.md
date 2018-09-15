---
title: Azure Desired State Configuration uzantısı işleyicisi | Microsoft Docs
description: Karşıya yükleme ve DSC uzantısını kullanarak bir Azure VM'de bir PowerShell DSC yapılandırmasını Uygula
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: robreed
ms.openlocfilehash: b9e96473a6f66dcbc675da1553deaed4ad61b249
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630949"
---
# <a name="powershell-dsc-extension"></a>PowerShell DSC uzantısı

## <a name="overview"></a>Genel Bakış

Windows PowerShell DSC uzantısı yayımlandı ve Microsoft tarafından desteklenmiyor. Uzantıyı yükler ve bir Azure VM'de bir PowerShell DSC yapılandırması uygular. DSC uzantısı, alınan VM DSC yapılandırmasını uygulamak için PowerShell DSC içine yapılan çağrılar. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için DSC sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

DSC uzantısı aşağıdaki işletim sisteminin destekler

Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows istemci 7/8.1

### <a name="internet-connectivity"></a>İnternet bağlantısı

DSC uzantısı Windows için hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema DSC uzantı ayarları bölümü için bir Azure Resource Manager şablonunda gösterir. 

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
| typeHandlerVersion | 2,73 | int |

### <a name="settings-property-values"></a>Ayarlar özellik değerleri

| Ad | Veri Türü | Açıklama
| ---- | ---- | ---- |
| settings.wmfVersion | dize | Sanal makinenizde yüklü Windows Management Framework sürümünü belirtir. 'Son' Bu özelliğin ayarlanması WMF en güncel sürümünü yükleyecektir. Bu özellik için geçerli tek olası değerler '4.0', '5.0' ve 'Son' dir. Bu olası değerler şunlardır: güncelleştirmeleri tabidir. 'Son' varsayılan değerdir. |
| Settings.Configuration.URL | dize | İndirileceği DSC yapılandırma zip dosyanızın URL konumu belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, SAS belirtecinizi için protectedSettings.configurationUrlSasToken özelliği ayarlayın gerekir. Settings.Configuration.Script ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir.
| Settings.Configuration.Script | dize | DSC yapılandırma tanımı içeren komut dosyasının dosya adını belirtir. Bu betik configuration.url özelliği tarafından belirtilen URL'den indirilen ZIP dosyasının kök klasöründe olması gerekir. Settings.Configuration.URL ve/veya settings.configuration.script tanımlanmışsa, bu özellik gereklidir.
| Settings.Configuration.Function | dize | DSC yapılandırma adını belirtir. Adlı yapılandırmasını configuration.script tarafından tanımlanan betiğinde yer almalıdır. Settings.Configuration.URL ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir.
| settings.configurationArguments | Koleksiyon | DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez.
| settings.configurationData.url | dize | DSC yapılandırma için giriş olarak kullanmak için yapılandırma verileri (.pds1) dosyasını indirileceği URL'sini belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, SAS belirtecinizi için protectedSettings.configurationDataUrlSasToken özelliği ayarlayın gerekir.
| settings.privacy.dataEnabled | dize | Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. 'Etkinleştir', 'Disable', bu özellik için yalnızca olası değerler şunlardır: ", veya $null. Bu özellik boş ya da boş bırakarak telemetri etkinleştirir
| settings.advancedOptions.forcePullAndApply | bool | Güncelleştirme ve yenileme modu çekme olduğunda DSC yapılandırmaları uygulamak DSC uzantısı sağlar.
| settings.advancedOptions.downloadMappings | Koleksiyon | WMF ve .NET gibi bağımlılıkları indirmek için alternatif konumlar tanımlar

### <a name="protected-settings-property-values"></a>Korumalı ayarları özellik değerleri

| Ad | Veri Türü | Açıklama
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | dize | DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken | dize | Configuration.URL tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken | dize | ConfigurationData.url tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla dağıtım sonrası yapılandırma gerektiren sanal makineler dağıtırken idealdir. OMS Aracısı VM uzantısını içeren örnek bir Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Galerisine](https://github.com/Azure/azure-quickstart-templates/tree/052db5feeba11f85d57f170d8202123511f72044/dsc-extension-iis-server-windows-vm). 

Sanal makine uzantısı için JSON yapılandırma içinde sanal makine kaynağı iç içe geçmiş veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yapılandırma yerleşimini etkiler. 

İç içe uzantısı kaynak, JSON yerleştirildi `"resources": []` sanal makinenin nesne. Uzantı JSON şablonu kökünde yerleştirilirken, kaynak adı üst sanal makineye bir başvuru içerir ve iç içe geçmiş yapılandırma türü yansıtır.  


## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, mevcut bir sanal makine için OMS Aracısı VM uzantısını dağıtmak için kullanılabilir. OMS anahtarı ve OMS kimliği bu OMS çalışma alanı ile değiştirin. 

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

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı paketi indirilir ve Azure sanal makinesinde bu konuma dağıtılan
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Uzantı durumu dosyası alt durum ve durum başarı/hata kodları yanı sıra ayrıntılı hata ve çalıştırma her bir uzantı açıklamasını içerir.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Uzantı çıkış günlükleri şu dizine kaydedilir:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 1000 | Genel hata | Bu hata için ileti uzantısı günlüklerde belirli özel durum tarafından sağlanan |
| 52 | Uzantı yükleme hatası | Bu hata için ileti belirli özel durum tarafından sağlanır |
| 1002 | WMF yükleme hatası | WMF yükleme hatası oluştu. |
| 1004 | Geçersiz Zip paketini | Geçersiz zip; Zip akışının paketi açılırken hata |
| 1100 | Bağımsız Değişken Hatası | Kullanıcı tarafından sağlanan girdiyi bir sorunu gösterir. Belirli özel durum tarafından sağlanan hata iletisi|


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).