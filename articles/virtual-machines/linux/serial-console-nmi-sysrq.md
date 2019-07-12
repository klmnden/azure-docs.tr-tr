---
title: Azure seri konsol SysRq ve NMI çağrı | Microsoft Docs
description: Seri konsol SysRq ve NMI kullanarak Azure sanal makineler'de çağırır.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 87db223465c0d6680b8d60807bf90afc81e52554
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708337"
---
# <a name="use-serial-console-for-sysrq-and-nmi-calls"></a>Seri konsol SysRq ve NMI çağrıları için kullanın.

## <a name="system-request-sysrq"></a>Sistem istek (SysRq)
Bir SysRq anahtarları, bir dizi önceden tanımlanmış bir eylemi tetikleyebilir Linux işlemi sistemi çekirdeğinin tarafından anlaşılan bir dizidir. Bu komutlar, genellikle sorun giderme sanal makine veya Kurtarma (örneğin, sanal makine yanıt vermiyorsa,) ile geleneksel yönetim gerçekleştirilemez olduğunda kullanılır. Seri konsol Azure'nın SysRq özelliğini kullanarak fiziksel klavyede girdiğiniz karakterleri ve SysRq anahtar tuşlarına basarak benzetimini yapacak.

SysRq sıralı teslim sonra Çekirdek yapılandırmasını sistemin nasıl yanıt vereceğini denetleyin. SysRq devre dışı bırakma ve etkinleştirme hakkında daha fazla bilgi için bkz: *SysRq yönetici kılavuzundaki* [metin](https://aka.ms/kernelorgsysreqdoc) | [markdown](https://aka.ms/linuxsysrq).  

Azure seri konsol, aşağıda gösterilen komut çubuğunda klavye simgesini kullanarak bir Azure sanal makine bir SysRq göndermek için kullanılabilir.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg)

"SysRq komutu Gönder" seçme ortak SysRq seçenekleri sağlamak veya bir dizi iletişim kutusuna girilen SysRq komutları kabul bir iletişim kutusu açılır.  SysRq dizi kullanarak güvenli bir yeniden başlatma gibi üst düzey bir işlemi gerçekleştirmek için kullanıcının bu izin verir: `REISUB`.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-sysreq_UI.png)

Sanal makinelerde, durduruldu veya duyarlı olmayan bir durumda olan çekirdek SysRq komutu kullanılamaz. (örneğin çekirdek Panik).

### <a name="enable-sysrq"></a>SysRq etkinleştir 
Bölümünde anlatıldığı gibi *SysRq yönetici kılavuzundaki* SysRq yukarıdaki yapılandırılabilir gibi tüm, none veya yalnızca belirli komutları kullanılabilir. Aşağıdaki adımları kullanarak tüm SysRq komutları etkinleştirebilirsiniz, ancak bir yeniden başlatma titanik'ten değil:
```
echo "1" >/proc/sys/kernel/sysrq
```
SysReq yapılandırma kalıcı hale getirmek için tüm SysRq komutları etkinleştirmek için aşağıdakileri yapabilirsiniz.
1. Bu satıra ekleyerek */etc/sysctl.conf* <br>
    `kernel.sysrq = 1`
1. Yeniden başlatmadan veya çalıştırarak sysctl güncelleştiriliyor <br>
    `sysctl -p`

### <a name="command-keys"></a>Komut anahtarları 
SysRq Yönetici Kılavuzu yukarıdaki:

|Komut| İşlev
| ------| ----------- |
|``b``  |   Hemen eşitleniyor veya disklerinizi çıkarma sırasında sistem yeniden başlatılır.
|``c``  |   Sistem tarafından bir NULL işaretçi gerçekleştirecek başvuru. Yapılandırılmış bir dosyası alınır.
|``d``  |   Tutulan tüm kilitleri gösterir.
|``e``  |   İnit dışındaki tüm işlemler bir SIGTERM gönderin.
|``f``  |   Bir bellek hog işlemi sonlandırmak için oom kaldırıcı çağırır, ancak hiçbir şey sonlandırılabilir, Acil değildir.
|``g``  |   Kgdb (çekirdek hata ayıklayıcısı) tarafından kullanılan
|``h``  |   Yardım görüntülenir (burada listelenenden herhangi bir tuşa Yardım, ayrıca görüntülenir ancak ``h`` :-) kolay
|``i``  |    İnit dışındaki tüm işlemler bir SIGKILL gönderin.
|``j``  |    Zorla "Yalnızca çözme," - dosya sistemleri tarafından FIFREEZE IOCTL dondurulmuş.
|``k``  |    Güvenli erişim anahtarı (SAK) tüm programları mevcut sanal Konsolu durur. NOT: SAK bölümünde aşağıdaki önemli yorumlara bakın.
|``l``  |    Bir yığının için tüm etkin CPU gösterir.
|``m``  |    Konsolu için geçerli bellek bilgileri döküm.
|``n``  |    RT görevleri mümkün kullanışlı hale getirmek için kullanılır
|``o``  |    Sisteminizi (yapılandırılmış ve desteklenen varsa) kapatır.
|``p``  |    Geçerli kaydeder ve konsola bayrakları döküm.
|``q``  |    CPU tüm armed hrtimers listesi dökümü (ama normal timer_list zamanlayıcılar) ve tüm clockevent cihazlar hakkında ayrıntılı bilgi.
|``r``  |    Klavye ham modunu kapatır ve XLATE için ayarlar.
|``s``  |    Tüm bağlı dosya sistemleri eşitleme girişiminde bulunur.
|``t``  |    Geçerli görev ve bilgilerine, konsola listesi dökümü.
|``u``  |    Tüm bağlı dosya sistemleri salt okunur yeniden bağlamak dener.
|``v``  |    Zorla framebuffer konsol geri yükler
|``v``  |    Neden ETM arabellek döküm [ARM özgü]
|``w``  |    Kesintisiz (engellenen) durumda olan görevleri dökümünü yapar.
|``x``  |    Xmon arabirimde ppc/powerpc platformları tarafından kullanılır. Genel PMU'ya kaydeder sparc64 üzerinde gösterir. MIPS tüm TLB girişlerde dökümü.
|``y``  |    Genel CPU yazmaçlarını [SPARC 64 özel] Göster
|``z``  |    Ftrace arabellek dökümü
|``0``-``9`` | Çekirdek iletileri konsolunuza yazdırılmasını denetleme Konsolu günlük düzeyini ayarlar. (``0``, böylece yalnızca Acil Durum iletileri gibi Paniğiyle veya OOPSes konsolunuza yapacağınız örnek, yapacağı için.)

