---
title: Azure Data Factory'de dönen pencere Tetikleyicileri oluşturma | Microsoft Docs
description: Azure veri fabrikasında ardışık atlayan pencere üzerinde çalışan bir tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: 312072a5de21ff1c6b602fed93b77c564b15a9f1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Bir işlem hattı atlayan pencere üzerinde çalışan bir Tetikleyici oluşturma
Bu makalede oluşturma, başlatma ve dönen pencere tetikleyici izlemek için adımları sağlar. Tetikleyiciler ve desteklenen türlerden hakkında genel bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Azure Data Factory genel olarak kullanılabilir (GA) olan sürüm 1, kullanıyorsanız, bkz: [sürüm 1 Azure Data Factory ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Dönen pencere tetikleyici bir ardışık düzen ile bire bir ilişkisi vardır ve yalnızca tekil bir ardışık düzen başvuruda bulunabilir.

## <a name="tumbling-window-trigger-type-properties"></a>Dönen pencere tetikleyici türü özellikleri
Atlayan pencere tetikleyici türü özellikleri şunlardır:

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
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
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

Aşağıdaki tabloda, yineleme ile ilgili ve zamanlama dönen pencere tetikleyici temel JSON öğe üst düzey bir genel bakış sağlar:

| JSON öğesi | Açıklama | Tür | İzin verilen değerler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| **Türü** | Tetikleyici türü. Sabit değer "TumblingWindowTrigger." türüdür | Dize | "TumblingWindowTrigger" | Evet |
| **runtimeState** | Çalışma zamanı tetikleyici geçerli durumu.<br/>**Not**: Bu öğe \<readOnly >. | Dize | "Başlatıldı" "Stopped" "Disabled" | Evet |
| **frequency** | Tetikleyici yineleneceği sıklığı (dakika veya birimi saatleri) temsil eden bir dize. Varsa **startTime** tarih değerlerini daha ayrıntılı **sıklığı** değeri **startTime** tarih, zaman penceresi sınırları hesaplanan değerlendirilir. Örneğin, varsa **sıklığı** değerdir saatlik ve **startTime** değerdir 2016-04-01T10:10:10Z, ilk penceredir (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | Dize | "dakika", "saat"  | Evet |
| **interval** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin, varsa **aralığı** 3 ve **sıklığı** "," saattir 3 saatte bir tetikleyici yinelenen. | Tamsayı | Pozitif bir tamsayı. | Evet |
| **startTime**| Geçmişte olabilen ilk örneğin. İlk tetikleyici aralığı (**startTime**, **startTime** + **aralığı**). | DateTime | Bir tarih saat değeri. | Evet |
| **endTime**| Geçmişte olabilen son a geçişi. | DateTime | Bir tarih saat değeri. | Evet |
| **gecikme** | Veri işleme penceresi başlangıcı geciktirmek için süre miktarı. Çalıştırma ardışık düzen beklenen yürütme süresi ve miktarını sonra başlatıldığında **gecikme**. **Gecikme** tetikleyici yeni çalıştırma tetiklemeden önce son süresini geçen bekleyeceği süreyi tanımlar. **Gecikme** penceresi değiştirmez **startTime**. Örneğin, bir **gecikme** 00:10:00 değerini 10 dakikalık bir gecikme anlamına gelir. | Timespan  | Varsayılan değer 00:00:00 olduğu bir saat değeri. | Hayır |
| **maxConcurrency** | Hazır olan windows harekete eşzamanlı tetikleyici çalıştırmalarının sayısı. Örneğin, dolgu yedeklemek için 24 Windows dün sonuçlar için saatlik çalışır. Varsa **maxConcurrency** = 10, olaylar yalnızca ilk 10 windows için tetikleyici (00:00-01:00 - 09:00-10:00). İlk 10 tetiklenen ardışık düzen çalıştırır tamamlandıktan sonra tetikleyici çalıştırır sonraki 10 windows (10:00-11:00-19:00-20:00) tetiklenir. Bu örnekle devam edersek **maxConcurrency** = 10, 10 windows 10 toplam ardışık düzen çalıştırır vardır, hazır olduğunda. Varsa yalnızca 1 penceresi hazır, da yalnızca 1 ardışık düzen Çalıştır yoktur. | Tamsayı | 1 ve 50 arasında bir tamsayı. | Evet |
| **retryPolicy: sayısı** | "Başarısız" işaretlenmiş ardışık düzen Çalıştır önce yeniden deneme sayısı  | Tamsayı | Varsayılan değer 0 (yeniden deneme) bulunduğu bir tamsayı. | Hayır |
| **retryPolicy: Intervalınseconds** | Saniye cinsinden belirtilen yeniden deneme girişimleri arasında gecikme. | Tamsayı | Varsayılan değer 30 olduğu saniye sayısı. | Hayır |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart ve WindowEnd sistem değişkenleri

Kullanabileceğiniz **WindowStart** ve **WindowEnd** dönen pencere tetikleyicinin sistem değişkenleri, **ardışık düzen** tanımı (diğer bir deyişle, bir sorgu parçası için). Sistem değişkenleri ardışık düzeninde için parametreler olarak geçirin **tetikleyici** tanımı. Aşağıdaki örnekte bu değişkenleri parametreler gösterilmektedir:

```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
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

Kullanılacak **WindowStart** ve **WindowEnd** sistem değişken değerleri ardışık düzen tanımı'ndaki "MyWindowStart" ve "MyWindowEnd" parametreleri uygun şekilde kullanın.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Windows doldurma senaryosunda uygulanma sırası
Yürütme (doldurma senaryoda özellikle) için birden çok windows olduğunda, windows için yürütme sırasını yeni için eski aralıklarını belirleyici,. Şu anda bu davranışı değiştirilemez.

### <a name="existing-triggerresource-elements"></a>Varolan TriggerResource öğeleri
Varolan aşağıdaki noktaları geçerli **TriggerResource** öğeleri:

* Varsa değeri **sıklığı** öğenin (veya pencere boyutu) tetikleyici değişiklikleri, önceden işlenmiş Windows'un durumda *değil* sıfırlayın. Tetikleyici yeni pencere boyutunu kullanılarak gerçekleştirilen son penceresinden windows için yangın devam eder.
* Varsa değeri **endTime** önceden işlenmiş (eklendi veya güncelleştirildi) tetikleyici değişiklikler, Windows'un durumunu öğesidir *değil* sıfırlayın. Tetikleyici yeni geliştirir **endTime** değeri. Varsa yeni **endTime** değerdir zaten çalıştırılan windows önce tetikleyici durdurur. Aksi takdirde, tetikleyici ne zaman durdurur yeni **endTime** değeriyle karşılaştı.

## <a name="sample-for-azure-powershell"></a>Azure PowerShell örnek
Bu bölümde Azure PowerShell oluşturma, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir.

1. Adlı bir JSON dosyası oluşturun **MyTrigger.json** C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip:

   > [!IMPORTANT]
   > JSON dosyasının kaydetmeden önce değerini ayarlamak **startTime** öğesi geçerli UTC saati için. Değerini **endTime** öğesi geçerli UTC saati geçmiş bir saat.

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

2. Kullanarak bir Tetikleyici oluşturma **kümesi AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. Tetikleyici durumunu onaylayın **durduruldu** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Tetikleyici başlamayı **başlangıç AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Tetikleyici durumunu onaylayın **başlatıldı** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. Tetikleyici kullanarak Azure PowerShell'de çalışan get **Get-AzureRmDataFactoryV2TriggerRun** cmdlet'i. Tetikleyici çalışmaları hakkında bilgi almak için düzenli aralıklarla aşağıdaki komutu yürütün. Güncelleştirme **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** tetikleyici tanımınızı değerler eşleşecek şekilde değerler:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Tetikleyici çalıştırır ve işlem hattını izlemek için Azure portalında çalıştırır, bkz: [işlem hattını izleme çalıştıran](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
