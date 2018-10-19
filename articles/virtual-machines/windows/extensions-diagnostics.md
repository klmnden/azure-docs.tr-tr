---
title: Windows için Azure tanılama uzantısı | Microsoft Docs
description: Azure Windows Azure tanılama uzantısı kullanarak Vm'leri izleme
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
ms.openlocfilehash: db4208c8fef27d2e2085e63ba3a986456d0544bf
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429125"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Windows sanal makineler için Azure tanılama uzantısı

## <a name="overview"></a>Genel Bakış

Azure tanılama VM uzantısı, Windows VM'nizi performans sayaçları ve olay günlükleri gibi izleme verilerini toplamanıza olanak tanır. Verileri toplamak istediğiniz ve Git, bir Azure depolama hesabı veya bir Azure olay hub'ı gibi verileri istediğiniz tabletleri ayrıntılı olarak belirtebilirsiniz. Bu veriler, Azure portalında grafikler oluşturun veya ölçüm uyarıları oluşturmak için de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Azure tanılama uzantısı, Windows 10 istemci, Windows Server 2008 R2, 2012, 2012 R2 ve 2016 karşı çalıştırabilirsiniz.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Azure tanılama uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

[Bu belgede Azure tanılama uzantı şeması ve özellik değerleri açıklanmaktadır.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında Azure tanılama uzantısını çalıştırmak için kullanılabilir. Bkz: [kullanımının izlenmesini ve bir Windows VM ve Azure Resource Manager şablonları ile tanılama](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, Azure tanılama uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. Korumalı ayarları ve özellikleri yukarıdaki uzantı şeması geçerli bir JSON ile değiştirin. 

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

`Set-AzureRmVMDiagnosticsExtension` Komutu, Azure tanılama uzantısını varolan bir sanal makineye eklemek için kullanılabilir. Ayrıca bkz: [PowerShell kullanarak Windows çalıştıran bir sanal makine Azure tanılamayı etkinleştirerek](ps-extensions-diagnostics.md).
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

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Bu makaleye bakın](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Azure tanılama uzantısı için daha kapsamlı sorun giderme kılavuzu.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure tanılama uzantısı hakkında daha fazla bilgi edinin](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Uzantı Şeması ve sürümlerini gözden geçirin](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)
