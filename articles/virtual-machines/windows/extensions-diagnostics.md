---
title: Windows Azure tanılama uzantısını | Microsoft Docs
description: Windows Azure tanılama uzantısını kullanarak Azure Windows VM izleme
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: a9621f6f93d8e14e53cfe05ca4a714e88b9d8572
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30928719"
---
# <a name="windows-azure-diagnostics-extension"></a>Windows Azure tanılama uzantısını

## <a name="overview"></a>Genel Bakış

Windows Azure tanılama VM uzantısı, Windows sanal makineden performans sayaçları ve olay günlüklerini gibi izleme verilerini toplamanıza olanak sağlar. Toplamak istediğiniz ve bir Azure Storage hesabı veya bir Azure Event Hub gibi gitmek için verilerin istediğiniz hangi verilerin granularly belirtebilirsiniz. Bu veriler, Azure portalında grafikleri oluşturma veya ölçüm uyarıları oluşturmak için de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Windows Azure tanılama uzantısını Windows 10 istemci, Windows Server 2008 R2, 2012, 2012 R2 ve 2016 karşı çalıştırabilirsiniz.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Windows Azure tanılama uzantısını hedef sanal makine internet'e bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı Şeması

[Bu belgede Windows Azure tanılama uzantısını şema ve özellik değerleri açıklanmaktadır.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablonu dağıtım sırasında Windows Azure tanılama uzantısını çalıştırmak için kullanılabilir. Bkz: [kullanım izleme ve tanılama Windows VM ve Azure Resource Manager şablonları ile](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, varolan bir sanal makineye Windows Azure tanılama uzantısını dağıtmak için kullanılabilir. Korumalı ayarları ve özellikleri yukarıdaki uzantısı şemadan geçerli JSON ile değiştirin. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureRmVMDiagnosticsExtension` Komutu, Windows Azure tanılama uzantısını varolan bir sanal makineye eklemek için kullanılabilir. Ayrıca bkz. [kullanım Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için PowerShell](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure CLI kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure CLI kullanarak şu komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Bu makaleye bakın](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Windows Azure tanılama uzantısını için daha kapsamlı sorun giderme kılavuzu.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki Adımlar
* [Windows Azure tanılama uzantısını hakkında daha fazla bilgi edinin](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Uzantı Şeması ve sürümlerini gözden geçirin](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)
