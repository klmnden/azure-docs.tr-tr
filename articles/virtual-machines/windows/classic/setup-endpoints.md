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
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: cca9adb40557cf7bf9e1d4129fc6bd61cbf0df4f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38618248"
---
# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Azure'da klasik bir Windows sanal makinesinde uç noktaları ayarlama yapma
Klasik dağıtım modelini kullanarak Azure'da oluşturduğunuz sanal makinelere otomatik olarak için tüm Windows üzerinde özel bir ağ kanalının aynı bulut hizmetinde veya sanal ağı diğer sanal makinelerle iletişim kurar. Ancak uç noktaları bir sanal makineye gelen ağ trafiği yönlendirmek için İnternet'te veya diğer sanal ağlara bilgisayarlarda gerektirir. Bu makale için de kullanılabilir olan [Linux sanal makineleri](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

İçinde **Resource Manager** dağıtım modelini kullanarak uç noktaları yapılandırılmış **ağ güvenlik grupları (Nsg'ler)**. Daha fazla bilgi için [Azure portalını kullanarak sanal makinelerinize dış erişime izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure portalında bir Windows sanal makine oluşturduğunuzda, Uzak Masaüstü ve Windows PowerShell uzaktan iletişimini yönelik olanlar gibi ortak uç noktaları genellikle sizin için otomatik olarak oluşturulur. Sanal makine oluştururken veya daha sonra gerektiğinde ek uç noktası yapılandırabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure PowerShell cmdlet'i bir VM uç nokta ayarlamayı kullanmak için bkz [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Bir uç nokta ACL'sini yönetmek için Azure PowerShell cmdlet'ini kullanmak için bkz: [yönetme erişim denetim listeleri (ACL'ler) uç noktalar için PowerShell kullanarak](../../../virtual-network/virtual-networks-acl-powershell.md).
* Resource Manager dağıtım modelinde bir sanal makine oluşturursanız, Azure PowerShell'i kullanarak [ağ güvenlik grupları oluşturma](../../../virtual-network/tutorial-filter-network-traffic.md) trafiği denetlemek için VM.
