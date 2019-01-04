---
title: Databricks Not - Azure ile verileri dönüştürme | Microsoft Docs
description: Bir Databricks not defteri çalıştırarak işleme veya dönüştürme veri öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: 8ab6dad36bf47430a925d21ca2464286e7e70002
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022079"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Bir Databricks not defteri çalıştırarak verileri dönüştürme

Azure Databricks not defteri etkinliği içinde bir [Data Factory işlem hattı](concepts-pipelines-activities.md) Azure Databricks çalışma alanınızda bir Databricks not defteri çalıştırır. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur.

## <a name="databricks-notebook-activity-definition"></a>Databricks not defteri etkinliği tanımı

Bir Databricks not defteri etkinliğini örnek JSON tanımı aşağıda verilmiştir:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Databricks not defteri etkinliği özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gereklidir|
|---|---|---|
|ad|İşlem hattındaki bir etkinliğin adı.|Evet|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks not defteri etkinliği için etkinlik DatabricksNotebook türüdür.|Evet|
|linkedServiceName|Databricks bağlı Databricks not defteri çalıştığı hizmetin adı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.|Evet|
|notebookPath|Databricks çalışma alanınızda çalıştırılması için Not defterini mutlak yolu. Bu yol, eğik çizgi ile başlamalıdır.|Evet|
|baseParameters|Anahtar-değer çiftleri dizisi. Temel parametreleri her etkinlik için kullanılabilir. Not defterini belirtilmemiş bir parametre alırsa, not defterindeki varsayılan değer kullanılır. Parametreler hakkında daha fazla bilgi edinin [Databricks not defterlerini](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Hayır|
|Kitaplıkları|İşi yürütecek kümede yüklenecek kitaplıkların bir listesi. Bir dizi olabilir \<dize, Nesne >.|Hayır|


## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikler için desteklenen kitaplıkları

Yukarıdaki Databricks etkinlik tanımında, bu kitaplık türleri belirtin: *jar*, *Yumurta*, *maven*, *pypı*,  *cran*.

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

Daha fazla ayrıntı için [Databricks belgeleri](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) kitaplık türleri için.

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks kitaplıkta karşıya yükleme

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Databricks çalışma alanı kullanıcı arabirimini kullanarak](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Kullanıcı Arabirimi kullanılarak eklenen kitaplığı dbfs yolunu elde etmek için kullanabileceğiniz [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Genellikle, Jar kitaplıkları dbfs altında depolanır: / FileStore/jar'lar kullanıcı arabirimini kullanarak. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs: / FileStore/jar dosyaları dışındaki*.



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Kopya kitaplığı Databricks CLI kullanma](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks fs cp SparkPi derleme 0.1.jar dbfs: / FileStore/jar dosyaları dışındaki*
