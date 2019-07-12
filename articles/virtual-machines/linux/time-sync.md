---
title: Azure'da Linux VM'ler için eşitleme zaman | Microsoft Docs
description: Zaman eşitleme Linux sanal makineleri için.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 3271804a80ededae650c3b6ad34fdb7f9f1e5b18
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708612"
---
# <a name="time-sync-for-linux-vms-in-azure"></a>Azure'da Linux VM'ler için zaman eşitleme

Zaman eşitleme, güvenlik ve olay bağıntısı için önemlidir. Bazen, dağıtılmış işlemler uygulama için kullanılır. Birden çok bilgisayar sistemleri arasındaki zaman doğruluğu eşitleme ile elde edilir. Eşitleme yeniden başlatma ve bir zaman kaynağı ve zamanı getirilirken bilgisayar arasındaki ağ trafiği dahil olmak üzere birden çok şey etkilenebilir. 

Azure, Windows Server 2016 çalıştıran altyapı tarafından desteklenir. Windows Server 2016, gelişmiş algoritmalar saat düzeltin ve yerel saat ile UTC eşitlemek için koşul için kullanılan sahiptir.  Windows Server 2016 doğru zamanı özelliği nasıl VMICTimeSync hizmetinin büyük ölçüde geliştirilmiş Vm'leri konak üzerinde doğru zaman yönetir. Geliştirmeler daha doğru başlangıç zamanını VM Başlat veya VM geri yükleme ve kesme gecikmesi düzeltme içerir. 

