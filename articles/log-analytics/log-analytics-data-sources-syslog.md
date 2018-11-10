---
title: Toplama ve çözümleme OMS Log Analytics, Syslog iletileri | Microsoft Docs
description: Syslog Linux için ortak olan olay günlüğü protokolüdür. Bu makale, Log Analytics ve OMS deposunda oluşturdukları kayıtları ayrıntılarını Syslog iletileri koleksiyonu yapılandırmak açıklamaktadır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 132935371a1b4f52892ae2fb956407079906e79a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004911"
---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog Log analytics'te veri kaynakları
Syslog Linux için ortak olan olay günlüğü protokolüdür.  Uygulamaları, yerel makinede depolanan veya bir Syslog Toplayıcıya teslim olabilir iletileri gönderir.  Linux için OMS aracısı yüklü olduğunda, aracıya ileti iletmek için yerel Syslog daemon'u yapılandırır.  Aracı bu durumda OMS deposunda karşılık gelen bir kaydın oluşturulduğu Log Analytics ileti gönderir.  

> [!NOTE]
> Log Analytics'e toplama rsyslog varsayılan arka plan programı olduğu rsyslog veya syslog-ng tarafından gönderilen iletilerin destekler. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünde varsayılan syslog daemon'u syslog olay toplaması için desteklenmiyor. Bu dağıtımları sürümünden Syslog verilerini toplamak için [rsyslog daemon](http://rsyslog.com) yüklenmeli ve sysklog değiştirmek için yapılandırılmış.
>
>

![Syslog koleksiyonu](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Syslog yapılandırma
Linux için OMS Aracısı yalnızca yapılandırmasında belirtilen önem dereceleri ve olanakları ile olayları toplar.  Azure portalı üzerinden ya da Linux aracıları yapılandırma dosyalarını yönetme, Syslog yapılandırabilirsiniz.

### <a name="configure-syslog-in-the-azure-portal"></a>Azure portalında Syslog yapılandırma
Syslog gelen yapılandırma [Log Analytics Gelişmiş ayarlar veri menüde](log-analytics-data-sources.md#configuring-data-sources).  Bu yapılandırma her bir Linux aracı yapılandırma dosyasını teslim edilir.

Yeni bir özellik adını yazarak ve tıklayarak ekleyebilirsiniz **+**.  Her özellik için yalnızca seçilen önem dereceleri iletilerle toplanacak.  Toplamak istediğiniz belirli bir özellik için önem derecelerini işaretleyin.  İletilerine filtre uygulamak için herhangi bir ek ölçüt sağlayamaz.

![Syslog yapılandırma](media/log-analytics-data-sources-syslog/configure.png)

Varsayılan olarak, tüm yapılandırma değişiklikleri tüm aracılara otomatik olarak gönderilir.  Syslog her Linux aracısını el ile yapılandırmak istiyorsanız, kutunun işaretini kaldırın *aşağıdaki yapılandırmayı Linux makinelerime Uygula*.

### <a name="configure-syslog-on-linux-agent"></a>Syslog Linux Aracısı'nı yapılandırma
Zaman [OMS aracısı yüklü bir Linux istemcide](log-analytics-quick-collect-linux-computer.md), tesis ve önem derecesi toplanan tüm iletileri tanımlayan bir varsayılan syslog yapılandırma dosyası yükler.  Yapılandırmasını değiştirmek için bu dosyayı değiştirebilirsiniz.  Yapılandırma dosyası, istemcinin yüklediği Syslog daemon'u bağlı olarak farklılık gösterir.

> [!NOTE]
> Syslog yapılandırmasını düzenlerseniz, değişikliklerin etkili olması syslog daemon'u başlatmak gerekir.
>
>

#### <a name="rsyslog"></a>rsyslog
Rsyslog için yapılandırma dosyası şu konumdadır **/etc/rsyslog.d/95-omsagent.conf**.  Varsayılan içeriğini aşağıda gösterilmektedir.  Bu, uyarı ya da daha yüksek bir düzeyinde tüm özellikleri için yerel aracı gönderilen syslog iletileri toplar.

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

Kendi yapılandırma dosyası bölümünü kaldırarak bir özelliğini kaldırabilirsiniz.  Bu özelliği'nın girdisini değiştirerek belirli bir özellik için toplanan önem dereceleri sınırlayabilirsiniz.  Örneğin, iletileri kullanıcı tesis veya üzeri hata önem derecesi ile sınırlamak için şu yapılandırma dosyasının bu satırı değiştirin:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng
Syslog-ng yapılandırma dosyası konumdadır **/etc/syslog-ng/syslog-ng.conf**.  Varsayılan içeriğini aşağıda gösterilmektedir.  Bu, tüm özellikleri ve tüm önem dereceleri için yerel aracı gönderilen syslog iletileri toplar.   

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

Kendi yapılandırma dosyası bölümünü kaldırarak bir özelliğini kaldırabilirsiniz.  Belirli bir özellik için listesinden kaldırarak toplanır önem dereceleri sınırlayabilirsiniz.  Örneğin, yalnızca uyarı ve kritik iletileri için kullanıcı tesis sınırlamak için bu bölümü şu yapılandırma dosyasının değiştirin:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Ek Syslog bağlantı noktalarından verileri toplama
OMS Aracısı bağlantı noktası 25224 yerel istemcide Syslog iletileri dinler.  Aracı yüklendiğinde varsayılan syslog yapılandırması uygulanır ve şu konumda bulundu:

* Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`

İki yapılandırma dosyası oluşturarak, bağlantı noktası numarasını değiştirebilirsiniz: FluentD yapılandırma dosyası ve yüklediğiniz Syslog daemon'u bağlı olarak rsyslog veya syslog ng dosyası.  

* FluentD yapılandırma dosyası bulunan yeni bir dosya olmalıdır: `/etc/opt/microsoft/omsagent/conf/omsagent.d` ve değeri değiştirin **bağlantı noktası** özel bağlantı noktası numaranızı girişi.

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

* Rsyslog için bulunan yeni bir yapılandırma dosyası oluşturmanız gerekir: `/etc/rsyslog.d/` ve özel bağlantı noktası numaranızı % SYSLOG_PORT % değerini değiştirin.  

    > [!NOTE]
    > Yapılandırma dosyanızda bu değeri değiştirirseniz `95-omsagent.conf`, varsayılan yapılandırma aracı geçerli olduğu durumlarda üzerine yazılır.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* Aşağıda gösterilen örnek yapılandırma kopyalayarak syslog-ng yapılandırmanın değiştirilmesi gerektiğini ve özel değiştirilen ayarlar syslog ng.conf yapılandırma dosyasının sonuna ekleme bulunan `/etc/syslog-ng/`.  Yapmak **değil** varsayılan etiket **% WORKSPACE_ID % _oms** veya **% WORKSPACE_ID_OMS**, değişikliklerinizi ayırt edilmesine yardımcı olmak için özel bir etiket tanımlayın.  

    > [!NOTE]
    > Yapılandırma dosyasındaki varsayılan değerleri değiştirirseniz, varsayılan yapılandırma aracı geçerli olduğu durumlarda bunların üzerine yazılacak.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Değişiklikler, Syslog ve OMS Aracısı'nı tamamladıktan sonra hizmet yapılandırma değişikliklerinin etkinleşmesi emin olmak için yeniden başlatılması gerekiyor.   

## <a name="syslog-record-properties"></a>Syslog kayıt özellikleri
Syslog kayıtları sahip bir tür **Syslog** ve aşağıdaki tabloda gösterilen özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayar. |
| Tesis |İleti üreten sisteminin bir parçası olarak tanımlar. |
| HostIP |İletiyi gönderen sistem IP adresi. |
| ana bilgisayar adı |İletiyi gönderen sistem adı. |
| Err |Olay önem derecesi. |
| SyslogMessage |İletinin metni. |
| ProcessID |İleti oluşturulan işlem kimliği. |
| eventTime |Tarih ve olayın oluşturulduğu saat. |

## <a name="log-queries-with-syslog-records"></a>Günlük sorguları ile Syslog kayıtları
Aşağıdaki tabloda, Syslog kayıtları almak günlük sorguları farklı örnekler sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Syslog |Tüm Syslog'lar. |
| Syslog &#124; nerede ERR "error" == |Önem derecesi hata içeren tüm Syslog kayıtları. |
| Syslog &#124; Summarize aggregatedvalue = count() bilgisayar tarafından |Syslog kayıtlarını Say bilgisayar tarafından. |
| Syslog &#124; Summarize aggregatedvalue = count() tesis tarafından |Syslog kayıtlarını Say tesis tarafından. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [günlük aramaları](log-analytics-queries.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.
* Kullanım [özel alanlar](log-analytics-custom-fields.md) syslog kayıtları verilerinden ayrı ayrı alanlara ayrıştırılamıyor.
* [Linux aracıları yapılandırma](log-analytics-quick-collect-linux-computer.md) diğer veri türlerini, toplanacak.
