---
title: Azure portalında bir VM oluşturma | Microsoft Docs
description: Azure portalında bir Windows sanal makine oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: 5fd2128ff436d3211f41c7dfdcc4c2b8aabd0eb0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232335"
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Azure Portal'da Windows çalıştıran bir sanal makine oluşturma
> [!div class="op_single_selector"]
> * [Azure portal](tutorial.md)
> * [PowerShell: Klasik dağıtım](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bilgi nasıl [bu adımları Resource Manager dağıtım modeli kullanarak gerçekleştirmeyi](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak **Azure portalında**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu öğretici, Azure Portal'da Windows çalıştıran bir Azure sanal makine (VM) oluşturma işlemi gösterilmektedir. Bir Windows Server görüntüsüne örnek olarak kullanacağız, ancak bu Azure'un sunduğu birçok görüntüden sadece biri. Görüntü seçenekleriniz aboneliğinize göre değişir unutmayın. Örneğin, Windows Masaüstü görüntüleri MSDN abonelerinin olabilir.

Bu bölümde, nasıl kullanılacağını gösterir **Pano** seçin ve ardından sanal makine oluşturmak için Azure portalında.

Kullanarak Vm'leri oluşturabilirsiniz [kendi görüntülerinizi](createupload-vhd.md). Bu ve diğer yöntemler hakkında bilgi edinmek için [bir Windows sanal makine oluşturmanın farklı yolları](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"> </a>Sanal makine oluşturma
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [Resource Manager dağıtım modelini kullanarak bir VM oluşturma](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure portalında.
* Sanal makinede oturum açın. Yönergeler için [Windows Server çalıştıran bir sanal makinede oturum açma](connect-logon.md).
* Verileri depolamak için bir diski kullanıma açın. Boş diskler hem de içeren veri diskleri ekleyebilirsiniz. Yönergeler için [Klasik dağıtım modeliyle oluşturulan bir Windows sanal makinesine veri diski](attach-disk.md).