### <a name="distribution-specific-documentation"></a>Dağıtım özgü belgelere yönlendirir ###
SysRq ve kilitlenme bilgi dökümü bir SysRq "Kilitlenme" komutunu aldığında oluşturmak için adımları Linux yapılandırmak için ilgili dağıtım özel belgeler için aşağıdaki bağlantılara bakın:

#### <a name="ubuntu"></a>Ubuntu ####
 - [Çekirdek kilitlenme dökümü](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)

#### <a name="red-hat"></a>Red Hat ####
- [SysRq tesis nedir ve nasıl kullanılır?](https://access.redhat.com/articles/231663)
- [RHEL sunucudan bilgi toplamak için SysRq özelliğini kullanma](https://access.redhat.com/solutions/2023)

#### <a name="suse"></a>SUSE ####
- [Çekirdek çekirdek döküm yakalama yapılandırın](https://www.suse.com/support/kb/doc/?id=3374462)

#### <a name="coreos"></a>CoreOS ####
- [Kilitlenme günlüklerini toplama](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="non-maskable-interrupt-nmi"></a>Maskelenemez olmayan kesme (NMI) 
Maskelenemez olmayan bir kesinti (NMI) yazılımı bir sanal makinede değil yoksayacak bir sinyal oluşturmak için tasarlanmıştır. Tarihsel olarak, NMIs belirli yanıt süreleri gerektiren sistemleri donanım sorunları izlemek için kullanılır.  Bugün, programcılar ve sistem yöneticileri bir mekanizma NMI hata ayıklama veya yanıt vermeyen sistemleri gidermek için genellikle kullanın.

Seri konsol, aşağıda gösterilen komut çubuğunda klavye simgesini kullanarak bir Azure sanal makine bir NMI göndermek için kullanılabilir. NMI teslim sonra sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetleyin.  Linux işletim sistemleri için kilitlenme yapılandırılabilir ve bir bellek dökümü işletim sistemi oluşturma, bir NMI alır.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg) <br>

Çekirdek parametreleri yapılandırmak için sysctl destekleyen Linux sistemleri için aşağıdaki komutu kullanarak bu NMI alırken bir Panik etkinleştirebilirsiniz:
1. Bu satıra ekleyerek */etc/sysctl.conf* <br>
    `kernel.panic_on_unrecovered_nmi=1`
1. Yeniden başlatmadan veya çalıştırarak sysctl güncelleştiriliyor <br>
    `sysctl -p`

Dahil olmak üzere Linux Çekirdek yapılandırmaları hakkında daha fazla bilgi için `unknown_nmi_panic`, `panic_on_io_nmi`, ve `panic_on_unrecovered_nmi`, bkz: [/ Proc/sys/çekirdek/belgelerine *](https://www.kernel.org/doc/Documentation/sysctl/kernel.txt). NMI ve kilitlenme bilgi dökümü bir NMI aldığında oluşturmak için adımları Linux yapılandırmak için ilgili dağıtım özel belgeler için aşağıdaki bağlantılara bakın:
 
### <a name="ubuntu"></a>Ubuntu 
 - [Çekirdek kilitlenme dökümü](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)

### <a name="red-hat"></a>Red Hat 
 - [Bir NMI nedir ve ne için kullanılır?](https://access.redhat.com/solutions/4127)
 - [Sistemimin NMI anahtar gönderildiğinde çökmesine neden nasıl yapılandırabilirim?](https://access.redhat.com/solutions/125103)
 - [Yönetici Kılavuzu kilitlenme dökümü](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/pdf/kernel_crash_dump_guide/kernel-crash-dump-guide.pdf)

### <a name="suse"></a>SUSE 
- [Çekirdek çekirdek döküm yakalama yapılandırın](https://www.suse.com/support/kb/doc/?id=3374462)

### <a name="coreos"></a>CoreOS 
- [Kilitlenme günlüklerini toplama](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri Konsolu Linux belgeleri sayfasında bulunduğu [burada](serial-console.md).
* Seri konsol uygulamasına önyükleme kullanılacağını [kaz ve tek kullanıcı moduna gir](serial-console-grub-single-user-mode.md)
* Seri konsol için de kullanılabilir olan [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md)