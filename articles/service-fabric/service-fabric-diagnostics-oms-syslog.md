---
title: Azure Service fabric'te Linux Küme Olayları izleyin | Microsoft Docs
description: Syslog Linux küme olayları izleme hakkında bilgi edinin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: 402e3dfe018c94ef068caf918b38aaad00064a49
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670379"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Service Fabric Linux kümesi Syslog olayları

Service Fabric platform olaylarını kümenizdeki önemli etkinlik hakkında bilgilendirmek için bir dizi kullanıma sunar. Kullanıma sunulan olayların tam listesi kullanılabilir [burada](service-fabric-diagnostics-event-generation-operational.md). Çeşitli şekillerde bu olayları tüketilebilir vardır. Bu makalede, Service Fabric, bu olayları için Syslog yazmak için yapılandırma tartışmak için kullanacağız.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="introduction"></a>Giriş

Syslog ile Linux kümeleri için Service Fabric platform olaylarına gönderilecek SyslogConsumer 6.4 sürümde sunulmuştur. Etkinleştirildikten sonra olayları otomatik olarak toplanan ve Log Analytics aracı tarafından gönderilen Syslog akar.

Her bir Syslog olayının 4 bileşenleri olan
* Özellik
* Kimlik
* İleti
* Önem Derecesi

SyslogConsumer özelliğini kullanarak tüm platform olayları yazar `Local0`. Geçerli bir birime config config değiştirerek güncelleştirebilirsiniz. Kimliktir `ServiceFabric`. Mesaj alanına, böylelikle sorgulandı ve çeşitli araçlar tarafından kullanılan JSON olarak serileştirilmiş tüm olay içerir. 

## <a name="enable-syslogconsumer"></a>SyslogConsumer etkinleştir

SyslogConsumer etkinleştirmek için kümenizin bir yükseltme gerçekleştirmek gerekir. `fabricSettings` Bölümü aşağıdaki kod ile güncelleştirilmesi gerekiyor. Bu kod yalnızca SyslogConsumer için ilgili bölümleri içerdiğini unutmayın

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

Duyurmak için değişiklikler aşağıda belirtilmiştir
1. Genel bölümünde, adlı yeni bir parametre yok `LinuxStructuredTracesEnabled`. **Bu, yapılandırılmış ve Syslog ile gönderildiğinde serileştirilmiş Linux olayları için gereklidir.**
2. Tanılama bölümünde, yeni ConsumerInstance: SyslogConsumer eklendi. Bu platform, başka bir tüketici olayların yoktur bildirir. 
3. Yeni bir bölüm olması gerekiyor SyslogConsumer `IsEnabled` olarak `true`. Local0 tesis otomatik olarak kullanmak üzere yapılandırılır. Başka bir parametre ekleyerek bunu geçersiz kılabilirsiniz.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="azure-monitor-logs-integration"></a>Azure İzleyici tümleştirmesi günlüğe kaydeder.
Bu Syslog olayları Azure İzleyici günlükleri gibi bir izleme Aracı'nda okuyabilirsiniz. Bu [yönergeleri] kullanarak Azure Marketi'nde kullanarak Log Analytics çalışma alanı oluşturabilirsiniz. (.. / azure-monitor/learn/quick-create-workspace.md) de Log Analytics aracısını toplayıp bu verileri çalışma alanına gönderme kümenize eklemeniz gerekir. Performans sayaçları toplamak için kullanılan aracının aynısı budur. 

1. Gidin `Advanced Settings` dikey penceresi

    ![Çalışma alanı ayarları](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Şuna tıklayın: `Data`
3. Şuna tıklayın: `Syslog`
4. Local0 izleme olanağı yapılandırın. İçinde fabricSettings değiştirdiyseniz, başka bir özelliği ekleyebilirsiniz.

    ![Syslog yapılandırma](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Baş üzerine tıklayarak sorgu Gezgini `Logs` çalışma kaynak menüsündeki sorgulamaya başlamak için

    ![Çalışma alanı günlükleri](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. Karşı sorgu `Syslog` aranırken tablo `ServiceFabric` ProcessName olarak. Aşağıdaki sorguyu bir olayda JSON Ayrıştır ve içeriğini görüntülemek nasıl örneğidir.

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Syslog sorgu](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

Yukarıdaki örnekte bir NodeDown olayıdır. Olayların tam listesini görüntüleyebileceğiniz [burada](service-fabric-diagnostics-event-generation-operational.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics aracısını dağıtmayı](service-fabric-diagnostics-oms-agent.md) üzerine düğümlerinizi performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı için günlükleri toplamak için
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri, Azure İzleyici günlüklerine bir parçası olarak sunulan
* [Azure İzleyici günlüklerine özel görünümlerini oluşturma için Görünüm Tasarımcısı'nı kullanın](../log-analytics/log-analytics-view-designer.md)
* Nasıl yapılır'için başvuru [Azure İzleyici, Syslog ile tümleştirme günlükleri](../log-analytics/log-analytics-data-sources-syslog.md).
