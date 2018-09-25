---
title: Azure Ağ İzleyicisi - Azure portalı ile bağlantı sorunlarını giderme | Microsoft Docs
description: Bağlantıyı kullanmayı öğrenin özelliği, Azure portalını kullanarak Azure Ağ İzleyicisi sorun giderme.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: jdial
ms.openlocfilehash: 21e004e12a5111eb0e5fc7764c1e07fcb68c447d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986209"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [Azure CLI](network-watcher-connectivity-cli.md)
> - [Azure REST API](network-watcher-connectivity-rest.md)

Bağlantı kullanmayı öğrenin belirli bir uç noktaya doğrudan TCP bağlantısı bir sanal makineden oluşturulan olup olmadığını doğrulamak için sorun giderme.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunu varsayar:

* Ağ İzleyicisi bağlantı sorunlarını gidermek için istediğiniz bölgede bir örneği.
* Bağlantı sorunlarını gidermek için sanal makineler.

> [!IMPORTANT]
> Bağlantı sorunlarını giderme, sorun giderme VM'ye sahip olması gerekir `AzureNetworkWatcherExtension` VM uzantısı yüklü. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). Hedef uç noktada bir uzantı gerekli değildir.

## <a name="check-connectivity-to-a-virtual-machine"></a>Bir sanal makine bağlantısını kontrol edin

Bu örnek, bir hedef sanal makinenin bağlantı bağlantı noktası 80 üzerinden denetler.

İçin Ağ İzleyicisi gelin ve tıklayın **bağlantı sorunlarını giderme**. Bağlantıyı denetlemek için sanal makineyi seçin. İçinde **hedef** bölümü seçin **bir sanal makine seçin** ve doğru sanal makine ve test etmek için bağlantı noktası seçin.

' A tıkladığınızda **denetleyin**, belirtilen bağlantı noktası sanal makineler arasındaki bağlantıyı denetlenir. Örnekte, hedef VM erişilemiyor, atlama listesi gösterilir.

![Bir sanal makine için denetim bağlantı sonuçları][1]

## <a name="check-remote-endpoint-connectivity"></a>Uzak uç noktası bağlantısını denetleyin

Uzak uç noktası için gecikme süresi ve bağlantı kontrol edin, tercih **el ile belirt** radyo düğmesinin **hedef** bölümünde URL'yi ve bağlantı noktasını girin ve tıklayın **denetleyin**.  Bu, Web siteleri ve depolama uç noktaları gibi uzak uç noktaları için kullanılır.

![Bir web sitesi için bağlantı sonuçlarını denetleyin][2]

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenme [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafiğe içine veya dışına VM'nizi ederek izin verilip verilmediğini Bul [denetleyin IP akışı doğrulama](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png