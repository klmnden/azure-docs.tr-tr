---
title: Microsoft Azure StorSimple sanal dizini sistem gereksinimleri | Microsoft Docs
description: Yazılım ve StorSimple Virtual Array'iniz için ağ gereksinimleri hakkında bilgi edinin
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.openlocfilehash: a6bea2b5447435930cb0e1f80073a11007e80415
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58876845"
---
# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple Sanal Dizini sistem gereksinimleri
## <a name="overview"></a>Genel Bakış
Bu makalede, dizi erişen depolama istemcilerinin ve Microsoft Azure StorSimple Virtual Array'iniz için en önemli sistem gereksinimlerini açıklar. Önce StorSimple sisteminizi dağıtma ve ardından geri gerektiğinde dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatlice gözden öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemcilerinin yazılım gereksinimleri** -desteklenen sanallaştırma platformları, web tarayıcıları, iSCSI başlatıcılarının, SMB istemcileri, en az bir sanal cihaz gereksinimlerini ve bu işletim sistemleri için tüm ek gereksinimleri açıklanmaktadır.
* **StorSimple cihazı için ağ gereksinimleri** -için iSCSI, bulut ya da Yönetim trafiğine izin vermek için güvenlik duvarını açma olması gereken bağlantı noktaları hakkında bilgi sağlar.

Bu makalede yayımlanmış StorSimple sistem gereksinimleri bilgileri yalnızca StorSimple sanal diziler için geçerlidir.

