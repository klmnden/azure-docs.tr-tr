---
title: Azure CLI 1.0 kullanarak klasik bir Linux VM oluşturma | Microsoft Docs
description: Azure CLI 1.0 Klasik dağıtım modelini kullanarak bir Linux sanal makinesi oluşturmayı öğrenin
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
ms.openlocfilehash: 13d0ef93c3828c514e46e37494a66f7003eac827
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931638"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a>Azure CLI 1.0 ile klasik bir Linux VM oluşturma
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager sürümü için bkz: [burada](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu konu Azure CLI 1.0 Klasik dağıtım modelini kullanarak bir Linux sanal makinesini (VM) oluşturma işlemini açıklar. Kullanılabilir bir Linux görüntüsü kullanıyoruz **GÖRÜNTÜLERİ** azure'da. Azure CLI 1.0 komutlarını, diğerlerinin yanında aşağıdaki yapılandırma seçenekleri sağlar:

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

