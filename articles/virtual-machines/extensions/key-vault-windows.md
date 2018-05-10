---
title: Windows için anahtar kasası VM uzantısı | Microsoft Docs
description: Bir sanal makine uzantısı kullanılarak sanal makinelere anahtar kasası gizli otomatik yenilemenin gerçekleştirme bir aracı dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: dragav
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: dragosav
ms.openlocfilehash: 19e177f086ea9fa817a36e67ace77aed39ee89b5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Windows için anahtar kasası sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

Anahtar kasası VM uzantısı gizli anahtarları Azure anahtar kasasında depolanan otomatik yenilemenin sağlar. Özellikle, anahtar kasalarını ve, bir değişiklik algılama üzerinde depolanan sertifikaların listesini gözlenen uzantısı izleyiciler alır ve karşılık gelen sertifikaları yükler. Anahtar kasası VM uzantısı yayımlanan ve Windows sanal makineleri üzerinde şu anda Microsoft tarafından desteklenir. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için anahtar kasası VM uzantısı için dağıtım seçeneklerini ayrıntıları. 

## <a name="prerequisites"></a>Önkoşullar

Anahtar kasası VM uzantısı anahtar kasası hizmetine kendi kimliğini doğrulamak için Yönetilen hizmet kimliği VM uzantısı bağlıdır. Lütfen daha fazla ayrıntı için MSI VM uzantısı için belgelere bakın.

### <a name="operating-system"></a>İşletim sistemi

Anahtar kasası VM uzantısı şu anda tüm Windows sürümlerini destekler.

| Dağıtım | Sürüm |
|---|---|
| Windows | Çekirdek |

### <a name="internet-connectivity"></a>İnternet bağlantısı

Windows için anahtar kasası VM uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON anahtar kasası VM uzantısı için şemayı gösterir. Uzantı korumalı ayarları gerektirmez - tüm ayarları güvenlik etkisi olmadan bilgi olarak kabul edilir. Uzantı izlenen parolaları, yoklama sıklığı ve hedef sertifika deposu listesini gerektirir. Bu avantajlar şunlardır:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2016-10-01 | tarih |
| Yayımcı | Microsoft.Azure.KeyVault.Edp | dize |
| type | KeyVaultForWindows | dize |
| typeHandlerVersion | 0.0 | Int |
| pollingIntervalInS | 3600 | Int |
| SertifikaDeposuAdı | UYGULAMAM | dize |
| CertificateStoreLocation  | LOCAL_MACHINE | dize |
| observedCertificates  | ["https://myvault.vault.azure.net:443/secrets/mycertificate"] | Dize dizisi


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Bir dağıtırken şablonları ideal veya dağıtım sonrası gerektiren daha fazla sanal makine sertifikaları yenileyin. Uzantı dağıtılabilir, tek tek sanal makineleri veya VM ölçek kümeleri. Şema ve yapılandırma, her iki şablon türleri için ortaktır. 

Sanal makine uzantısı JSON yapılandırması şablonu, sanal makine kaynak parçanın içinde özellikle iç içe gerekir `"resources": []` sanal makinenin nesnesi.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2016-10-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForWindows",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, e.g.: "LOCAL_MACHINE">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net:443/secrets/mycertificate"
                }         
            }
      }
    }
```

## <a name="azure-powershell-deployment"></a>Azure PowerShell dağıtım

PowerShell Azure anahtar kasası VM uzantısı bir varolan sanal makine veya sanal makine ölçek kümesini dağıtmak için kullanılabilir. 

* VM uzantısı dağıtmak için:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMName> + '/KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Set-AzureRmVmExtension -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Uzantı örneği için Service Fabric kümesinin parçası olarak bir VM ölçek kümesi üzerinde dağıtmak için:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = <VMSSName> + '_KeyVaultForWindows' 
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForWindows"
    
        # Start the deployment
        Add-AzureRmVmssExtension -VirtualMachineScaleSet <VMSS> -Name $extName -Publisher $extPublisher -Type $extType -Setting $settings
    
    ```

Lütfen aşağıdaki kısıtlamaları/gereksinimlerini unutmayın:
- Orta Güney ABD bölgesinde dağıtım yapılması gerekir
- Anahtar kasası sınırlamaları:
    - Dağıtım zamanında mevcut olması gerekir 
    - Aynı bölge ve kaynak grubu dağıtım olarak bulunması gerekir
    - Dağıtım ve şablon dağıtımı için etkinleştirilmelidir.

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Anahtar kasası VM uzantısı'nda Azure CLI dağıtmak için örnekleri kısa bir süre sonra kullanılabilir olacak. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure PowerShell kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure PowerShell kullanarak şu komutu çalıştırın.

```powershell
Get-AzureRMVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

Uzantı yürütme çıktısını aşağıdaki dosyasına kaydedilir:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.Edp.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
