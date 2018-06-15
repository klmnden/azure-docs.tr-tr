---
title: IP akış giriş doğrulayın Azure Ağ İzleyicisi | Microsoft Docs
description: Bu sayfada bir genel bakış sunulmaktadır Ağ İzleyicisi IP'si akış özelliği doğrulayın
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 8a59047a586f3d7ad7c1f29b218396bd688caafd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181608"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>IP akış giriş Azure Ağ İzleyicisi doğrulayın

IP akışı, bir paket izin verilen veya için veya bir sanal makineden reddedildi denetimleri doğrulayın. Bilgi yönü, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktası oluşur. Paket bir güvenlik grubu tarafından engellenirse Paket reddedildi kuralının adı döndürülür. Kaynak veya hedef IP seçilebilir durumdayken IP akış doğrulayın yardımcı Yöneticiler bağlantı sorunları veya sürümden internet ve gelen veya şirket içi ortamına hızla tanılamak.

IP akış doğrulayın hedefleyen bir sanal makinenin ağ arabirimi. Trafik akışı için ya da bu ağ arabirimini yapılandırılan ayarlara göre doğrulanır. IP akış doğrulayın bir ağ güvenlik grubu kural giriş ve çıkış trafiği için veya bir sanal makineden durumunda engelliyor onaylayan yararlıdır.

Ağ İzleyicisi IP akış çalıştırmayı planladığınız tüm bölgelerde oluşturulması gerekiyor örneği doğrulayın. Ağ İzleyicisi bölgesel bir hizmettir ve yalnızca olması çalıştırılmıştır aynı bölgede kaynaklara karşı. Kullanılan örnek sonuçları etkilemez IP'sini akış doğrulayın, NIC ile ilişkili yol olduğundan hala döndürülüp döndürülmediğini gibi.

![1][1]

## <a name="next-steps"></a>Sonraki adımlar

Bir paket izin verilen veya portal aracılığıyla belirli bir sanal makine için reddedildi, yapacağınızı öğrenmek için aşağıdaki makaleyi ziyaret edin. [Trafik akışı IP doğrulayın'yle bir VM'yi Portalı'nı kullanarak izin verilip verilmediğini denetleme](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












