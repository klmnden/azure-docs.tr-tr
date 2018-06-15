---
title: Azure Traffic Manager ayarlarını doğrulayın | Microsoft Docs
description: Bu makalede, trafik Yöneticisi ayarlarınızı doğrulamanıza yardımcı olur
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: aadff1806a7cb22347283143563467366e857569
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876358"
---
# <a name="verify-traffic-manager-settings"></a>Trafik Yöneticisi ayarlarını doğrulayın

Trafik Yöneticisi ayarlarınızı sınamak için birden çok, çeşitli yerlerde testleri çalıştırma istemciniz gerekir. Ardından, bir kerede aşağı Traffic Manager profilinize uç noktaları getirin.

* Hızlı bir şekilde (örneğin, 30 saniye) değişiklikleri yaymak şekilde DNS TTL değeri düşük ayarlayın.
* Azure bulut hizmetlerinizi ve Web siteleri test ettiğiniz profilinde IP adreslerini bilirsiniz.
* Bir IP adresi için DNS adını çözümlemek ve bu adresi görüntülemek olanak sağlayan araçları kullanın.

DNS adlarını IP adreslerine profilinizde uç noktaları çözmek görmek için denetleniyor. Adları trafik Yöneticisi profilinde tanımlanmış trafik yönlendirme yöntemini ile tutarlı şekilde çözümlenmelidir. Gibi araçları kullanabilirsiniz **nslookup** veya **derinliklerine** DNS adlarını çözümlemek için.

Aşağıdaki örnekler, Traffic Manager profilinizin test yardımcı olur.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Trafik Yöneticisi profili nslookup ve ipconfig Windows'da kullanma denetleyin

1. Bir komut veya Windows PowerShell komut istemini yönetici olarak açın.
2. Tür `ipconfig /flushdns` DNS çözümleyicisi önbelleğini temizlemek için.
3. `nslookup <your Traffic Manager domain name>` yazın. Örneğin, aşağıdaki komutu ön ek etki alanı adıyla denetler *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Normal sonuç, aşağıdaki bilgileri gösterir:

    + Bu trafik yöneticisi etki alanı adı çözümlemek için erişilen DNS sunucusunun IP adresini ve DNS adı.
    + Trafik yöneticisi etki alanı adı komut satırında, trafik yöneticisi etki alanı çözümler IP adresi ve "nslookup" sonra yazdığınız. İkinci IP adresi denetlemek için önemli olabilir. Bulut Hizmetleri veya test ettiğiniz trafik Yöneticisi profili Web sitelerinde biri için bir genel sanal IP (VIP) adresi eşleşmelidir.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Yük devretme trafik yönlendirme yöntemini test etme

1. Yukarı tüm uç noktaları bırakın.
2. Tek bir istemci kullanarak, DNS çözümlemesi nslookup veya benzer bir yardımcı programını kullanarak, şirketinizin etki alanı adı isteyin.
3. Çözümlenen IP adresini birincil endpoint eşleştiğinden emin olun.
4. Bu trafik Yöneticisi uygulama kapalı olduğunu düşündüğü izleme dosyayı kaldırın veya birincil uç noktanızı getirin.
5. DNS zaman yaşam için (TTL) trafik Yöneticisi profili ve iki dakika bekleyin. Örneğin, DNS TTL 300 saniye (5 dakika) ise, yedi dakika beklemeniz gerekir.
6. Nslookup kullanarak DNS istemci önbelleği ve istek DNS çözümlemenizin temizlenir. Windows'da ipconfig/flushdns komutuyla, DNS önbelleğini boşaltabilirsiniz.
7. Çözümlenen IP adresini ikincil uç noktanız eşleştiğinden emin olun.
8. Her uç noktası sırayla hale getirme işlemi yineleyin. DNS sonraki uç noktası IP adresi listesinde döndürdüğünden emin olun. Tüm uç noktaları kapalı olduğunda, birincil uç noktası IP adresi yeniden edinmelidir.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemini test etme

1. Yukarı tüm uç noktaları bırakın.
2. Tek bir istemci kullanarak, DNS çözümlemesi nslookup veya benzer bir yardımcı programını kullanarak, şirketinizin etki alanı adı isteyin.
3. Çözümlenen IP adresini noktalarınızı birini eşleştiğinden emin olun.
4. DNS istemci önbelleğini temizlemek ve her uç noktası için 2 ve 3. adımları yineleyin. Her uç noktalarınızı için döndürülen farklı IP adreslerini görmeniz gerekir.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Performans trafik yönlendirme yöntemini test etme

Etkili bir şekilde performans trafik yönlendirme yöntemini sınamak için dünyanın farklı bölümlerinde bulunan istemciler olması gerekir. İstemcileri hizmetlerinizi test etmek için kullanılan farklı Azure bölgelerinde oluşturabilirsiniz. Genel bir ağ varsa, uzaktan dünya diğer bölümlerinde istemciler için oturum açın ve buradan testleri çalıştırın.

Alternatif olarak, ücretsiz web tabanlı DNS araması ve vardır kullanılabilir hizmetler derinliklerine. Bu araçlardan bazıları dünyanın çeşitli konumlardan DNS adı çözümlemesini denetleyin olanağı verir. Örnekler "DNS aramalarında" araması yapın. Gomez veya açılış konuşması gibi üçüncü taraf hizmetleri profillerinizi trafiği beklendiği gibi dağıtıyorsanız onaylamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)
* [Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
* [Düzeyi düşürülmüş Traffic Manager durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)