>[!NOTE]
>Windows Saat hizmeti hızlı bir genel bakış için bu göz atın [üst düzey genel bakış videosu](https://aka.ms/WS2016TimeVideo).
>
> Daha fazla bilgi için [Windows Server 2016 doğru zamanı](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Genel Bakış

Bir bilgisayar saatini doğruluk ne kadar yakın bilgisayar saati Eşgüdümlü Evrensel Saat (UTC) zamanı standart açıktır gauged. UTC yalnızca devre dışı olarak 300 yıl cinsinden bir saniye olabilir kesin atomik saatler çok uluslu bir örnek tarafından tanımlanır. Ancak UTC doğrudan okunurken özel bir donanım gerektirir. Bunun yerine, saat sunucuları UTC'ye eşitlenir ve ölçeklenebilirlik ve sağlamlık sağlamak için diğer bilgisayarlardan erişilir. Her bilgisayar eşitleme hizmeti çalıştıran kullanmak için hangi saat sunucuları bilir ve bilgisayar saatini düzeltilmesi gerekip gerekmediğini düzenli olarak denetler ve gerekirse süreyi ayarlar. 

Azure ana Katman 1 Microsoft'a ait cihazlardan, GPS anten ile kendi zaman iç Microsoft saat sunucuları eşitlenir. Azure sanal makineler'de ya da doğru zaman geçirmek için konakta bağlıdır (*konak zaman*) VM veya VM'yi açın doğrudan saat saat sunucusu veya her ikisinin bir birleşiminde alabilirsiniz. 

Tek başına bir donanımda Linux işletim sistemi yalnızca ana bilgisayar donanımına okur. önyükleme saati. Bundan sonra saatin kesme Zamanlayıcı Linux çekirdek kullanarak korunur. Bu yapılandırmada, saat, zaman içinde farklı. Azure'da yeni Linux dağıtımları Vm'leri konaktan daha sık saat güncelleştirmeleri için Sorgulanacak Linux Integration services (LIS) dahil VMICTimeSync sağlayıcısını kullanabilirsiniz.

Sanal makine konak etkileşim saati da etkileyebilir. Sırasında [Bakımı koruma bellek](maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot), Vm'leri 30 saniyeye kadar duraklatıldı. Örneğin, bakım başlamadan önce VM saat 10:00:00 AM'den gösterir ve 28 saniye sürer. VM çağrıldıktan sonra sanal makinedeki saat 10:00:00 28 saniye olabilecek'da, yine de gösterebilir devre dışı. Bu, doğru VMICTimeSync hizmet konak ve istemleri değişiklikler için VM'ler üzerinde gerçekleştirilecek dengelemek için neler olduğunu izler.

Zaman eşitleme çalışmıyor olmadan, sanal makinedeki saatin hataları accumulate. Yalnızca bir VM olduğunda, iş yüküne yüksek oranda doğru zaman tutma gerektirmedikçe etkisini önemli olmayabilir. Ancak çoğu durumda, biz birden fazla varsa, birbirine bağlı işlemler ve süre tüm dağıtım tutarlı olması gerekiyor izlemek için zaman kullanan VM'ler. Zaman VM'ler arasında farklı olduğunda, aşağıdaki etkileri görebilirsiniz:

- Kimlik doğrulaması başarısız olur. Güvenlik protokolleri Kerberos ister veya sertifika bağımlı teknolojisini kullanan sistemler arasında tutarlı olan zaman.
- Günlükleri (veya diğer verileri) zamanında kabul etmiyorsanız ne bir sistemde bulmamız çok zordur. Aynı olay, farklı zamanlarda yapmayı bağıntı zor oluştu gibi görünür.
- Saat kapalıysa, faturalandırma yanlış hesaplanması.



## <a name="configuration-options"></a>Yapılandırma seçenekleri

Genellikle zaman eşitleme, Azure'da barındırılan Linux VM'ler için yapılandırmak için üç yol vardır:

- Azure Market görüntüleri için varsayılan yapılandırma, NTP zaman ve VMICTimeSync konak zamanı kullanır. 
- Konak yalnızca VMICTimeSync kullanma.
- Başka bir, dış saat sunucusu ile veya VMICTimeSync konak zamanı kullanmadan kullanın.


### <a name="use-the-default"></a>Varsayılanı kullan

Varsayılan olarak, çoğu Azure Market görüntüleri Linux için yapılandırılmış iki farklı kaynaktan eşitlemek için: 

- NTP zaman alan bir NTP sunucusundan birincil olarak. Örneğin, Ubuntu 16.04 LTS Market kullanım görüntüleri **ntp.ubuntu.com**.
- İkincil olarak VMICTimeSync hizmeti vm'lerine konak zaman iletişim kurmak ve bakım için sanal makine duraklatıldıktan sonra düzeltmeler yapmak için kullanılır. Azure konakları, doğru zamanı tutmak için Microsoft'a ait katman 1 cihazları kullanın.

Yeni Linux dağıtımları, VMICTimeSync hizmet duyarlık zaman Protokolü (MTP) kullanır, ancak önceki dağıtımların PTP destekleyemeyebilir ve fall için NTP zaman konaktan almak için geri.

NTP doğru eşitleme doğrulamak için şunu çalıştırın `ntpq -p` komutu.

### <a name="host-only"></a>Yalnızca konak 

NTP sunucuları time.windows.com ve ntp.ubuntu.com gibi ortak olduğundan, bunları ile zaman eşitlemeye trafik internet üzerinden gönderilmesi gerekir. Paket gecikmeler değişen zaman eşitleme kalitesini olumsuz yönde etkileyebilir. Yalnızca konak eşitlenecek geçerek NTP kaldırma sürenizi bazen geliştirebilir eşitleme sonuçları.

Yalnızca ana bilgisayar saati eşitleme yapar varsayılan yapılandırmayı kullanarak zaman eşitleme karşılaşırsanız, algılama sorunları geçiliyor. Sanal makinenizin üzerinde zaman eşitleme artardı görmek için yalnızca ana bilgisayar eşitleme deneyin. 

### <a name="external-time-server"></a>Dış saat sunucusu

Belirli bir zaman eşitleme gereksinimleriniz varsa da ayrıca dış saat sunucuları kullanma seçeneği mevcuttur. Dış saat sunucuları olmayan Microsoft veri merkezleri ya da işleme artık saniye içinde özel bir şekilde barındırılan makinelerle süre gerekmemesi sağlayarak, test senaryoları için yararlı olabilecek belirli bir zaman sağlayabilir.

Bir dış saat sunucusu, bir varsayılan yapılandırmaya benzer sonuçlar sağlamak için VMICTimeSync hizmeti ile birleştirebilirsiniz. Bir dış saat sunucusu VMICTimeSync ile birleştirerek, sanal makineleri için bakım duraklatıldığında neden olabilecek sorunları başa çıkmak için en iyi seçenektir. 

## <a name="tools-and-resources"></a>Araçlar ve kaynaklar

Zaman eşitleme yapılandırmanızı denetlemek için bazı temel komutlar vardır. Linux dağıtım belgelerine bu dağıtım için zaman eşitlemesini yapılandırmak için en iyi yolu üzerinde daha fazla ayrıntı sahip olur.

### <a name="integration-services"></a>Tümleştirme hizmetleri

Tümleştirme hizmeti (hv_utils) yüklenip yüklenmediğini denetleyin.

```bash
lsmod | grep hv_utils
```
Şuna benzer bir şey görmeniz gerekir:

```
hv_utils               24418  0
hv_vmbus              397185  7 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
```

Hyper-V tümleştirme hizmetleri arka plan programı çalışıp çalışmadığını.

```bash
ps -ef | grep hv
```

Şuna benzer bir şey görmeniz gerekir:

```
root        229      2  0 17:52 ?        00:00:00 [hv_vmbus_con]
root        391      2  0 17:52 ?        00:00:00 [hv_balloon]
```


### <a name="check-for-ptp"></a>PTP denetle

Bir duyarlık zaman Protokolü (MTP) saat kaynağı Linux yeni sürümleriyle kullanılabilir VMICTimeSync Sağlayıcısı'nın bir parçası olarak. Red Hat Enterprise Linux CentOS veya daha eski sürümlerindeki 7.x [Linux Tümleştirme hizmetleri](https://github.com/LIS/lis-next) indirilir ve güncelleştirilmiş sürücüyü yüklemek için kullanılır. PTP kullanırken, Linux cihaz form/dev/ptp olacaktır*x*. 

Hangi PTP saat kaynakları kullanılabilir bakın.

```bash
ls /sys/class/ptp
```

Bu örnekte, döndürülen değer olduğu *ptp0*, bu saat adı denetlemek için kullanacağız. Cihaz doğrulamak için saat adı denetleyin.

```bash
cat /sys/class/ptp/ptp0/clock_name
```

Bu döndürmelidir **hyperv**.

### <a name="chrony"></a>chrony

Red Hat Enterprise Linux ve CentOS 7.x, [chrony](https://chrony.tuxfamily.org/) PTP kaynak saat kullanmak üzere yapılandırılmış. Ağ Zaman Protokolü arka plan programının (ntpd) kullanarak, PTP kaynakları desteklemiyor **chronyd** önerilir. PTP etkinleştirmek için güncelleştirme **chrony.conf**.

```bash
refclock PHC /dev/ptp0 poll 3 dpoll -2 offset 0
```

Red Hat ve NTP hakkında daha fazla bilgi için bkz. [yapılandırma NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-configure_ntp). 

Chrony hakkında daha fazla bilgi için bkz. [chrony kullanarak](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-using_chrony).

Chrony hem TimeSync kaynakları aynı anda etkinleştirilip etkinleştirilmediğini bir olarak işaretleyebilirsiniz **tercih** yedek olarak başka bir kaynağı ayarlar. NTP Hizmetleri uzun bir süre sonra büyük farklarından dışında saati güncelleştirmez çünkü VMICTimeSync duraklatılmış VM olaylardan NTP tabanlı araçlar tek başına daha çok daha hızlı bir şekilde saatin kurtarır.


### <a name="systemd"></a>systemd 

Ubuntu ve SUSE saati eşitleme kullanılarak yapılandırılan [systemd](https://www.freedesktop.org/wiki/Software/systemd/). Ubuntu üzerinde daha fazla bilgi için bkz. [zaman eşitleme](https://help.ubuntu.com/lts/serverguide/NTP.html). SUSE hakkında daha fazla bilgi için bkz: 4.5.8 bölümünde [SUSE Linux Enterprise Server 12 SP3 sürüm notları](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/12-SP3/#InfraPackArch.ArchIndependent.SystemsManagement).



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Windows Server 2016 doğru zamanı](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time).


