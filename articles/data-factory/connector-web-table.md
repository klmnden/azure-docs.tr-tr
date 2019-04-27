---
title: Azure Data Factory kullanarak Web tablodan veri kopyalama | Microsoft Docs
description: Web tablosu Bağlayıcısı, Azure veri sağlayan fabrikası hakkında havuz Data Factory tarafından desteklenen veri depolarının bir web tablodan veri kopyalayın öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: e578b3a6b3905569567b568b0130c1ed1b90d915
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60557775"
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Web tablosu, Azure Data Factory kullanarak verileri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-web-table-connector.md)
> * [Geçerli sürüm](connector-web-table.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir Web tablosu veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

Bu Web tablo Bağlayıcısı arasındaki fark [REST'e bağlayıcı](connector-rest.md) ve [HTTP Bağlayıcısı](connector-http.md) şunlardır:

- **Web tablosu Bağlayıcısı** tablo bir HTML Web sayfası içeriği ayıklar.
- **REST'e bağlayıcı** özellikle RESTful API'lerinden veri kopyalama desteği.
- **HTTP Bağlayıcısı** örn herhangi bir HTTP uç noktasından veri almaya genel dosya indirilemedi. 

## <a name="supported-capabilities"></a>Desteklenen özellikler

Web tablosu veritabanından herhangi bir desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Web tablo bağlayıcı destekler **HTML sayfasından tablo içeriğini ayıklanması**.

## <a name="prerequisites"></a>Önkoşullar

Bu Web tablo bağlayıcısını kullanmak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Web tablo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Web tablosu için bağlı hizmet, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Web** |Evet |
| url | Web kaynağına URL'si |Evet |
| authenticationType | İzin verilen değeri şudur: **Anonim**. |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "WebLinkedService",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "url" : "https://en.wikipedia.org/wiki/",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Web tablosu veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Web tablodan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **WebTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **WebTable** | Evet |
| yol |Tablo içeren bir kaynak için göreli bir URL. |Hayır. Yalnızca bağlı hizmet tanımında belirtilen URL yolu belirtilmemiş olduğunda kullanılır. |
| dizin |Kaynak tablodaki dizini. Bkz [Get dizini bir HTML sayfasında bir tablonun](#get-index-of-a-table-in-an-html-page) bölüm için bir HTML sayfasında bir tablo dizininin başlangıç adımları. |Evet |

**Örnek:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": {
            "referenceName": "<Web linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Web tablosu kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="web-table-as-source"></a>Kaynak olarak Web tablosu

Web tablodan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **WebSource**, hiçbir ek özellikler desteklenir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromWebTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Web table input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "WebSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Bir HTML sayfasında bir tablo dizinini Al

Yapılandırmanız gereken bir tablo dizininin almak için [veri kümesi özellikleri](#dataset-properties), örneğin Excel 2016 aşağıdaki gibi bir araç olarak kullanabilirsiniz:

1. Başlatma **Excel 2016** geçin **veri** sekmesi.
2. Tıklayın **yeni sorgu** araç çubuğunda işaret **diğer kaynaklardan** tıklatıp **Web'den**.

    ![Power Query menüsü](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. İçinde **Web'den** iletişim kutusuna **URL** bağlı hizmet JSON içinde kullanırsınız (örneğin: https://en.wikipedia.org/wiki/) yolu belirtmek için veri kümesi ile birlikte (örneğin: AFI % 27s_100_Years... 100_Movies) tıklayıp **Tamam**.

    ![Web iletişim kutusundan](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL'si: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Görürseniz **erişim Web içeriği** iletişim kutusunda, sağdaki seçin **URL**, **kimlik doğrulaması**, tıklatıp **Connect**.

   ![Web içerik iletişim kutusuna erişin](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. ' A tıklayın bir **tablo** öğesi içeriği tablosundan görebilirsiniz ve ardından ağaç görünümünde **Düzenle** altındaki düğmesini.  

   ![Gezgin iletişim kutusu](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. İçinde **sorgu Düzenleyicisi** penceresinde tıklayın **Gelişmiş Düzenleyici** araç çubuğunda.

    ![Gelişmiş Düzenleyici düğmesi](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusuna dizin "Kaynak" yanındaki sayıdır.

    ![Gelişmiş Düzenleyici - dizin](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, [Excel için Microsoft Power Query](https://www.microsoft.com/download/details.aspx?id=39379) dizin alınamıyor. Bkz: [bir web sayfasına bağlanma](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) makale Ayrıntılar için. Kullanıyorsanız benzer adımlarla [Microsoft Power BI Desktop için](https://powerbi.microsoft.com/desktop/).


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
