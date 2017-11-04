---
title: "Bir Azure VM için şablonu indirin | Microsoft Docs"
description: "Resource Manager dağıtım modelinde dağıtımları otomatikleştirme ile yardımcı olmak için bir VM templatefor indirin"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 9e4c0c3cf0e233447369a24b1d5fe27495abd1cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="download-the-template-for-a-vm"></a>VM için şablon indirme
Azure portal veya PowerShell kullanarak bir VM oluşturduğunuzda, Resource Manager şablonu sizin için otomatik olarak oluşturulur. Bir dağıtımı hızlı bir şekilde çoğaltmak için bu şablonu kullanın. Bu şablon bir kaynak grubundaki tüm kaynakları hakkında bilgi içerir. Bir sanal makine için bu şablonu VM ağ kaynakları da dahil olmak üzere bu kaynak grubundaki desteklenmesi amacıyla oluşturulan her şeyi içeren anlamına gelir.

## <a name="download-the-template-using-the-portal"></a>Portalı kullanarak şablonu indirme
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Hub menüsünde seçin **sanal makineleri**.
3. Listeden sanal makineyi seçin.
4. Seçin **Otomasyon betiğini**.
5. Seçin **karşıdan** ve .zip dosyasını yerel bilgisayarınıza kaydedin.
6. .Zip dosyasını açın ve dosyaları bir klasöre ayıklayın. .Zip dosyasını içerir:
   
   * Deploy.ps1
   * Deploy.sh 
   * deployer.RB
   * DeploymentHelper.cs
   * Parameters.JSON
   * Template.JSON

Template.json şablonu dosyasıdır.

## <a name="download-the-template-using-powershell"></a>PowerShell kullanarak şablonu indirme
.Json şablon dosyasını kullanarak da yükleyebilirsiniz [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet'i. Kullanabileceğiniz `-path` parametresi için .json dosyası yolu ve dosya adı sağlayın. Bu örnek adlı kaynak grubu için şablonu indirmeyi gösteren **myResourceGroup** için **C:\users\public\downloads** klasör, yerel bilgisayarınızda.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak kaynakları dağıtma hakkında daha fazla bilgi için bkz: [Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

