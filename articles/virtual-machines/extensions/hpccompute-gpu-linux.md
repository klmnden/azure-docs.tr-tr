---
title: NVIDIA GPU sürücüsünün uzantısı - Azure Linux sanal makineleri | Microsoft Docs
description: N serisi NVIDIA GPU sürücülerini yüklemek için Microsoft Azure uzantısı, Linux çalıştıran Vm'leri işlem.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/03/2018
ms.author: roiyz
ms.openlocfilehash: d95a1b510411f913a05762494dd48d6a5b6f84fd
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413680"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Linux için NVIDIA GPU sürücüsünün uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Linux N serisi Vm'lerde NVIDIA GPU sürücüleri yükler. VM ailesi bağlı olarak, uzantı CUDA veya kılavuz sürücüleri de yükler. NVIDIA yüklediğinizde bu uzantıyı kullanan sürücüler, kabul etme ve NVIDIA son kullanıcı lisans sözleşmesi koşullarını kabul etmiş olursunuz. Sürücü Kurulumu tamamlamak için yükleme işlemi sırasında sanal makinenizi yeniden başlatabilirsiniz.

Bir uzantı NVIDIA GPU sürücüleri yüklemek de kullanılabilir [Windows N serisi Vm'lerde](hpccompute-gpu-windows.md).

NVIDIA son kullanıcı lisans sözleşmesi koşullarını burada bulunur- https://go.microsoft.com/fwlink/?linkid=874330

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı şu OSs destekler:

| Dağıtım | Sürüm |
|---|---|
| Linux: Ubuntu | 16.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4 |
| Linux: CentOS | 7.3, 7.4 |

### <a name="internet-connectivity"></a>İnternet bağlantısı

Microsoft Azure uzantısı için NVIDIA GPU sürücüleri, hedef sanal makineyi internet'e bağlı ve erişimi gerektirir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema uzantısı gösterir.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.0",
    "settings": {
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.HpcCompute | dize |
| type | NvidiaGpuDriverLinux | dize |
| typeHandlerVersion | 1.0 | int |


## <a name="deployment"></a>Dağıtım


### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu 

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla dağıtım sonrası yapılandırma gerektiren sanal makineler dağıtırken idealdir.

Sanal makine uzantısı için JSON yapılandırma içinde sanal makine kaynağı iç içe geçmiş veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yapılandırma yerleşimini etkiler. Daha fazla bilgi için [ayarlamak için alt kaynakları ad ve tür](../../azure-resource-manager/resource-manager-template-child-resource.md). 

Aşağıdaki örnek, uzantıyı sanal makine kaynağı içinde iç içe varsayar. İç içe uzantısı kaynak, JSON yerleştirildi `"resources": []` sanal makinenin nesne.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.0",
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzureRmVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.0 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.0 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Çıkış kodları

| Çıkış Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 0 | İşlem başarılı |
| 1 | Uzantı yanlış kullanımı. | Yürütme çıkış günlüğü ile desteğe başvurun. |
| 10 | Linux Tümleştirme hizmetleri Hyper-V ve Azure kullanılabilir veya yüklü değil. | Lspci çıkışı denetleyin. |
| 11 | NVIDIA GPU üzerinde bu VM boyutu bulunamadı. | Kullanım bir [VM boyutu ve işletim sistemi desteklenen](../linux/n-series-driver-setup.md). |
| 14 | İşlem başarısız | |
| 21 | Ubuntu'da güncelleştirilemedi | "Sudo apt-get güncelleştir" onay çıktısı |


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantıları hakkında daha fazla bilgi için bkz. [Linux yönelik sanal makine uzantılarına ve özelliklerine](features-linux.md).

N serisi VM'ler hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](../linux/sizes-gpu.md).