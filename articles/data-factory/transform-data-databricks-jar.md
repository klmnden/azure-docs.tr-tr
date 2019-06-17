---
title: Databricks Jar - Azure ile verileri dönüştürme | Microsoft Docs
description: İşleme veya dönüştürme verileri Databricks jar dosyasını çalıştırarak öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/15/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: d299a785d50657ef40c0c49cb2dce33b8939fd02
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860998"
---
# <a name="transform-data-by-running-a-jar-activity-in-azure-databricks"></a>Azure Databricks'te bir Jar etkinliği çalıştırarak verileri dönüştürme

Azure Databricks etkinlik Jar bir [Data Factory işlem hattı](concepts-pipelines-activities.md) Jar Spark, Azure Databricks kümesinde çalışır. Bu makalede yapılar [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-jar-activity-definition"></a>Databricks Jar etkinlik tanımı

Bir Databricks etkinlik Jar örnek JSON tanımı aşağıda verilmiştir:

```json
{
    "name": "SparkJarActivity",
    "type": "DatabricksSparkJar",
    "linkedServiceName": {
        "referenceName": "AzureDatabricks",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mainClassName": "org.apache.spark.examples.SparkPi",
        "parameters": [ "10" ],
        "libraries": [
            {
                "jar": "dbfs:/docs/sparkpi.jar"
            }
        ]
    }
}

```

## <a name="databricks-jar-activity-properties"></a>Databricks Jar etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|:--|---|:-:|
|name|İşlem hattındaki bir etkinliğin adı.|Evet|
|description|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|türü|Databricks Jar etkinliği için etkinlik DatabricksSparkJar türüdür.|Evet|
|linkedServiceName|Databricks bağlı Jar etkinliğin çalıştığı hizmetin adı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.|Evet|
|mainClassName|Yürütülecek main metodunu içeren sınıfın tam adı. Bu sınıf kitaplığı olarak sağlanan bir JAR bulunması gerekir.|Evet|
|parametreler|Main yöntemi için geçirilecek parametreler.  Bu dizeler dizisidir.|Hayır|
|Kitaplıkları|İşi yürütecek kümede yüklenecek kitaplıkların bir listesi. Bir dizi olabilir < dize, Nesne >|Evet (en az bir mainClassName yöntemi içeren)|

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
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Daha fazla ayrıntı bakın [Databricks belgeleri](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) kitaplık türleri için.

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks kitaplıkta karşıya yükleme

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Databricks çalışma alanı kullanıcı arabirimini kullanarak](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Kullanıcı Arabirimi kullanılarak eklenen kitaplığı dbfs yolunu elde etmek için kullanabileceğiniz [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Genellikle Jar kitaplıkları dbfs altında depolanır: / FileStore/jar'lar kullanıcı arabirimini kullanarak. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs: / FileStore/iş-jar dosyaları dışındaki* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Kopya kitaplığı Databricks CLI kullanma](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)
Databricks CLI kullanma [(yükleme adımlarını)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Örnek - dbfs için JAR kopyalanıyor: *dbfs cp SparkPi derleme 0.1.jar dbfs:/docs/sparkpi.jar*
