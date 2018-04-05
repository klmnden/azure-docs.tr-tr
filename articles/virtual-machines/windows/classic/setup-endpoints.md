---
title: Klasik Windows VM üzerindeki uç noktaları ayarlama | Microsoft Docs
description: Azure portalında Klasik Windows VM için uç nokta Azure'da Windows sanal makine ile iletişime izin verecek şekilde ayarlamak öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
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
ms.openlocfilehash: d64feff341e389df4079c0603a414f0d40b754e7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Azure Klasik Windows sanal makine üzerindeki uç noktaları kurma
Tüm Windows Azure'da Klasik dağıtım modeli kullanarak oluşturduğunuz sanal makineleri otomatik olarak şunları yapabilecek özel ağ kanal diğer sanal makineler aynı bulut hizmetinde veya sanal ağ ile iletişim kurar. Ancak, Internet veya diğer sanal ağlardaki bilgisayarlara bir sanal makineye gelen ağ trafiğini yönlendirmek için uç noktalar gerektirir. Bu makalede ayrıca kullanılabilir [Linux sanal makineleri](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

İçinde **Resource Manager** dağıtım modeli, uç noktalar kullanılarak yapılandırılmış olan **ağ güvenlik grupları (Nsg'ler)**. Daha fazla bilgi için bkz: [VM'nizi Azure portalını kullanarak dış erişmesine izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure Portal'da Windows sanal makine oluşturduğunuzda, Uzak Masaüstü ve Windows PowerShell uzaktan iletişim için olanlar gibi ortak uç noktaları genellikle sizin için otomatik olarak oluşturulur. Sanal makine oluşturulurken veya daha sonra gerektikçe ek uç noktaları yapılandırabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Sonraki adımlar
* VM uç nokta ayarlamayı bir Azure PowerShell cmdlet'ini kullanmak için bkz: [Ekle AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Bir uç noktada bir ACL yönetmek için bir Azure PowerShell cmdlet'ini kullanmak için bkz: [yönetme erişim denetim listeleri (ACL'ler) uç noktaları için PowerShell kullanarak](../../../virtual-network/virtual-networks-acl-powershell.md).
* Resource Manager dağıtım modelinde bir sanal makine oluşturduysanız, Azure PowerShell kullanabileceğiniz [ağ güvenlik grupları oluşturma](../../../virtual-network/tutorial-filter-network-traffic.md) VM denetim trafiği için.
