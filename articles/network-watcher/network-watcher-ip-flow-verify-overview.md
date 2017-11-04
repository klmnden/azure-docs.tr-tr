---
title: "IP akış giriş doğrulayın Azure Ağ İzleyicisi | Microsoft Docs"
description: "Bu sayfada bir genel bakış sunulmaktadır Ağ İzleyicisi IP'si akış özelliği doğrulayın"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b2ad45e76320c59d18dce7b39166679801b4170a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>IP akış giriş Azure Ağ İzleyicisi doğrulayın

IP akışı, bir paket izin verilen veya için veya 5-tanımlama bilgilerine dayalı bir sanal makineden reddedildi denetimleri doğrulayın. Bu bilgiler yönü, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktası oluşur. Paket bir güvenlik grubu tarafından engellenirse Paket reddedildi kuralının adı döndürülür. Kaynak veya hedef IP seçilebilir olsa da, bu özellik hızla gelen veya Internet ve gelen veya şirket içi ortamına bağlantı sorunları tanılamak yöneticilerin yardımcı olur.

IP akış doğrulayın hedefleyen bir sanal makinenin ağ arabirimi. Trafik akışı için ya da bu ağ arabirimini yapılandırılan ayarlara göre doğrulanır. Bu özellik, bir ağ güvenlik grubu kural giriş ve çıkış trafiği için veya bir sanal makineden durumunda engelliyor onaylayan yararlıdır.

Ağ İzleyicisi IP akış çalıştırmayı planladığınız tüm bölgelerde oluşturulması gerekiyor örneği doğrulayın. Ağ İzleyicisi bölgesel bir hizmettir ve yalnızca olması çalıştırılmıştır aynı bölgede kaynaklara karşı. Bu mu NIC ile ilişkili yol hala döndürülen IP akış sonuçlarını doğrulamak etkiler.

![1][1]

## <a name="next-steps"></a>Sonraki adımlar

Bir paket izin verilen veya portal aracılığıyla belirli bir sanal makine için reddedildi, yapacağınızı öğrenmek için aşağıdaki makaleyi ziyaret edin. [Trafik akışı IP doğrulayın'yle bir VM'yi Portalı'nı kullanarak izin verilip verilmediğini denetleme](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












