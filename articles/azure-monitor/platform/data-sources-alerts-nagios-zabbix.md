---
title: Azure İzleyici'de Nagios ve Zabbix uyarıları Topla | Microsoft Docs
description: Nagios ve Zabbix izleme araçları açık kaynaklıdır. Diğer kaynaklardan gelen uyarıların yanı sıra bunları analiz etmek için yararlı Azure İzleyici ile bu Araçları'ndan uyarılar toplayabilirsiniz.  Bu makalede, bu sistemlerden uyarılarını toplamak Linux için Log Analytics aracısını yapılandırmak açıklar.
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
ms.openlocfilehash: 0ed6747573edf4c059eb29d28107a22706c52856
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426198"
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-azure-monitor-from-log-analytics-agent-for-linux"></a>Linux için Log Analytics Aracısı'ndan Nagios ve Zabbix'ten Azure İzleyicisi'nde uyarılarını Topla 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

[Nagios](https://www.nagios.org/) ve [Zabbix](https://www.zabbix.com/) olan izleme araçları açık kaynak. Diğer kaynaklardan günlük verileri çözümlemek için yararlı Azure İzleyici ile bu Araçları'ndan uyarılar toplayabilirsiniz.  Bu makalede, bu sistemlerden uyarılarını toplamak Linux için Log Analytics aracısını yapılandırmak açıklar.


> [!NOTE]
> [Azure İzleyici tarafından oluşturulan uyarıların](alerts-overview.md) günlük verilerinden ayrı olarak depolanan ve günlük sorgularından erişilebilir değil.

 
## <a name="prerequisites"></a>Önkoşullar
Linux için Log Analytics aracısını sürümüne Nagios toplama uyarıları destekleyen 4.2.x ve Zabbix sürümüne 2.x.

## <a name="configure-alert-collection"></a>Uyarı koleksiyonunu yapılandırma

### <a name="configuring-nagios-alert-collection"></a>Nagios uyarı toplamayı yapılandırma
Uyarılarını toplamak için Nagios sunucusunda aşağıdaki adımları gerçekleştirin.

1. Kullanıcı vermek **omsagent** Nagios günlük dosyası okuma erişimi `/var/log/nagios/nagios.log`. Nagios.log dosya varsayılarak grubun sahibi olduğu `nagios`, kullanıcı ekleyebilir **omsagent** için **nagios** grubu. 

    sudo usermod - a -G nagios omsagent

2.  Yapılandırma dosyası değişiklik `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Aşağıdaki girişler mevcut ve kullanıma açıklamalı olduğundan emin olun:  

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

3. Omsagent Daemon programını yeniden başlatın

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Zabbix uyarı toplamayı yapılandırma
Zabbix sunucudan uyarılarını toplamak için kullanıcı ve parola belirtmeniz gerekir *düz metin*.  İdeal değildir ancak ilgili uyarılar yakalamak için salt okunur izinlere sahip bir Zabbix kullanıcı oluşturma öneririz.

Nagios sunucuda uyarıları toplamak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma dosyası değişiklik `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Mevcut ve kullanıma açıklamalı aşağıdaki girişler olduğundan emin olun.  Kullanıcı adı ve parola Zabbix ortamınız için değerlerle değiştirin.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Omsagent Daemon programını yeniden başlatın

    `sudo sh /opt/microsoft/omsagent/bin/service_control restart`


## <a name="alert-records"></a>Uyarı kayıtları
Nagios ve Zabbix uyarı kayıtları alabilir kullanarak [oturum sorguları](../log-query/log-query-overview.md) Azure İzleyici'de.

### <a name="nagios-alert-records"></a>Nagios uyarı kayıtları

Nagios tarafından toplanan kayıtlarına sahip uyarı bir **türü** , **uyarı** ve **Analytics'teki** , **Nagios**.  Bunlar aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*Uyarı* |
| `SourceSystem` |*Nagios* |
| `AlertName` |Uyarının adı. |
| `AlertDescription` | Uyarı açıklaması. |
| `AlertState` | Ana bilgisayar ve hizmet durumu.<br><br>Tamam<br>UYARI<br>AYARLAMA<br>AŞAĞI |
| `HostName` | Uyarıyı oluşturan ana bilgisayar adı. |
| `PriorityNumber` | Uyarı öncelik düzeyi. |
| `StateType` | Uyarı durumu türü.<br><br>YAZILIM - değil yeniden denetlenmesine sorunu.<br>SABİT - olan sorunu belirtilen sayıda yeniden denetlenmesine.  |
| `TimeGenerated` |Tarihi ve uyarının oluşturulduğu saat. |


### <a name="zabbix-alert-records"></a>Zabbix uyarı kayıtları
Zabbix tarafından toplanan kayıtlarına sahip uyarı bir **türü** , **uyarı** ve **Analytics'teki** , **Zabbix**.  Bunlar aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*Uyarı* |
| `SourceSystem` |*Zabbix* |
| `AlertName` | Uyarının adı. |
| `AlertPriority` | Uyarının önem derecesi.<br><br>Sınıflandırılmamış<br>bilgi<br>uyarı<br>ortalama<br>Yüksek<br>Olağanüstü durum  |
| `AlertState` | Uyarı durumu.<br><br>0 - durumunun güncel olup.<br>1 - durum bilinmiyor.  |
| `AlertTypeNumber` | Birden çok sorun olayı uyarı oluşturabilen olup olmadığını belirtir.<br><br>0 - durumunun güncel olup.<br>1 - durum bilinmiyor.    |
| `Comments` | Uyarı için ek açıklamalar. |
| `HostName` | Uyarıyı oluşturan ana bilgisayar adı. |
| `PriorityNumber` | Uyarının önem derecesini belirten değer.<br><br>0 - değil olarak sınıflandırılmış<br>1 - bilgiler<br>2 - uyarı<br>3 - ortalama<br>4 - yüksek<br>5 - olağanüstü durum |
| `TimeGenerated` |Tarihi ve uyarının oluşturulduğu saat. |
| `TimeLastModified` |Tarih ve saat uyarının durumunu en son değiştirildiği. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uyarılar](alerts-overview.md) Azure İzleyici'de.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 