* 8000 serisi cihazlar için Git [StorSimple 8000 serisi Cihazınızı için sistem gereksinimleri](storsimple-system-requirements.md).
* 7000 Serisi cihazlar için Git [StorSimple 5000-7000 Serisi cihazınız için sistem gereksinimleri](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Yazılım gereksinimleri
Yazılım gereksinimleri, desteklenen web tarayıcıları, SMB sürümleri, sanallaştırma platformları ve en az bir sanal cihaz gereksinimleri hakkında bilgi içerir.

### <a name="supported-virtualization-platforms"></a>Desteklenen sanallaştırma platformları
| **Hiper yönetici** | **Sürüm** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 ve üzeri |
| VMware ESXi |5.0, 5.5, 6.0 ve 6.5. |

> [!IMPORTANT]
> VMware araçları, StorSimple sanal dizisi yüklemeyin; Bu, desteklenmeyen bir yapılandırma neden olur.

### <a name="virtual-device-requirements"></a>Sanal cihaz gereksinimleri
| **Bileşen** | **Gereksinim** |
| --- | --- |
| En az kaç sanal işlemci (çekirdek) |4 |
| Minimum bellek (RAM) |8 GB <br> Dosya sunucusu, 8 GB 2 milyondan az dosyaları ve 2-4 milyon dosyaları için 16 GB için|
| Disk alanı<sup>1</sup> |OS disk - 80 GB <br></br>Veri diski - 500 GB ile 8 TB |
| En düşük ağ arabirimleri sayısı |1 |
| Internet bant genişliği<sup>2</sup> |Gereken en düşük bant genişliği: 5 MB/sn <br> Önerilen bant genişliği: 100 Mbps <br> Veri aktarımı ölçekler Internet bant genişliği hızı. Örneğin, 100 GB veri günlük yedeklemeler, bir gün içinde tamamlamak değil çünkü hangi yedekleme hatalarına neden 5 MB/sn hızında aktarmak için 2 gün sürer. 100 MB/sn bant genişliği ile 2.5 saat içinde 100 GB veri aktarılabilir.   |

<sup>1</sup> - ölçülü kaynak sağlanan

<sup>2</sup> -ağ gereksinimleri, günlük veri değişikliği hızınıza bağlı olarak değişebilir. Bir cihaz bir günde 10 GB veya daha fazla değişiklik yedekleme gerekiyorsa (veri sıkıştırılmış kaldırılmış veya açılamadı) Örneğin, ardından günlük yedekleme 5 Mbps bağlantı üzerinden 4,25 saate kadar ele geçirebilir.

### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları
| **Bileşen** | **Sürüm** | **Ek gereksinimler/Notlar** |
| --- | --- | --- |
| Microsoft Edge |En son sürümü | |
| Internet Explorer |En son sürümü |Internet Explorer 11 ile test |
| Google Chrome |En son sürümü |Chrome 46 ile test |

### <a name="supported-storage-clients"></a>Desteklenen depolama istemcileri
Aşağıdaki yazılım gereksinimleri, StorSimple sanal (bir iSCSI sunucusu olarak yapılandırılmış) dizininiz erişim iSCSI başlatıcıları içindir.

| **Desteklenen işletim sistemleri** | **Gerekli sürümü** | **Ek gereksinimler/Notlar** |
| --- | --- | --- |
| Windows Server |2008R2 SP1, 2012, 2012R2 |StorSimple, ölçülü kaynak kullanan ve tam olarak sağlanan oluşturabilir birimleri. Bu kısmen sağlanan birimler oluşturamazsınız. StorSimple iSCSI birimlerini yalnızca desteklenir: <ul><li>Windows temel disklerde birimleri basit.</li><li>Bir birimi biçimlendirmek için Windows NTFS.</li> |

Aşağıdaki yazılım gereksinimleri, StorSimple sanal (dosya sunucusu olarak yapılandırılmış) dizininiz erişen SMB istemcileri içindir.

| **SMB sürümü** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Kopyalamayın veya StorSimple sanal dizisi dosya sunucusunda Windows şifreleme dosya sistemi (EFS) tarafından korunan dosyaları depolamak; Bu, desteklenmeyen bir yapılandırma neden olur.


### <a name="supported-storage-format"></a>Desteklenen depolama biçimi
Yalnızca Azure blok blobu depolama desteklenir. Sayfa blobları desteklenmez. Daha fazla bilgi [blok blobları ve sayfa blobları hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Ağ gereksinimleri
Aşağıdaki tablo, iSCSI, SMB, Bulut veya yönetim trafiği için izin vermek için güvenlik duvarını açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* gelen istemci isteklerini Cihazınızı erişim yönünü gösterir. *Çıkış* veya *giden* , StorSimple Cihazınızı gönderir dışarıdan, veri dağıtımı dışında yön ifade eder: Örneğin, Internet'e giden.

| **Bağlantı noktası No<sup>1</sup>** | **Daraltma veya genişletme** | **Bağlantı noktası kapsamı** | **Gerekli** | **Notlar** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |Çıkış |WAN |Hayır |Giden bağlantı noktası, güncelleştirmeleri almak için Internet erişimi için kullanılır. <br></br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur. |
| TCP 443 (HTTPS) |Çıkış |WAN |Evet |Giden bağlantı noktası, bulut veri erişimi için kullanılır. <br></br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur. |
| UDP 53 (DNS) |Çıkış |WAN |Bazı durumlarda; notlara bakın. |Yalnızca bir Internet tabanlı bir DNS sunucusu kullanıyorsanız bu bağlantı noktası gereklidir. <br></br> Bir dosya sunucusunu dağıtma, yerel DNS sunucusunu kullanmanızı öneririz olduğunu unutmayın. |
| UDP 123 (NTP) |Çıkış |WAN |Bazı durumlarda; notlara bakın. |Yalnızca bir Internet tabanlı bir NTP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.<br></br> Bir dosya sunucusu dağıtıyorsanız, Active Directory etki alanı denetleyicileriyle saati eşitleme öneririz olduğunu unutmayın. |
| TCP 80 (HTTP) |İçinde |LAN |Evet |Bu, yerel yönetim için StorSimple cihazında yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br></br> HTTP üzerinden yerel UI erişme HTTPS için otomatik olarak yeniden olduğunu unutmayın. |
| TCP 443 (HTTPS) |İçinde |LAN |Evet |Bu, yerel yönetim için StorSimple cihazında yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. |
| TCP 3260'ın (iSCSI) |İçinde |LAN |Hayır |Bu bağlantı noktası üzerinde iSCSI verilere erişmek için kullanılır. |

<sup>1</sup> genel Internet'te açılmasını hiçbir gelen bağlantı noktası gerekir.

> [!IMPORTANT]
> Güvenlik Duvarı'nı değiştirmeyin veya StorSimple cihazını ve Azure arasında herhangi bir SSL trafiği şifresini çözme emin olun.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları
Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamalarını sanal diziniz ve StorSimple cihaz Yöneticisi hizmetine bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Buna karşılık, izleme ve güvenlik duvarı kuralları StorSimple'ınızı ve gerektiğinde güncelleştirme için ağ yöneticisine da gerekir. 

StorSimple liberally çoğu zaman sabit IP adreslerinin, temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> 
> * (Kaynak) IP'ler cihaz her zaman tüm bulut özellikli ağ arabirimleri için ayarlanması gerekir. 
> * IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).
> 
> 

| URL deseni | Bileşen/işlevi |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` <br>`https://login.windows.net`|StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti|
| `http://*.backup.windowsazure.com` |Cihaz kaydı |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptal etme |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Destek paketi |
| `https://*.data.microsoft.com` |Telemetri hizmeti, Windows bkz [müşteri deneyimini ve tanılama telemetrisi güncelleştirmesi](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Virtual Array'iniz dağıtmak için portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md)
