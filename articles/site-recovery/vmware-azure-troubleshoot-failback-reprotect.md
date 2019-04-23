---
title: Azure Site Recovery ile azure'a VMware VM'LERİNDE olağanüstü durum kurtarma sırasında şirket içine yeniden çalışma sorunlarını giderme | Microsoft Docs
description: Bu makalede, azure'da Azure Site Recovery ile VMware VM'LERİNDE olağanüstü durum kurtarma sırasında yeniden çalışma ve yeniden koruma sorunlarını gidermenin yolları açıklanır.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 20cb7a446befb1d31f0e069d91d0230fc4a2a901
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999476"
---
# <a name="troubleshoot-failback-to-on-premises-from-azure"></a>Azure'dan şirket içine yeniden çalışma ilgili sorunları giderme

Bu makalede kullanarak azure'a yük devredildikten sonra şirket içi VMware altyapınızı geri Azure Vm'leri yük devrederken çalıştırdığınızca sorunları nasıl giderilir [Azure Site Recovery](site-recovery-overview.md).

Yeniden çalışma, temelde iki ana adımdan oluşur. Birinci adım için yük devretme işleminden sonra çoğaltmaya başlamak için şirket içi Azure Vm'lerini yeniden koruma gerekir. İkinci adım, şirket içi sitede yeniden çalıştırmak için Azure tarafından sağlanan bir yük devretme çalıştırmaktır.

## <a name="troubleshoot-reprotection-errors"></a>Yeniden koruma sorunlarını giderme

Bu bölümde, ortak yeniden koruma hataları ve bunların nasıl düzeltileceği açıklanmaktadır.

### <a name="error-code-95226"></a>Hata kodu 95226

**Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için olmadığından yeniden koruma başarısız oldu.**

Bu hata oluşur. zaman:

* Azure VM, şirket içi yapılandırma sunucusuna erişemiyor. VM bulunan ve yapılandırma sunucusuna kayıtlı.
* Inmage Scout uygulama hizmeti, yük devretme sonrasında Azure sanal makinesinde çalışmıyor. Hizmet, şirket içi yapılandırma sunucusu ile iletişim için gereklidir.

Bu sorunu çözmek için:

* Azure sanal ağı şirket içi yapılandırma sunucusu ile iletişim kurmak Azure VM verdiğinden emin olun. Şirket içi veri Merkezinize bir siteden siteye VPN ayarlama veya özel Azure VM'nin sanal ağ eşlemesi ile bir Azure ExpressRoute bağlantısı yapılandırın.
* VM, şirket içi yapılandırma sunucusu ile iletişim kurabilir, VM'de oturum açın. Ardından, Inmage Scout uygulama hizmeti olup olmadığını denetleyin. Çalışır durumda olduğunu görürseniz, hizmeti el ile başlatın. Hizmet başlatma türü onay ayarı **otomatik**.

### <a name="error-code-78052"></a>Hata kodu 78052

**Sanal makine için koruma işlemi tamamlanamadı.**

İlk duruma döndürmeden ana hedef sunucusunda aynı ada sahip bir VM zaten durumunda bu sorun oluşabilir.

Bu sorunu çözmek için:

* Yeniden koruma adları birbiriyle çakışır burada yoksa farklı bir konak üzerinde makinede oluşturur, böylece farklı bir ana bilgisayar farklı bir ana hedef sunucusunda'ı seçin.
* Ana hedef ad çakışması burada gerçekleşmeyecek farklı bir konağa taşımak için VMotion'ı da kullanabilirsiniz. Varolan bir VM'yi geçirilmeyen bir makine ise, yeni sanal makine aynı ESXi ana bilgisayarındaki oluşturulabilir olacak şekilde yeniden adlandırın.


### <a name="error-code-78093"></a>Hata kodu 78093

**VM, askıda bir durumda veya erişilebilir değil çalışmıyor.**

Bu sorunu çözmek için:

İşlem sunucusu ile iletişim kurarak çoğaltma sunucusu şirket içi yapılandırma ve can ile Mobility hizmetini kaydeder başlatılması devredilen VM yeniden korumak için Azure VM çalıştırılması gerekir. Makineyi hatalı bir ağdaysa veya (yanıt vermiyor veya kapatma) çalışmadığından, yeniden korumaya başlamak için sanal makine üzerindeki Mobility hizmetinin yapılandırma sunucusuna erişemiyor.

* Tekrar şirket içine iletişim başlatılabilmesi VM'yi yeniden başlatın.
* Azure sanal makine başlatıldıktan sonra yeniden koruma işi yeniden başlatın.

### <a name="error-code-8061"></a>Hata kodu 8061

**Veri deposu ESXi konağından erişilebilir değil.**

Denetleme [ana hedef önkoşulları ve desteklenen veri depoları](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) yeniden çalışma için.


## <a name="troubleshoot-failback-errors"></a>Yeniden çalışma sorunlarını giderme

Bu bölümde, yeniden çalışma sırasında karşılaşabileceğiniz genel hatalar açıklanmaktadır.

### <a name="error-code-8038"></a>Hata kodu 8038

**Şirket içi sanal makineyi hata nedeniyle çevrimiçine alınamadı.**

Şirket içi VM sağlanan yeterli belleğe sahip olmayan bir konak üzerinde olana olduğunda bu sorun ortaya çıkar. 

Bu sorunu çözmek için:

* Daha fazla bellek ESXi ana bilgisayarındaki sağlayın.
* Ayrıca, VM VM'yi önyüklemek için yeterli belleğe sahip başka bir ESXi konağına taşımak için VMotion'ı kullanabilirsiniz.
