---
title: Azure Ağ İzleyicisi - Azure portal ile bağlantı sorunlarını giderme | Microsoft Docs
description: Bağlantı kullanmayı öğrenin Özelliği Azure portalını kullanarak Azure Ağ İzleyicisi, sorun giderme.
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
ms.openlocfilehash: cf7b71a49b63a95ed535210125120c6b76d9de8f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure REST API'si](network-watcher-connectivity-rest.md)

Bağlantı kullanmayı öğrenin bir doğrudan belirli bir uç noktası TCP bağlantısı bir sanal makineden oluşturulan olup olmadığını doğrulamak için sorun giderme.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunuz varsayılmaktadır:

* Ağ İzleyicisi, bir bağlantı sorunlarını giderme istediğiniz bölgede bir örneği.
* Bağlantıları ile ilgili sorunları giderme için sanal makineler.

> [!IMPORTANT]
> Bağlantı sorunlarını giderme, sorun giderme VM olmasını gerektirir `AzureNetworkWatcherExtension` VM uzantısı yüklü. Bir Windows VM uzantısı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve Linux VM ziyaret edin: [Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). Hedef uç noktada uzantısı gerekli değildir.

## <a name="check-connectivity-to-a-virtual-machine"></a>Bir sanal makineye bağlantısını kontrol edin.

Bu örnekte, bağlantı noktası 80 üzerinden bir hedef sanal makine bağlantısı denetler.

Ağ İzleyicisi gidin ve tıklayın **bağlantı sorunlarını giderme**. Bağlantısını denetlemek için sanal makineyi seçin. İçinde **hedef** bölümü seçin **bir sanal makine seçin** ve doğru sanal makine ve test etmek için bağlantı noktası seçin.

Tıkladığınızda **denetleyin**, belirtilen bağlantı noktası sanal makineler arasında bağlantı denetlenir. Örnekte, hedef VM erişilemiyor, atlama listesi gösterilir.

![Bir sanal makine için denetim bağlantı sonuçları][1]

## <a name="check-remote-endpoint-connectivity"></a>Uzak uç noktada bağlantısını kontrol edin.

Bağlantısını ve uzak uç nokta için gecikme süresini denetlemek için seçin **el ile belirt** radyo düğmesini **hedef** bölümünde, url ve bağlantı noktasını girin ve tıklatın **denetleyin**.  Bu, Web siteleri ve depolama uç noktaları gibi uzak uç noktalar için kullanılır.

![Bir web sitesi için bağlantı sonuçları denetleyin][2]

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenin [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafik içinde veya dışında VM ziyaret ederek izin verilip verilmediğini Bul [denetleyin IP akış doğrulayın](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png