---
title: "Windows sanal makineler için görüntüleri hakkında | Microsoft Docs"
description: "Görüntüleri azure'da Windows sanal makinelerle kullanılma hakkında bilgi edinin."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>Windows sanal makineler için görüntüler hakkında
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bulma ve Resource Manager modelinde görüntüleri kullanma hakkında daha fazla bilgi için bkz: [burada](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>İmajlarla çalışma

Azure aboneliğinize kullanılabilir görüntülerini yönetmek için Azure PowerShell modülü ve Azure Portalı'nı kullanabilirsiniz. Tam olarak görmek veya yapmak istediğinizi sabitleme için Azure PowerShell modülü daha fazla komut seçenekleri sağlar. Azure portal GUI çoğu günlük yönetim görevleri için sağlar.

Burada, Azure PowerShell modülü kullanmak bazı örnekler verilmiştir.

* **Tüm görüntüleri alma**:`Get-AzureVMImage`geçerli aboneliğinizde kullanılabilen tüm görüntülerin listesini döndürür: görüntüleri ve Azure veya iş ortakları tarafından sağlanan. Sonuçta elde edilen listenin büyük olabilir. Sonraki örnekler daha kısa bir listesini almak nasıl gösterir.
* **Görüntü aileleri alma**:`Get-AzureVMImage | select ImageFamily` dizeleri göstererek görüntü aileleri listesini alır **ImageFamily** özelliği.
* **Belirli bir ailesinde tüm görüntüleri alma**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **VM görüntüleri Bul**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` yalnızca VM görüntüleri için geçerli olan DataDiskConfiguration özelliğini filtreleyerek Bu cmdlet'i çalışır. Bu örnek ayrıca yalnızca etiket ve resim adı çıkışı filtreler.
* **Genelleştirilmiş bir yansıma Kaydet**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Özel bir yansıma Kaydet**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > İşletim sistemi diski içerir ve veri diskleri ekli bir VM görüntüsü oluşturmak için OSState parametresi gereklidir. Parametreyi kullanmazsanız, cmdlet bir işletim sistemi görüntüsü oluşturur. Parametresinin değerini gösteren görüntü genelleştirilmiş veya özelleştirilmiş olup olmadığını, temel işletim sistemi diski yeniden kullanılmak üzere hazırlanmıştır olup olmadığı.

* **Bir görüntü Sil**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Sonraki Adımlar
Ayrıca [Azure portalını kullanarak bir Windows makinesine oluşturma](tutorial.md).
