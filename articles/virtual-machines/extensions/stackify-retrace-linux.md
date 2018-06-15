---
title: Azure Linux Aracısı uzantısı yeniden izlemesine stackify | Microsoft Docs
description: Bir Linux sanal makinede Stackify yeniden izlemesine Linux Aracısı'nı dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: darinhoward
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/12/2018
ms.author: danis
ms.openlocfilehash: 376c5a087f74fbe087db9fa2df38b2ba4e6cf1ff
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33943085"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Linux Aracısı uzantısı yeniden izlemesine stackify

## <a name="overview"></a>Genel Bakış
Stackify bulmak ve sorunları hızlıca düzeltmek yardımcı olması için uygulamanız hakkında ayrıntılar izlemek ürünleri sağlar. Geliştirici ekipleri için yeniden izlemesine, tam olarak tümleşik, çok ortam, uygulama performansını üst güç olur. Her geliştirme ekibi ihtiyaç duyduğu çeşitli araçları birleştirir.

Yeniden izlemesine, aşağıdaki özelliklerin tümüne tek bir platformda tüm ortamlar arasında sunar yalnızca aracıdır.

* Uygulama performansı Yönetimi (APM)
* Uygulama ve sunucu günlüğe kaydetme
* Hata izleme ve izleme
* Sunucu, uygulama ve özel ölçümleri

**Yaklaşık Linux Aracısı uzantısı Stackify**

Bu uzantı yeniden izlemesine için Linux aracısı için bir yükleme yolu sağlar. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi 
Yeniden izlemesine Aracısı bu Linux dağıtımları karşı çalıştırabilirsiniz

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS, 16.10 ve 17.04 |
| Debian | 7,9 + ve 8.2 +, 9 |
| RedHat | 6.7 +, 7.1 + |
| CentOS | 6.3 +, 7.0 + |

### <a name="internet-connectivity"></a>İnternet bağlantısı
Linux için Agent Stackify uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

Stackify bağlantılara izin vermek için bkz. ağ yapılandırmanızı ayarlamanız gerekebilir https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewall. 


## <a name="extension-schema"></a>Uzantı şeması
---

Aşağıdaki JSON şeması yeniden izlemesine Aracısı Stackify uzantısı gösterir. Genişletme gerektirip `environment` ve `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

## <a name="template-deployment"></a>Şablon dağıtımı 

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonu yeniden izlemesine Linux Aracısı Stackify uzantısı bir Azure Resource Manager şablon dağıtımı sırasında çalıştırmak için kullanılabilir.  

Bir sanal makine uzantısı için JSON içinde sanal makine kaynağı iç içe geçmiş veya kök veya Resource Manager JSON şablonu en üst düzeyinde yerleştirilir. JSON yerleşimini kaynak adı ve türü değeri etkiler. Daha fazla bilgi için küme adı ve türü alt kaynakları için bkz.

Aşağıdaki örnek, yeniden izlemesine Linux Stackify uzantısı içinde sanal makine kaynağı iç içe geçmiş varsayar. Uzantı kaynak iç içe geçme, JSON "Kaynaklar" yerleştirilir: sanal makinenin [] nesne.

Genişletme gerektirip `environment` ve `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

JSON uzantısı şablon kökünde yerleştirirken, kaynak adı üst sanal makine için referans içeriyor ve iç içe geçmiş yapılandırma türü yansıtır.

```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "<parentVmResource>/StackifyExtension",
        "apiVersion": "[variables('apiVersion')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Stackify.LinuxAgent.Extension",
            "type": "StackifyLinuxAgentExtension",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "environment": "myEnvironment"
            },
            "protectedSettings": {
              "activationKey": "myActivationKey"
            }
        }
    }
```


## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureRmVMExtension` Komutu yeniden izlemesine Linux Aracısı Stackify sanal makine uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanması gerekir.

Genişletme gerektirip `environment` ve `activationKey`.

```
$PublicSettings = @{"environment" = "myEnvironment"}
$ProtectedSettings = @{"activationKey" = "myActivationKey"}

Set-AzureRmVMExtension -ExtensionName "Stackify.LinuxAgent.Extension" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Stackify.LinuxAgent.Extension" `
    -ExtensionType "StackifyLinuxAgentExtension" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım 

Azure CLI Aracı'nı yeniden izlemesine Linux Aracısı Stackify sanal makine uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir.  

Genişletme gerektirip `environment` ve `activationKey`.

```azurecli
az vm extension set --publisher 'Stackify.LinuxAgent.Extension' --version 1.0 --name 'StackifyLinuxAgentExtension' --protected-settings '{"activationKey":"myActivationKey"}' --settings '{"environment":"myEnvironment"}'  --resource-group 'myResourceGroup' --vm-name 'myVmName'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="error-codes"></a>Hata kodları

| Hata kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 10 | Yükleme hatası | wget gereklidir |
| 20 | Yükleme hatası | Python gereklidir |
| 30 | Yükleme hatası | sudo gereklidir |
| 40 | Yükleme hatası | etkinleştirme anahtarı gereklidir |
| 51 | Yükleme hatası | İşletim sistemi distro desteklenmiyor |
| 60 | Yükleme hatası | ortam gereklidir |
| 70 | Yükleme hatası | Bilinmeyen |
| 80 | Hata etkinleştir | Hizmeti kurulumu başarısız oldu |
| 90 | Hata etkinleştir | Hizmet başlatma başarısız oldu |
| 100 | Hata devre dışı | Hizmet durdurulamadı |
| 110 | Hata devre dışı | Hizmet kaldırma başarısız |
| 120 | Kaldırma hatası | Hizmet durdurulamadı |

Daha fazla yardıma gereksinim duyarsanız Stackify desteğine başvurabilirsiniz https://support.stackify.com.