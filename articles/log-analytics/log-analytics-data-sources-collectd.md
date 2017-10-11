---
title: "OMS günlük analizi CollectD veri toplamak | Microsoft Docs"
description: "CollectD düzenli aralıklarla veri uygulamaları ve sistem düzeyi bilgileri toplayan bir açık kaynak Linux arka plan programı kullanılır.  Bu makalede, günlük analizi CollectD gelen veri toplama hakkında bilgi sağlar."
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
ms.date: 05/02/2017
ms.author: magoedte
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Veri günlük analizi Linux aracıları CollectD Topla
[CollectD](https://collectd.org/) uygulamaları ve sistem düzeyi bilgileri düzenli aralıklarla performans ölçümleri toplayan bir açık kaynak Linux arka plan programı kullanılır. Örnek uygulamalar, Java sanal makine (JVM), MySQL Server ve Nginx içerir. Bu makalede günlük analizi CollectD gelen performans verileri toplama hakkında bilgi sağlar.

Kullanılabilen eklentileri tam listesi bulunabilir [tablo, eklenti](https://collectd.org/wiki/index.php/Table_of_Plugins).

![CollectD genel bakış](media/log-analytics-data-sources-collectd/overview.png)

Aşağıdaki CollectD yapılandırma OMS Aracısı Linux için OMS aracısının rota CollectD verilere Linux için dahil edilir.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Ayrıca, bunun yerine aşağıdaki yapılandırma 5.5 kullanmadan önce bir collectD sürümleri kullanıyorsanız.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Varsayılan CollectD yapılandırmasını kullanan`write_http` performans ölçüm verilerini 26000 bağlantı noktası üzerinden Linux için OMS Aracısı'na göndermek için eklenti. 

> [!NOTE]
> Gerekirse, bu bağlantı noktası özel tanımlı bir bağlantı noktası için yapılandırılabilir.

Linux için OMS aracısının de CollectD ölçümünün 26000 numaralı bağlantı noktasında dinler ve bunları OMS şema ölçümlere dönüştürür. Linux yapılandırması için OMS aracısının aşağıdadır `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Desteklenen sürümleri
- Günlük analizi, şu anda CollectD sürüm 4.8 destekler ve üstü.
- Yukarıdaki veya Linux v1.1.0-217 için OMS aracısının CollectD ölçüm koleksiyonu için gereklidir.


## <a name="configuration"></a>Yapılandırma
Günlük analizi CollectD veri koleksiyonunu yapılandırmak için temel adımlar verilmiştir.

1. OMS Aracısı write_http eklentisi kullanarak Linux veri göndermesini CollectD yapılandırın.  
2. CollectD veri uygun bağlantı noktasında dinlemek Linux için OMS Aracısı yapılandırın.
3. CollectD ve Linux için OMS aracısı yeniden başlatın.

### <a name="configure-collectd-to-forward-data"></a>Veri iletmek için CollectD yapılandırın 

1. Linux için OMS aracısının rota CollectD verilere `oms.conf` CollectD'ın yapılandırma dizinine eklenmesi gerekir. Bu dosya hedef makinenizi Linux distro üzerinde bağlıdır.

    CollectD yapılandırma dizininize /etc/collectd.d/ içinde yer alıyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    CollectD yapılandırma dizininize /etc/collectd/collectd.conf.d/ içinde yer alıyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >5.5 önce CollectD sürümleri için etiketleri değiştirmek zorunda kalacaksınız `oms.conf` yukarıda gösterildiği gibi.
    >

2. Collectd.conf istenen Workspace'in omsagent yapılandırma dizinine kopyalayın.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Linux için CollectD ve OMS aracısı aşağıdaki komutlarla yeniden başlatın.

    sudo hizmet collectd yeniden sudo /opt/microsoft/omsagent/bin/service_control yeniden başlatma

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>Günlük analizi şema dönüştürme CollectD ölçümleri
Bilinen bir model zaten Linux için OMS aracısı tarafından toplanan altyapı Ölçümler ve yeni ölçümler arasında aşağıdaki şema eşleme CollectD tarafından toplanan korumak için kullanılır:

| CollectD ölçüm alan | Günlük analizi alan |
|:--|:--|
| ana bilgisayar | Bilgisayar |
| Eklentisi | None |
| plugin_instance | Örnek adı<br>Varsa **plugin_instance** olan *null* sonra InstanceName = "*_Total*" |
| type | ObjectName |
| type_instance | CounterName<br>Varsa **type_instance** olan *null* sonra CounterName =**boş** |
| dsnames] | CounterName |
| dstypes | None |
| değerler] | CounterValue |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 
* Kullanım [özel alanlar](log-analytics-custom-fields.md) tek tek alanlarına syslog kayıtları verilerden ayrıştırılamadı.

