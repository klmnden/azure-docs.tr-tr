---
title: StorSimple 8000 serisi sistem gereksinimleri | Microsoft Docs
description: Yazılım, ağ ve yüksek kullanılabilirlik gereksinimleri ve Microsoft Azure StorSimple çözümü için en iyi uygulamaları açıklar.
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
ms.openlocfilehash: f05e3e85d36ffc23a193a6771a0271c71b2f8544
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60631915"
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000 serisi yazılım, yüksek kullanılabilirlik ve ağ gereksinimleri

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple hoşgeldiniz. Bu makalede, önemli sistem gereksinimleri ve cihaz erişen depolama istemcilerinin ve StorSimple cihazınız için en iyi uygulamaları açıklar. Önce StorSimple sisteminizi dağıtma ve ardından geri gerektiğinde dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatlice gözden öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemcilerinin yazılım gereksinimleri** -desteklenen işletim sistemleri ve bu işletim sistemleri için tüm ek gereksinimleri açıklanmaktadır.
* **StorSimple cihazı için ağ gereksinimleri** -için iSCSI, bulut ya da Yönetim trafiğine izin vermek için güvenlik duvarını açma olması gereken bağlantı noktaları hakkında bilgi sağlar.
* **StorSimple için yüksek kullanılabilirlik gereksinimleri** - yüksek kullanılabilirlik gereksinimlerini açıklar ve StorSimple cihaz ve ana bilgisayar için'en iyi yöntemler.

## <a name="software-requirements-for-storage-clients"></a>Depolama istemcilerinin yazılım gereksinimleri

Aşağıdaki yazılım gereksinimleri, StorSimple cihazınıza erişmesine depolama istemciler için verilmiştir.

