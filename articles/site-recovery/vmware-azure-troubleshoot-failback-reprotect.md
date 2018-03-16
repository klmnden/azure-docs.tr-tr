---
title: "Azure vm'lerinde Azure Site Recovery ile şirket içi VMware için yeniden çalışma sırasında hatalarını giderme | Microsoft Docs"
description: "Bu makale ortak yeniden çalışma ve yükü hatalarını yeniden çalışma sırasında VMware Azure'dan Azure Site Recovery kullanarak sorun gidermek için yolları açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: rajanaki
ms.openlocfilehash: 480c3524ad4fb8a8c6ea02f09b8d27f254da9b08
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Azure'dan VMware için sorun giderme

Bu makalede Azure kullanarak yük devretme işleminden sonra şirket içi VMware altyapınızı geri Azure VM'ler başarısız olduğunda karşılaşabileceğiniz sorunları gidermek nasıl [Azure Site Recovery](site-recovery-overview.md).

Yeniden çalışma temelde iki ana adımdan oluşur. Birinci adım için yük devretme sonrasında, çoğaltma başlatmaları böylece şirket içi Azure Vm'leri koruyun gerekir. İkinci adım, şirket içi sitenize başarısız azure'dan bir yük devretme çalıştırmaktır.

## <a name="troubleshoot-reprotection-errors"></a>Yükü hatalarında sorun giderme

Bu bölümde, ortak yükü hataları ve bunların nasıl düzeltileceği ayrıntıları.

### <a name="error-code-95226"></a>Hata kodu 95226

**Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için olmadığından yeniden koruma başarısız oldu.**

Bu hata oluştuğunda zaman:

* Azure VM şirket içi yapılandırma sunucusuna erişemiyor. VM bulunan ve yapılandırma sunucusuna kayıtlı.
* Inmage Scout uygulama hizmeti yük devretme sonrasında Azure VM'de çalışmıyor. Hizmet, şirket içi yapılandırma sunucusu ile iletişim için gereklidir.

Bu sorunu gidermek için:

* Azure VM ağı şirket içi yapılandırma sunucusu ile iletişim kurmak Azure VM izin verdiğini denetleyin. Siteden siteye VPN, şirket içi veri merkezine ayarlama veya özel Azure VM sanal ağ eşleme ile bir Azure ExpressRoute bağlantı yapılandırın.
* VM şirket içi yapılandırma sunucusu ile iletişim kurabilir, VM'ye oturum açın. Sonra Inmage Scout uygulama hizmeti denetleyin. Şu çalışmadığını görüyorsanız, hizmeti elle başlatın. Hizmet başlatma türü onay ayarlanmış **otomatik**.

### <a name="error-code-78052"></a>Hata kodu 78052

**Sanal makine için koruma tamamlanamadı.**

Yeniden çalıştırma işlemini gerçekleştiriyorsanız ana hedef sunucusunda aynı ada sahip bir VM zaten varsa bu sorun oluşabilir.

Bu sorunu gidermek için:

* Yükü makine adları birbiriyle çakışır burada olmayan farklı bir konak üzerinde oluşturur, böylece farklı bir ana bilgisayardaki farklı bir ana hedef sunucusu seçin.
* Ana hedef için farklı bir konak adı çakışma burada gerçekleşmeyecek taşımak için VMotion'ı da kullanabilirsiniz. Var olan VM parazit makine ise, böylece yeni VM aynı ESXi ana bilgisayarda oluşturulabilir yeniden adlandırın.


### <a name="error-code-78093"></a>Hata kodu 78093

**VM, askıdaki bir durumda veya erişilebilir çalışmıyor.**

Bu sorunu gidermek için:

Böylece Mobility hizmeti Yazmaçları can ve yapılandırma sunucusu şirket içi işlem sunucusuyla iletişim kurarak çoğaltma işlemi başlatma başarısız oldu üzerinden VM yeniden korumak için Azure VM çalıştırması gerekir. Makine yanlış bir ağ veya çalışan (durum veya kapatma aşağı askıda değil) ise, yapılandırma sunucusu yükü başlamak için VM Mobility hizmeti ulaşamıyor.

* VM içi geri iletişim başlatmak için yeniden başlatın.
* Azure sanal makine başlatıldıktan sonra yeniden koruma işi yeniden başlatın.

### <a name="error-code-8061"></a>Hata kodu 8061

**Veri deposu ESXi ana bilgisayardan erişilebilir değil.**

Denetleme [ana hedef önkoşulları ve desteklenen veri depoları](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) yeniden çalışma için.


## <a name="troubleshoot-failback-errors"></a>Yeniden çalışma hatalarında sorun giderme

Bu bölümde, yeniden çalışma sırasında karşılaşabileceğiniz yaygın hatalar açıklanmaktadır.

### <a name="error-code-8038"></a>Hata kodu 8038

**Şirket içi sanal makineyi hata nedeniyle getirilemedi.**

Şirket içi VM sağlanan yeterli belleğe sahip olmayan bir ana bilgisayarda getirilene bu sorunu olur. 

Bu sorunu gidermek için:

* Daha fazla bellek ESXi ana bilgisayarda sağlayın.
* Ayrıca, VM VM önyükleme için yeterli belleğe sahip başka bir ESXi konağına taşımak için VMotion'ı kullanabilirsiniz.
