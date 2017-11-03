---
title: Microsoft Azure StorSimple sanal dizinin sistem gereksinimleri | Microsoft Docs
description: "Yazılım ve StorSimple sanal dizinizi için ağ gereksinimleri hakkında bilgi edinin"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/10/2017
ms.author: alkohli
ms.openlocfilehash: 4dc228ce8a7a73dd32bde77d529698bdcb7f490c
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple Sanal Dizini sistem gereksinimleri
## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple sanal dizinin ve dizi erişme depolama istemcileri önemli sistem gereksinimlerini açıklar. Önce StorSimple sisteminizi dağıtma ve ardından geri gerektiği gibi dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatle gözden geçirmenizi öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemciler için yazılım gereksinimleri** -desteklenen sanallaştırma platformları, web tarayıcıları, iSCSI başlatıcıları, SMB istemcileri, minimum sanal cihaz gereksinimleri ve bu işletim sistemleri için tüm ek gereksinimleri açıklanmaktadır.
* **StorSimple cihazı için ağ gereksinimleri** -için iSCSI, Bulut veya yönetim trafiğe izin verme, Güvenlik Duvarı'nda açılması gereken bağlantı noktaları hakkında bilgi sağlar.

Bu makalede yayımlanmış StorSimple sistem gereksinimleri bilgileri yalnızca StorSimple sanal diziler için geçerlidir.

