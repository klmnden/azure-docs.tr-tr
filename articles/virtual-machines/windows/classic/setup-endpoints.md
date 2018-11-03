---
title: Bir Klasik Windows VM'de uç noktaları ayarlama | Microsoft Docs
description: Azure'da Windows sanal makineyle iletişimi izin vermek için Azure portalında Klasik Windows VM'de uç noktaları ayarlama öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/23/2018
ms.author: cynthn
ms.openlocfilehash: 373003eb8be0482ed96d7d11ecd879237f69b47a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957174"
---
# <a name="set-up-endpoints-on-a-windows-virtual-machine-by-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak Windows sanal makinesinde uç noktaları ayarlama
Klasik dağıtım modelini kullanarak Azure'da oluşturduğunuz Windows sanal makineleri (VM'ler) otomatik olarak özel bir ağ kanalı diğer vm'lerle aynı bulut hizmetinde veya sanal ağ üzerinden iletişim kurabilir. Ancak internet veya diğer sanal ağlara bilgisayarlarda bir VM'ye gelen ağ trafiği yönlendirmek için uç noktalar gerektirir. 

Uç noktaları ayarlama üzerinde de ayarlayabilirsiniz [Linux sanal makineleri](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makale, klasik dağıtım modelini kapsamaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.  
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

İçinde **Resource Manager** dağıtım modeli, uç noktaları kullanılarak yapılandırılır **ağ güvenlik grupları (Nsg'ler)**. Daha fazla bilgi için [Azure portalını kullanarak, bir VM'ye dış erişim izni ver](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure portalında bir Windows VM oluşturduğunuzda, Uzak Masaüstü ve Windows PowerShell uzaktan iletişimini uç noktaları gibi ortak uç noktaları genellikle sizin için otomatik olarak oluşturulur. Daha sonra gerektiğinde, ek uç noktalar yapılandırabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure PowerShell cmdlet'i bir VM uç nokta ayarlamayı kullanmak için bkz [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Bir uç nokta ACL'sini yönetmek için Azure PowerShell cmdlet'ini kullanmak için bkz: [yönetme erişim denetim listeleri (ACL'ler) uç noktalar için PowerShell kullanarak](../../../virtual-network/virtual-networks-acl-powershell.md).
* Resource Manager dağıtım modelinde bir sanal makine oluşturursanız, Azure PowerShell'i kullanarak [ağ güvenlik grupları oluşturma](../../../virtual-network/tutorial-filter-network-traffic.md) trafiği denetlemek için VM.
