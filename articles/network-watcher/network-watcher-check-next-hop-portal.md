---
title: "Azure Ağ İzleyicisi sonraki atlama - Azure portal ile sonraki atlama Bul | Microsoft Docs"
description: "Bu makalede, sonraki atlama türü nedir nasıl bulabilir ve Azure portalını kullanarak sonraki atlama kullanarak IP adresi anlatmaktadır."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: jimdial
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 445ec8c7eeb8dd715d3778b44372d16666da7fb8
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-the-portal"></a>Hangi bir sonraki atlama türü sonraki atlama yetenek Portalı'nı kullanarak Azure Ağ İzleyicisi içinde kullandığını bulmak

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST API'si](network-watcher-check-next-hop-rest.md)

Sonraki atlama özelliğini get sonraki atlama türü ve belirtilen bir sanal makineye dayalı IP adresi sağlayan Ağ İzleyicisi özelliğidir. Bu özellik, bir sanal makine trafiğe bir ağ geçidi, internet veya sanal ağlar hedefine almak için geçiyorsa belirlemede yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo sonraki atlama sonraki atlama türü ve kaynak IP adresini bulmak için kullanır. Sonraki atlama hakkında daha fazla bilgi için [sonraki atlama genel bakış](network-watcher-next-hop-overview.md).

Bu senaryoda, şunları yapacaksınız:

* Sonraki atlama bir sanal makineden alın.

## <a name="get-next-hop"></a>Sonraki atlama Al

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi kaynağınız Azure portalında gidin.

### <a name="step-2"></a>2. Adım

Tıklatın **sonraki atlama** Gezinti bölmesinde, sanal makine ve ağ arabirimi seçin, kaynak ve hedef IP doldurun ve tıklatın **sonraki atlama** düğmesi.

> [!NOTE]
> Sonraki atlama VM kaynak çalıştırmak için ayrılır gerektirir.

![sonraki atlama genel bakış][1]

### <a name="step-3"></a>3. Adım

Görev tamamlandığında, sonuçları sağlanır. IP adresi ve sonraki atlama olan aygıt türü görüntülenir. Aşağıdaki tabloda kullanılabilir döndürülen değer portalda görüntülenir.

**Sonraki atlama türü**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

Özel bir rota bu trafiği yönlendirmek için kullanıldıysa, kullanıcı tanımlı yönlendirme (UDR) de sonuçlarıyla gösterilir.

![sonraki atlama sonuçları Al][2]

## <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubu ayarlarınızı program aracılığıyla ziyaret ederek gözden öğrenin [NSG Denetim Ağ İzleyicisi](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














