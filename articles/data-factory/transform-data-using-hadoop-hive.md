---
title: Hadoop Hive etkinliği Azure Data Factory kullanarak verileri dönüştürme | Microsoft Docs
description: Hive etkinliği Azure data factory'de bir üzerinde-istek/bilgisayarınızı kendi HDInsight kümesinde Hive sorguları çalıştırmak için nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 3852b2d18b48be63cbc612159facb6273f23dc2b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60848120"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Hadoop Hive etkinliği Azure Data Factory kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-hive-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-hive.md)

HDInsight Hive etkinliği bir Data factory'de [işlem hattı](concepts-pipelines-activities.md) üzerinde Hive sorguları yürüten [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [üzerine](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümesi. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](introduction.md) yapıp [öğretici: verileri dönüştürme](tutorial-transform-data-spark-powershell.md) bu makaleyi okuduktan önce. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```
## <a name="syntax-details"></a>Söz dizimi ayrıntıları
| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| name                | Etkinliğin adı                                         | Evet      |
| description         | Etkinliğin ne için kullanıldığını açıklayan metin                | Hayır       |
| türü                | Hive etkinliği için Hdınsighthive etkinliği türüdür        | Evet      |
| linkedServiceName   | Data Factory öğesinde bağlantılı hizmet olarak HDInsight kümesine başvuru kayıtlı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| scriptLinkedService | Bir Azure depolama bağlı hizmeti başvuru yürütülecek Hive betiğini depolamak için kullanılır. Bu bağlı hizmeti belirtmezseniz, Azure depolama bağlı HDInsight bağlı hizmette tanımlanan hizmeti kullanılır. | Hayır       |
| ScriptPath          | ScriptLinkedService tarafından başvurulan Azure storage'da depolanan betik dosyasının yolunu belirtin. Dosya adı büyük/küçük harfe duyarlıdır. | Evet      |
| getDebugInfo        | Günlük dosyaları Azure depolama için ne zaman kopyalanır belirtir HDInsight küme tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: Yok. | Hayır       |
| arguments           | Hadoop işi için bağımsız değişkenleri dizisini belirtir. Bağımsız değişkenleri, her görev için komut satırı bağımsız değişkenleri geçirilir. | Hayır       |
| defines             | Parametreler içinde Hive betiğine başvurmak için anahtar/değer çiftleri belirtin. | Hayır       |
| queryTimeout        | Zaman aşımı değerini (dakika cinsinden) sorgulayın. HDInsight küme Kurumsal güvenlik paketi etkin olduğunda uygulanabilir. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
