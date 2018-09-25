---
title: Klasik Azure CLI kullanarak klasik bir Linux VM oluşturma | Microsoft Docs
description: Klasik dağıtım modelini kullanarak Azure Klasik CLI ile Linux sanal makinesi oluşturmayı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: 507d9e12a37d7bf187a3e56b04cb47ac0104773d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982058"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-classic-cli"></a>Klasik Azure CLI ile klasik bir Linux VM oluşturma
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager sürümü için bkz: [burada](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu konuda Klasik dağıtım modelini kullanarak Azure Klasik CLI ile bir Linux sanal makinesini (VM) oluşturma işlemini açıklar. Kullanılabilir bir Linux görüntüsü kullanıyoruz **GÖRÜNTÜLERİ** azure'da. Azure Klasik CLI komutları, diğerlerinin yanında aşağıdaki yapılandırma seçenekleri sağlar:

* Sanal Makineyi bir sanal ağa bağlama
* Var olan bir bulut hizmeti için VM ekleme
* VM için mevcut bir depolama hesabı ekleme
* Bir kullanılabilirlik kümesi veya konuma VM ekleme

> [!IMPORTANT]
> VM'niz için ana bilgisayar adı veya şirketler arası bağlantılar kurma tarafından doğrudan bağlanabilmesi için bir sanal ağ kullanmak için isterseniz, sanal Makineyi oluştururken sanal ağı belirtin emin olun. Bir VM, yalnızca sanal Makineyi oluşturduğunuzda sanal ağa katılmak için yapılandırılabilir. Sanal ağlar hakkında daha fazla bilgi için bkz: [Azure sanal ağa genel bakış](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak bir Linux VM oluşturma
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

