---
title: "Toplamak ve analiz etmek OMS günlük analizi Syslog iletileri | Microsoft Docs"
description: "Syslog Linux için ortak bir olay günlüğü protokolüdür. Bu makalede, Syslog iletileri koleksiyonu günlük analizi ve OMS depoya oluşturdukları kayıtları ayrıntılarını yapılandırmak açıklar."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 7513f405d5c7c05a8e6e2b7b0e6313f23a319c84
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog veri kaynaklarında günlük analizi
Syslog Linux için ortak bir olay günlüğü protokolüdür.  Uygulamaları yerel makinede depolanan olabilir veya bir Syslog Toplayıcıya teslim iletileri gönderir.  Linux için OMS Aracısı yüklendiğinde, aracıya iletilerini iletmek için yerel Syslog arka plan programı yapılandırır.  Aracı bu durumda günlük karşılık gelen bir kayıt OMS depoya oluşturulduğu analizi iletiyi gönderir.  

> [!NOTE]
> Günlük analizi rsyslog varsayılan arka plan programı olduğu rsyslog veya syslog-ng tarafından gönderilen iletileri koleksiyonu destekler. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünü varsayılan syslog arka plan syslog olay toplaması için desteklenmiyor. Bu dağıtımları bu sürümünden Syslog verileri toplamak için [rsyslog arka plan programı](http://rsyslog.com) sysklog değiştirmek için yapılandırılmış ve yüklü gerekir.
>
>

![Syslog koleksiyonu](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Syslog yapılandırma
Linux için OMS aracısının yalnızca tesisler ve yapılandırmasıyla belirtilen önem derecelerine sahip olayları toplar.  OMS portalı üzerinden veya Linux aracıları yapılandırma dosyalarını yönetme tarafından Syslog yapılandırabilirsiniz.

### <a name="configure-syslog-in-the-oms-portal"></a>Syslog OMS portalında yapılandırın
Syslog gelen yapılandırma [günlük analizi ayarları veri menüde](log-analytics-data-sources.md#configuring-data-sources).  Bu yapılandırma her Linux Aracısı yapılandırma dosyasına teslim edilir.

Adını yazıp'yi tıklatarak yeni bir tesis ekleyebilirsiniz  **+** .  Her özelliği için yalnızca seçili önem derecelerine sahip iletileri toplanacaktır.  Toplamak istediğiniz belirli olanağı için önem derecelerine denetleyin.  İletileri Filtrele için herhangi bir ek ölçüt sağlayamaz.

![Syslog yapılandırın](media/log-analytics-data-sources-syslog/configure.png)

Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracıları için gönderilir.  Syslog her Linux Aracısı'nı el ile yapılandırmak istiyorsanız, kutunun işaretini *aşağıdaki yapılandırmayı Linux makinelerime Uygula*.

### <a name="configure-syslog-on-linux-agent"></a>Syslog Linux Aracısı'nı yapılandırma
Zaman [OMS Aracısı Linux istemcide yüklü](log-analytics-linux-agents.md), tesis ve toplanan iletileri önemini tanımlayan bir varsayılan syslog yapılandırma dosyası yükler.  Yapılandırmasını değiştirmek için bu dosyayı değiştirebilirsiniz.  Yapılandırma dosyası, istemcinin yüklediği Syslog arka plan programı bağlı olarak farklıdır.

> [!NOTE]
> Syslog yapılandırma düzenlerseniz, syslog arka plan programı değişikliklerin etkili olması yeniden başlatmanız gerekir.
>
>

#### <a name="rsyslog"></a>rsyslog
Rsyslog yapılandırma dosyası şu konumdadır **/etc/rsyslog.d/95-omsagent.conf**.  Varsayılan içeriğini aşağıda verilmiştir.  Bu, uyarı ya da daha yüksek bir düzeyinde tüm tesisler için yerel Aracısı'ndan gönderilen syslog iletileri toplar.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Yapılandırma dosyasının kendi bölümü kaldırarak bir tesis kaldırabilirsiniz.  Bu tesis 's girdisini değiştirerek belirli olanağını toplanan önem derecelerine sınırlayabilirsiniz.  Örneğin, iletilere kullanıcı tesis hata veya daha büyük bir önem derecesi ile sınırlandırmak için aşağıdaki yapılandırma dosyasına satırını değiştirin:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog ng
Syslog ng için yapılandırma dosyası konumdadır **/etc/syslog-ng/syslog-ng.conf**.  Varsayılan içeriğini aşağıda verilmiştir.  Tüm Tesisler ve tüm önem dereceleridir için yerel Aracısı'ndan gönderilen syslog iletileri toplar.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Yapılandırma dosyasının kendi bölümü kaldırarak bir tesis kaldırabilirsiniz.  Belirli bir özellik için listeden kaldırarak toplanır önem derecelerine sınırlayabilirsiniz.  Örneğin, yalnızca uyarı ve kritik iletileri için kullanıcı tesis sınırlamak için şu yapılandırma dosyasının bu bölümü değiştirin:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Ek Syslog bağlantı noktalarından verileri toplama
OMS Aracısı bağlantı noktası 25224 yerel istemcide Syslog iletileri dinler.  Aracıyı yüklediğinizde, varsayılan bir syslog yapılandırma uygulanır ve şu konumda bulunamadı:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

Bağlantı noktası numarasını iki yapılandırma dosyası oluşturarak değiştirebilirsiniz: FluentD yapılandırma dosyası ve bir rsyslog veya syslog ng dosyası yüklediğiniz Syslog arka plan programı bağlı olarak.  

* FluentD yapılandırma dosyası bulunan yeni bir dosya olmalıdır: `/etc/opt/microsoft/omsagent/conf/omsagent.d` ve değeri değiştirin **bağlantı noktası** giriş, özel bir bağlantı noktası numarası.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Rsyslog için bulunan yeni bir yapılandırma dosyası oluşturmanız gerekir: `/etc/rsyslog.d/` ve % SYSLOG_PORT % değerini, özel bir bağlantı noktası numarasıyla değiştirin.  

    > [!NOTE]
    > Yapılandırma dosyasında bu değeri değiştirirseniz, `95-omsagent.conf`, varsayılan bir yapılandırma aracının geçerli olduğu durumlarda üzerine yazılır.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* Aşağıda gösterilen örnek yapılandırma kopyalayarak syslog ng config değiştirilmemelidir ve özel değiştirilmiş ayarları syslog ng.conf yapılandırma dosyasının sonuna ekleme bulunan `/etc/syslog-ng/`.  Yapmak **değil** varsayılan etiket **% WORKSPACE_ID % _oms** veya **% WORKSPACE_ID_OMS**, yaptığınız değişiklikleri ayırt etmeye yardımcı olması için bir özel etiket tanımlayın.  

    > [!NOTE]
    > Yapılandırma dosyasındaki varsayılan değerleri değiştirirseniz, varsayılan bir yapılandırma aracının geçerli olduğu durumlarda bunların üzerine yazılacak.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Değişiklikler, Syslog ve OMS Aracısı'nı tamamladıktan sonra hizmet yapılandırma değişikliklerinin etkinleşmesi emin olmak için yeniden başlatılması gerekiyor.   

## <a name="syslog-record-properties"></a>Syslog kaydı Özellikler
Syslog kayıtları sahip bir tür **Syslog** ve aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayar. |
| Tesis |İleti oluşturulan sisteminin parçası tanımlar. |
| HostIP |İleti gönderilirken sistem IP adresi. |
| Ana bilgisayar adı |İleti gönderilirken sistem adı. |
| Önem düzeyi |Olay önem derecesi. |
| SyslogMessage |İleti metni. |
| İşlem kimliği |İleti oluşturulan işlemin kimliği. |
| EventTime |Tarih ve olayın oluşturulduğu saat. |

## <a name="log-queries-with-syslog-records"></a>Syslog kayıtlarla günlük sorguları
Aşağıdaki tabloda, Syslog kayıtları almak günlük sorgularının farklı örnekler verilmektedir.

| Sorgu | Açıklama |
|:--- |:--- |
| Türü Syslog = |Tüm Syslog modüllerini. |
| Türü Syslog önem düzeyi = hata = |Tüm Syslog kayıtları hata önem derecesi. |
| Tür = Syslog &#124; Bilgisayar tarafından ölçü Count() işlevi |Bilgisayar tarafından sayısı, Syslog kaydeder. |
| Tür = Syslog &#124; Ölçü count() tesis tarafından |Tesis tarafından sayısı, Syslog kaydeder. |

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](log-analytics-log-search-upgrade.md) yükseltilmişse, yukarıdaki sorguların aşağıdaki gibi değiştirilmesi gerekir.

> | Sorgu | Açıklama |
|:--- |:--- |
| Syslog |Tüm Syslog modüllerini. |
| Syslog &#124; Burada önem düzeyi "error" == |Tüm Syslog kayıtları hata önem derecesi. |
| Syslog &#124; AggregatedValue özetlemek bilgisayar tarafından count() = |Bilgisayar tarafından sayısı, Syslog kaydeder. |
| Syslog &#124; AggregatedValue özetlemek tesis tarafından count() = |Tesis tarafından sayısı, Syslog kaydeder. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.
* Kullanım [özel alanlar](log-analytics-custom-fields.md) tek tek alanlarına syslog kayıtları verilerden ayrıştırılamadı.
* [Linux aracılarını yapılandırma](log-analytics-linux-agents.md) diğer veri türleri toplanacak.
