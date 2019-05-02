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
ms.date: 02/11/2019
ms.author: roiyz
ms.openlocfilehash: 5a184c72da8af0d451902a164c8b71a94a01883f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683170"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Linux için NVIDIA GPU sürücüsünün uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Linux N serisi Vm'lerde NVIDIA GPU sürücüleri yükler. VM ailesi bağlı olarak, uzantı CUDA veya kılavuz sürücüleri de yükler. NVIDIA yüklediğinizde bu uzantıyı kullanan sürücüler, kabul etme ve koşullarını kabul etmiş [NVIDIA son kullanıcı lisans sözleşmesi](https://go.microsoft.com/fwlink/?linkid=874330). Yükleme işlemi sırasında sürücü kurulumu tamamlamak için VM yeniden başlatılabilir.

Bir uzantı NVIDIA GPU sürücüleri yüklemek de kullanılabilir [Windows N serisi Vm'lerde](hpccompute-gpu-windows.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı, belirli işletim sistemi sürümü için sürücü desteğine bağlı olarak aşağıdaki işletim sistemi dağıtım paketlerini destekler.

| Dağıtım | Version |
|---|---|
| Linux: Ubuntu | 16.04 LTS, 18.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4, 7.5, 7.6 |
| Linux: CentOS | 7.3, 7.4, 7.5, 7.6 |

### <a name="internet-connectivity"></a>İnternet bağlantısı

Microsoft Azure uzantısı için NVIDIA GPU sürücüleri, hedef sanal Makineyi internet'e bağlı ve erişimi gerektirir.

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
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Özellikler

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Yayımcı | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.2 | int |

### <a name="settings"></a>Ayarlar

Tüm ayarlar isteğe bağlıdır. Varsayılan davranış çekirdek güncelleştirmeyi olmasa bile sürücü yükleme için gerekli, en son desteklenen sürücü ve CUDA Araç Seti (uygunsa gibi) yükleyin.

| Ad | Açıklama | Varsayılan Değer | Geçerli Değerler | Veri Türü |
| ---- | ---- | ---- | ---- | ---- |
| updateOS | Çekirdek sürücüsü yüklemesi için gerekli değildir, güncelleştirme | false | TRUE, false | boole |
| driverVersion | NV: Kılavuz sürücü sürümü<br> NC/ND: CUDA Araç Seti sürüm. Seçilen CUDA için en son sürücüleri otomatik olarak yüklenir. | en son | KILAVUZ: "418.70", "410.92", "410.71", "390.75", "390.57", "390.42"<br> CUDA: "10.0.130", "9.2.88", "9.1.85" | string |
| installCUDA | CUDA Toolkit'i yükle. NC/ND serisi VM'ler için yalnızca ilgilidir. | true | TRUE, false | boole |


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
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnek, yukarıdaki Azure Resource Manager ve PowerShell örnekleri yansıtır ve ayrıca varsayılan olmayan sürücü yüklemesi için örnek olarak özel ayarları ekler. Özellikle, işletim sistemi çekirdek güncelleştirir ve belirli bir CUDA Araç Seti sürüm sürücü yükler.

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
    "updateOS": true, `
    "driverVersion": "9.1.85", `
  }'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
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
| 1 | Uzantı yanlış kullanımı | Yürütme çıktı günlüğüne bakın |
| 10 | Linux Tümleştirme hizmetleri Hyper-V ve Azure'da kullanılabilir veya yüklü değil | Lspci onay çıktısı |
| 11 | NVIDIA GPU üzerinde bu VM boyutu bulunamadı. | Kullanım bir [desteklenen VM boyutu ve işletim sistemi](../linux/n-series-driver-setup.md) |
| 12 | Görüntü teklifi desteklenmiyor |
| 13 | VM boyutu desteklenmiyor | N serisi VM dağıtmak için kullanın |
| 14 | İşlem başarısız | Yürütme çıktı günlüğüne bakın |


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantıları hakkında daha fazla bilgi için bkz. [Linux yönelik sanal makine uzantılarına ve özelliklerine](features-linux.md).

N serisi VM'ler hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](../linux/sizes-gpu.md).
