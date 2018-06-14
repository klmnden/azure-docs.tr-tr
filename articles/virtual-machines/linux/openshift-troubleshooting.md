---
title: Azure'da OpenShift dağıtım sorunlarını giderme | Microsoft Docs
description: Azure'da OpenShift dağıtım sorunlarını giderin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 35e554d3a9c7e7d56546ae9723c33eb59e906472
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
ms.locfileid: "24139459"
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>Azure'da OpenShift dağıtım sorunlarını gider

OpenShift kümeniz başarıyla dağıtma, şu görevleri sorunu belirlemelerine daraltmak için sorun giderme deneyin. Dağıtım durumunu görüntülemek ve çıkış kodları aşağıda karşı karşılaştırın:

- Çıkış kodu 3: bilgisayarınızı Red Hat abonelik kullanıcı adı / parola veya kuruluş kimliği / etkinleştirme anahtarı yanlış
- Çıkış kodu 4: bilgisayarınızı Red Hat havuzu kimliği hatalı olduğundan veya hiçbir yetkilendirmeler kullanılabilir
- Çıkış kodu 5: sağlanamıyor Docker ince havuzu birim
- Çıkış kodu 6: OpenShift Küme yüklemesi başarısız oldu
- Çıkış kodu 7: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut çözümü sağlayıcısı yapılandırması başarısız oldu - ana düğüm sorun ana yapılandırma
- Çıkış kodu 8: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut çözümü sağlayıcısı yapılandırması başarısız oldu - ana düğüm sorun düğüm yapılandırması
- Çıkış kodu 9: OpenShift Küme yüklemesi başarılı oldu ancak Azure bulut çözümü sağlayıcısı yapılandırması başarısız oldu - Infra veya uygulama düğümü sorun düğüm yapılandırması
- Çıkış kodu 10: OpenShift Küme yükleme başarılı oldu, ancak başarısız oldu - ana düğümler düzeltme Azure bulut çözümü sağlayıcısı yapılandırma veya ana unschedulable olarak ayarlamak için
- Çıkış kodu 11: ölçümleri başarısız dağıtmak
- Çıkış kodu 12: günlüğü başarısız dağıtmak

Çıkış kodları 7-10 OpenShift küme yüklendi, ancak Azure bulut çözümü sağlayıcısı yapılandırması başarısız oldu. Ana düğüm (OpenShift kaynak) veya savunma düğümü (OpenShift kapsayıcı platformu) ve her küme düğümüne orada SSH sorunlarını gidermek için SSH kullanabilirsiniz.

Çıkış kodları 7-9 hatalarıyla için ortak bir nedeni, hizmet sorumlusu abonelik veya kaynak grubu için uygun izinlere sahip olduğunu oluşturur. Bu sorun ise, doğru izinleri atamak ve el ile başarısız komut dosyası ve tüm sonraki komut dosyaları yeniden çalıştırın.

(Örneğin, systemctl yeniden atomik-openshift-node.service) komut dosyaları yeniden çalıştırmadan önce başarısız oldu. hizmet yeniden başlattığınızdan emin olun.

İçin daha fazla sorun giderme, SSH bağlantı noktası 2200 (kaynak), ana düğüm veya bağlantı noktası 22 (kapsayıcı platformu) savunma düğümde. Şu dizine göz atın ve kök (sudo su-) olması gerekir: /var/lib/waagent/custom-script/download.

"0" ve "1" adlı klasörleri burada görürsünüz İki dosya, "stderr" ve "stdout." her bu klasörler, gördüğünüz Hatanın oluştuğu belirlemek için bu dosyalar ile arayın.
