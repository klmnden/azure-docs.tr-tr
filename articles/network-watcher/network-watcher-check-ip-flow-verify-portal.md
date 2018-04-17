---
title: Trafik doğrulayın Azure Ağ İzleyicisi IP akış doğrulama - Azure portalı | Microsoft Docs
description: Bu makalede, trafik için veya bir sanal makineden izin verilen veya reddedilen denetlemek açıklar
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b6d929f025c8b95709b7c0eb28ee78310e5f12a5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Trafiği olup veya izin verilen IP akış doğrulayın ile bir VM'den/VM'ye Azure Ağ İzleyicisi'nin bir bileşeni reddedildi denetleyin

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST API'si](network-watcher-check-ip-flow-verify-rest.md)


IP akış doğrulayın özelliğidir Ağ İzleyicisi, trafik için veya bir sanal makineden izin verilip verilmediğini doğrulamanızı sağlar. Doğrulama gelen veya giden trafiği için çalıştırılabilir. Bu senaryo, bir sanal makine dış kaynak veya başka bir kaynak için iletişim kurabilirsiniz geçerli durumunu almak yararlıdır. IP akış doğrulayın, ağ güvenlik grubu (NSG) kurallarınızı düzgün şekilde yapılandırıldığından ve NSG kuralları tarafından engellenen akışları sorun giderme doğrulamak için kullanılabilir. IP kullanan başka bir nedeni akış durumda engellenen istediğiniz trafiği engellenmiş düzgün tarafından NSG emin olmak için doğrulayın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturmak](network-watcher-create.md) Ağ İzleyicisi mevcut bir örneği olması veya bir Ağ İzleyicisi oluşturun. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu senaryo, bir sanal makine bağlantı noktası 443 üzerinden başka bir makineye konuşun olmadığını doğrulamak için akış IP doğrulayın kullanır. Trafiği reddedilirse, bu trafiği engelleyen güvenlik kuralı döndürür. Akış IP doğrulama hakkında daha fazla bilgi için [IP akış doğrulayın genel bakış](network-watcher-ip-flow-verify-overview.md)

### <a name="run-ip-flow-verify"></a>Çalışma IP akış doğrulayın

Ağ İzleyicisi gidin ve tıklayın **IP akış doğrulayın**. Sanal makine ve gelen trafiği doğrulamak istediğiniz ağ arabirimi seçin. Ek filtreleme bilgileri girin ve tıklayın **denetleyin**.

Tıkladığınızda **denetleyin**, belirttiğiniz ölçütlere göre akışı denetlenir. Ya da sonucudur **izin verilen erişim** veya **erişim reddedildi**. Erişimi reddedilirse, akışa ağ güvenlik grubu (NSG) ve güvenlik kuralı sağlanır. Trafik reddi beklenen bir davranış ise, kural başarılı oldu.

> [!NOTE]
> IP akış doğrulayın gerektirir VM kaynak ayrılır.

Aşağıdaki görüntüden gördüğünüz giden HTTPS trafiğine izin.

![IP akış doğrulayın genel bakış][1]

Aşağıdaki resimde görüldüğü gibi trafik için değiştirilir gelen ve gelen bağlantı noktası 123 için değişti. Trafik şimdi Reddedildi, "erişim engellendi" trafiği reddeden ağ güvenlik grubu ve güvenlik kuralı ile birlikte sağlanan ileti.

![IP akış sonuçları][2]

## <a name="next-steps"></a>Sonraki adımlar

Trafik engelleniyor ve olmamalıdır, bkz: [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) tanımlanan ağ güvenlik grubu ve güvenlik kuralları izlemek için.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













