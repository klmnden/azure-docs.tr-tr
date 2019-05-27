---
title: Azure Data Factory'de atlayan pencere Tetikleyicileri oluşturma | Microsoft Docs
description: Azure Data factory'de bir atlayan pencere üzerinde bir işlem hattı çalıştırmalarını, tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: shlo
ms.openlocfilehash: 6fbdee71ab1123c258a5191a78e38f51eb41cbab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66152926"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Bir atlayan pencere üzerinde bir işlem hattı çalıştırmalarını tetiği oluşturma
Bu makalede, oluşturmak, başlatmak ve bir atlayan pencere tetikleyicisi izlemek için adımları sağlar. Tetikleyiciler ve desteklenen türler hakkında genel bilgi için bkz. [işlem hattı yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md).

Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Atlayan pencere tetikleyicisi, işlem hattı ile bire bir ilişkisi vardır ve yalnızca tek bir işlem hattı başvurabilir.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Azure portalında bir atlayan pencere tetikleyicisi oluşturmak için seçin **Tetikleyici > atlayan Pencere > sonraki**ve ardından atlayan pencere tanımlayan özellikleri yapılandırın.

![Azure portalında bir atlayan pencere tetikleyicisi oluşturma](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>Atlayan pencere tetikleyicisi türü özellikleri
Bir atlayan pencere aşağıdaki tetikleyici türü özellikleri vardır:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "parameter1": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter2": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
            }
        }
    }
}
```

Aşağıdaki tabloda, ilgili yinelenmesi ve zamanlanmasıyla atlayan pencere tetikleyicisi olan ana JSON öğeleri üst düzey bir genel bakış sağlar:

| JSON öğesi | Açıklama | Tür | İzin verilen değerler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| **type** | Tetikleyici türü. Sabit değer "TumblingWindowTrigger." türüdür | String | "TumblingWindowTrigger" | Evet |
| **runtimeState** | Çalışma zamanı tetikleyicisinin geçerli durumu.<br/>**Not**: Bu öğe \<salt okunur >. | String | "Started" "Stopped" "Disabled" | Evet |
| **frequency** | Tetikleyicinin yineleneceği sıklık birimi (dakika veya saat) temsil eden bir dize. Varsa **startTime** tarih değerleri daha ayrıntılı **sıklığı** değeri **startTime** tarih, zaman penceresi sınırları hesaplanır değerlendirilir. Örneğin, varsa **sıklığı** değerdir, saatlik ve **startTime** değerdir 2017-09-01T10:10:10Z, ilk penceredir (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | String | "minute", "hour"  | Evet |
| **interval** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin, varsa **aralığı** 3'tür ve **sıklığı** "hour" olup tetikleyici 3 saatte bir yinelenir. | Integer | Pozitif bir tamsayı. | Evet |
| **startTime**| Geçmişte olabilen ilk örneğin. İlk tetikleyici aralığı (**startTime**, **startTime** + **aralığı**). | DateTime | Bir tarih saat değeri. | Evet |
| **endTime**| Geçmişte olabilen son a geçişi. | DateTime | Bir tarih saat değeri. | Evet |
| **gecikme** | Pencere için veri işleme, başlangıcı geciktirmek üzere süre miktarı. Beklenen yürütme süresi ve miktarını sonra işlem hattı çalıştırması başlatıldığında **gecikme**. **Gecikme** tetikleyici yeni bir çalıştırma tetiklemeden önce son süresini geçen bekleyeceği süreyi tanımlar. **Gecikme** penceresi değiştirmez **startTime**. Örneğin, bir **gecikme** 00:10:00 değerini 10 dakikalık bir gecikme anlamına gelir. | Timespan<br/>(ss)  | Varsayılan değer 00:00:00 olduğu bir timespan değeri. | Hayır |
| **maxConcurrency** | Windows için hazır olan tetikleyen eş zamanlı tetikleyici çalıştırmaları sayısı. Örneğin, arka dolgusu için 24 Windows dün sonuçları için saatlik çalıştırılır. Varsa **maxConcurrency** = 10, tetikleyici yalnızca ilk 10 windows için olay harekete geçirildi (00:00-01:00 - 09:00-10:00). İlk 10 tetiklenen işlem hattı çalıştırması tamamlandıktan sonra tetikleyici çalıştırmaları sonraki 10 windows (10:00-11:00-19:00-20:00) tetiklenir. Bu örnek ile devam etmeden **maxConcurrency** 10 windows 10 toplam işlem hattı çalıştırma hazır olduğunda, 10 =. Yoktur, yalnızca 1 penceresi hazır da yalnızca 1 işlem hattı çalıştırması yok. | Integer | 1 ile 50 arasında bir tamsayı. | Evet |
| **retryPolicy: Sayısı** | "Başarısız" işaretlenmiş işlem hattı çalıştırmasını önce yeniden deneme sayısı  | Integer | Varsayılan değer 0 (yeniden deneme yok) olduğu bir tamsayı. | Hayır |
| **retryPolicy: İntervalınseconds** | Saniye cinsinden belirtilen yeniden deneme girişimleri arasındaki gecikme. | Integer | Varsayılan değer 30 olduğu saniye sayısı. | Hayır |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart ve WindowEnd sistem değişkenleri

Kullanabileceğiniz **WindowStart** ve **WindowEnd** atlayan pencere tetikleyicisi sistem değişkenlerinin, **işlem hattı** tanımı (diğer bir deyişle, bir sorgunun parçası için). Sistem değişkenleri parametre olarak, işlem hattınızda geçirmek **tetikleyici** tanımı. Aşağıdaki örnek bu değişkenleri parametreler gösterilmektedir:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "MyWindowStart": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "MyWindowEnd": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                }
            }
        }
    }
}
```

