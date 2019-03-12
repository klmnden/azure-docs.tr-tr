---
title: Azure Data Factory'de atlayan pencere tetikleyicisi Bağımlılıklar Oluştur | Microsoft Docs
description: Azure Data factory'de bir atlayan pencere tetikleyicisi üzerinde bağımlılık oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: f3a547f5b953262d7263d1be0897161cf4091a1d
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57574398"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Atlayan pencere tetikleyici bağımlılığı oluşturma

Bu makalede bir atlayan pencere tetikleyicisi üzerinde bir bağımlılık oluşturmak için adımları sağlar. Atlayan pencere tetikleyicileri hakkında genel bilgi için bkz: [atlayan pencere tetikleyicisi oluşturma](how-to-create-tumbling-window-trigger.md).

Bağımlılık zinciri oluşturun ve bir tetikleyici yalnızca data factory'de başka bir tetikleyici başarılı yürütme sonrasında yürütülür emin olmak amacıyla bu gelişmiş özellik bir atlayan pencere bağımlılık oluşturmak için kullanın.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Data Factory kullanıcı Arabiriminde bir bağımlılık oluşturun

Bir tetikleyici üzerinde bağımlılık oluşturmak için seçin **Tetikleyici > Gelişmiş > Yeni**, uygun uzaklığı ve boyutu ile bağlı tetikleyiciyi seçin. Seçin **son** ve etkili olması bağımlılıklar için veri fabrikası değişiklikleri yayımlayın.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png)

## <a name="tumbling-window-dependency-properties"></a>Atlayan pencere bağımlılık özellikleri

Atlayan pencere tetikleyicisi bağımlılık aşağıdaki özelliklere sahiptir:

```json
{
    "name": "DemoTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00.000Z",
            "delay": "00:00:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": "-02:00:00",
                    "offset": "-01:00:00",
                    "referenceTrigger": {
                        "referenceName": "DemoDependency1",
                        "type": "TriggerReference"
                    }
                }
            ]
        }
    }
}
```

Aşağıdaki tabloda atlayan pencere bağımlılık tanımlamak için gerekli öznitelik listesini sağlar.

| **Özellik adı** | **Açıklama**  | **Tür** | **Gerekli** |
|---|---|---|---|
| Tetikleyici  | Tüm mevcut atlayan pencere tetikleyicileri, aşağı bu düşüş görüntülenir. Bağımlılık gerçekleştirilecek Tetikleyici seçin.  | TumblingWindowTrigger | Evet |
| Uzaklık | Bağımlılık tetikleyici uzaklığı. Saat aralık biçiminde bir değer sağlayın ve hem negatif hem pozitif uzaklıkları izin verilir. Tetikleyici kendisine bağlı olarak ve diğer tüm durumlarda ise isteğe bağlı bu parametre zorunludur. Kendi kendine bağımlılık her zaman negatif bir uzaklık olmalıdır. | Timespan | Kendi kendine bağımlılık: Diğer Evet: Hayır |
| Pencere boyutu | Bağımlılık atlayan pencere boyutu. Saat aralık biçiminde bir değer girin. Bu parametre isteğe bağlıdır. | Timespan | Hayır  |
|||||

## <a name="tumbling-window-self-dependency-properties"></a>Atlayan pencere self bağımlılık özellikleri

Önceki pencere başarıyla tamamlanana kadar burada tetikleyici sonraki pencereye devam etmemelisiniz senaryolarda, bir kendi kendini bağımlılık oluşturun. Kendi kendine bağımlılık tetikleyici özellikleri vardır:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```

Bağımlılık özellikleri çizimleri senaryoları ve atlayan pencere kullanımını aşağıda verilmiştir.

## <a name="dependency-offset"></a>Bağımlılık uzaklığı

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png)

## <a name="dependency-size"></a>Bağımlılık boyutu

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png)

## <a name="self-dependency"></a>Kendi kendine bağımlılık

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png)

## <a name="usage-scenarios"></a>Kullanım senaryoları

### <a name="dependency-on-another-tumbling-window-trigger"></a>Bağımlı başka bir atlayan pencere tetikleyicisi

Örneğin, başka bir günlük iş son yedi gün toplayarak bağlı olarak günlük bir telemetri işleme iş çıkış ve yedi günlük toplama penceresi akışları oluşturur:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png)

### <a name="dependency-on-itself"></a>Kendisine bağımlı

Çıkış akışları işin hiçbir boşlukları günlük işlemiyle:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png)

## <a name="monitor-dependencies"></a>Bağımlılıklarını İzle

Bağımlılık zinciri izleyebilirsiniz ve karşılık gelen windows tetikleyiciden izleme sayfası çalıştırın. Gidin **İzleme > tetikleme çalıştırmalarını**.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png)

Tüm Seçili pencerenin bağımlı tetikleyici çalıştırmalarını görüntülemek için unz büyütece tıklayın.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png)

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [atlayan pencere tetikleyicisi oluşturma](how-to-create-tumbling-window-trigger.md).