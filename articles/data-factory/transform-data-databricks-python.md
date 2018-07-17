---
title: Azure Databricks Python ile - verileri dönüştürme | Microsoft Docs
description: İşleme veya dönüştürme verileri Databricks Python çalıştırarak öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: fa19ff6e6293e1d5d082ee40d55b456500fcb1ad
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076486"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Azure Databricks üzerinde bir Python etkinliği çalıştırarak verileri dönüştürme

Azure Databricks Python etkinliğinde bir [Data Factory işlem hattı](concepts-pipelines-activities.md) bir Python dosyası, Azure Databricks kümesinde çalışır. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur.

## <a name="databricks-python-activity-definition"></a>Databricks Python etkinlik tanımı

Bir Databricks Python etkinliği örnek JSON tanımı aşağıda verilmiştir:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Databricks Python etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|---|---|---|
|ad|İşlem hattındaki bir etkinliğin adı.|Evet|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Python etkinliği için etkinlik DatabricksSparkPython türüdür.|Evet|
|linkedServiceName|Databricks bağlı Python etkinliğin çalıştığı hizmetin adı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.|Evet|
|pythonFile|Yürütülecek Python dosyası URI'si. Yalnızca DBFS yolları desteklenir.|Evet|
|parametreler|Python dosyasına geçirilecek komut satırı parametreleri. Bu dizeler dizisidir.|Hayır|
|Kitaplıkları|İşi yürütecek kümede yüklenecek kitaplıkların bir listesi. Bir dizi olabilir < dize, Nesne >|Hayır|

## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikler için desteklenen kitaplıkları

Yukarıdaki Databricks etkinlik tanımında Bu kitaplık türleri belirtin: *jar*, *Yumurta*, *maven*, *pypı*,  *cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "http://cran.us.r-project.org"
            }
        }
    ]
}

```

Daha fazla ayrıntı bakın [Databricks belgeleri](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) kitaplık türleri için.

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks kitaplıkta karşıya yükleme

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Databricks çalışma alanı kullanıcı arabirimini kullanarak](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Kullanıcı Arabirimi kullanılarak eklenen kitaplığı dbfs yolunu elde etmek için kullanabileceğiniz [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Genellikle Jar kitaplıkları dbfs altında depolanır: / FileStore/jar'lar kullanıcı arabirimini kullanarak. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs: / FileStore/jar dosyaları dışındaki* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Kopya kitaplığı Databricks CLI kullanma](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks fs cp SparkPi derleme 0.1.jar dbfs: / FileStore/jar dosyaları dışındaki*