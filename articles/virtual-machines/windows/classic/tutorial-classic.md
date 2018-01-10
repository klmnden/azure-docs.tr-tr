---
title: "Azure portalında bir VM oluşturma | Microsoft Docs"
description: "Azure Portal'da Windows sanal makine oluşturun."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 99a67821ab926983205e2327c428e854d20a0cf5
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Windows Azure portalında çalışan bir sanal makine oluşturma
> [!div class="op_single_selector"]
> * [Azure portalı](tutorial.md)
> * [PowerShell: Klasik dağıtım](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bilgi nasıl [Resource Manager dağıtım modelini kullanarak bu adımları uygulamadan](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak **Azure portal**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu öğretici Azure portalda Windows çalıştıran bir Azure sanal makine (VM) oluşturulacağını gösterir. Örnek olarak bir Windows Server görüntüsü kullanacağız, ancak bu Azure'un sunduğu birçok görüntüden sadece biri. Görüntü seçenekleriniz aboneliğinize göre değişir unutmayın. Örneğin, Windows Masaüstü görüntülerini MSDN aboneleri için kullanılabilir.

Bu bölümde, nasıl kullanılacağını gösterir **Pano** seçin ve ardından sanal makine oluşturmak için Azure Portalı'nda.

Kullanarak sanal makineleri oluşturabilirsiniz [kendi görüntüleri](createupload-vhd.md). Bu ve diğer yöntemler hakkında bilgi edinmek için bkz: [Windows sanal makine oluşturmanın farklı yollarını](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"></a>Sanal makine oluşturma
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager dağıtım modeli kullanarak bir VM oluşturma](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure portalında.
* Sanal makinede oturum açın. Yönergeler için bkz: [Windows Server çalıştıran bir sanal makinede oturum açın](connect-logon.md).
* Verileri depolamak için bir disk ekleyin. Boş diskleri ve verileri içeren diskleri ekleyebilirsiniz. Yönergeler için bkz: [Klasik dağıtım modeli kullanılarak oluşturulmuş bir Windows sanal makine veri diski ekleme](attach-disk.md).
