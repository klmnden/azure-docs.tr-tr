---
title: Klasik bir Linux sanal makinesinde uç noktaları ayarlama | Microsoft Docs
description: Azure'da bir Linux sanal makineyle iletişimi izin vermek için Azure portalında bir Linux VM için uç noktaları ayarlama hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 03cb90dcdcc7a7407898ddd8cbc30f1af0833676
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295696"
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Azure'da bir Linux Klasik sanal makinede uç noktaları ayarlama yapma
Klasik dağıtım modelini kullanarak Azure'da oluşturduğunuz tüm Linux sanal makineleri, özel bir ağ kanalı diğer sanal makinelerle aynı bulut hizmetinde veya sanal ağ üzerinden otomatik olarak iletişim kurabilir. Ancak uç noktaları bir sanal makineye gelen ağ trafiği yönlendirmek için İnternet'te veya diğer sanal ağlara bilgisayarlarda gerektirir. Bu makale için de kullanılabilir olan [Windows sanal makineleri](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

İçinde **Resource Manager** dağıtım modelini kullanarak uç noktaları yapılandırılmış **ağ güvenlik grupları (Nsg'ler)**. Daha fazla bilgi için [bağlantı noktalarını ve uç noktaları açma](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure portalında Linux sanal makinesi oluşturduğunuzda, bir uç nokta için güvenli Kabuk (SSH) genellikle sizin için otomatik olarak oluşturulur. Sanal makine oluştururken veya daha sonra gerektiğinde ek uç noktası yapılandırabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanarak bir sanal makine uç noktası oluşturabilirsiniz [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Çalıştırma **azure sanal makine uç noktası oluşturma** komutu.
* Resource Manager dağıtım modelinde bir sanal makine oluşturduysanız, Azure CLI'yı Resource Manager moduna kullanabilirsiniz [ağ güvenlik grupları oluşturma](../../../virtual-network/tutorial-filter-network-traffic-cli.md) trafiği denetlemek için VM.
