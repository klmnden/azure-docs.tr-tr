---
title: Azure Linux Aracısı uzantısı yeniden izlemesine stackify | Microsoft Docs
description: Stackify yeniden izlemesine Linux Aracısı bir Linux sanal makinesine dağıtın.
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
ms.author: roiyz
ms.openlocfilehash: b9c035c1c9088957f59550bf6564cc02bc7972f4
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58792431"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Stackify yeniden izlemesine Linux Aracısı uzantısı

## <a name="overview"></a>Genel Bakış

Stackify bulun ve sorunları hızlı bir şekilde çözmek için uygulamayla ilgili ayrıntıları izleyen ürünleri sağlar. Geliştirici takımlar, yeniden izlemesine, tamamen tümleşik, çok ortamı, uygulama performans Süper gücünü içindir. Bunu her geliştirme ekibinin ihtiyaç duyduğu çeşitli araçları bir araya getirir.

Yeniden izlemesine tek bir platformdaki tüm ortamlardaki tüm aşağıdaki özellikleri sunan yalnızca aracıdır.

* Uygulama performansı Yönetimi (APM)
* Uygulama ve sunucu günlüğü
* Hata izleme ve izleme
* Sunucu, uygulama ve özel ölçümler

**Linux Aracısı uzantısı hakkında Stackify**

Bu uzantı, yeniden izlemesine için Linux aracısı için bir yükleme yolu sağlar. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi 

Yeniden izlemesine aracı bu Linux dağıtımları karşı çalıştırabilirsiniz

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS, 16.10 ve 17.04 |
| Debian | 7,9 + ve 8.2 +, 9 |
| Red Hat | 6.7+, 7.1+ |
| CentOS | 6.3+, 7.0+ |

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için aracıyı Stackify uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

Stackify'i bağlantılara izin vermek için bkz. ağ yapılandırmanızı ayarlamanız gerekebilir https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewall. 


## <a name="extension-schema"></a>Uzantı şeması

---

Aşağıdaki JSON şema Stackify yeniden izlemesine Aracı Uzantısı gösterir. Uzantı gerektirir `environment` ve `activationKey`.

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

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında yeniden izlemesine Linux Aracısı Stackify uzantısı çalıştırmak için kullanılabilir.  

Sanal makine uzantısı için JSON içinde sanal makine kaynağı iç içe veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yerleşimini etkiler. Daha fazla bilgi için küme adı ve türü alt kaynakları için bkz.

Aşağıdaki örnekte, yeniden izlemesine Linux Stackify uzantı içinde sanal makine kaynağı iç içe varsayılır. İç içe uzantısı kaynak, JSON "Kaynaklar" yerleştirilir: sanal makinenin [] nesne.

Uzantı gerektirir `environment` ve `activationKey`.

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

Uzantı JSON şablonu kökünde yerleştirilirken, kaynak adı üst sanal makineye bir başvuru içerir ve iç içe geçmiş yapılandırma türü yansıtır.

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

`Set-AzVMExtension` Komutu, varolan bir sanal makineye yeniden izlemesine Linux Aracısı Stackify sanal makine uzantısını dağıtmak için kullanılabilir. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanan gerekir.

Uzantı gerektirir `environment` ve `activationKey`.

```powershell
$PublicSettings = @{"environment" = "myEnvironment"}
$ProtectedSettings = @{"activationKey" = "myActivationKey"}

Set-AzVMExtension -ExtensionName "Stackify.LinuxAgent.Extension" `
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

Azure CLI aracını yeniden izlemesine Linux Aracısı Stackify sanal makine uzantısı varolan bir sanal makineye dağıtmak için kullanılabilir.  

Uzantı gerektirir `environment` ve `activationKey`.

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
| 80 | Hata etkinleştir | Hizmet kurulumu başarısız oldu |
| 90 | Hata etkinleştir | Hizmet başlatma başarısız oldu |
| 100 | Hata devre dışı bırak | Hizmeti durdurma başarısız oldu |
| 110 | Hata devre dışı bırak | Hizmet kaldırılamadı |
| 120 | Kaldırma hatası | Hizmeti durdurma başarısız oldu |

Daha fazla yardıma ihtiyacınız olursa Stackify'i desteğe başvurabilirsiniz https://support.stackify.com.