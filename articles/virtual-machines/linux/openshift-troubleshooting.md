---
title: "Azure'da OpenShift dağıtım sorunlarını giderme | Microsoft Docs"
description: "Azure'da OpenShift dağıtım sorunlarını giderme"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: ea3664870480f6ed170972a5f52940dc4a852219
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshooting-openshift-deployment-in-azure"></a>Azure'da OpenShift dağıtım sorunlarını giderme

OpenShift küme başarıyla dağıtma, sorunu belirlemelerine daraltmak için yapılabilir sorun giderme bazı görevler vardır. Dağıtım durumunu görüntülemek ve çıkış kodları aşağıda karşı karşılaştırın.

- Çıkış kodu 3:, Red Hat abonelik kullanıcı adı / parola veya kuruluş kimliği etkinleştirme anahtarı hatalı olduğu
- Çıkış kodu: 4: Red Hat havuzu kimliği geçersiz veya yok yetkilendirmeler kullanılabilir
- Çıkış kodu 5: Sağlanamıyor Docker ince havuzu birim
- Çıkış kodu 6: OpenShift Küme yüklemesi başarısız oldu
- Çıkış kodu 7: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut sağlayıcısı yapılandırması başarısız oldu - ana düğüm sorun ana yapılandırma
- Çıkış kodu 8: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut sağlayıcısı yapılandırması başarısız oldu - ana düğüm sorun düğüm yapılandırması
- Çıkış kodu 9: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut sağlayıcısı yapılandırması başarısız oldu - Infra veya uygulama düğümü sorun düğüm yapılandırması
- Çıkış kodu 10: OpenShift Küme yükleme başarılı oldu, ancak başarısız oldu - ana düğümler düzeltme Azure bulut sağlayıcısı yapılandırma veya ana unschedulable olarak ayarlamak için
- Çıkış kodu 11: Ölçümleri dağıtamadı
- Çıkış kodu 12: Dağıtmak günlüğe kaydetme başarısız oldu

Çıkış kodları 7-10 OpenShift küme yüklediniz mi ancak Azure bulut sağlayıcısı yapılandırması başarısız oldu. SSH ana düğümü (kaynak) ya da savunma düğümü (kapsayıcı platformu) ve her bir küme içindeki düğümlerin orada SSH olabilir ve sorunları giderin.

Çıkış kodları 7 - hatalarıyla için ortak bir nedeni, hizmet sorumlusu abonelik veya kaynak grubu için uygun izinlere sahip değil 9 oluşturur. Bu gerçekten sorun ise, doğru izinleri atayın ve el ile tüm sonraki komut dosyaları başarısız komut dosyasını yeniden çalıştırın.
(Örneğin, systemctl yeniden atomik-openshift-node.service) komut dosyaları yeniden çalıştırmadan önce başarısız oldu. hizmet yeniden başlattığınızdan emin olun.

İçin daha fazla sorun giderme, bağlantı noktası 2200 ana düğümünüz içine SSH (kaynak) veya savunma bağlantı noktası 22 düğümde (kapsayıcı platformu). Şu dizine gidin ve kök (sudo su-) olması gerekir: /var/lib/waagent/custom-script/download

'0' ve '1' adlı bir klasör görmeniz gerekir. Her bu klasörler, iki dosya, stderr ve stdout görürsünüz. Hatanın oluştuğu belirlemek için bu dosyalar ile bakabilirsiniz.