Kullanılacak **WindowStart** ve **WindowEnd** sistem değişken değerleri işlem hattı tanımındaki "MyWindowStart" ve "MyWindowEnd" parametreleri uygun şekilde kullanın.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Windows doldurma senaryoda yürütme sırası
Yürütme (özellikle bir doldurma senaryosunda) için birden çok windows olduğunda, windows için yürütme sırasını Yeniye aralıkları belirleyici,. Şu anda bu davranışın değiştirilmesi mümkün değildir.

### <a name="existing-triggerresource-elements"></a>Varolan TriggerResource öğeleri
Aşağıdaki noktaları varolan uygulama **TriggerResource** öğeleri:

* Varsa değerini **sıklığı** öğenin (veya pencere boyutu) tetikleyici değişikliklerin, önceden işlenmiş windows durumudur *değil* sıfırlayın. Tetikleyici yeni pencere boyutunu kullanarak yürütülen son penceresinden Windows ateşlenmesine devam eder.
* Varsa değerini **endTime** zaten işlendi (eklenmiş veya güncelleştirilmiştir) tetikleyici değişikliklerin, Windows'un durumunu öğesidir *değil* sıfırlayın. Tetikleyici yeni geliştirir **endTime** değeri. Varsa yeni **endTime** değerdir zaten çalıştırılan windows önce tetikleyici durdurur. Aksi takdirde, tetikleyici olmadığında duruyor yeni **endTime** değer ile karşılaştı.

## <a name="sample-for-azure-powershell"></a>Azure PowerShell ile ilgili örnek

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölüm oluşturmak için başlatın ve bir tetikleyici izlemek için Azure PowerShell kullanmayı gösterir.

1. Adlı bir JSON dosyası oluşturun **MyTrigger.json** C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > JSON dosyasını kaydetmeden önce ayarlayın **startTime** geçerli UTC saati için öğesi. Değerini **endTime** geçerli UTC saatine son bir saat için öğesi.

    ```json
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```

2. Bir tetikleyici kullanarak oluşturma **kümesi AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. Tetikleyici durumunu olduğundan emin olun **durduruldu** kullanarak **Get-AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Kullanarak tetikleyiciyi başlatın **başlangıç AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Tetikleyici durumunu olduğundan emin olun **başlatıldı** kullanarak **Get-AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. Tetikleyici çalıştırmaları Azure PowerShell kullanarak get **Get-AzDataFactoryV2TriggerRun** cmdlet'i. Tetikleyici çalıştırmaları hakkında bilgi almak için düzenli aralıklarla aşağıdaki komutu yürütün. Güncelleştirme **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** değerleri, tetikleyici tanımında eşleştirmek için değerler:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Tetikleyici çalıştırmaları ve işlem hattı izlemek için Azure portalında çalışır, bkz: [işlem hattı çalıştırmalarını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz. [işlem hattı yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
