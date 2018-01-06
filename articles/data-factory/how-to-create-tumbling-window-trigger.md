---
title: "Azure Data Factory'de dönen pencere Tetikleyicileri oluşturma | Microsoft Docs"
description: "Azure veri fabrikasında ardışık atlayan pencere üzerinde çalışan bir tetikleyici oluşturmayı öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Bir işlem hattı atlayan pencere üzerinde çalışan bir Tetikleyici oluşturma
Bu makalede oluşturma, başlatma ve dönen pencere tetikleyici izlemek için adımları sağlar. Tetikleyiciler ve destekliyoruz türleri hakkında genel bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Dönen pencere Tetikleyicileri durumu korurken ek olarak, belirtilen başlangıç saati düzenli zaman aralığından sırasında ateşlenir tetikleyici türüdür. Dönen windows sabit boyutlu çakışmayan ve bitişik zaman aralıkları bir dizi var. Dönen pencere tetikleyici bir ardışık düzen ile 1:1 ilişkisi vardır ve yalnızca tekil bir ardışık düzen başvuruda bulunabilir.

## <a name="tumbling-window-trigger-type-properties"></a>Dönen pencere tetikleyici türü özellikleri
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

Aşağıdaki tabloda temel öğe dönen pencere tetikleyici zamanlama ve yineleme için ilgili üst düzey bir genel bakış sağlar.

| **JSON adı** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
|:--- |:--- |:--- |:--- |
| **türü** | Tetikleyici türü. Bu "TumblingWindowTrigger" sabit | Dize | Evet |
| **runtimeState** | <readOnly>Olası değerler: Başlatıldı, durduruldu, devre dışı | Dize | Evet, salt okunur |
| **Sıklık** |*Sıklığı* tetikleyici yineleneceği sıklığı birim temsil eden dize. "Dakika" ve "saat" desteklenen değerler: Başlangıç saati sıklığından daha ayrıntılı tarih kısımlarını varsa, bunlar dikkate penceresi sınırları hesaplamak için alınır. İçin örn: sıklığı saatlik başlangıç saati 2016 ise-04-01T10:10:10Z, ilk penceredir (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z.)  | Dize. Desteklenen türler "dakika", "saat" | Evet |
| **aralığı** |*Aralığı* pozitif bir tamsayı olan ve aralığını gösterir *sıklığı* , tetikleyici ne sıklıkla çalışacağını belirler. Örneğin, varsa *aralığı* 3 ve *sıklığı* "saat", tetikleyici 3 saatte yinelenen. | Tamsayı | Evet |
| **startTime**|*startTime* bir tarih-saat değil. *startTime* ilk geçtiği ve geçmiş olabilir. İlk tetikleyici aralığı (startTime, startTime + aralığı) olacaktır. | Tarih Saat | Evet |
| **endTime**|*endTime* bir tarih-saat değil. *endTime* son a geçişi ve geçmiş olabilir. | Tarih Saat | Evet |
| **gecikme** | Veri işleme penceresini başlatır gecikme belirtin. Ardışık Düzen çalıştırmak, beklenen yürütme süresi + gecikme sonra başlatılır. Gecikme tanımlar nasıl tetikleyici vadesi geçmiş bekleyeceğini yeni çalıştırmak tetiklemeden önce zaman. Penceresi başlangıç zamanı değiştirmez. | TimeSpan (örnek: 00:10:00, 10 dakika gecikme anlamına gelir) |  Hayır. Varsayılan değer "00: 00:00" |
| **max eşzamanlılık** | Hazır olan windows harekete eşzamanlı tetikleyici çalıştırmalarının sayısı. Örnek: biz doldurma için saatlik dün için çalışıyorsanız, 24 windows olacaktır. Varsa eşzamanlılık = 10, olaylar yalnızca ilk 10 windows için tetikleyici (00:00-01:00 - 09:00-10:00). İlk 10 tetiklenen ardışık düzen çalıştırır tamamlandıktan sonra tetikleyici çalıştırır sonraki 10 (10:00-11:00-19:00-20:00) tetiklenir. Eşzamanlılık örneği ile devam etmeden = 10, 10 windows 10 ardışık düzen çalışması olacaktır hazır olduğunda. Varsa yalnızca 1 penceresi hazır, yalnızca olacaktır 1 ardışık düzen Çalıştır. | Tamsayı | Evet. Olası değerler 1-50 |
| **retryPolicy: sayısı** | "Başarısız" işaretlenmiş ardışık düzen Çalıştır önce yeniden deneme sayısı  | Tamsayı |  Hayır. Varsayılan değer 0 yeniden deneme |
| **retryPolicy: Intervalınseconds** | Yeniden deneme girişimleri arasında saniye cinsinden gecikme | Tamsayı |  Hayır. Varsayılan değer 30 saniyedir |