* 8000 serisi cihazlar için Git [StorSimple 8000 serisi cihazınız için sistem gereksinimleri](storsimple-system-requirements.md).
* 7000 Serisi cihazlar için Git [StorSimple 5000-7000 Serisi cihazınız için sistem gereksinimleri](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Yazılım gereksinimleri
Yazılım gereksinimleri, desteklenen web tarayıcıları, SMB sürümleri, sanallaştırma platformları ve en düşük sanal cihaz gereksinimleri hakkında bilgi içerir.

### <a name="supported-virtualization-platforms"></a>Desteklenen sanallaştırma platformları
| **Hiper yönetici** | **Sürüm** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 ve sonraki sürümler |
| VMware ESXi |5.5 ve 6.0 |

### <a name="virtual-device-requirements"></a>Sanal cihaz gereksinimleri
| **Bileşen** | **Gereksinim** |
| --- | --- |
| En az sayıda sanal işlemci (çekirdek) |4 |
| Minimum bellek (RAM) |8 GB <br> Bir dosya sunucusu, 8 GB 2 milyondan az dosyaları için ve 2-4 milyon dosyaları için 16 GB|
| Disk alanı<sup>1</sup> |İşletim sistemi diski - 80 GB <br></br>Veri diski - 8 TB 500 GB |
| Ağ arabirimi en az durum sayısı |1 |
| En düşük Internet bant genişliği<sup>2</sup> |5 MB/sn |

<sup>1</sup> - ince sağlanan

<sup>2</sup> -ağ gereksinimleri, günlük veri değişikliği hızını bağlı olarak değişebilir. Bir aygıt bir günde 10 GB veya daha fazla değişiklik yedekleme gerekiyorsa (verileri sıkıştırılmış yinelenenleri kaldırılmış veya bulunamadı) Örneğin, ardından günlük yedekleme 5 MB/sn bağlantı üzerinden 4,25 saate kadar ele geçirebilir.

### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları
| **Bileşen** | **Sürüm** | **Ek gereksinimler/Notlar** |
| --- | --- | --- |
| Microsoft Edge |En son sürümü | |
| Internet Explorer |En son sürümü |Internet Explorer 11 ile test |
| Google Chrome |En son sürümü |Chrome 46 ile test |

### <a name="supported-storage-clients"></a>Desteklenen depolama istemcileri
StorSimple sanal (iSCSI sunucusu olarak yapılandırılmış) dizinizi erişim iSCSI başlatıcıları için aşağıdaki yazılım gereksinimleri verilmiştir.

| **Desteklenen işletim sistemleri** | **Gerekli sürümü** | **Ek gereksinimler/Notlar** |
| --- | --- | --- |
| Windows Server |2008R2 SP1, 2012, 2012 R2 |Ölçülü kaynak kullanan ve tamamen sağlanan StorSimple oluşturabilir birimler. Kısmen sağlanan birimler oluşturamazsınız. StorSimple iSCSI birimleri için yalnızca desteklenir: <ul><li>Windows temel disklerdeki basit birimler.</li><li>Windows bir birimi biçimlendirmek için NTFS.</li> |

StorSimple sanal (dosya sunucusu olarak yapılandırılmış) dizinizi erişen SMB istemciler için aşağıdaki yazılım gereksinimleri verilmiştir.

| **SMB sürümü** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Kopyalamayın veya StorSimple sanal dizinin dosya sunucusuna Windows şifreleme dosya sistemi (EFS) tarafından korunan dosyaları depolamak; Bu, desteklenmeyen bir yapılandırmada neden olacaktır. 
> 

### <a name="supported-storage-format"></a>Desteklenen depolama biçimi
Yalnızca Azure blok blob depolama desteklenir. Sayfa bloblarını desteklenmez. Daha fazla bilgi [blok blobları ve sayfa BLOB'ları hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Ağ gereksinimleri
Aşağıdaki tablo, iSCSI, SMB, Bulut veya yönetim trafiği için izin vermek için Güvenlik Duvarı'nda açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* gelen istemci isteklerini Cihazınızı erişim yönünü belirtir. *Out* veya *giden* , StorSimple Cihazınızı verileri gönderir harici olarak dağıtım yönü başvuruyor: Örneğin, Internet'e giden.

| **Bağlantı noktası No<sup>1</sup>** | **Giriş veya çıkış** | **Bağlantı noktası kapsamı** | **Gerekli** | **Notlar** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |Çıkışı |WAN |Hayır |Giden bağlantı noktası, güncelleştirmeleri almak için Internet erişimi için kullanılır. <br></br>Giden web proxy yapılandırılabilir kullanıcıdır. |
| TCP 443 (HTTPS) |Çıkışı |WAN |Evet |Giden bağlantı noktası, bulut veri erişimi için kullanılır. <br></br>Giden web proxy yapılandırılabilir kullanıcıdır. |
| UDP 53 (DNS) |Çıkışı |WAN |Bazı durumlarda; notlarına bakın. |Yalnızca bir Internet tabanlı DNS sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir. <br></br> Bir dosya sunucusu dağıtma, yerel DNS sunucusu kullanmanızı öneririz olduğunu unutmayın. |
| UDP 123 (NTP) |Çıkışı |WAN |Bazı durumlarda; notlarına bakın. |Yalnızca bir Internet tabanlı NTP sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir.<br></br> Bir dosya sunucusu dağıtma, Active Directory etki alanı denetleyicileriniz ile eşitleme süresi öneririz olduğunu unutmayın. |
| TCP 80 (HTTP) |İçinde |LAN |Evet |Bu, yerel yönetim için StorSimple cihazında yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br></br> HTTP üzerinden yerel UI erişme HTTPS için otomatik olarak yeniden olduğunu unutmayın. |
| TCP 443 (HTTPS) |İçinde |LAN |Evet |Bu, yerel yönetim için StorSimple cihazında yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. |
| TCP 3260 (iSCSI) |İçinde |LAN |Hayır |Bu bağlantı noktası üzerinde iSCSI verilere erişmek için kullanılır. |

<sup>1</sup> hiçbir gelen bağlantı noktalarının genel Internet'te açılması gerekir.

> [!IMPORTANT]
> Güvenlik Duvarı'nı değil değiştirmek veya tüm SSL trafiği StorSimple cihazı ve Azure arasında şifresini emin olun.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL düzenleri
Ağ yöneticileri genellikle gelen filtrelemek için URL desenlerini ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucuları gibi diğer Microsoft uygulamaları sanal dizinizi ve StorSimple cihaz Yöneticisi hizmeti bağlıdır. Bu uygulamalarla ilişkili URL desenlerini, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerini değiştirebilirsiniz anlamak önemlidir. Bu sırayla izleyebilir ve güvenlik duvarı kuralları, StorSimple için ve gerektiğinde güncelleştirmek için ağ yöneticisine gerektirecektir. 

Çoğu durumda liberally sabit IP adresleri, StorSimple göre giden trafiği için güvenlik duvarı kuralları ayarlamanızı öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> 
> * (Kaynak) IP'leri cihaz için tüm bulut etkin ağ arabirimlerinin her zaman ayarlanması gerekir. 
> * IP'leri ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).
> 
> 

| URL deseni | Bileşen/işlevi |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` <br>`https://login.windows.net`|StorSimple cihaz Yöneticisi hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik doğrulama hizmeti|
| `http://*.backup.windowsazure.com` |Cihaz kaydı |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Sertifika iptali |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Microsoft Update sunucularına<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Destek Paketi |
| `http://*.data.microsoft.com ` |Windows, telemetri hizmetinde bkz [müşteri deneyimi ve tanılama telemetri güncelleştirmesi](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-step"></a>Sonraki adım
* [StorSimple sanal dizinizi dağıtmak için portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md)

