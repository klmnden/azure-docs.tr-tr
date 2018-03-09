---
title: "Azure vm'lerinde Azure Site Recovery ile şirket içi VMware için yeniden çalışma sırasında hatalarını giderme | Microsoft Docs"
description: "Bu makalede ortak yeniden çalışma ve yükü hatalarını yeniden çalışma sırasında VMware Azure Site RECOVERY'yi kullanarak Azure'dan gidermenin yolları açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2017
ms.author: rajanaki
ms.openlocfilehash: 1c54ae96273880caede1f50f3a0705c41f15f26e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Azure'dan VMware için sorun giderme

Bu makalede Azure kullanarak yük devretme işleminden sonra şirket içi VMware altyapınızı geri Azure VM'ler başarısız olduğunda karşılaşabileceğiniz sorunları gidermek nasıl [Azure Site Recovery](site-recovery-overview.md).

Yeniden çalışma temelde iki ana adımdan oluşur. Yük devretme sonrasında çoğaltma başlatmaları böylece şirket içi, Azure Vm'leri koruyun gerekir. İkinci adım azure'dan şirket içi sitede yeniden çalıştırmak için bir yük devretme çalıştırmaktır. 

## <a name="troubleshoot-reprotection-errors"></a>Yükü hatalarında sorun giderme

Bu bölümde, ortak yükü hataları ve bunların nasıl düzeltileceği ayrıntıları.

### <a name="error-code-95226"></a>Hata kodu 95226

**Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için olmadığından yeniden koruma başarısız oldu.**

Bu hata oluştuğunda zaman:

1. Azure VM şirket içi yapılandırma sunucusuna erişemiyor. VM olamaz bulunan ve yapılandırma sunucusuna kayıtlı. 
2. Inmage Scout Application service, yük devretme sonrasında Azure VM'de çalışmıyor. Hizmet, şirket içi yapılandırma sunucusu ile iletişim için gereklidir.

Bu sorunu gidermek için:

1. Azure VM ağı şirket içi yapılandırma sunucusu ile iletişim kurmak Azure VM izin verdiğini denetleyin. Bunu yapmak için şirket içi veri merkezine siteden siteye VPN ayarlamak veya özel Azure VM sanal ağ eşleme ile bir ExpressRoute bağlantı yapılandırabilirsiniz. 
2. VM şirket içi yapılandırma sunucusu ile iletişim kurabilir, VM oturum ve 'Inmage Scout Application Service' denetleyin. Şu çalışmadığını görüyorsanız, hizmeti elle başlatın ve hizmet başlangıç türünü otomatik olarak ayarlanmış olduğundan emin olun.

### <a name="error-code-78052"></a>Hata kodu 78052

***Sanal makine için koruma tamamlanamadı.**

Yeniden çalıştırma işlemini gerçekleştiriyorsanız ana hedef sunucusunda aynı ada sahip bir VM zaten varsa bu durum oluşabilir.

Aşağıdakileri yapın bu sorunu çözmek için:
1. Başka bir ana bilgisayardaki farklı bir ana hedef sunucusu seçin, böylece yükü makine nerede adları değil birbiriyle çakışır farklı bir konak üzerinde oluşturur. 
2. Ayrıca, VMotion'ı farklı bir konak üzerinde ad çakışması gerçekleşmeyecek ana hedefe erişebilirsiniz. Var olan VM parazit makine ise, böylece yeni VM aynı ESXi ana bilgisayarda oluşturulabilir yeniden adlandırın.

### <a name="error-code-78093"></a>Hata kodu 78093

**VM, askıdaki bir durumda veya erişilebilir çalışmıyor.**

Başarısız bir VM üzerinde yeniden korumak için Azure VM çalıştırması gerekir. Mobility hizmetinin yapılandırma sunucusu şirket ile kaydeder böylece budur ve işlem sunucusu ile iletişim kurarak çoğaltma başlatabilirsiniz. Makine yanlış bir ağdaysa veya (askıda durumu veya kapatma) çalışmadığından, yapılandırma sunucusu yükü başlamak için VM Mobility hizmeti ulaşamıyor. 

1. VM içi geri iletişim başlatmak için yeniden başlatın.
2. Azure sanal makinesini başlattıktan sonra yeniden koruma işi yeniden başlatın

### <a name="error-code-8061"></a>Hata kodu 8061

**Veri deposu ESXi ana bilgisayardan erişilebilir değil.**
 
Denetleme [ana hedef önkoşulları ve desteklenen veri depoları](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) yeniden çalışma için.


## <a name="troubleshoot-failback-errors"></a>Yeniden çalışma hatalarında sorun giderme

Bu bölümde, yeniden çalışma sırasında karşılaşabileceğiniz yaygın hatalar açıklanmaktadır.

### <a name="error-code-8038"></a>Hata kodu 8038

**Şirket içi sanal makineyi hata nedeniyle getirilemedi**

Bu, şirket içi VM sağlanan yeterli belleğe sahip olmayan bir ana bilgisayarda getirilene ortaya çıkar. Bu sorunu gidermek için:

1. Daha fazla bellek ESXi ana bilgisayarda sağlayın.
2. Ayrıca, VMotion'ı VM önyükleme için yeterli belleğe sahip başka bir ESXi ana VM'ye olabilir.
