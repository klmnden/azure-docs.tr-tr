---
title: OMS günlük analizi özel JSON verileri toplama | Microsoft Docs
description: Özel JSON veri kaynakları için Linux OMS Aracısı'nı kullanarak günlük analizi içine toplanabilir.  Bu özel veri kaynaklarının basit betik dosyalarını curl veya FluentD'ın 300 + eklentileri gibi JSON döndürüyor olabilir. Bu makalede, bu veri koleksiyonu için gerekli yapılandırmayı açıklar.
services: log-analytics
documentationcenter: ''
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
ms.openlocfilehash: 800ee1269556e7c2d56fbbf2b497c10509b5c78c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23855225"
---
# <a name="collecting-custom-json-data-sources-with-the-oms-agent-for-linux-in-log-analytics"></a>Günlük analizi Linux için OMS aracısının özel JSON veri kaynaklarıyla toplama
Özel JSON veri kaynakları için Linux OMS Aracısı'nı kullanarak günlük analizi içine toplanabilir.  Bu özel veri kaynaklarının JSON gibi döndüren basit betik dosyalarını olabilir [curl](https://curl.haxx.se/) veya biri [FluentD'ın 300 + eklentileri](http://www.fluentd.org/plugins/all). Bu makalede, bu veri koleksiyonu için gerekli yapılandırmayı açıklar.

> [!NOTE]
> Linux v1.1.0 için OMS aracısının-217 + özel JSON verileri için gerekli

## <a name="configuration"></a>Yapılandırma

### <a name="configure-input-plugin"></a>Giriş eklentisi yapılandırma

Günlük analizi JSON verileri toplamak için ekleme `oms.api.` giriş eklentisi FluentD etiketinde başlangıcı.

Örneğin, aşağıdaki ayrı yapılandırma dosyası `exec-json.conf` içinde `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Bu FluentD eklentisi kullanır `exec` her 30 saniyede bir curl komutunu çalıştırmak için.  Bu komutun çıktısı, JSON çıkış eklenti tarafından toplanır.

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
Yapılandırma dosyası altında eklenen `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` aşağıdaki komutla değiştirilen sahipliğini olmasını gerektirir.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Çıktı eklentisi yapılandırma 
Ana yapılandırmasında aşağıdaki çıktı eklentisi yapılandırma eklemek `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` ya da ayrı yapılandırma dosyası yerleştirilen gibi`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

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

### <a name="restart-oms-agent-for-linux"></a>Linux için OMS aracıyı yeniden başlatın
OMS aracısı aşağıdaki komutla Linux hizmeti yeniden başlatın.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Çıktı
Verileri bir kayıt türü ile günlük analizi toplanacağını `<FLUENTD_TAG>_CL`.

Örneğin, özel etiket `tag oms.api.tomcat` kayıt türüne sahip günlük analytics'te `tomcat_CL`.  Aşağıdaki günlük arama ile bu türdeki tüm kayıtları alınamadı.

    Type=tomcat_CL

İç içe JSON veri kaynakları desteklenir, ancak dizini üst alanı dışına temel. Örneğin, aşağıdaki JSON verilerini bir günlük analizi aramadan döndürülen `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 
 