### <a name="using-system-variables-windowstart-and-windowend"></a>Sistem değişkenleri kullanma: WindowStart ve WindowEnd

WindowStart ve dönen pencere tetikleyicinin WindowEnd kullanmak isterseniz, **ardışık düzen** tanımı (yani parçası olarak bir sorgu için), değişkenleri olarak parametreler, ardışık düzene geçirdiğiniz gerekir **tetikleyici**tanımı sözlüğüdür:
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

Ardından ardışık düzen tanımında WindowStart ve WindowEnd değerleri kullanmak için parametrelerinizi uygun şekilde kullanın "MyWindowStart" ve "MyWindowEnd"

### <a name="notes-on-backfill"></a>Doldurma ile ilgili notlar
Yürütme (esp. doldurma senaryoda) için birden çok windows olduğunda, Windows'un yürütme sırasını kararlıdır ve en yeni için eski aralıklarını olacaktır. Şimdi itibariyle bu davranışı değiştirmek için bir yolu yoktur.

### <a name="updating-an-existing-triggerresource"></a>Varolan bir TriggerResource güncelleştiriliyor
* Tetikleyici sıklığı (veya pencere boyutu) değiştirdiyseniz zaten işlenen windows durum *değil* sıfırlayın. Tetikleyici yeni pencere boyutunu kullanılarak yürütülen sonuncu Windows'dan tetikleme devam eder.
* Bitiş zamanı (eklendi veya güncelleştirildi) tetikleyici değişiklikleri, zaten windows durumunu işlediğinde *değil* sıfırlayın. Tetikleyici yalnızca yeni bitiş saati dokunmaz. Bitiş saati zaten yürütülen windows önce ise, tetikleyici durdurur. Aksi takdirde, yeni bitiş saati karşılaşıldığında durur.

## <a name="sample-using-azure-powershell"></a>Azure PowerShell kullanarak örnek
Bu bölümde Azure PowerShell oluşturma, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir.

1. C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip MyTrigger.json adlı bir JSON dosyası oluşturun:

   > [!IMPORTANT]
   > Ayarlama **startTime** geçerli UTC saati için ve **endTime** geçerli UTC geçmiş bir saat için JSON dosyayı kaydetmeden önce zaman.

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
2. Kullanarak bir Tetikleyici oluşturma **kümesi AzureRmDataFactoryV2Trigger** cmdlet'i.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
4. Tetikleyici başlamayı **başlangıç AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
5. Tetikleyici durumunu onaylayın **başlatıldı** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet'i.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
6.  PowerShell kullanarak tetikleyici çalıştırır alma **Get-AzureRmDataFactoryV2TriggerRun** cmdlet'i. Tetikleyici çalışmaları hakkında bilgi almak için aşağıdaki komutu düzenli aralıklarla çalıştırın: güncelleştirme **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** tetikleyici tanımında değerleriyle eşleşecek şekilde değerleri .

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
Tetikleyici çalıştırır/ardışık çalıştığında Azure portalında izlemek için bkz: [işlem hattını izleme çalıştırır](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
