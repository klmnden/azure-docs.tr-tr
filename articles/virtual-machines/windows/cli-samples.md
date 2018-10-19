---
title: Windows Azure CLI örnekleri | Microsoft Docs
description: Windows Azure CLI örnekleri
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/01/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6861399b63b7f06bac7599704a6dd1aa87800ebf
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403347"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Azure CLI örnekleri Windows'ı için sanal makineleri

Aşağıdaki tabloda, Windows sanal makineleri dağıtmak için Azure CLI kullanılarak oluşturulan bash betiklerine yönelik bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Sanal makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | En düşük yapılandırmayla bir Windows sanal makine oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Birden çok yüksek oranda kullanılabilir sanal makineler ve yük dengeli küme yapılandırması oluşturur. |
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [Bir VM oluşturun ve çalışma DSC yapılandırması](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Desired State Configuration (DSC) uzantısı kullanır. |
|**Ağ sanal makineler**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | İki sanal makine ve tüm ilgili kaynakları bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifreleme](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Bir VM'nin Log Analytics ile izleme](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükler ve Log Analytics çalışma alanındaki VM kaydeder.  |
| | |
