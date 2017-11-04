---
title: "Azure Data Factory kullanarak Web tablodan veri taşıma | Microsoft Docs"
description: "Azure Data Factory kullanarak bir Web sayfası bir tablodaki veri taşıma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 17ebb1d61f3fff85580fe4f616477c5084d1537a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Azure Data Factory kullanarak bir Web tablo kaynağından veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-web-table-connector.md)
> * [Sürüm 2 - Önizleme](../connector-web-table.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Web tablo Bağlayıcısı](../connector-web-table.md).

Bu makalede kopya etkinliği Azure Data Factory'de bir desteklenen havuz veri deposuna bir Web sayfasında bir tablodan veri taşımak için nasıl kullanılacağı açıklanmaktadır. Bu makalede derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği ve kaynakları/havuzlarını desteklenen veri depoları listesi ile veri taşıma için genel bir bakış sunar.

Veri Fabrikası şu anda yalnızca veri taşımayı Web tablodan diğer veri depolarına destekler, ancak verileri diğer veriler taşıma olmayan bir Web tablo hedefine depolar.

> [!IMPORTANT]
> Bu Web bağlayıcı şu anda bir HTML sayfasından yalnızca ayıklanan tablo içeriği destekler. Bir HTTP/s uç noktasından verileri almak için kullanmak [HTTP Bağlayıcısı](data-factory-http-connector.md) yerine.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun. 

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz. 
- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir web tablodan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama Web tablosundan Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Web tabloya tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri Web bağlantılı hizmeti için belirli bir açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Web** |Evet |
| Url |Web kaynağı URL'si |Evet |
| authenticationType |Anonim. |Evet |

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **WebTable** aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Veri kümesi türü. ayarlanmalıdır **WebTable** |Evet |
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
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Şu anda kopyalama etkinliği kaynağında olduğunda türü **WebSource**, hiçbir ek özellikler desteklenir.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>JSON örnek: veri kopyalama Web tablosundan Azure Blob
Aşağıdaki örnek gösterilmektedir:

1. Bağlı hizmet türü [Web](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [WebTable](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [WebSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri saatte bir Azure blob Web tablosundan kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

Aşağıdaki örnek, bir Azure blob Web tablodan veri kopyalama gösterilmektedir. Ancak, verileri doğrudan belirtilen havuzlarını hiçbirini kopyalanabilir [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopya etkinliği Azure Data Factory kullanarak makale.

**Web hizmeti bağlı** Bu örnek anonim kimlik doğrulaması ile bağlantılı Web hizmetini kullanır. Bkz: [Web bağlantılı hizmeti](#linked-service-properties) bölümü için farklı tür kimlik doğrulaması kullanabilirsiniz.

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

**Azure Storage bağlı hizmeti**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**WebTable girdi veri kümesi** ayarı **dış** için **true** Data Factory hizmetinin veri kümesi data factory dış ve verileri bir etkinlik tarafından üretilen değil bildirir üreteci.

> [!NOTE]
> Bkz: [bir HTML sayfasında tablosunun Get dizini](#get-index-of-a-table-in-an-html-page) bir HTML sayfasında bir tablo dizininin alma adımları için bölüm.  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Azure Blob dataset çıktı**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Kopyalama etkinliği ile kanalı**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **WebSource** ve **havuz** türü ayarlanmış **BlobSink**.

Bkz: [WebSource türü özellikleri](#copy-activity-type-properties) WebSource tarafından desteklenen özelliklerin listesi için.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Bir HTML sayfasında bir tablonun dizini alma
1. Başlatma **Excel 2016** ve geçiş **veri** sekmesi.  
2. Tıklatın **yeni sorgu** araç çubuğunda işaret **diğer kaynaklardan** tıklatıp **Web'den**.

    ![Power Query menüsü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. İçinde **Web'den** iletişim kutusunda, girin **URL** bağlantılı hizmeti JSON içinde kullanırsınız (örneğin: https://en.wikipedia.org/wiki/) yolu belirtin veri kümesi için birlikte (örneğin: AFI % 27s_ 100_Years... 100_Movies) tıklatıp **Tamam**.

    ![Web iletişim kutusundan](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Görürseniz **erişim Web içeriği** iletişim kutusunda, sağa seçin **URL**, **kimlik doğrulaması**, tıklatıp **Bağlan**.

   ![Web içerik iletişim kutusuna erişin](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Tıklatın bir **tablo** öğesi tablosundan içeriğine bakın ve ardından ağaç görünümünde **Düzenle** altındaki düğmesini.  

   ![Gezgin iletişim](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. İçinde **sorgu Düzenleyicisi'ni** penceresinde tıklatın **Gelişmiş Düzenleyici** araç çubuğunda.

    ![Gelişmiş Düzenleyici düğmesi](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusunda "Kaynak" yanındaki dizin numarasıdır.

    ![Düzenleyicisi - Gelişmiş dizin](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, [Excel için Microsoft Power Query](https://www.microsoft.com/download/details.aspx?id=39379) dizini alınamadı. Bkz: [bir web sayfası Bağlan](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) Ayrıntılar için makale. Adımlar kullanıyorsanız benzer [Masaüstü için Microsoft Power BI](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
