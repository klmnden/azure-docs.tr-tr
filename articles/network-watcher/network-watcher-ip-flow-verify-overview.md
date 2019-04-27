---
title: Giriş IP akışı doğrulama Azure Ağ İzleyicisi | Microsoft Docs
description: Bu sayfa genel bir bakış sağlar. Ağ İzleyicisi IP akışı doğrulama yeteneği
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: kumud
ms.openlocfilehash: 88cb7e2cd04d13ade5c581a1ff2dc09669d89ab2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532602"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Giriş IP akışı doğrulama Azure Ağ İzleyicisi

IP akışı doğrulama denetimleri bir paket izin veya için veya bir sanal makineden reddedildi. Yönü, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktası bilgileri oluşur. Paket bir güvenlik grubu tarafından reddedilirse, paketi reddeden kuralın adı döndürülür. Herhangi bir kaynak veya hedef IP seçilebilir ancak IP akışı doğrulama yardımcı Yöneticiler gelen veya internet ve gelen veya şirket içi ortamına bağlantı sorunları hızla tanılayın.

IP akışı doğrulama alt ağı veya sanal makine bir NIC gibi ağ arabirimine uygulanmış tüm ağ güvenlik grupları (Nsg) kurallarını bakar. Trafik akışı veya ağ arabirimi için yapılandırılan ayarlara dayanan doğrulanır. IP akışı doğrulama, bir ağ güvenlik grubu kuralı veya bir sanal makineden girişi veya çıkışı trafiği engelleyip engellemediğini onaylayan olarak kullanışlıdır.

Ağ İzleyicisi IP akışı çalıştırmayı planladığınız tüm bölgelerde oluşturulması gerekiyor örneğini doğrulayın. Ağ İzleyicisi bölgesel bir hizmettir ve yalnızca olması çalıştırdık aynı bölge içindeki kaynaklara karşı. Kullanılan örnek sonuçları etkilemez IP akışı doğrulama, gibi NIC veya alt ağ ile ilişkili hiçbir yolu hala olması döndürülür.

![1][1]

## <a name="next-steps"></a>Sonraki adımlar

Bir paket izin veya portal aracılığıyla belirli bir sanal makine için reddedildi, öğrenmek için aşağıdaki makaleyi ziyaret edin. [Trafik IP akışı doğrulama ile sanal makine şirket portalını kullanarak izin verilip verilmediğini denetlemek](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png

