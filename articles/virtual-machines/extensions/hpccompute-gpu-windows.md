---
title: NVIDIA GPU sürücüsünün uzantısı - Azure Windows sanal makineleri | Microsoft Docs
description: N serisi NVIDIA GPU sürücülerini yüklemek için Microsoft Azure uzantısı, Windows çalıştıran VM'ler işlem.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: roiyz
ms.openlocfilehash: 004d6125de6762303db91f3a5ef9ffa16e6e501f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705948"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>Windows için NVIDIA GPU sürücüsünün uzantısı

## <a name="overview"></a>Genel Bakış

Bu uzantı, Windows N serisi Vm'lerde NVIDIA GPU sürücüleri yükler. VM ailesi bağlı olarak, uzantı CUDA veya kılavuz sürücüleri de yükler. NVIDIA yüklediğinizde bu uzantıyı kullanan sürücüler, kabul etme ve koşullarını kabul etmiş [NVIDIA son kullanıcı lisans sözleşmesi](https://go.microsoft.com/fwlink/?linkid=874330). Yükleme işlemi sırasında sürücü kurulumu tamamlamak için VM yeniden başlatılabilir.

El ile yükleme yönergeleri sürücüleri ve geçerli desteklenen sürümler kullanılabilir [burada](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup).
Bir uzantı NVIDIA GPU sürücüleri yüklemek de kullanılabilir [Linux N serisi Vm'lerde](hpccompute-gpu-linux.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Bu uzantı şu OSs destekler:

| Dağıtım | Version |
|---|---|
| Windows 10 | Core |
| Windows Server 2016 | Core |
| Windows Server 2012R2 | Core |

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
    "type": "NvidiaGpuDriverWindows",
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
| publisher | Microsoft.HpcCompute | dize |
| type | NvidiaGpuDriverWindows | dize |
| typeHandlerVersion | 1.2 | int |


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
    "type": "NvidiaGpuDriverWindows",
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
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
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

Uzantı yürütme çıktısı, aşağıdaki dizine kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverMicrosoft\
```

### <a name="error-codes"></a>Hata kodları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 0 | İşlem başarılı |
| 1\. | İşlem başarılı. Yeniden başlatma gerekiyor. |
| 100 | İşlem değil, desteklenen veya tamamlanamadı. | Olası nedenler: PowerShell sürümü desteklenmiyor, VM boyutu, veri indirme hatası bir N-serisi VM değil. Hatanın nedenini belirlemek için günlük dosyalarına bakın. |
| 240, 840 | İşlem zaman aşımı. | İşlemi yeniden deneyin. |
| -1 | Özel durum oluştu. | Özel durumun nedenini belirlemek için günlük dosyalarına bakın. |
| -5 x | İşlem nedeniyle yeniden başlatma kesildi. | VM'yi yeniden başlatın. Yükleme yeniden başlatıldıktan sonra devam edecek. Kaldırma el ile çağrılmalıdır. |


### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantıları hakkında daha fazla bilgi için bkz. [sanal makine uzantıları ve özellikleri Windows için](features-windows.md).

N serisi VM'ler hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](../windows/sizes-gpu.md).
