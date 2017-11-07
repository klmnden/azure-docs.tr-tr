---
title: Azure Data Factory kullanarak Web tablodan veri kopyalama | Microsoft Docs
description: "Web tablo bağlayıcı Azure veri sağlayan fabrikası hakkında veri depoları havuzlarını Data Factory ile desteklenen web tablosundan verilerini öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: affe0d7da4b6e40da3b7fdb10d53b9e793ec19bd
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Web tablodan veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-web-table-connector.md)
> * [Sürüm 2 - Önizleme](connector-web-table.md)

Bu makalede kopya etkinliği Azure Data Factory'de Web tablo veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.


> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Web tablo Bağlayıcısı](v1/data-factory-web-table-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Web tablo veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Web tablo bağlayıcı destekler **tablo içeriği HTML sayfasından ayıklama**. Bir HTTP/s uç noktasından verileri almak için kullanmak [HTTP Bağlayıcısı](connector-http.md) yerine.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](create-self-hosted-integration-runtime.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Data Factory varlıklarını belirli Web tablo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Web tablo için bağlı hizmet aşağıdaki özellikleri desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Web** |Evet |
| URL | Web kaynağı URL'si |Evet |
| authenticationType | Değer izin verilen: **anonim**. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "url" : "https://en.wikipedia.org/wiki/",
            "authenticationType": "Anonymous"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Web tablosu veri kümesi tarafından desteklenen özellikler listesini sağlar.

Web tablodan veri kopyalamak için veri kümesi için tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **WebTable** | Evet |
| Yol |Tabloyu içeren kaynak için göreli bir URL. |Hayır. Bağlantılı hizmet tanımında belirtilen URL yolu belirtilmediğinde kullanılır. |
| Dizin |Tablo kaynak dizini. Bkz: [bir HTML sayfasında tablosunun Get dizini](#get-index-of-a-table-in-an-html-page) bir HTML sayfasında bir tablo dizininin alma adımları için bölüm. |Evet |

**Örnek:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Web tablosu kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="web-table-as-source"></a>Web tablo kaynağı olarak

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

## <a name="get-index-of-a-table-in-an-html-page"></a>Bir HTML sayfasında bir tablonun dizini alma

1. Başlatma **Excel 2016** ve geçiş **veri** sekmesi.
2. Tıklatın **yeni sorgu** araç çubuğunda işaret **diğer kaynaklardan** tıklatıp **Web'den**.

    ![Power Query menüsü](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. İçinde **Web'den** iletişim kutusunda, girin **URL** bağlantılı hizmeti JSON içinde kullanırsınız (örneğin: https://en.wikipedia.org/wiki/) yolu belirtin veri kümesi için birlikte (örneğin: AFI % 27s_ 100_Years... 100_Movies) tıklatıp **Tamam**.

    ![Web iletişim kutusundan](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Görürseniz **erişim Web içeriği** iletişim kutusunda, sağa seçin **URL**, **kimlik doğrulaması**, tıklatıp **Bağlan**.

   ![Web içerik iletişim kutusuna erişin](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. Tıklatın bir **tablo** öğesi tablosundan içeriğine bakın ve ardından ağaç görünümünde **Düzenle** altındaki düğmesini.  

   ![Gezgin iletişim](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. İçinde **sorgu Düzenleyicisi'ni** penceresinde tıklatın **Gelişmiş Düzenleyici** araç çubuğunda.

    ![Gelişmiş Düzenleyici düğmesi](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusunda "Kaynak" yanındaki dizin numarasıdır.

    ![Düzenleyicisi - Gelişmiş dizin](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, [Excel için Microsoft Power Query](https://www.microsoft.com/download/details.aspx?id=39379) dizini alınamadı. Bkz: [bir web sayfası Bağlan](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) Ayrıntılar için makale. Adımlar kullanıyorsanız benzer [Masaüstü için Microsoft Power BI](https://powerbi.microsoft.com/desktop/).


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).