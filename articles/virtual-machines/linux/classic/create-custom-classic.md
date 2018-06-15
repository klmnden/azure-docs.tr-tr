---
title: Azure CLI 1.0 kullanarak klasik bir Linux VM oluşturma | Microsoft Docs
description: Azure CLI 1.0 Klasik dağıtım modeli kullanarak bir Linux sanal makine oluşturma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: 6d3f0dd0c82ad32df4d6e17058d9b1bea57c301f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841444"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a>Azure CLI 1.0 klasik bir Linux VM oluşturma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager sürümü için bkz: [burada](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu konuda Linux sanal makine (VM) oluşturmak Azure CLI Klasik dağıtım modeli kullanarak 1.0 açıklar. Linux görüntüden kullanılabilir kullanırız **GÖRÜNTÜLERİ** Azure üzerinde. Azure CLI 1.0 komutları diğerlerinin yanı sıra aşağıdaki yapılandırma seçenekleri sağlar:

* VM sanal ağa bağlanma
* Var olan bir bulut hizmeti VM ekleme
* Varolan bir depolama hesabı VM ekleme
* Bir kullanılabilirlik kümesini veya konumuna VM ekleme

> [!IMPORTANT]
> Bir sanal ağ için ana bilgisayar adı veya şirket içi bağlantılar kurma tarafından doğrudan bağlanabilmesi için kullanmak için VM istiyorsanız, VM oluştururken sanal ağı belirttiğinizden emin olun. Bir VM sanal ağ yalnızca VM oluşturduğunuzda katılmak için yapılandırılabilir. Sanal ağlar hakkında daha fazla bilgi için bkz: [Azure Virtual Network'e genel bakış](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanarak bir Linux VM oluşturma
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

