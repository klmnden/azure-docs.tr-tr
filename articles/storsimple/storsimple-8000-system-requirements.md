---
title: StorSimple 8000 serisi sistem gereksinimleri | Microsoft Docs
description: Yazılım, ağ ve yüksek oranda kullanılabilirlik gereksinimleri ve Microsoft Azure StorSimple çözümünüzle için en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 1a9cdf31c5924d22d968cd99383417ba371cd1c3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
ms.locfileid: "28011070"
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000 serisi yazılım, yüksek kullanılabilirlik ve ağ gereksinimleri

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple hoşgeldiniz. Bu makalede önemli sistem gereksinimleri ve StorSimple cihazınız için ve aygıta erişirken depolama istemciler için en iyi uygulamaları açıklar. Önce StorSimple sisteminizi dağıtma ve ardından geri gerektiği gibi dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatle gözden geçirmenizi öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemciler için yazılım gereksinimleri** -desteklenen işletim sistemleri ve bu işletim sistemleri için tüm ek gereksinimleri açıklanmaktadır.
* **StorSimple cihazı için ağ gereksinimleri** -için iSCSI, Bulut veya yönetim trafiğe izin verme, Güvenlik Duvarı'nda açılması gereken bağlantı noktaları hakkında bilgi sağlar.
* **StorSimple için yüksek kullanılabilirlik gereksinimlerini** - yüksek kullanılabilirlik gereksinimlerini açıklar ve StorSimple cihaz ve ana bilgisayar için'en iyi uygulamalar.

## <a name="software-requirements-for-storage-clients"></a>Depolama istemciler için yazılım gereksinimleri

StorSimple Cihazınızı erişim depolama istemciler için aşağıdaki yazılım gereksinimleri verilmiştir.

