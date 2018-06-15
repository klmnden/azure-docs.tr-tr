---
title: Azure PowerShell örnekleri | Microsoft Docs
description: Azure PowerShell örnekleri
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 0bca4fb6874bd265f0ae9faeb4219abeb4ffb6d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23926357"
---
# <a name="azure-powershell-samples-for-networking"></a>Ağ iletişimi için Azure PowerShell örnekleri

Aşağıdaki tabloda Azure PowerShell kullanılarak oluşturulan komut dosyalarını bağlantılarını içerir.

| | |
|-|-|
|**Azure kaynakları arasında bağlantı**||
| [Çok katmanlı uygulamalar için bir sanal ağ oluşturma](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Bir sanal ağ ile ön uç ve arka uç alt ağlar oluşturur. Arka uç alt ağ trafiği SQL, bağlantı noktası 1433 sınırlı olsa da HTTP için ön uç alt ağ trafiği sınırlıdır. |
| [Eş iki sanal ağlar](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Oluşturur ve iki sanal ağ aynı bölgede bağlanır. |
| [Bir ağ sanal gereç yoluyla trafiği yönlendirme](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Ön uç ve arka uç alt ağları ve iki alt ağlar arasında trafiği yönlendirmek için bir VM sanal ağ oluşturur. |
| [Gelen ve giden VM ağ trafiği filtreleme](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Bir sanal ağ ile ön uç ve arka uç alt ağlar oluşturur. Ön uç alt ağa gelen ağ trafiğini, HTTP ve HTTPS ile sınırlıdır... Arka uç alt ağından internet giden trafiğe izin verilmez. |
|**Yük Dengeleme ve trafik yönü**||
| [Yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Birkaç yüksek oranda kullanılabilir sanal makineleri ve yükü dengelenmiş yapılandırma oluşturur. |
| [Yük Dengelemesi birden çok Web sitesi vm'lerinde](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Bir Azure kullanılabilirlik kümesi, bir Azure yük dengeleyici erişilebilir katılmış birden fazla IP yapılandırması ile iki VM'ler oluşturur. |
| [Uygulama yüksek kullanılabilirlik için birden çok bölgede doğrudan trafiği](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  İki uygulama hizmeti planları, iki web uygulamaları, trafik Yöneticisi profili ve iki trafik Yöneticisi uç noktaları oluşturur. |
| | |
