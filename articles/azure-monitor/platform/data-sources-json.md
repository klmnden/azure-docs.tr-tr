---
title: Azure İzleyici'de özel JSON verileri toplama | Microsoft Docs
description: Özel JSON veri kaynakları, Azure İzleyici'de Linux için Log Analytics aracısını kullanarak halinde toplanabilir.  Bu özel veri kaynaklarının curl veya FluentD'ın 300'ü aşkın eklentileri gibi JSON döndüren basit betik dosyalarını olabilir. Bu makalede, bu veri toplama için gerekli yapılandırmayı açıklar.
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
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 101719668fee155e84b7a767647a662ca845f0f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60804655"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-azure-monitor"></a>Azure İzleyici'de Linux için Log Analytics aracısını özel JSON veri kaynaklarıyla toplama
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Özel JSON veri kaynakları toplanmasını içine [Azure İzleyici](data-platform.md) Linux için Log Analytics aracısını kullanarak.  Bu özel veri kaynakları gibi JSON döndüren basit betik dosyalarını olabilir [curl](https://curl.haxx.se/) veya biri [FluentD'ın 300'ü aşkın eklentileri](https://www.fluentd.org/plugins/all). Bu makalede, bu veri toplama için gerekli yapılandırmayı açıklar.


> [!NOTE]
> Linux v1.1.0 için log Analytics aracısını-217 + özel JSON verileri için gerekli

## <a name="configuration"></a>Yapılandırma

### <a name="configure-input-plugin"></a>Giriş eklentisini yapılandırma

Azure İzleyici JSON verilerini toplamak için ekleme `oms.api.` bir giriş eklentisi FluentD etiketinde başlatma.

Örneğin, ayrı bir yapılandırma dosyası verilmiştir `exec-json.conf` içinde `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Bu FluentD eklentisi `exec` her 30 saniyede bir curl komutu çalıştırın.  Bu komutun çıktısı, JSON çıkış eklenti tarafından toplanır.

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
Yapılandırma dosyası altında eklenen `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` aşağıdaki komutla değiştirilen sahipliğine sahip olmasını gerektirir.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Çıkış eklentisini yapılandırma 
Ana yapılandırma aşağıdaki çıktı eklentisi yapılandırma eklemek `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` veya ayrı bir yapılandırma dosyası yerleştirilmiş olarak `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-log-analytics-agent-for-linux"></a>Linux için log Analytics aracısını yeniden başlatın
Aşağıdaki komutla Linux hizmet için Log Analytics aracısını yeniden başlatın.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Çıktı
Azure İzleyici'de bir kayıt türü ile veri toplanacağını `<FLUENTD_TAG>_CL`.

Örneğin, özel etiket `tag oms.api.tomcat` kayıt türü ile Azure İzleyicisi'nde `tomcat_CL`.  Bu tür aşağıdaki günlük sorgusu ile tüm kayıtlar alabilir.

    Type=tomcat_CL

İç içe JSON veri kaynakları için desteklenir, ancak bu dizine alınmış üst alanın dışına temel. Örneğin, aşağıdaki JSON verilerini bir günlük sorgusu döndürüldüğü `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 