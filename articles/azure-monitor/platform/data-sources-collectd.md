---
title: Azure İzleyici'de toplanan verileri toplamanızı | Microsoft Docs
description: Toplanan, uygulamalar ve sistem düzeyindeki bilgileri düzenli aralıklarla verileri toplar bir açık kaynak Linux daemon ' dir.  Bu makalede, Azure İzleyici'de toplanan veri toplanmasına hakkında bilgi sağlar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/27/2018
ms.author: magoedte
ms.openlocfilehash: 2118f137f2c0d32f891a170c3509bceee7ba13ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60764957"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Azure İzleyici'de Linux aracıları hakkında toplanan veri topla
[Toplanan](https://collectd.org/) olduğu bir açık kaynak Linux daemon, düzenli aralıklarla uygulama ve sistem düzeyindeki bilgileri performans ölçümleri toplar. Örnek uygulamalar, Java sanal makinesi (JVM), MySQL sunucusu ve Ngınx içerir. Bu makalede, Azure İzleyici'de toplanan performans veri toplanmasına hakkında bilgi sağlar.

Kullanılabilen eklentileri tam bir listesi şu yolda bulunabilir: [tablo, eklentileri](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Toplanan genel bakış](media/data-sources-collectd/overview.png)

Linux için Log Analytics aracısını Log Analytics aracısını Linux için toplanan verileri yönlendirmek için aşağıdaki toplanan yapılandırma dahildir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Buna ek olarak, toplanan bir sürümünü kullanıyorsanız önce 5.5, bunun yerine aşağıdaki yapılandırmayı kullanın.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Varsayılan toplanan yapılandırması kullanır`write_http` performans ölçüm verilerini Linux için Log Analytics aracısını 26000 bağlantı noktası üzerinden göndermek için eklenti. 

> [!NOTE]
> Gerekirse, bu bağlantı noktası özel tanımlı bir bağlantı noktası için yapılandırılabilir.

Linux için Log Analytics aracısını da toplanan ölçümler için 26000 numaralı bağlantı noktasında dinler ve bunları Azure İzleyici şema ölçümleri için dönüştürür. Linux yapılandırma için Log Analytics aracısını verilmiştir `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Desteklenen sürümler
- Azure İzleyici şu anda toplanan sürüm 4.8 destekler ve üstü.
- Toplanan ölçüm koleksiyonu için log Analytics aracısını Linux v1.1.0-217 veya üzeri gereklidir.


## <a name="configuration"></a>Yapılandırma
Azure İzleyici'de toplanan verileri toplamayı yapılandırmak için temel adımlar verilmiştir.

1. Toplanan write_http eklentisi kullanarak Linux için Log Analytics aracısını veri göndermek için yapılandırın.  
2. Toplanan verileri uygun bağlantı noktasında dinlemek Linux için Log Analytics aracısını yapılandırın.
3. Linux için toplanan ve Log Analytics aracısını yeniden başlatın.

### <a name="configure-collectd-to-forward-data"></a>Toplanan veri iletmek için yapılandırma 

1. Linux için Log Analytics aracısını toplanan verileri yönlendirmek için `oms.conf` toplanan'ın yapılandırma dizine eklenmesi gerekir. Bu dosya hedef üzerinde makinenizin Linux distro bağlıdır.

    Toplanan yapılandırma dizininize /etc/collectd.d/ içinde yer alıyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Toplanan yapılandırma dizininize /etc/collectd/collectd.conf.d/ içinde yer alıyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Toplanan sürümleri 5.5 önce etiketleri değiştirmek zorunda kalacaksınız `oms.conf` yukarıda da gösterildiği gibi.
    >

2. Collectd.conf istenen çalışma alanınızın omsagent yapılandırma dizinine kopyalayın.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Linux için toplanan ve Log Analytics aracısını aşağıdaki komutlarla yeniden başlatın.

    sudo hizmeti toplanan yeniden sudo /opt/microsoft/omsagent/bin/service_control yeniden başlatma

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>Azure İzleyici şema dönüştürme için toplanan ölçümleri
Linux için Log Analytics aracısını yeni ölçümler ile toplanmış altyapı ölçümleri arasında tanıdık bir model tarafından toplanan aşağıdaki şema eşleme toplanan korumak için kullanılır:

| Toplanan ölçüm alan | Azure İzleyici alan |
|:--|:--|
| `host` | Computer |
| `plugin` | None |
| `plugin_instance` | Örnek adı<br>Varsa **plugin_instance** olduğu *null* ardından InstanceName = " *_Total*" |
| `type` | ObjectName |
| `type_instance` | CounterName<br>Varsa **type_instance** olduğu *null* ardından CounterName =**boş** |
| `dsnames[]` | CounterName |
| `dstypes` | None |
| `values[]` | Ort |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 
* Kullanım [özel alanlar](custom-fields.md) syslog kayıtları verilerinden ayrı ayrı alanlara ayrıştırılamıyor.
