---
title: Bağlı hizmetler Azure Data factory'de | Microsoft Docs
description: Bağlı hizmetler Data factory'de hakkında bilgi edinin. Bağlı hizmetler data factory'de işlem/veri depoları bağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: shlo
ms.openlocfilehash: ba2041495e1e3c63ee322a0b748753ad6cb68914
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870140"
---
# <a name="linked-services-in-azure-data-factory"></a>Azure veri fabrikasında bağlı hizmetler
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-create-datasets.md)
> * [Geçerli sürüm](concepts-datasets-linked-services.md)

İçinde kullanılan nasıl Azure Data Factory işlem hatları ve JSON biçiminde nasıl tanımlandığına hangi bağlı Hizmetleri, bu makalede açıklanır.

Data Factory kullanmaya yeni başladıysanız bkz [Azure Data Factory'ye giriş](introduction.md) genel bakış.

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. A **işlem hattı** mantıksal bir gruplandırmasıdır **etkinlikleri** birlikte gerçekleştiren bir görev. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, verileri bir şirket içi SQL Server'dan Azure Blob depolama alanına kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Ardından, verileri işlemek için çıkış verileri üretmek üzere Blob depolama alanından gönderilmiş olan bir Azure HDInsight kümesinde bir Hive betiği çalıştıran bir Hive etkinliği kullanabilirsiniz. Son olarak, çıktı verilerini Azure SQL veri ambarı'na çözümleri oluşturulur hangi iş zekası raporlama en üstünde (BI) kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz. İşlem hatları ve etkinlikler hakkında daha fazla bilgi için bkz: [işlem hatları ve etkinlikler](concepts-pipelines-activities.md) Azure Data factory'de.

Artık, bir **veri kümesi** yalnızca işaret eden veya kullanmak istediğiniz verilere başvuran verilerin adlandırılmış bir görünümüdür olduğundan, **etkinlikleri** girdi ve çıktı olarak.

Bir veri kümesi oluşturmadan önce oluşturmanız gerekir bir **bağlı hizmet** data factory'de veri deponuza bağlamak için. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu, bu şekilde düşünün; veri kümesi bağlı veri depolarındaki veri yapısını temsil eder ve bağlı hizmet, veri kaynağına bağlantı tanımlar. Örneğin, bir Azure depolama bir depolama hesabını veri fabrikasına bağlı hizmeti. Bir Azure Blob veri kümesi blob kapsayıcıyı ve işlenecek giriş bloblarını içeren Azure depolama hesap dahilindeki klasörü temsil eder.

Örnek senaryo aşağıda verilmiştir. Verileri Blob depolama alanından SQL veritabanına kopyalamak için iki bağlı hizmet oluşturursunuz: Azure depolama ve Azure SQL veritabanı. Ardından, iki veri kümesi oluşturursunuz: (Azure depolama bağlı hizmetini ifade eder) azure Blob veri kümesi ve Azure SQL tablosu veri kümesi (Bu, Azure SQL veritabanı bağlı hizmetini ifade eder). Azure depolama ve Azure SQL veritabanı bağlı hizmeti, Data Factory, Azure depolama ve Azure SQL veritabanı, sırasıyla bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi blob kapsayıcısı ve Blob Depolama alanınızda giriş bloblarını içeren blob klasörü belirtir. Azure SQL tablosu veri kümesi, verilerin kopyalanacağı olduğu SQL veritabanınızda SQL tablosunu belirtir.

Aşağıdaki diyagramda, Data Factory'de işlem hattı, etkinlik, veri kümesi ve bağlı hizmet arasındaki ilişkiler gösterilmektedir:

![İşlem hattı, etkinlik, veri kümesi, bağlı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>JSON bağlantılı hizmeti
Bağlı hizmet, Data Factory JSON biçiminde şu şekilde tanımlanır:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanmaktadır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Bağlı hizmetin adı. Bkz: [Azure Data Factory - adlandırma kuralları](naming-rules.md). |  Evet |
türü | Bağlı hizmet türü. Örneğin: (Veri deposu) AzureStorage veya AzureBatch (işlem). TypeProperties açıklamasına bakın. | Evet |
typeProperties | Tür özellikleri için her veri deposunun farklı veya işlem. <br/><br/> Türler ve tür özellikleri için desteklenen veri deposuna, bkz: [veri kümesi türü](concepts-datasets-linked-services.md#dataset-type) bu makaledeki tablo. Bir veri deposuna belirli tür özellikleri hakkında bilgi edinmek için veri deposu Bağlayıcısı makalesi gidin. <br/><br/> Desteklenen işlem türleri ve tür özellikleri için bkz. [işlem bağlı Hizmetleri](compute-linked-services.md). | Evet |
connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır

## <a name="linked-service-example"></a>Bağlı hizmet örneği
Aşağıdaki bağlı hizmet bir Azure depolama bağlı hizmetidir. Türü için bir AzureStorage ayarlandığına dikkat edin. Tür özellikleri Azure depolama bağlı hizmeti için bir bağlantı dizesi içerir. Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için bu bağlantı dizesini kullanır.

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu araçlar ve SDK'lar birini kullanarak bağlı hizmetler oluşturabilirsiniz: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager şablonu ve Azure portalı

## <a name="data-store-linked-services"></a>Bağlı hizmetler veri deposu
Bağlanan veri depoları bulunabilir bizim [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats). Farklı mağazalarda gereken belirli bağlantı özelliklerinin listesini inceleyin.

## <a name="compute-linked-services"></a>İşlem bağlantılı hizmetler
Başvuru [desteklenen ortam işlem](compute-linked-services.md) ayrıntılarını farklı bilgi işlem ortamları için farklı yapılandırmaları yanı sıra veri fabrikanıza bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu araçlar ve SDK'lar birini kullanarak işlem hatlarını ve veri kümeleri oluşturmak için adım adım yönergeler için aşağıdaki öğreticiye bakın.

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Hızlı Başlangıç: PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md)
- [Hızlı Başlangıç: REST API kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md)
- [Hızlı Başlangıç: Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)
