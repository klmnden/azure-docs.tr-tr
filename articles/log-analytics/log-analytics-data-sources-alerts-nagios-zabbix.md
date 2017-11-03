---
title: "OMS günlük analizi Nagios ve Zabbix uyarıları Topla | Microsoft Docs"
description: "Nagios ve Zabbix izleme araçları açık kaynaktır. Bunları yanı sıra diğer kaynaklardan uyarıları çözümlemek amacıyla yararlı günlük analizi bu Araçları'ndan uyarıları toplayabilirsiniz.  Bu makalede, bu sistemlerden uyarılarını toplamak Linux için OMS aracısının yapılandırma açıklar."
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
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Linux için Nagios ve günlük analizi OMS aracısından Zabbix uyarılarını Topla 
[Nagios](https://www.nagios.org/) ve [Zabbix](http://www.zabbix.com/) olan açık kaynak izleme araçları.  Uyarıları bu Araçları'ndan günlük analizi ile birlikte çözümlemek için toplayabilirsiniz [diğer kaynaklardan uyarıları](log-analytics-alerts.md).  Bu makalede, bu sistemlerden uyarılarını toplamak Linux için OMS aracısının yapılandırma açıklar.
 
## <a name="configure-alert-collection"></a>Uyarı koleksiyonunu yapılandırma

### <a name="configuring-nagios-alert-collection"></a>Nagios uyarı koleksiyonunu yapılandırma
Uyarılarını toplamak için Nagios sunucusunda aşağıdaki adımları gerçekleştirin.

1. Kullanıcının izni **omsagent** okuma erişimi Nagios günlük dosyasına (yani `/var/log/nagios/nagios.log`). Nagios.log dosya varsayılarak grupla sahibi `nagios`, kullanıcı ekleyebilir **omsagent** için **nagios** grubu. 

    sudo usermod - a -G nagios omsagent

2.  Konumunda yapılandırma dosyasını değiştirme (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Mevcut ve kullanıma açıklamalı aşağıdaki girdileri çalıştığından emin olun:  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Omsagent arka plan programı yeniden başlatın

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Zabbix uyarı koleksiyonunu yapılandırma
Zabbix sunucudan uyarılarını toplamak için bir kullanıcı ve parola belirtmeniz gerekir *açık metin*. Bu ideal değildir, ancak kullanıcı oluşturmak ve onlu izlemek için izinleri öneririz.

Uyarılarını toplamak için Nagios sunucusunda aşağıdaki adımları gerçekleştirin.

1. Konumunda yapılandırma dosyasını değiştirme (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Mevcut ve kullanıma açıklamalı aşağıdaki girdileri olduğundan emin olun.  Kullanıcı adı ve parola Zabbix ortamınız için değerlerle değiştirin.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Omsagent arka plan programı yeniden başlatın

    sudo Paylaş /opt/microsoft/omsagent/bin/service_control yeniden başlatın


## <a name="alert-records"></a>Uyarı kaydeder
Nagios ve Zabbix uyarı kayıtları almak kullanarak [oturum aramaları](log-analytics-log-searches.md) günlük analizi içinde.

### <a name="nagios-alert-records"></a>Nagios uyarı kaydeder

Nagios tarafından toplanan kayıtları uyarı bir **türü** , **uyarı** ve **SourceSystem** , **Nagios**.  Aşağıdaki tabloda özellikleri sahiptirler.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*Nagios* |
| AlertName |Uyarı adı. |
| AlertDescription | Uyarı açıklaması. |
| AlertState | Hizmet veya ana bilgisayar durumu.<br><br>TAMAM<br>UYARI<br>AYARLAMA<br>AŞAĞI |
| Ana bilgisayar adı | Uyarı oluşturan ana bilgisayar adı. |
| PriorityNumber | Uyarı öncelik düzeyi. |
| StateType | Uyarının durumunu türü.<br><br>SOFT - değil yeniden sorun.<br>Sabit - bırakıldı sorunu belirtilen kaç kez yeniden.  |
| TimeGenerated |Uyarının oluşturulduğu tarih ve saat. |


### <a name="zabbix-alert-records"></a>Zabbix uyarı kaydeder
Zabbix tarafından toplanan kayıtları uyarı bir **türü** , **uyarı** ve **SourceSystem** , **Zabbix**.  Aşağıdaki tabloda özellikleri sahiptirler.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*Zabbix* |
| AlertName | Uyarı adı. |
| AlertPriority | Uyarının önem derecesi.<br><br>Sınıflandırılmamış<br>Bilgi<br>Uyarı<br>Ortalama<br>Yüksek<br>Olağanüstü durum  |
| AlertState | Uyarı durumu.<br><br>0 - durumu güncel değil.<br>1 - durumu bilinmiyor.  |
| AlertTypeNumber | Uyarı birden çok sorun Olay Oluştur olup olmadığını belirtir.<br><br>0 - durumu güncel değil.<br>1 - durumu bilinmiyor.    |
| Yorumlar | Ek açıklamalar uyarı için. |
| Ana bilgisayar adı | Uyarı oluşturan ana bilgisayar adı. |
| PriorityNumber | Uyarının önem derecesini belirten değer.<br><br>0 - Sınıflandırılmamış<br>1 - bilgileri<br>2 - uyarı<br>3 - ortalama<br>4 - yüksek<br>5 - olağanüstü durum |
| TimeGenerated |Uyarının oluşturulduğu tarih ve saat. |
| TimeLastModified |Tarih ve saat uyarının durumunu en son değiştirildiği. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uyarıları](log-analytics-alerts.md) günlük analizi içinde.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 
