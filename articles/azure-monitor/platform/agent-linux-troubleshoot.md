---
title: Azure Log Analytics Linux Aracısı sorunlarını giderme | Microsoft Docs
description: Azure İzleyici'de Linux için Log Analytics Aracısı belirtileri, nedenleri ve en sık karşılaşılan sorunlara yönelik çözümler açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 83f9cc050694344cdc5f4f5a2070bc875fcba3d9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071669"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Linux için Log Analytics Aracısı ile ilgili sorunları giderme 

Bu makalede, Azure İzleyici'de Linux için Log Analytics aracısını ile karşılaşabilirsiniz ve bunların çözülmesine yönelik olası çözümler önerir hatalarını giderme hakkında Yardım sağlar.

Bu adımların hiçbiri işinize yaramazsa aşağıdaki Destek kanallarını da kullanılabilir:

* Avantajları Premier Destek ile bir destek isteği açabilirsiniz [Premier](https://premier.microsoft.com/).
* Azure destek sözleşmeleri olan müşteriler, bir destek isteği açabilirsiniz [Azure portalında](https://manage.windowsazure.com/?getsupport=true).
* OMI özelliğiyle sorunları tanılayın [OMI sorun giderme kılavuzu](https://github.com/Microsoft/omi/blob/master/Unix/doc/diagnose-omi-problems.md).
* Dosya bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues).
* Gönderilen fikirleri ve hataları gözden geçirmek için Log Analytics geri bildirim sayfasını ziyaret edin [ https://aka.ms/opinsightsfeedback ](https://aka.ms/opinsightsfeedback) veya yeni bir dosya.  

## <a name="important-log-locations-and-log-collector-tool"></a>Önemli günlük konumları ve günlük Toplayıcı aracı

 Dosya | Yol
 ---- | -----
 Linux günlük dosyası için log Analytics aracısını | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
 Log Analytics Aracısını Yapılandırma günlük dosyası | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Sorun giderme için veya bir GitHub sorunu göndermeden önce önemli günlükleri almak için günlük Toplayıcı aracımızı kullanmanızı öneririz. Aracı hakkında daha fazla bilgi ve çalıştırmak nasıl edinebilirsiniz [burada](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md).

## <a name="important-configuration-files"></a>Önemli yapılandırma dosyaları

 Kategori | Dosya Konumu
 ----- | -----
 Syslog | `/etc/syslog-ng/syslog-ng.conf` veya `/etc/rsyslog.conf` veya `/etc/rsyslog.d/95-omsagent.conf`
 Nagios, Zabbix, Log Analytics çıkış ve genel aracı performansı | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Ek yapılandırmalar | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 >[!NOTE]
 >Performans sayaçları ve Syslog için yapılandırma dosyalarını düzenleyerek yazılır gelen koleksiyon yapılandırıldıysa [veri menüsü Log Analytics Gelişmiş ayarlar](../../azure-monitor/platform/agent-data-sources.md#configuring-data-sources) çalışma alanınız için Azure portalında. Log Analytics koleksiyonundan yapılandırması tüm aracılar için devre dışı bırakmak için devre dışı bırak **Gelişmiş ayarlar** veya tek bir aracı aşağıdaki komutu çalıştırın:  
> `sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable`

## <a name="installation-error-codes"></a>Yükleme hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| NOT_DEFINED | Gerekli bağımlılıkları yüklü olmadığından auoms auditd eklentisi yüklü değil | Paket auditd auoms başarısız oldu, yüklemeyi. |
| 2 | Kabuk pakete sağlanan seçeneği geçersiz. Çalıştırma `sudo sh ./omsagent-*.universal*.sh --help` kullanım için |
| 3 | Kabuk pakete sağlanan seçeneği yoktur. Çalıştırma `sudo sh ./omsagent-*.universal*.sh --help` kullanım için. |
| 4 | Geçersiz paket veya geçersiz proxy ayarları yazın. omsagent -*rpm*.sh paketler, yalnızca RPM tabanlı sistemler ve omsagent - yüklenebilir*deb*.sh paketleri Debian tabanlı sistemlerde yalnızca yüklenebilir. Bu Evrensel Yükleyicisi'nden kullanmanız önerilir [en son sürüm](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux). Ayrıca proxy ayarlarınızı doğrulamak için gözden geçirin. |
| 5 | Kabuk paket kök olarak yürütülmelidir veya ekleme sırasında döndürülen 403 hatası oluştu. Komutunu kullanarak çalıştırmak `sudo`. |
| 6 | Geçersiz paket mimari veya ekleme sırasında; döndürülen hata 200 hata oluştu omsagent -*x64.sh paketler, yalnızca 64-bit sistemler ve omsagent - yüklenebilir*x86.sh paketleri 32-bit sistemlerde yalnızca yüklenebilir. İndirme, mimariden için doğru paketi [en son sürüm](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest). |
| 17 | OMS paketi yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 19 | OMI paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 20 | SCX paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 21 | Sağlayıcı Setleri yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 22 | İle birlikte gelen paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın |
| 23 | SCX veya OMI paket zaten yüklü. Kullanım `--upgrade` yerine `--install` Kabuk paket yüklemek için. |
| 30 | Paket iç hata oluştu. Dosya bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) çıktısından ayrıntılarla. |
| 55 | Kilitli veya eksik curl desteklenmeyen bir openssl sürümü veya olamaz bağlanmak için Azure İzleyici ya da dpkg program. |
| 61 | Python ctypes kitaplığı eksik. Python ctypes kitaplığı veya paket (python ctypes) yükleyin. |
| 62 | Eksik tar programı, yükleme hedefi. |
| 63 | Yükleme sed. eksik sed programı |
| 64 | Eksik curl programı, yükleme curl. |
| 65 | Eksik gpg programı, yükleme gpg. |

## <a name="onboarding-error-codes"></a>Onboarding hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| 2 | Geçersiz seçenek omsadmin betiğe sağlanan. Çalıştırma `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` kullanım için. |
| 3 | Omsadmin betiği için sağlanan geçersiz yapılandırma. Çalıştırma `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` kullanım için. |
| 4 | Geçersiz proxy omsadmin betiğe sağlanan. Proxy doğrulayın ve bkz bizim [bir HTTP proxy'sinin kullanmaya yönelik belgeler](log-analytics-agent.md#network-firewall-requirements). |
| 5 | Azure İzleyici'den alınan 403 HTTP hata. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 6 | Azure İzleyici'den alınan olmayan 200 HTTP hata. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 7 | Azure İzleyicisi'ne bağlanılamıyor. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 8 | Log Analytics çalışma alanına ekleme hatası. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 30 | İç komut dosyası hata. Dosya bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) çıktısından ayrıntılarla. |
| 31 | Hata oluşturma Aracısı kimliği Dosya bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) çıktısından ayrıntılarla. |
| 32 | Sertifika oluşturulurken bir hata oluştu. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 33 | Omsconfig için metaconfiguration oluşturulurken bir hata oluştu. Dosya bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) çıktısından ayrıntılarla. |
| 34 | Metaconfiguration oluşturma betiği mevcut değil. Onboarding yeniden `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`. |

## <a name="enable-debug-logging"></a>Hata ayıklama günlük kaydını etkinleştirme
### <a name="oms-output-plugin-debug"></a>OMS çıkış eklenti hata ayıklama
 FluentD sağlayarak eklentisi özgü günlük düzeylerini giriş ve çıkışları için farklı günlük düzeyleri belirtmek için sağlar. Bir OMS çıkış farklı günlük düzeyini belirtmek için genel aracı yapılandırmasını Düzenle `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.  

 Yapılandırma dosyası bitmeden önce OMS çıkış eklentisinin değiştirmek `log_level` özelliğinden `info` için `debug`:

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Hata ayıklama günlüğü toplu karşıya yükleme için Azure izleyici türü, veri öğeleri ve göndermek için harcanan süre ayrılmış görmenizi sağlar:

*Örnek etkin hata ayıklama günlüğü:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Ayrıntılı çıkış
OMS çıkış eklentiyi kullanmak yerine, ayrıca veri öğeleri doğrudan çıkarabilirsiniz `stdout`, olan Linux günlük dosyası için Log Analytics aracısını görünür.

Log Analytics genel aracı yapılandırma dosyasında yer alan `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, OMS yorum ekleyerek eklentisi çıktısını bir `#` önünde her satırı:

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Çıktı eklentisi, aşağıdaki bölümde kaldırarak metindeki açıklamayı silin `#` önünde her satırı:

```
<match **>
  type stdout
</match>
```

## <a name="issue--unable-to-connect-through-proxy-to-azure-monitor"></a>Sorun:  Azure İzleyici için Ara sunucu aracılığıyla bağlanılamadı

### <a name="probable-causes"></a>Olası nedenleri
* Ekleme sırasında belirtilen proxy yanlış
* Azure Otomasyonu hizmet uç noktaları ve Azure İzleyici, veri merkezinizdeki listede değilsiniz 

### <a name="resolution"></a>Çözüm
1. Azure İzleyici'seçeneği ile aşağıdaki komutu kullanarak Linux için Log Analytics aracısı için Reonboard `-v` etkin. Proxy sunucusuna Azure İzleyici aracılığıyla bağlanan aracısının ayrıntılı çıkış sağlar. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Bölümü gözden geçirin [proxy ayarlarını güncelleştirme](agent-manage.md#update-proxy-settings) aracının bir proxy sunucu üzerinden iletişim kurmak için düzgün şekilde yapılandırdığınızdan doğrulayın.    
* Çift aşağıdaki Azure İzleyici uç noktaların izin verilenler listesinde olup olmadığını denetleyin:

    |Aracı Kaynağı| Bağlantı Noktaları | Yön |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.oms.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.blob.core.windows.net | Bağlantı noktası 443| Gelen ve giden |  
    |*.azure-automation.net | Bağlantı noktası 443| Gelen ve giden | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Ekleme çalışırken bir 403 hatası alıyorsunuz

### <a name="probable-causes"></a>Olası nedenleri
* Tarih ve saat Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

### <a name="resolution"></a>Çözüm

1. Komut tarih ile Linux sunucunuzdaki zamanını kontrol edin. Saati geçerli saatten 15 dakika +/-ise, ardından ekleme başarısız olur. İçin doğru Bu güncelleştirme tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için Log Analytics aracısını en son sürümünü yüklediğinizi doğrulayın.  En yeni sürümü artık zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı Kimliğiniz ve çalışma alanı anahtarı bu makalenin önceki kısımlarında yükleme yönergelerini kullanarak Reonboard.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağa ekledikten sonra bir 500 ve 404 hatası günlük dosyasına bakın
Log Analytics çalışma alanına ilk Linux veri yükleme oluşma zamanı bilinen bir sorundur. Bu, gönderilen veya hizmet deneyimi olan verileri etkilemez.


## <a name="issue-you-see-omiagent-using-100-cpu"></a>Sorun: % 100 CPU kullanarak omiagent bakın

### <a name="probable-causes"></a>Olası nedenleri
Nss pem paket'teki bir gerileme [v1.0.3 5.el7](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-5.el7.x86_64.rpm.html) biz diyorsunuz ciddi performans sorunu, neden Redhat/Centos 7.x dağıtımlar çok gündeme. Bu sorun hakkında daha fazla bilgi edinmek için aşağıdaki belgeleri kontrol edin: Hata [libcurl 1667121 performans gerileme](https://bugzilla.redhat.com/show_bug.cgi?id=1667121).

Performans ilgili hatalar her zaman meydana yok ve yeniden çok zordur. Böyle omiagent sorun yaşarsanız, belirli bir eşiği aşıldığında omiagent yığın izlemesini toplar betik omiHighCPUDiagnostics.sh kullanmanız gerekir.

1. Betiği indirin <br/>
`wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/LogCollector/source/omiHighCPUDiagnostics.sh`

2. CPU eşiği % 30 ile 24 saat boyunca tanılama Çalıştır <br/>
`bash omiHighCPUDiagnostics.sh --runtime-in-min 1440 --cpu-threshold 30`

3. Birçok Curl ve NSS işlev çağrıları, çözüm adımları izleyerek fark ederseniz, çağrı yığını omiagent_trace dosyasında yazılan.

### <a name="resolution-step-by-step"></a>Çözümleme (adım adım)

1. Nss pem paketi yükseltme [v1.0.3 5.el7_6.1](https://centos.pkgs.org/7/centos-updates-x86_64/nss-pem-1.0.3-5.el7_6.1.x86_64.rpm.html). <br/>
`sudo yum upgrade nss-pem`

2. Nss pem yükseltme için kullanılabilir değilse (çoğunlukla Centos üzerinde gerçekleşir), ardından 7.29.0-46 için curl düşürme. Durumunda, yanlışlıkla "yum güncelleştirme" çalıştırın, ardından curl 7.29.0-51 için yükseltilir ve sorunu yeniden ortaya. <br/>
`sudo yum downgrade curl libcurl`

3. OMI yeniden başlatın: <br/>
`sudo scxadmin -restart`

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalında herhangi bir veri görmediğinizden

### <a name="probable-causes"></a>Olası nedenleri

- Azure İzleyici ekleme başarısız oldu
- Azure İzleyici bağlantı engellenir
- Linux veri için log Analytics aracısını yedeklenir

### <a name="resolution"></a>Çözüm
1. Aşağıdaki dosya varsa kontrol ederek Azure İzleyici ekleme başarılı olup olmadığını kontrol edin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard kullanarak `omsadmin.sh` komut satırı yönergeleri
3. Bir ara sunucu kullanıyorsanız, daha önce sağlanan proxy çözümleme adımlarına bakın.
4. Linux için Log Analytics aracısını hizmetiyle iletişim kuramadığında bazı durumlarda, aracı üzerinde veri 50 MB'tır tam arabellek boyutu için sıraya alınır. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatılması gerekiyor: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.


## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Sorun: İletilen Syslog iletilerini görmediğinizden 

### <a name="probable-causes"></a>Olası nedenleri
* Linux sunucusuna uygulanan yapılandırma gönderilen özellikleri ve/veya günlük düzeyleri koleksiyonunu izin vermiyor
* Syslog doğru bir şekilde Linux sunucusuna iletilmez değil
* Saniye başına iletilen ileti sayısını işlemek Linux için Log Analytics aracısını temel yapılandırması için çok büyük

### <a name="resolution"></a>Çözüm
* Syslog yapılandırmasını Log Analytics çalışma alanındaki tüm özellikleri ve doğru günlük düzeyleri olduğunu doğrulayın. Gözden geçirme [Syslog koleksiyonunu, Azure portalında yapılandırma](../../azure-monitor/platform/data-sources-syslog.md#configure-syslog-in-the-azure-portal)
* Yerel syslog Daemon'ları Mesajlaşma doğrulayın (`rsyslog`, `syslog-ng`) yönlendirilmiş iletiler alabilir.
* İletileri engellenmediğinden emin olmak için Syslog sunucusunda güvenlik duvarı ayarlarını kontrol edin
* Log Analytics kullanarak bir Syslog ileti benzetimini `logger` komutu
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Sorun: Errno adresi zaten kullanımda omsagent günlük dosyası görüntüleniyor
Görürseniz `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>` omsagent.log içinde.

### <a name="probable-causes"></a>Olası nedenleri
Linux tanılama uzantısı (LAD) Log Analytics Linux VM uzantısı ile yan yana yüklenir ve syslog verileri toplama omsagent olarak aynı bağlantı noktasını kullanıyorsa bu hatayı gösterir.

### <a name="resolution"></a>Çözüm
1. Kök olarak (25224 bir örnektir ve ortamınızda LAD tarafından kullanılan farklı bir bağlantı noktası gördüğünüzü mümkündür. Not) aşağıdaki komutları yürütün:

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    Ardından doğru düzenlemeniz gerekir `rsyslogd` veya `syslog_ng` yapılandırma dosyası ve 25229 noktasına yazılacak LAD ilgili yapılandırmasını değiştirin.

2. Sanal makine çalışıyorsa `rsyslogd`, değiştirilecek dosyasıdır: `/etc/rsyslog.d/95-omsagent.conf` (, aksi takdirde varsa `/etc/rsyslog`). Sanal makine çalışıyorsa `syslog_ng`, değiştirilecek dosyasıdır: `/etc/syslog-ng/syslog-ng.conf`.
3. Omsagent yeniden `sudo /opt/microsoft/omsagent/bin/service_control restart`.
4. Syslog hizmeti yeniden başlatın.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Sorun: Temizleme seçeneğini kullanarak omsagent kaldırma belirleyemiyoruz

### <a name="probable-causes"></a>Olası nedenleri

* Linux tanılama uzantısı yüklenir
* Linux tanılama uzantısı yüklenir ve kaldırılır, ancak yine de mdsd tarafından kullanılan omsagent hakkında bir hata iletisi görür ve kaldırılamaz.

### <a name="resolution"></a>Çözüm
1. Linux tanılama uzantısı (LAD) kaldırın.
2. Şu konumda mevcut değilse makineden Linux tanılama uzantısı dosyalarını kaldırın: `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` ve `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Sorun: Nagios veriler veri göremez 

### <a name="probable-causes"></a>Olası nedenleri
* Omsagent kullanıcı Nagios günlük dosyasından okumak için gerekli izinlere sahip değil
* Nagios kaynak ve filtre omsagent.conf dosyasından açıklamalı olmayan verilmemiş

### <a name="resolution"></a>Çözüm
1. İzleyerek Nagios dosyasını okumaya omsagent kullanıcı ekleme [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts).
2. Genel yapılandırma dosyasından Linux için Log Analytics aracısını içinde `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, emin **hem** filtre ve Nagios kaynak açıklamalı olmayan.

    ```
    <source>
      type tail
      path /var/log/nagios/nagios.log
      format none
      tag oms.nagios
    </source>

    <filter oms.nagios>
      type filter_nagios_log
    </filter>
    ```

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Sorun: Tüm Linux verileri görme hakkındaki değil 

### <a name="probable-causes"></a>Olası nedenleri
* Azure İzleyici ekleme başarısız oldu
* Azure İzleyici bağlantı engellenir
* Sanal makine yeniden başlatıldı
* OMI paket el ile karşılaştırıldığında ne Linux paket için Log Analytics aracısını tarafından yüklü olduğu için daha yeni bir sürüme yükseltildi
* DSC kaynak günlükleri *sınıfı bulunamadı* hata `omsconfig.log` günlük dosyası
* Verilerin log Analytics aracısını yedeklenir
* DSC günlükleri *geçerli yapılandırması yok. İle - Path parametresini bir yapılandırma dosyası belirtin ve önce geçerli bir yapılandırma oluşturmak için başlangıç-DscConfiguration komutu yürütün.* içinde `omsconfig.log` günlük dosyası, ancak hiçbir günlük iletisi yok hakkında `PerformRequiredConfigurationChecks` operations.

### <a name="resolution"></a>Çözüm
1. Auditd paket gibi tüm bağımlılıkları yükleyin.
2. Aşağıdaki dosya varsa kontrol ederek Azure İzleyici ekleme başarılı olup olmadığını denetleyin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.  Bunu reonboard omsadmin.sh komut satırını kullanarak, yoksa [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
4. Bir ara sunucu kullanıyorsanız, proxy sorun giderme adımları yukarıdaki denetleyin.
5. Sanal makine yeniden başlatıldıktan sonra bazı Azure dağıtım sistemlerinde OMID OMI sunucusu arka plan programı başlamıyor. Bu denetim, değişiklik izleme veya UpdateManagement çözümü ile ilgili verileri görmüyor içinde neden olur. Çalıştırarak OMI sunucusuna el ile başlatmak için geçici çözüm olan `sudo /opt/omi/bin/service_control restart`.
6. OMI paket el ile yeni bir sürüme yükselttikten sonra çalışmaya devam etmesi için Log Analytics aracısını el ile başlatılması gerekir. Bu adım yükseltildikten sonra nerede OMI sunucusuna otomatik olarak başlamaz bazı işlemleri için gereklidir. Çalıştırma `sudo /opt/omi/bin/service_control restart` OMI yeniden başlatmak için.
7. DSC kaynak görürseniz *sınıfı bulunamadı* omsconfig.log çalıştırma, hata `sudo /opt/omi/bin/service_control restart`.
8. Linux için Log Analytics aracısını için Azure İzleyici, konuşurken bazı durumlarda, aracı üzerinde veri tam arabellek boyutu için yedeklenir: 50 MB. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatılması gerekiyor `/opt/microsoft/omsagent/bin/service_control restart`.

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 veya daha sonra bu sorun düzeltilene
    >

* Varsa `omsconfig.log` günlük dosyası, belirtmez `PerformRequiredConfigurationChecks` işlemleri düzenli aralıklarla sistem üzerinde çalışan, cron iş/hizmet ile ilgili bir sorun olabilir. Sıralanmış işin mevcut altında olduğundan emin olun `/etc/cron.d/OMSConsistencyInvoker`. Sıralanmış iş oluşturmak için aşağıdaki komutları çalıştırmanız varsa:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Ayrıca cron hizmetinin çalıştığından emin olun. Kullanabileceğiniz `service cron status` Debian, Ubuntu, SUSE, veya `service crond status` ile RHEL, CentOS, Oracle Linux'ın bu hizmet durumunu denetleyin. Hizmet yok, ikili dosyaları yüklemek ve aşağıdakileri kullanarak hizmeti başlatın:

    **Ubuntu/Debian**

    ```
    # To Install the service binaries
    sudo apt-get install -y cron
    # To start the service
    sudo service cron start
    ```

    **SUSE**

    ```
    # To Install the service binaries
    sudo zypper in cron -y
    # To start the service
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

    **RHEL/CeonOS**

    ```
    # To Install the service binaries
    sudo yum install -y crond
    # To start the service
    sudo service crond start
    ```

    **Oracle Linux**

    ```
    # To Install the service binaries
    sudo yum install -y cronie
    # To start the service
    sudo service crond start
    ```

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Sorun: Syslog ya da Linux performans sayaçları için portal koleksiyonundan yapılandırırken ayarları uygulanmaz

### <a name="probable-causes"></a>Olası nedenleri
* Linux için Log Analytics aracısını en son yapılandırmayı çekilen değil
* Portalda değiştirilen ayarlar uygulanmadı

### <a name="resolution"></a>Çözüm
**Arka planı:** `omsconfig` yeni portalı tarafı yapılandırması için beş dakikada görünen Linux yapılandırma aracı için Log Analytics aracısıdır. Bu yapılandırma, ardından /etc/opt/microsoft/omsagent/conf/omsagent.conf bulunan Linux yapılandırma dosyaları için Log Analytics aracısını uygulanır.

* Bazı durumlarda, Linux yapılandırma aracı için Log Analytics aracısını en son yapılandırma uygulanmıyor portal yapılandırması hizmet ile iletişim kurmak mümkün olmayabilir.
  1. Bu maddeyi `omsconfig` aracısının yüklü çalıştırarak `dpkg --list omsconfig` veya `rpm -qi omsconfig`.  Yüklenmezse, Linux için Log Analytics aracısını en son sürümünü yeniden yükleyin.

  2. Bu maddeyi `omsconfig` aracı, aşağıdaki komutu çalıştırarak Azure İzleyici ile kurabilir `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`. Bu komut, bu aracı yapılandırmasını döndürür Syslog ayarları, Linux performans sayaçları ve özel günlükler de dahil olmak üzere hizmetinden alır. Bu komut başarısız olursa, aşağıdaki komutu çalıştırarak `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`. Bu komut, Azure İzleyici konuşmak ve son yapılandırmayı almak için omsconfig aracı zorlar.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Sorun: Herhangi bir özel günlük veri görmediğinizden 

### <a name="probable-causes"></a>Olası nedenleri
* Azure İzleyici ekleme başarısız oldu.
* Ayar **aşağıdaki yapılandırmayı Linux Sunucularıma uygulamak** seçilmemiş.
* omsconfig en son özel günlük yapılandırması hizmetinden çekilen değil.
* Linux kullanıcı için log Analytics aracısını `omsagent` izinleri veya bulunamamasından dolayı özel günlük erişemiyor.  Aşağıdaki iletileri görebilirsiniz:
 * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
 * `[DATETIME] [error]: file not accessible by omsagent.`
* Bilinen sorun ile Linux sürümü 1.1.0-217 için Log Analytics aracısını düzeltilen yarış durumu

### <a name="resolution"></a>Çözüm
1. Azure İzleyici ekleme başarılı oldu aşağıdaki dosya varsa denetleyerek doğrulayın: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`. Değilse, ya da varsa:  

  1. Omsadmin.sh komut satırını kullanarak Reonboard [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
  2. Altında **Gelişmiş ayarlar** ayar Azure portalında emin **aşağıdaki yapılandırmayı Linux Sunucularıma uygulamak** etkinleştirilir.  

2. Bu maddeyi `omsconfig` aracı, aşağıdaki komutu çalıştırarak Azure İzleyici ile kurabilir `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`.  Bu komut, bu aracı yapılandırmasını döndürür Syslog ayarları, Linux performans sayaçları ve özel günlükler de dahil olmak üzere hizmetinden alır. Bu komut başarısız olursa, aşağıdaki komutu çalıştırarak `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py`. Bu komut, Azure İzleyici konuşmak ve son yapılandırmayı almak için omsconfig aracı zorlar.

**Arka planı:** -Ayrıcalıklı bir kullanıcısı olarak çalışan Linux için Log Analytics aracısını yerine `root`, olarak aracıyı çalıştıran `omsagent` kullanıcı. Çoğu durumda, okumak için bu kullanıcı için belirli dosyaları için sırayla yönelik açık izinlerinin verilmesi gerekir. İzni vermek için `omsagent` kullanıcı, aşağıdaki komutları çalıştırın:

1. Ekleme `omsagent` belirli bir grup kullanıcıya `sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Gerekli dosya Evrensel okuma erişimi verin `sudo chmod -R ugo+rx <FILE DIRECTORY>`

Bir yarış durumu ile Log Analytics aracısını 1.1.0-217'den önceki Linux sürümü için bilinen bir sorun yoktur. En son aracıya güncelleştirdikten sonra çıkış eklentisi en son sürümünü almak için aşağıdaki komutu çalıştırın `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Sorun: Yeni bir çalışma alanına reonboard çalıştığınız
Yeni bir çalışma alanı için bir aracı için reonboard çalıştığınızda, Log Analytics Aracısı yapılandırması önce reonboarding temizlenmesi gerekir. Eski yapılandırmada aracısından temizlemek için kabuk Paketle çalıştırın. `--purge`

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```
Veya

```
sudo sh ./onboard_agent.sh --purge
```

Reonboard sonra kullanmaya devam edebilirsiniz `--purge` seçeneği

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>Azure portalında log Analytics Aracısı uzantısı durumu ile başarısız olarak işaretlenir: Sağlama başarısız oldu

### <a name="probable-causes"></a>Olası nedenleri
* Log Analytics aracısını işletim sisteminden kaldırıldı
* Log Analytics Aracısı hizmeti kapalı, devre dışı veya yapılandırılmamış

### <a name="resolution"></a>Çözüm 
Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin.
1. Azure Portalı'ndan uzantısını kaldırın.
2. Ardından aracıyı yüklemek [yönergeleri](../../azure-monitor/learn/quick-collect-linux-computer.md).
3. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart`.
* Birkaç dakika bekleyin ve sağlama durumu değişikliklerini **sağlama başarılı**.


## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Sorun: Log Analytics aracısını yükseltme isteğe bağlı

### <a name="probable-causes"></a>Olası nedenleri

Log Analytics aracısını paketleri konaktaki geçmiştir.

### <a name="resolution"></a>Çözüm 
Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin.

1. Denetlemek için en son sürüm [sayfa](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/).
2. Yükleme betiğini indirin (1.4.2-124 örnek sürüm olarak):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. Paketleri yürüterek yükseltme `sudo sh ./omsagent-*.universal.x64.sh --upgrade`.
