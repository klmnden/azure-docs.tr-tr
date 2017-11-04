---
title: "Windows VM görüntüleri seçin | Microsoft Docs"
description: "Yayımcı, teklif, SKU ve sürümü Market VM görüntüleri belirlemek için Azure PowerSHell kullanmayı öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: c9b35ff5f3fbd33639805b5a4f105df32562a691
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Azure PowerShell ile Azure Market'te Windows VM görüntüleri bulma

Bu konuda, Azure Marketi'nde VM görüntüleri bulmak için Azure PowerShell kullanmayı açıklar. Bir Windows VM oluşturduğunuzda bir Market görüntüsü belirtmek için bu bilgileri kullanın.

Yüklü ve en son yapılandırıldığından emin olun [Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Yaygın olarak kullanılan Windows görüntülerinin tablosu
| PublisherName | Sunduğu | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |Veri Merkezi Sunucu Çekirdek 2016 |
| MicrosoftWindowsServer |WindowsServer |Kapsayıcılar ile 2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016 Nano Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008 R2 SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Belirli görüntüleri bulma


Azure Resource Manager ile yeni bir sanal makine oluştururken bazı durumlarda aşağıdaki görüntü özelliklerinin birleşimine sahip bir görüntü belirtmeniz gerekir:

* Yayımcı
* Sunduğu
* SKU

Örneğin, bu değerler içeren kullanın [kümesi AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet'ini veya bir kaynak grubu şablonu içinde belirtmelisiniz oluşturulacak VM türü.

Bu değerleri belirlemek gerekiyorsa, çalıştırabilirsiniz [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), ve [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) gitmek için cmdlet'leri görüntüler. Bu değerler belirler:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

İlk olarak aşağıdaki komutlarla yayımcıları listeleyin:

```azurepowershell-interactive
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Seçtiğiniz yayımcı adını girin ve aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Seçtiğiniz teklif adını girin ve aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Çıktısından `Get-AzureRMVMImageSku` komutu, sahip olduğunuz tüm bilgilerin görüntüsü yeni bir sanal makine için belirtmeniz gerekir.

Aşağıda tam bir örnek gösterilmiştir:

```azurepowershell-interactive
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Çıktı:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

"MicrosoftWindowsServer" yayımcısı için:

```azurepowershell-interactive
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Çıktı:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

"WindowsServer" teklifi için:

```azurepowershell-interactive
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Çıktı:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Bu listeden seçtiğiniz SKU adını kopyaladığınızda, `Set-AzureRMVMSourceImage` PowerShell cmdlet’i veya bir kaynak grubu şablonu için ihtiyacınız olan tüm bilgilere sahip olursunuz.

## <a name="next-steps"></a>Sonraki adımlar
Artık tam olarak kullanmak istediğiniz görüntüyü seçebilirsiniz. Yalnızca bulundu, görüntü bilgileri kullanarak bir sanal makineyi hızlı bir şekilde oluşturmak için bkz: [PowerShell ile Windows sanal makine oluşturma](quick-create-powershell.md).