| Desteklenen işletim sistemleri | Gerekli sürümü | Ek gereksinimler/Notlar |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |StorSimple iSCSI birimleri kullanmak üzere yalnızca şu Windows disk türleri desteklenir:<ul><li>Temel diskteki basit birim</li><li>Bir dinamik diskteki basit ve yansıtılmış birim</li></ul>Yalnızca yazılım iSCSI başlatıcılarının işletim sisteminde yerel olarak mevcut desteklenir. Donanım iSCSI başlatıcılarının desteklenmez.<br></br>Bir StorSimple iSCSI birimi kullanıyorsanız, Windows Server 2012 ve 2016 ölçülü kaynak sağlama ve ODX özellikleri desteklenir.<br><br>StorSimple, ölçülü kaynak kullanan ve tam olarak sağlanan oluşturabilir birimleri. Bu kısmen sağlanan birimler oluşturamazsınız.<br><br>Ölçülü kaynak kullanan bir birimi biçimlendirmeden uzun sürebilir. Toplu silme ve ardından yeniden biçimlendirme yerine yeni bir tane oluşturarak öneririz. Yine, bir birim yeniden biçimlendirmek ancak tercih:<ul><li>Alan geri kazanma gecikmeleri önlemek için yeniden biçimlendirme önce aşağıdaki komutu çalıştırın: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Biçimlendirme tamamlandıktan sonra alan geri kazanmayı yeniden etkinleştirmek için aşağıdaki komutu kullanın:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Bölümünde anlatıldığı gibi Windows Server 2012 düzeltmeyi [KB 2878635](https://support.microsoft.com/kb/2870270) Windows Server bilgisayarınıza.</li></ul></li></ul></ul> SharePoint için StorSimple anlık görüntü yöneticisini veya StorSimple bağdaştırıcısını yapılandırıyorsanız, Git [isteğe bağlı bileşenler için yazılım gereksinimleri](#software-requirements-for-optional-components). |
| VMware ESX |5.5 ve 6.0 |VMware vSphere ile iSCSI istemci olarak desteklenir. VAAI engelleme özelliği, VMware vsphere StorSimple cihazlarda desteklenir. |
| Linux RHEL/CentOS |5, 6 ve 7 |Açık iSCSI başlatıcısı sürüm 5, 6 ve 7 ile Linux iSCSI istemciler için destek. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX StorSimple ile şu anda desteklenmiyor.


## <a name="software-requirements-for-optional-components"></a>İsteğe bağlı bileşenler için yazılım gereksinimleri

Aşağıdaki yazılım gereksinimleri, isteğe bağlı StorSimple bileşenler (StorSimple Snapshot Manager ve SharePoint için StorSimple bağdaştırıcısı) içindir.

| Bileşen | Ana bilgisayar platformu | Ek gereksinimler/Notlar |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Windows Server'da StorSimple Snapshot Manager kullanmak istediğiniz uygulamayla tutarlı yedeklemeler yapılmasını ve yansıtılmış dinamik diskler, yedekleme/geri yükleme için gereklidir.<br> StorSimple Snapshot Manager yalnızca Windows Server 2008 R2 SP1 (64-bit), Windows Server 2012 R2 ve Windows Server 2012 desteklenir.<ul><li>Pencere Server 2012 kullanıyorsanız, StorSimple Snapshot Manager'ı yüklemeden önce .NET 3.5 – 4.5 yüklemeniz gerekir.</li><li>Windows Server 2008 R2 SP1 kullanıyorsanız, StorSimple Snapshot Manager'ı yüklemeden önce Windows Management Framework 3.0 yüklemeniz gerekir.</li></ul> |
| SharePoint için StorSimple Bağdaştırıcısı |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>SharePoint için StorSimple bağdaştırıcısı yalnızca SharePoint 2010 ve SharePoint 2013 üzerinde desteklenir.</li><li>KKY SQL Server Enterprise Edition, 2008 R2 veya 2012 sürümünü gerektirir.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>StorSimple cihazınız için ağ gereksinimleri

StorSimple Cihazınızı kilitli aygıttır. Ancak, bağlantı noktaları iSCSI, Bulut ve yönetim trafiği için izin vermek için güvenlik duvarını açılması gerekir. Aşağıdaki tabloda, güvenlik duvarından açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* gelen istemci isteklerini Cihazınızı erişim yönünü gösterir. *Çıkış* veya *giden* , StorSimple Cihazınızı gönderir dışarıdan, veri dağıtımı dışında yön ifade eder: Örneğin, Internet'e giden.

| Bağlantı noktası No<sup>1,2</sup> | Daraltma veya genişletme | Bağlantı noktası kapsamı | Gerekli | Notlar |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |Çıkış |WAN |Hayır |<ul><li>Giden bağlantı noktası, güncelleştirmeleri almak için Internet erişimi için kullanılır.</li><li>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur.</li><li>Sistem güncelleştirmeleri izin vermek için bu bağlantı noktası de sabit IP'ler denetleyici için açık olması gerekir.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |Çıkış |WAN |Evet |<ul><li>Giden bağlantı noktası, bulut veri erişimi için kullanılır.</li><li>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur.</li><li>Sistem güncelleştirmeleri izin vermek için bu bağlantı noktası de sabit IP'ler denetleyici için açık olması gerekir.</li><li>Bu bağlantı noktası, çöp toplama için iki denetleyicide de kullanılır.</li></ul> |
| UDP 53 (DNS) |Çıkış |WAN |Bazı durumlarda; notlara bakın. |Yalnızca bir Internet tabanlı bir DNS sunucusu kullanıyorsanız bu bağlantı noktası gereklidir. |
| UDP 123 (NTP) |Çıkış |WAN |Bazı durumlarda; notlara bakın. |Yalnızca bir Internet tabanlı bir NTP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir. |
| TCP 9354 |Çıkış |WAN |Evet |Giden bağlantı noktası tarafından StorSimple cihazı StorSimple cihaz Yöneticisi hizmeti ile iletişim kurmak için kullanılır. |
| 3260'ın (iSCSI) |İçinde |LAN |Hayır |Bu bağlantı noktası üzerinde iSCSI verilere erişmek için kullanılır. |
| 5985 |İçinde |LAN |Hayır |Gelen bağlantı noktası, StorSimple Snapshot Manager tarafından StorSimple cihazı ile iletişim kurmak için kullanılır.<br>Uzaktan Windows PowerShell için StorSimple için HTTP üzerinden bağlandığınızda bu bağlantı noktası da kullanılır. |
| 5986 |İçinde |LAN |Hayır |Uzaktan Windows PowerShell için StorSimple için HTTPS üzerinden bağlandığınızda bu bağlantı noktası kullanılır. |

<sup>1</sup> genel Internet'te açılmasını hiçbir gelen bağlantı noktası gerekir.

<sup>2</sup> birden çok bağlantı noktası bir ağ geçidi yapılandırması gerçekleştirmek, giden yönlendirilen trafik sırasını açıklanan bağlantı noktası yönlendirme sırasını göre belirlenecektir [bağlantı noktası yönlendirme](#routing-metric)aşağıdaki.

<sup>3</sup> yönlendirilebilir ve doğrudan İnternet'e veya yapılandırılmış web Ara sunucusu üzerinden bağlanmak denetleyici IP'leri StorSimple Cihazınızda sabit olmalıdır. Sabit IP adresleri, cihaza güncelleştirmelerin uygulanması ve çöp toplama için kullanılır. Cihaz denetleyicisinin de sabit IP'ler aracılığıyla İnternet'e bağlanamıyorsanız, StorSimple Cihazınızı güncelleştirmeniz mümkün olmayacaktır ve çöp toplama düzgün çalışmaz.

> [!IMPORTANT]
> Güvenlik Duvarı'nı değiştirmeyin veya StorSimple cihazını ve Azure arasında herhangi bir SSL trafiği şifresini çözme emin olun.


### <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları

Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamaları, StorSimple cihazını ve StorSimple cihaz Yöneticisi hizmetine bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Buna karşılık, izleme ve güvenlik duvarı kuralları StorSimple'ınızı ve gerektiğinde güncelleştirme için ağ yöneticisine da gerekir.

StorSimple liberally çoğu zaman sabit IP adreslerinin, temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> (Kaynak) IP'ler cihaz için tüm etkin ağ arabirimlerinin her zaman ayarlanması gerekir. IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>Azure portalı için URL desenleri

| URL deseni | Bileşen/işlevi | Cihaz IP'ler |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik doğrulama hizmeti |Bulut özellikli ağ arabirimleri |
| `https://*.backup.windowsazure.com` |Cihaz kaydı |Yalnızca veri 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptal etme |Bulut özellikli ağ arabirimleri |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut özellikli ağ arabirimleri |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |Denetleyici IP'leri yalnızca sabit |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Denetleyici IP'leri yalnızca sabit |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek Paketi |Bulut özellikli ağ arabirimleri |

#### <a name="url-patterns-for-azure-government-portal"></a>Azure kamu portalında için URL desenleri

| URL deseni | Bileşen/işlevi | Cihaz IP'ler |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login.microsoftonline.us` |StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik doğrulama hizmeti |Bulut özellikli ağ arabirimleri |
| `https://*.backup.windowsazure.us` |Cihaz kaydı |Yalnızca veri 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptal etme |Bulut özellikli ağ arabirimleri |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut özellikli ağ arabirimleri |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |Denetleyici IP'leri yalnızca sabit |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Denetleyici IP'leri yalnızca sabit |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek Paketi |Bulut özellikli ağ arabirimleri |

### <a name="routing-metric"></a>Yönlendirme ölçüm

Bir yönlendirme ölçüm arabirimler ve verileri belirtilen ağlara yönlendirme ağ geçidi ile ilişkilendirilir. Aynı hedefe birden çok yol var öğrenir, yönlendirme ölçüm, belirli bir hedefe en iyi yolu hesaplamak için yönlendirme protokolü tarafından kullanılır. Alt yönlendirme ölçüm, daha yüksek öncelik.

Kanal trafiğini, birden çok ağ arabirimleri ve ağ geçitleri yapılandırıldıysa StorSimple bağlamında, arabirimler kullanılan göreli sırasını belirlemek için oyuna yönlendirme ölçümleri gelir. Yönlendirme ölçümleri, kullanıcı tarafından değiştirilemez. Ancak kullanabilirsiniz `Get-HcsRoutingTable` yönlendirme tablosu (ve ölçümleri), StorSimple Cihazınızda yazdırmak için cmdlet'i. Get-HcsRoutingTable cmdlet'i hakkında daha fazla bilgi [sorun giderme StorSimple dağıtım](storsimple-troubleshoot-deployment.md).

Güncelleştirme 2 ve sonraki sürümler için kullanılan yönlendirme ölçüm algoritma şu şekilde açıklanabilir.

* Önceden belirlenmiş bir dizi ağ arabirimlerine atandı.
* Bunlar bulut etkin olduğunda, çeşitli ağ arabirimlerine atanmış değerleri veya Bulut devre dışı bırakıldı, ancak yapılandırılmış bir ağ geçidi ile aşağıda gösterilen örnek bir tabloyu göz önünde bulundurun. Burada atanan değerleri yalnızca örnek değerler olduğuna dikkat edin.

    | Ağ arabirimi | Bulut özellikli | Ağ geçidi ile Bulut devre dışı |
    |-----|---------------|---------------------------|
    | Veri 0  | 1            | -                        |
    | Veri 1  | 2            | 20                       |
    | Veri 2  | 3            | 30                       |
    | Veri 3  | 4            | 40                       |
    | Veri 4  | 5            | 50                       |
    | Veri 5  | 6            | 60                       |


* Bulut trafiği ağ arabirimleri üzerinden yönlendirilir sırası şöyledir:
  
    *Veri 0 > veri 1 > tarih 2 > veri 3 > veri 4 > veri 5*
  
    Bu, aşağıdaki örnekte açıklanabilir.
  
    İki bulut özellikli bir ağ arabirimi, Data 0 ve verileri 5 ile StorSimple cihazını göz önünde bulundurun. Veri 1 ile veri 4 Bulut devre dışı ancak yapılandırılmış bir ağ geçidi gerekir. Trafik bu cihaz için yönlendirilir sıralaması olacaktır:
  
    *Data 0 (1) > Data 5 (6) > Data 1 (20) > Data 2 (30) > Data 3 (40) > Data 4 (50)*
  
    *Parantez içinde sayıları ilgili yönlendirme ölçümleri gösterir.*
  
    Data 0 başarısız olursa, bulut trafiği veri 5'ten yönlendirilmiş. Data 0 hem veri 5 arızalanması durumunda bir ağ geçidi diğer tüm ağ üzerinde yapılandırılmış düşünüldüğünde, bulut trafiği veri 1'den geçer.
* Bir bulut özellikli bir ağ arabirimi başarısız olursa, daha sonra 30 saniyelik bir gecikmenin arabirimine bağlamak için 3 yeniden denemelerle sunulur. Tüm yeniden deneme işlemleri başarısız olursa, trafik yönlendirme tablosu tarafından belirlenen şekilde sonraki kullanılabilir bulut özellikli arabirimine yönlendirilir. Başarısız tüm bulut özellikli ağ arabirimleri, ardından cihaz üzerinde başka bir denetleyici (Bu durumda hiçbir yeniden) için başarısız olur.
* Bir iSCSI etkin ağ arabirimi için bir VIP hatası varsa, 3 yeniden deneme ile 2 saniye gecikme olacaktır. Bu davranış önceki sürümlerden aynı stayed. Tüm iSCSI ağ arabirimleri başarısız olursa, denetleyici yük devretmesi (yeniden başlatma tarafından accompanied) oluşur.
* Bir VIP başarısız olduğunda bir uyarı StorSimple Cihazınızda oluşturulur. Daha fazla bilgi için Git [uyarı hızlı başvuru](storsimple-8000-manage-alerts.md).
* Yeniden deneme sayısı bakımından iSCSI bulut öncelikli olur.
  
    Aşağıdaki örnek göz önünde bulundurun: Bir iki ağ arabirimi etkin cihazı olan StorSimple Data 0 ve 1 veri. Veri 0, bulut özellikli her iki bulut veri 1 bilgileriyse ve iSCSI etkin. Bu cihazdaki diğer ağ arabirimi, Bulut veya iSCSI için etkinleştirilir.
  
    Verilen veri 1 başarısız olursa, son iSCSI ağ arabirimi ise, bu diğer denetleyiciye veri 1'e bir denetleyici yük devretme kümesinde sonuçlanır.

### <a name="networking-best-practices"></a>En iyi ağ

StorSimple çözümünüzün en iyi performans için yukarıdaki ağ gereksinimlerine ek olarak aşağıdaki en iyi yöntemler için uyar:

* StorSimple Cihazınızı ayrılmış 40 MB/sn bant genişliği (veya daha fazla) olduğundan emin olun her zaman kullanılabilir. Bu bant genişliği Paylaşılmaması gerektiğini (veya ayırma QoS ilkeleri kullanılarak garanti) ile diğer uygulamaları.
* Ağ bağlantısı İnternet'e her zaman kullanılabilir olduğundan emin olun. Ara sıra ya da güvenilir olmayan Internet bağlantıları olmadan, Internet bağlantısı olmayan aygıtlar için desteklenmeyen bir yapılandırma neden olur.
* İSCSI ve bulut trafiği, ağ arabirimleri için iSCSI ve bulut erişim Cihazınızda ayrılmış tarafından yalıtın. Daha fazla bilgi için bkz. nasıl [ağ arabirimlerini değiştirebilir](storsimple-8000-modify-device-config.md#modify-network-interfaces) StorSimple Cihazınızda.
* Bir bağlantı toplama Denetim Protokolü'nü (LACP) yapılandırması, ağ arabirimleri için kullanmayın. Bu desteklenmeyen bir yapılandırmadır.

## <a name="high-availability-requirements-for-storsimple"></a>StorSimple için yüksek kullanılabilirlik gereksinimleri

StorSimple çözümünüzle birlikte gelen donanım platform, veri merkezinizde yüksek oranda kullanılabilir, hataya dayanıklı bir depolama altyapısı için dayanak sağlayan kullanılabilirlik ve güvenilirlik özelliklerine sahiptir. Ancak, gereksinimler ve StorSimple çözümünüzü kullanılabilirliğini sağlamaya yardımcı olmak için uymanız en iyi yöntemler vardır. StorSimple'ı dağıtmadan önce dikkatle bağlı ana bilgisayarları ve StorSimple cihaz için en iyi yöntemler ve aşağıdaki gereksinimleri gözden geçirin.

İzleme ve donanım bileşenleri StorSimple cihazınızın bakım hakkında daha fazla bilgi için Git [İzleyicisi donanım bileşenleri ve durum StorSimple cihaz Yöneticisi hizmetini](storsimple-8000-monitor-hardware-status.md) ve [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Yüksek kullanılabilirlik gereksinimleri ve StorSimple cihazınız için yordamlar

Dikkatli bir şekilde StorSimple cihazınızın yüksek kullanılabilirlik sağlamak için aşağıdaki bilgileri gözden geçirin.

#### <a name="pcms"></a>PCMs

StorSimple cihazları, yedekli, sık erişimli çıkarılabilen güç ve soğutma modülleri (PCMs) içerir. Her PCM tüm kasa hizmet sağlamak için yeterli kapasiteye sahiptir. Yüksek kullanılabilirlik sağlamak için her iki PCMs yüklenmesi gerekir.

* PCMs güç kaynağı başarısız olursa, kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlayın.
* Bir PCM başarısız olursa, bir değişikliği hemen isteyin.
* Yalnızca değiştirme varsa ve yüklemeye hazır olduğunda bir PCM'yi kaldırın.
* Her iki PCMs eşzamanlı olarak kaldırmayın. Yedek pili modülü PCM modülü içerir. Her iki PCMs pil koruma olmadan bir kapatma neden olur ve cihaz durumuna kaydedilmeyecek. Pil hakkında daha fazla bilgi için Git [yedek pil modülü korumak](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Denetleyici modülleri

StorSimple cihazları, yedekli, sık erişimli çıkarılabilen denetleyicisi modülleri dahil edildi. Denetleyici modülleri Aktif/Pasif bir biçimde çalışır. Herhangi bir zamanda belirli bir denetleyici modülü etkin ve Pasif bir Denetleyici Modülü olsa hizmeti sunuyor. Edilgen Denetleyici Modülü açılana ve etkin Denetleyici Modülü hata verirse veya işlemsel duruma kaldırıldı. Her Denetleyici Modülü, tüm kasa hizmet sağlamak için yeterli kapasiteye sahip. Yüksek kullanılabilirlik sağlamak için her iki Denetleyici Modülü yüklenmelidir.

* Her iki Denetleyici Modülü de her zaman yüklü olduğundan emin olun.
* Bir Denetleyici Modülü başarısız olursa, bir değişikliği hemen isteyin.
* Yalnızca değiştirme varsa ve yüklemeye hazır olduğunda, başarısız Denetleyici Modülü kaldırın. Bir modülü uzun süreler hava akışı etkiler kaldırma ve bu nedenle soğutma sistemine.
* Ağ bağlantıları için her iki Denetleyici Modülü de aynıdır ve bağlı ağ arabirimleri bir özdeş ağ yapılandırmasına sahip emin olun.
* Bir Denetleyici Modülü başarısız olursa veya değiştirme, bir Denetleyici Modülü başarısız Denetleyici Modülü değiştirmeden önce etkin durumda olduğundan emin olun. Bir denetleyici etkin olduğunu doğrulamak için Git [Cihazınızda etkin denetleyiciyi belirleme](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Her iki Denetleyici Modülü de aynı anda kaldırmayın. Denetleyici yük devretmesi Sürüyor ise, başlatmayın hazır bekleyen denetleyicinin modülü kapatın veya kasanın kaldırın.
* Denetleyici yük devretme sonrasında, her iki Denetleyici Modülü kaldırmadan önce en az beş dakika bekleyin.

#### <a name="network-interfaces"></a>Ağ arabirimleri

StorSimple cihaz denetleyicisi modülleri her dört sahip 1 Gigabit ve iki 10 gigabitlik Ethernet ağ arabirimleri.

* Ağ bağlantıları için her iki Denetleyici Modülü de aynıdır ve denetleyici modül arabirimleri özdeş ağ yapılandırmasına sahip bağlandığı ağ arabirimleri emin olun.
* Mümkün olduğunda, ağ bağlantıları arasında bir ağ aygıtı hatası durumunda hizmet kullanılabilirliğini sağlamak için farklı anahtarlara dağıtın.
* Yalnızca ya da son kalan iSCSI özellikli arabirimi (atanan IP ile) modemi çıkarmayı arabirimi ilk devre dışı bırakın ve ardından kabloyu. Ardından arabirimi takılı ise, ilk edilgen denetleyiciye yük devretmek etkin denetleyiciyi neden olur. Edilgen denetleyici de takılı karşılık gelen arabirimlerinden varsa, ardından her iki denetleyici de birden çok kez bir denetleyicisinde sonlandırma önce yeniden başlatılır.
* En az iki veri arabirimleri her denetleyici modülünden ağına bağlayın.
* İki etkinleştirdiyseniz 10 GbE arabirimleri farklı anahtarlara kullananlar dağıtın.
* MPIO sunucularda mümkün olduğunda, sunucularınızın bağlantı, ağ ve arabirim hatalarından etkilenmemesini sağlamak için kullanın.

Cihazınız için yüksek kullanılabilirlik ve performans ağ hakkında daha fazla bilgi için Git [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD ve HDD

StorSimple cihazları katı hal diskleri (SSD'ler) içerir ve alanları kullanılarak korunan sabit disk sürücülerinin (HDD'ler) yansıtılır. Yansıtma alanları kullanımını, cihaz bir veya daha fazla SSD veya HDD hatasını tolere mümkün olmasını sağlar.

* Tüm SSD ve HDD modülleri yüklü olduğundan emin olun.
* Bir SSD veya HDD başarısız olursa, bir değişikliği hemen isteyin.
* Bir SSD veya HDD başarısız olursa veya değiştirme gerektirir, yalnızca SSD veya HDD değiştirme gerektiren kaldırdığınızdan emin olun.
* Herhangi bir noktada sisteminden birden fazla SSD veya HDD sürede kaldırmayın.
  2 veya daha fazla disk (HDD, SSD), belirli türde bir hata ya da art arda hata kısa zaman çerçevesi içinde sistem hatası ve olası veri kaybına neden olabilir. Bu meydana gelirse, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) Yardım almak için.
* Değiştirme sırasında izlemek **paylaşılan bileşenleri** içinde **donanım sistem durumu** SSD ve HDD sürücülerini dikey. Yeşil onay durumu disklerin sağlam veya Tamam, kırmızı bir ünlem işaret ederken başarısız bir SSD veya HDD gösterir gösterir.
* Sistem hata oluşması halinde korumak için ihtiyacınız olan tüm birimler için bulut anlık görüntülerini yapılandırmanızı öneririz.

#### <a name="ebod-enclosure"></a>EBOD muhafazası

StorSimple cihaz model 8600 birincil muhafaza yanı sıra bir genişletilmiş bırakılmazsa, diskleri (EBOD) kasası içerir. EBOD denetleyicisi bir EBOD içerir ve alanları kullanılarak korunan sabit disk sürücülerinin (HDD'ler) yansıtılır. Yansıtma alanları kullanımını, cihaz bir veya daha fazla HDD'ler hatasını tolere mümkün olmasını sağlar. EBOD muhafazası yedekli SAS kablolu jbod'lere birincil Motoru'nu bağlıdır.

* Emin olun hem EBOD muhafazası denetleyicisi modülleri SAS kablolu jbod'lere hem tüm sabit disk sürücüleri her zaman yüklenir.
* Bir EBOD muhafazası Denetleyici Modülü başarısız olursa, bir değişikliği hemen isteyin.
* Bir EBOD muhafazası Denetleyici Modülü başarısız olursa, başarısız modülü değiştirmeden önce bir denetleyici modülü etkin olduğundan emin olun. Bir denetleyici etkin olduğunu doğrulamak için Git [Cihazınızda etkin denetleyiciyi belirleme](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* StorSimple cihaz Yöneticisi hizmeti bileşen durumunu sürekli olarak izlemek erişerek bir EBOD denetleyicisi modülü değiştirme sırasında **İzleyici** > **donanım sistem durumu**.
* Bir SAS kablo, başarısız veya değiştirme (Microsoft Support böyle bir belirlenmesi için söz konusu) gerektirir, değişikliği gerektiren SAS kablosu kaldırdığınızdan emin olun.
* Aynı anda iki SAS kablolu jbod'lere herhangi bir noktada sisteminden sürede kaldırmayın.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Ana bilgisayarlar için yüksek kullanılabilirlik önerisi

StorSimple cihazınıza bağlı konaklar yüksek kullanılabilirliğini sağlamak için bu en iyi yöntemleri dikkatle inceleyin.

* StorSimple ile yapılandırma [iki düğümlü dosya sunucusu küme yapılandırmaları][1]. Hata ve yedeklilik konak tarafında oluşturma tek hata noktaları kaldırarak, çözümün tamamını yüksek oranda kullanılabilir hale gelir.
* Sürekli olarak kullanılabilir (CA) paylaşımları Windows Server 2012 (SMB 3.0) depolama denetleyicileri yük devretme sırasında yüksek kullanılabilirlik için kullanın. Windows Server 2012 dosya sunucusu kümesi ve kullanılabilir paylaşımları sürekli olarak yapılandırmak için ek bilgi için bu başvuruda [tanıtım videosu](https://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple sistemi limitler hakkında bilgi edinmek](storsimple-8000-limits.md).
* [StorSimple çözümünüzü dağıtmayı öğrenirsiniz](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