| Desteklenen işletim sistemleri | Gerekli sürümü | Ek gereksinimler/Notlar |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |StorSimple iSCSI birimleri, yalnızca aşağıdaki Windows disk türleri üzerinde kullanım için desteklenir:<ul><li>Temel diskteki basit birim</li><li>Dinamik diskteki basit ve yansıtılmış birim</li></ul>Yalnızca yazılım iSCSI başlatıcıları işletim sisteminde yerel olarak mevcut desteklenir. Donanım iSCSI başlatıcıları desteklenmez.<br></br>Bir StorSimple iSCSI birim kullanıyorsanız, Windows Server 2012 ve 2016 ölçülü kaynak sağlama ve ODX özellikler desteklenir.<br><br>Ölçülü kaynak kullanan ve tamamen sağlanan StorSimple oluşturabilir birimler. Kısmen sağlanan birimler oluşturamazsınız.<br><br>Ölçülü kaynak kullanan bir birim yeniden biçimlendirme uzun sürebilir. Toplu silme ve yeniden biçimlendirme yerine yeni bir tane oluşturma öneririz. Yine ancak, bir birim yeniden biçimlendirmek tercih eder:<ul><li>Alan geri kazanma gecikmelerden kaçınmak için yeniden biçimlendirme önce aşağıdaki komutu çalıştırın: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Biçimlendirme tamamlandıktan sonra alan geri kazanma yeniden etkinleştirmek için aşağıdaki komutu kullanın:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Bölümünde açıklandığı gibi Windows Server 2012 düzeltmeyi [KB 2878635](https://support.microsoft.com/kb/2870270) , Windows Server bilgisayarınıza.</li></ul></li></ul></ul> SharePoint için StorSimple Snapshot Manager veya StorSimple bağdaştırıcısı yapılandırıyorsanız, Git [isteğe bağlı bileşenler için yazılım gereksinimleri](#software-requirements-for-optional-components). |
| VMware ESX |5.5 ve 6.0 |VMware vSphere ile iSCSI istemcisi olarak desteklenir. VAAI engelleme özelliği, VMware vsphere StorSimple cihazlarda desteklenir. |
| Linux RHEL/CentOS |5, 6 ve 7 |Açık iSCSI başlatıcısı sürümlerini 5, 6 ve 7 Linux iSCSI istemciler için destek. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX StorSimple ile şu anda desteklenmiyor.


## <a name="software-requirements-for-optional-components"></a>İsteğe bağlı bileşenler için yazılım gereksinimleri

(StorSimple Snapshot Manager ve SharePoint için StorSimple bağdaştırıcısı) isteğe bağlı StorSimple bileşenleri için aşağıdaki yazılım gereksinimleri verilmiştir.

| Bileşen | Ana bilgisayar platformu | Ek gereksinimler/Notlar |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Kullanım StorSimple anlık görüntü Yöneticisi'nin Windows Server Yedekleme/geri yükleme yansıtılmış dinamik disklerin ve herhangi bir uygulamayla tutarlı yedeklemeler için gereklidir.<br> StorSimple Snapshot Manager yalnızca Windows Server 2008 R2 SP1 (64 bit), Windows Server 2012 R2 ve Windows Server 2012 desteklenir.<ul><li>Pencere Server 2012 kullanıyorsanız, StorSimple Snapshot Manager yüklemeden önce .NET 3.5 – 4.5 yüklemeniz gerekir.</li><li>Windows Server 2008 R2 SP1'i kullanıyorsanız, StorSimple Snapshot Manager yüklemeden önce Windows Management Framework 3.0 yüklemeniz gerekir.</li></ul> |
| SharePoint için StorSimple Bağdaştırıcısı |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>SharePoint için StorSimple bağdaştırıcısı yalnızca SharePoint 2010 ve SharePoint 2013 üzerinde desteklenir.</li><li>KKY SQL Server Enterprise Edition, sürüm 2008 R2 veya 2012 gerektirir.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>StorSimple cihazınız için ağ gereksinimleri

StorSimple Cihazınızı kilitli aygıttır. Ancak, bağlantı noktaları iSCSI, Bulut ve yönetim trafiği için izin vermek için Güvenlik Duvarı'nda açılması gerekir. Aşağıdaki tabloda, Güvenlik Duvarı'nda açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* gelen istemci isteklerini Cihazınızı erişim yönünü belirtir. *Out* veya *giden* , StorSimple Cihazınızı verileri gönderir harici olarak dağıtım yönü başvuruyor: Örneğin, Internet'e giden.

| Bağlantı noktası No<sup>1,2</sup> | Giriş veya çıkış | Bağlantı noktası kapsamı | Gerekli | Notlar |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |Çıkışı |WAN |Hayır |<ul><li>Giden bağlantı noktası, güncelleştirmeleri almak için Internet erişimi için kullanılır.</li><li>Giden web proxy yapılandırılabilir kullanıcıdır.</li><li>Sistem güncelleştirmelere izin vermek için bu bağlantı noktası da IP'leri sabit denetleyici için açık olması gerekir.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |Çıkışı |WAN |Evet |<ul><li>Giden bağlantı noktası, bulut veri erişimi için kullanılır.</li><li>Giden web proxy yapılandırılabilir kullanıcıdır.</li><li>Sistem güncelleştirmelere izin vermek için bu bağlantı noktası da IP'leri sabit denetleyici için açık olması gerekir.</li><li>Bu bağlantı noktası, atık toplama için her iki denetleyicilerinde de kullanılır.</li></ul> |
| UDP 53 (DNS) |Çıkışı |WAN |Bazı durumlarda; notlarına bakın. |Yalnızca bir Internet tabanlı DNS sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir. |
| UDP 123 (NTP) |Çıkışı |WAN |Bazı durumlarda; notlarına bakın. |Yalnızca bir Internet tabanlı NTP sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir. |
| TCP 9354 |Çıkışı |WAN |Evet |Giden bağlantı noktası tarafından StorSimple cihaz StorSimple cihaz Yöneticisi hizmeti ile iletişim kurmak için kullanılır. |
| 3260 (iSCSI) |İçinde |LAN |Hayır |Bu bağlantı noktası üzerinde iSCSI verilere erişmek için kullanılır. |
| 5985 |İçinde |LAN |Hayır |Gelen bağlantı noktası StorSimple Snapshot Manager tarafından StorSimple aygıtıyla iletişim kurmak için kullanılır.<br>Uzaktan Windows PowerShell için StorSimple için HTTP üzerinden bağlandığınızda bu bağlantı noktası da kullanılır. |
| 5986 |İçinde |LAN |Hayır |Uzaktan Windows PowerShell için StorSimple için HTTPS üzerinden bağlandığınızda bu bağlantı noktası kullanılır. |

<sup>1</sup> hiçbir gelen bağlantı noktalarının genel Internet'te açılması gerekir.

<sup>2</sup> birden çok bağlantı noktası bir ağ geçidi yapılandırmasını gerçekleştirmek, giden yönlendirilen trafiği sipariş açıklanan bağlantı noktası yönlendirme sipariş göre belirlenir [bağlantı noktası yönlendirme](#routing-metric), aşağıdaki.

<sup>3</sup> StorSimple Cihazınızda IP'leri sabit denetleyici yönlendirilebilir ve Internet'e doğrudan veya yapılandırılmış web proxy'si aracılığıyla bağlanılamıyor olması gerekir. Sabit IP adreslerini cihaz güncelleştirmeler Bakım ve atık toplama için kullanılır. Aygıt denetleyicileri sabit IP'leri üzerinden İnternete bağlanamazsa, StorSimple Cihazınızı güncelleştirmek mümkün olmaz ve atık toplama düzgün şekilde çalışmaz.

> [!IMPORTANT]
> Güvenlik Duvarı'nı değil değiştirmek veya tüm SSL trafiği StorSimple cihazı ve Azure arasında şifresini emin olun.


### <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL düzenleri

Ağ yöneticileri genellikle gelen filtrelemek için URL desenlerini ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. StorSimple Cihazınızı ve StorSimple cihaz Yöneticisi hizmeti Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucuları gibi diğer Microsoft uygulamaları bağlıdır. Bu uygulamalarla ilişkili URL desenlerini, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerini değiştirebilirsiniz anlamak önemlidir. Bu sırayla izleyebilir ve güvenlik duvarı kuralları, StorSimple için ve gerektiğinde güncelleştirmek için ağ yöneticisine gerektirecektir.

Çoğu durumda liberally sabit IP adresleri, StorSimple göre giden trafiği için güvenlik duvarı kuralları ayarlamanızı öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> (Kaynak) IP'leri cihaz için tüm etkin ağ arabirimlerinin her zaman ayarlanması gerekir. IP'leri ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>Azure portal için URL düzenleri

| URL deseni | Bileşen/işlevi | Cihaz IP'leri |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple Cihaz Yöneticisi hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik doğrulama hizmeti |Bulut etkin ağ arabirimleri |
| `https://*.backup.windowsazure.com` |Cihaz kaydı |Yalnızca veri 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Sertifika iptali |Bulut etkin ağ arabirimleri |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut etkin ağ arabirimleri |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |IP'ler yalnızca sabit denetleyici |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |IP'ler yalnızca sabit denetleyici |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek Paketi |Bulut etkin ağ arabirimleri |

#### <a name="url-patterns-for-azure-government-portal"></a>URL desenlerini Azure kamu portalı

| URL deseni | Bileşen/işlevi | Cihaz IP'leri |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login.microsoftonline.us` |StorSimple Cihaz Yöneticisi hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik doğrulama hizmeti |Bulut etkin ağ arabirimleri |
| `https://*.backup.windowsazure.us` |Cihaz kaydı |Yalnızca veri 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Sertifika iptali |Bulut etkin ağ arabirimleri |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut etkin ağ arabirimleri |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |IP'ler yalnızca sabit denetleyici |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |IP'ler yalnızca sabit denetleyici |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek Paketi |Bulut etkin ağ arabirimleri |

### <a name="routing-metric"></a>Yönlendirme ölçümü

Bir yönlendirme ölçümü arabirimleri ve verileri belirtilen ağa yönlendirme ağ geçidi ile ilişkilidir. Aynı hedefe birden çok yol var öğrenir yönlendirme ölçüm yönlendirme protokolü tarafından belirli bir hedefe en iyi yolu hesaplamak için kullanılır. Daha düşük bir yönlendirme metriği, daha yüksek öncelik.

Birden çok ağ arabirimleri ve ağ geçitleri kanal trafiği için yapılandırıldıysa, StorSimple bağlamında oyuna içinde arabirimler kullanılan göreli sırayı belirlemek için yönlendirme ölçümleri gelecektir. Yönlendirme ölçümleri kullanıcı tarafından değiştirilemez. Ancak kullanabilirsiniz `Get-HcsRoutingTable` yönlendirme tablosunu (ve ölçümleri), StorSimple Cihazınızda yazdırmak için cmdlet. Get-HcsRoutingTable cmdlet hakkında daha fazla bilgi [sorun giderme StorSimple dağıtım](storsimple-troubleshoot-deployment.md).

Güncelleştirme 2 ve sonraki sürümler için kullanılan yönlendirme ölçüm algoritmasını şu şekilde açıklanabilir.

* Önceden tanımlanmış değerler atanan ağ arabirimlerine.
* Bunlar bulut etkin olduğunda çeşitli ağ arabirimleri için atanan değerlerin ya da devre dışı bulut ile ancak yapılandırılmış bir ağ geçidi aşağıda gösterilen örnek bir tabloyu göz önünde bulundurun. Burada atanan değerlerin yalnızca örnek değerler olduğunu unutmayın.

    | Ağ arabirimi | Bulut etkin | Ağ geçidi ile Bulut devre dışı |
    |-----|---------------|---------------------------|
    | Veri 0  | 1            | -                        |
    | Veri 1  | 2            | 20                       |
    | Veri 2  | 3            | 30                       |
    | Veri 3  | 4            | 40                       |
    | Veri 4  | 5            | 50                       |
    | Veri 5  | 6            | 60                       |


* Bulut trafiği ağ arabirimleri aracılığıyla yönlendirilir sırası şöyledir:
  
    *Veri 0 > veri 1 > tarih 2 > veri 3 > veri 4 > veri 5*
  
    Bu, aşağıdaki örnekte açıklanabilir.
  
    StorSimple cihazı iki bulut etkin ağ arabirimleri, veri 0 ve verileri 5 ile göz önünde bulundurun. Veri 1 ile veri 4 Bulut devre dışı ancak yapılandırılmış bir ağ geçidi sahip. Trafik bu aygıt için yönlendirilir sıralaması olacaktır:
  
    *Veri 0 (1) > veri 5 (6) > veri 1 (20) > veri 2 (30) > veri 3 (40) > veri 4 (50)*
  
    *Parantez içindeki sayılar ilgili yönlendirme ölçümleri gösterir.*
  
    Veri 0 başarısız olursa, bulut trafiği veri 5'ten yönlendirilmiş. O veri 0 ve verileri 5 arızalanması durumunda diğer tüm ağ üzerinde yapılandırılmış bir ağ geçidi, bulut trafik verileri 1'den geçer.
* Bir bulut etkin ağ arabiriminin başarısız olursa, 3 yeniden denemeyi arabirimine bağlamak için bir 30 saniyelik bir gecikme ile olan. Tüm yeniden denemeler başarısız olursa, trafik yönlendirme tablosu tarafından belirlenen şekilde sonraki kullanılabilir bulut etkin arabirimine yönlendirilir. Başarısız tüm bulut etkin ağ arabirimleri, ardından cihaz üzerinden diğer denetleyici (Bu durumda hiçbir reboot) için başarısız olur.
* Bir iSCSI etkin ağ arabiriminin VIP hatası varsa, 3 yeniden denemeyi 2 saniye gecikmeyle olacaktır. Bu davranış önceki sürümlerden aynı stayed. Tüm iSCSI ağ arabirimleri başarısız olursa, denetleyici yük devretmesi (yeniden başlatma işlemi tarafından accompanied) meydana gelir.
* VIP başarısız olduğunda bir uyarı da StorSimple Cihazınızda tetiklenir. Daha fazla bilgi için Git [uyarı hızlı başvuru](storsimple-8000-manage-alerts.md).
* Yeniden deneme sayısı bakımından iSCSI bulut üzerinden öncelikli olur.
  
    Aşağıdaki örneği göz önünde bulundurun: A aygıtın iki ağ arabirimin StorSimple, veri 0 ve 1 Data. Veri 0 bulut etkin Data 1 hem bulut iken ve iSCSI etkin. Bu aygıtta diğer ağ arabirimleri, bulut ya da iSCSI için etkinleştirilir.
  
    Verilen veri 1 başarısız son iSCSI ağ arabirimi ise, bu diğer denetleyicisinde verileri 1'e bir denetleyici yük devretme kümesinde sonuçlanır.

### <a name="networking-best-practices"></a>Ağ en iyi uygulamalar

StorSimple çözümünüzün en iyi performans için yukarıdaki ağ gereksinimlerine ek olarak aşağıdaki en iyi yöntemleri uyar:

* StorSimple Cihazınızı ayrılmış 40 MB/sn bant genişliği (veya daha fazla) sahip olduğundan emin olun her zaman kullanılabilir. Bu bant genişliği Paylaşılmaması gerektiğini (veya ayırma QoS ilkeleri kullanılarak garanti) diğer uygulamalar ile.
* Internet ağ bağlantısı her zaman kullanılabilir olduğundan emin olun. Aralıklı ya da güvenilir olmayan Internet bağlantıları doğabilecek, Internet bağlantısı aygıtlar için desteklenmeyen bir yapılandırmada neden olur.
* Ağ arabirimleri aygıtınızda iSCSI ve bulut erişimi için ayrılmış tarafından iSCSI ve bulut trafiği yalıtabilirsiniz. Daha fazla bilgi için bkz: nasıl yapılır [ağ arabirimleri değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces) , StorSimple Cihazınızda.
* Bir bağlantı toplama Denetim Protokolü (LACP) yapılandırması ağ arabirimleri için kullanmayın. Bu desteklenmeyen bir yapılandırmadır.

## <a name="high-availability-requirements-for-storsimple"></a>StorSimple için yüksek oranda kullanılabilirlik gereksinimleri

StorSimple çözümünüzle birlikte gelen donanım platformu, veri merkezinizdeki bir yüksek oranda kullanılabilir, hataya dayanıklı bir depolama altyapısı için bir temel sağlayan kullanılabilirliği ve güvenilirliği özelliklere sahiptir. Ancak, gereksinimler ve StorSimple çözümünüzün kullanılabilirliğini sağlamaya yardımcı olmak için uymanız en iyi yöntemler vardır. StorSimple dağıtmadan önce dikkatle bağlı ana bilgisayarlar ve StorSimple cihazı için en iyi yöntemler ve aşağıdaki gereksinimleri gözden geçirin.

İzleme ve StorSimple Cihazınızı donanım bileşenlerinin bakımı hakkında daha fazla bilgi için Git [İzleyicisi donanım bileşenleri ve durumunu StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-monitor-hardware-status.md) ve [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Yüksek oranda kullanılabilirlik gereksinimleri ve StorSimple cihazınız için yordamlar

Dikkatle StorSimple Cihazınızı yüksek kullanılabilirliğini sağlamak için aşağıdaki bilgileri gözden geçirin.

#### <a name="pcms"></a>PCMs

StorSimple cihazları yedekli, hot Swap güç ve soğutma modülleri (PCMs) içerir. Her PCM tüm kasa servis sağlamak için yeterli kapasiteye sahiptir. Yüksek kullanılabilirlik sağlamak için her iki PCMs yüklenmesi gerekir.

* Güç kaynağı başarısız olursa kullanılabilirlik sağlamak için farklı güç kaynakları, PCMs bağlayın.
* Bir PCM başarısız olursa, yenisini hemen isteyin.
* Yalnızca değiştirme varsa ve bunu yüklenmeye hazır olduğunda başarısız PCM kaldırın.
* Her iki PCMs eşzamanlı olarak kaldırmayın. PCM modülü, yedek pil modülü içerir. PCMs her ikisi de kaldırmak pil koruma kapatılmadan neden olacak ve cihaz durumu kaydedilmeyecek. Pil hakkında daha fazla bilgi için Git [yedek pil modülü korumak](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Denetleyici modülleri

StorSimple cihazları yedekli, hot Swap denetleyicisi modülleri içerir. Denetleyici modülleri bir etkin/pasif şekilde çalışır. Herhangi bir anda belirli bir denetleyici modülü etkin ve bir Denetleyici Modülü pasif olsa da, ağlardaki hizmet. Pasif denetleyiciyi modülü açık olduğundan ve etkin Denetleyici Modülü hata verirse veya işlemsel duruma kaldırıldı. Her Denetleyici Modülü tüm kasa servis sağlamak için yeterli kapasiteye sahiptir. Yüksek kullanılabilirlik sağlamak için her iki denetleyicisi modülleri yüklenmesi gerekir.

* Her iki denetleyicisi modülleri her zaman yüklü olduğundan emin olun.
* Bir Denetleyici Modülü başarısız olursa, yenisini hemen isteyin.
* Yalnızca değiştirme varsa ve bunu yüklenmeye hazır olduğunda başarısız Denetleyici Modülü kaldırın. Uzun süreler hava akışı etkiler için bir modül kaldırma ve bu nedenle sistem soğutma.
* Ağ bağlantıları hem denetleyicisi modüllerle aynıdır ve aynı ağ yapılandırması bağlı ağ arabirimine sahip emin olun.
* Denetleyici Modülü başarısız veya değiştirilmesi gerekiyorsa, başarısız olan Denetleyici Modülü değiştirmeden önce bir denetleyici modülü etkin durumda olduğundan emin olun. Bir denetleyici etkin olduğunu doğrulamak için şu adrese gidin [Cihazınızı etkin denetleyicisinde tanımlamak](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Her iki denetleyicisi modülleri aynı anda kaldırmayın. Denetleyici yük devretme işlemi devam ediyor, etmeyin bekleme Denetleyici Modülü kapatın veya kasanın kaldırın.
* Denetleyici yük devretme sonrasında, her iki Denetleyici Modülü kaldırmadan önce en az beş dakika bekleyin.

#### <a name="network-interfaces"></a>Ağ arabirimleri

StorSimple cihaz denetleyicisi modülleri her olan dört 1 Gigabit ve iki 10 Gigabit Ethernet ağ arabirimleri.

* Ağ bağlantıları hem denetleyicisi modüllerle aynıdır ve aynı ağ yapılandırmasına sahip olmak üzere bu Denetleyici Modülü arabirimler bağlı ağ arabirimleri emin olun.
* Mümkün olduğunda, bir ağ aygıtı hatası durumunda hizmet kullanılabilirliğini sağlamak için farklı anahtarlar arasındaki ağ bağlantıları'nı dağıtın.
* Yalnızca ya da son kalan iSCSI etkin arabirimi (atanan IP) ile çıkarırken varsa arabirimi önce devre dışı bırakın ve kablolarını çıkarın. Ardından arabirimi ilk takılı ise, pasif denetleyiciyi üzerinden vermesine etkin denetleyicisi neden olur. Pasif denetleyiciyi de takılı karşılık gelen arabirimlerinden varsa, ardından iki denetleyiciye de birden çok kez bir denetleyicisinde şekilde önce yeniden başlatılır.
* En az iki veri arabirimleri her denetleyici modülünden ağına bağlayın.
* İki etkinleştirilirse, 10 GbE arabirimleri farklı anahtarlara kullananlar dağıtın.
* Mümkün olduğunda, MPIO sunucularda sunucuları bağlantı, ağ veya arabirim hatalarına dayanabileceğinden emin olmak için kullanın.

Cihazınız yüksek kullanılabilirlik ve performans için ağ hakkında daha fazla bilgi için Git [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD ve HDD

Kullanılarak korunan sabit disk sürücülerinin (HDD'ler) alanları yansıtılmış ve katı hal diskleri (SSD'ler) StorSimple cihazları içerir. Yansıtılmış alanlardaki kullanımını cihaz bir veya daha fazla SSD veya de HDD hatasını tolere mümkün olmasını sağlar.

* Tüm SSD ve HDD modülleri yüklü olduğundan emin olun.
* Bir SSD veya HDD başarısız olursa, yenisini hemen isteyin.
* Bir SSD veya HDD başarısız veya değiştirilmesi gerekiyorsa, yalnızca SSD veya değiştirme gerektirir HDD kaldırdığınızdan emin olun.
* Herhangi bir noktada sisteminden birden fazla SSD veya HDD zamanında kaldırmayın.
  2 veya daha fazla disk (HDD, SSD), belirli türde bir hata veya kısa süre içinde art arda başarısız sistemi arızası ve olası veri kaybına neden olabilir. Bu gerçekleşirse, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) Yardım için.
* Değiştirme sırasında izlemek **paylaşılan bileşenleri** içinde **donanım durumu** SSD ve HDD sürücüleri için dikey. Yeşil onay durumu disklerde iyi ya da Tamam, kırmızı bir ünlem işaret ise bir başarısız SSD veya HDD gösterir gösterir.
* Bir sistem hatası durumunda korumak için gereken tüm birimler için bulut anlık görüntüleri yapılandırmanızı öneririz.

#### <a name="ebod-enclosure"></a>EBOD muhafazası

StorSimple cihaz modeli 8600 genişletilmiş demet, diskleri (EBOD) kasası birincil muhafaza yanı sıra içerir. Kullanılarak korunan sabit disk sürücülerinin (HDD'ler) alanları yansıtılmış ve bir EBOD EBOD denetleyicisi içerir. Yansıtılmış alanlardaki kullanımını cihaz bir veya daha fazla HDD hatasını tolere mümkün olmasını sağlar. EBOD muhafazası yedek SAS kabloları birincil Motoru'nu bağlı.

* Olduğundan emin olun hem EBOD muhafazası denetleyicisi modülleri hem SAS kabloları ve tüm sabit disk sürücülerinin her zaman yüklenir.
* EBOD muhafazası Denetleyici Modülü başarısız olursa, yenisini hemen isteyin.
* EBOD muhafazası Denetleyici Modülü başarısız olursa, başarısız modülü değiştirmeden önce bir denetleyici modülü etkin olduğundan emin olun. Bir denetleyici etkin olduğunu doğrulamak için şu adrese gidin [Cihazınızı etkin denetleyicisinde tanımlamak](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Sürekli bir EBOD denetleyicisi modülü değiştirme sırasında erişerek StorSimple cihaz Yöneticisi hizmeti bileşeni'nin durumunu izlemek **İzleyici** > **donanım durumu**.
* Bir SAS kablosu başarısız veya değiştirme (Microsoft Support böyle bir belirlenmesi için söz konusu) gerekiyorsa, değiştirme gerektiren SAS kablosu kaldırdığınızdan emin olun.
* Aynı anda iki SAS kabloları herhangi bir noktada sisteminden zamanında kaldırmayın.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Ana bilgisayarlar için yüksek kullanılabilirlik önerileri

StorSimple Cihazınızı bağlı konaklar yüksek kullanılabilirliğini sağlamak için bu en iyi uygulamaları dikkatle gözden geçirin.

* StorSimple ile yapılandırma [iki düğümlü dosya sunucusu küme yapılandırmaları][1]. Hata ve konak tarafındaki artıklık oluşturmak tek hata noktaları kaldırarak, çözümün tamamında yüksek oranda kullanılabilir hale gelir.
* Kullanılabilir paylaşımları sürekli olarak (CA) kullanılabilir depolama alanı denetleyicileri yük devretme sırasında yüksek kullanılabilirlik için Windows Server 2012 (SMB 3.0) kullanın. Dosya sunucusu kümesi ve kullanılabilir paylaşımları sürekli olarak Windows Server 2012 ile yapılandırmak için ek bilgi için bu başvuru [video demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple sistemi sınırları hakkında bilgi almak](storsimple-8000-limits.md).
* [StorSimple çözümünüzün dağıtmayı öğrenin](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
