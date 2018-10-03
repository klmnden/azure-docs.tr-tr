---
title: Özel JSON verilerini OMS Log Analytics'e toplama | Microsoft Docs
description: Özel JSON veri kaynakları, Linux için OMS Aracısı'nı kullanarak Log Analytics'e halinde toplanabilir.  Bu özel veri kaynaklarının curl veya FluentD'ın 300'ü aşkın eklentileri gibi JSON döndüren basit betik dosyalarını olabilir. Bu makalede, bu veri toplama için gerekli yapılandırmayı açıklar.
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
ms.date: 05/04/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 9725a3df04ef28fc3a076c3c6ca6663e36b186a8
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040277"
---
# <a name="collecting-custom-json-data-sources-with-the-oms-agent-for-linux-in-log-analytics"></a>Log analytics'te Linux için OMS Aracısı özel JSON veri kaynaklarıyla toplama
Özel JSON veri kaynakları, Linux için OMS Aracısı'nı kullanarak Log Analytics'e halinde toplanabilir.  Bu özel veri kaynakları gibi JSON döndüren basit betik dosyalarını olabilir [curl](https://curl.haxx.se/) veya biri [FluentD'ın 300'ü aşkın eklentileri](http://www.fluentd.org/plugins/all). Bu makalede, bu veri toplama için gerekli yapılandırmayı açıklar.

> [!NOTE]
> V1.1.0 Linux için OMS Aracısı-217 + özel JSON verileri için gerekli

## <a name="configuration"></a>Yapılandırma

### <a name="configure-input-plugin"></a>Giriş eklentisini yapılandırma

Log analytics'te JSON verileri toplamak için ekleme `oms.api.` bir giriş eklentisi FluentD etiketinde başlatma.

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

### <a name="restart-oms-agent-for-linux"></a>Linux için OMS aracısını yeniden başlatın
Aşağıdaki komutla hizmeti Linux için OMS aracısını yeniden başlatın.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Çıktı
Bir kayıt türü ile Log Analytics'te veri toplanacağını `<FLUENTD_TAG>_CL`.

Örneğin, özel etiket `tag oms.api.tomcat` kayıt türü ile Log analytics'te `tomcat_CL`.  Bu tür aşağıdaki günlük araması ile tüm kayıtlar alabilir.

    Type=tomcat_CL

İç içe JSON veri kaynakları için desteklenir, ancak bu dizine alınmış üst alanın dışına temel. Örneğin, aşağıdaki JSON verilerini bir Log Analytics arama olarak döndürüldüğü `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [günlük aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 
 