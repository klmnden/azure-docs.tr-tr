---
title: Databricks not defteri ile - Azure veri dönüştürme | Microsoft Docs
description: Nasıl işleneceğini veya dönüştürme veri Databricks not defteri çalıştırarak öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: d4a57e45d5ddf55906fcf575df39135a227418ec
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Bir Databricks not defteri çalıştırarak veri dönüştürme

Azure Databricks dizüstü bilgisayar etkinliği bir [Data Factory işlem hattı](concepts-pipelines-activities.md) Azure Databricks çalışma alanınızda bir Databricks not defteri çalıştırır. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Azure Databricks Apache Spark çalıştırmak için yönetilen bir platformdur.

## <a name="databricks-notebook-activity-definition"></a>Databricks dizüstü etkinlik tanımı

Databricks dizüstü bilgisayar etkinliği örnek JSON tanımını şöyledir:

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
            }
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Databricks dizüstü etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|---|---|---|
|ad|İşlem hattında etkinlik adı.|Evet|
|açıklama|Etkinlik yaptığı açıklayan metin.|Hayır|
|type|Databricks dizüstü bilgisayar etkinliği için etkinlik DatabricksNotebook türüdür.|Evet|
|linkedServiceName|Databricks bağlı Databricks not defteri çalıştığı hizmetin adı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.|Evet|
|notebookPath|Not Defteri Databricks çalışma alanında çalıştırılması mutlak yolu. Bu yolu bir eğik çizgiyle başlamalıdır.|Evet|
|baseParameters|Anahtar-değer çiftleri dizisi. Temel parametreleri çalıştırın her etkinlik için kullanılabilir. Not Defteri belirtilmemiş bir parametre alırsa, not defteri varsayılan değerinden kullanılır. Parametreler hakkında daha fazla bulma [Databricks not defterlerini](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Hayır|
