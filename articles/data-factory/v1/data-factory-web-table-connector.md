---
title: Azure Data Factory kullanarak Web tablodan veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak bir Web sayfasındaki bir tablodan veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 81b7bf7c230c66087bf286ebd9369d992e93be90
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250626"
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Azure Data Factory kullanarak bir Web tablo kaynaktan veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-web-table-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-web-table.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de Web tablo Bağlayıcısı](../connector-web-table.md).

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir desteklenen havuz veri deposuna bir Web sayfasındaki bir tablodaki verileri taşımak için nasıl kullanılacağını özetlenmektedir. Bu makalede yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ve kaynakları/havuz desteklenen veri depolarının listesi ile veri taşıma genel bir bakış sunan makalesi.

Data factory şu anda yalnızca hareketli bir Web tablosundaki verileri diğer veri depolarına destekler, ancak diğer veriler veri taşıma değil bir Web tablosu hedef depolar.

> [!IMPORTANT]
> Bu Web Bağlayıcısı şu anda yalnızca ayıklanan tablo içeriğini bir HTML sayfasından destekler. Bir HTTP/s uç noktasından veri almak için kullanın [HTTP Bağlayıcısı](data-factory-http-connector.md) yerine.

## <a name="prerequisites"></a>Önkoşullar

Bu Web tablo bağlayıcıyı kullanmak için bir şirket içinde barındırılan Integration Runtime ' (diğer adıyla veri yönetimi ağ geçidi) ayarlamak ve yapılandırmak gereken `gatewayName` havuz özelliğinde bağlı hizmeti. Örneğin, Web tablodan Azure Blob depolama alanına kopyalamak için Azure depolama bağlı hizmetinin aşağıdaki gibi yapılandırın:

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. 

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz. 
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. 
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Web tablodan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Veri Web tablodan Azure Blob kopyalama](#json-example-copy-data-from-web-table-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Web tabloya tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, Web bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Web** |Evet |
| Url |Web kaynağına URL'si |Evet |
| authenticationType |Anonim. |Evet |

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulaması

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
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **WebTable** aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Veri kümesi türü. ayarlanmalıdır **WebTable** |Evet |
| path |Tablo içeren bir kaynak için göreli bir URL. |Hayır. Yalnızca bağlı hizmet tanımında belirtilen URL yolu belirtilmemiş olduğunda kullanılır. |
| index |Kaynak tablodaki dizini. Bkz [Get dizini bir HTML sayfasında bir tablonun](#get-index-of-a-table-in-an-html-page) bölüm için bir HTML sayfasında bir tablo dizininin başlangıç adımları. |Evet |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Şu anda, kopyalama etkinliği kaynak olduğunda tür **WebSource**, hiçbir ek özellikler desteklenir.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>JSON örneği: Azure Blob için veri Web tablodan kopyalama
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [Web](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [WebTable](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [WebSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek her saat için bir Azure blob Web tablodan veri kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

Aşağıdaki örnek, bir Azure blobuna Web tablodan veri kopyalama işlemi gösterilmektedir. Ancak, verileri doğrudan belirtilen havuzlarını hiçbirini kopyalanabilir [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, Azure veri fabrikasında kopyalama etkinliği kullanarak.

**Web hizmeti bağlı** Bu örnek anonim kimlik doğrulaması ile bağlantılı Web hizmetini kullanır. Bkz: [Web bağlı hizmet](#linked-service-properties) bölüm için farklı türde kimlik doğrulaması kullanabilirsiniz.

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
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

**WebTable girdi veri kümesi** ayarı **dış** için **true** Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

> [!NOTE]
> Bkz [Get dizini bir HTML sayfasında bir tablonun](#get-index-of-a-table-in-an-html-page) bölüm için bir HTML sayfasında bir tablo dizininin başlangıç adımları.  
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


**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1).

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



**Kopyalama etkinliği ile işlem hattı**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **WebSource** ve **havuz** türü ayarlandığında **BlobSink**.

WebSource türü özellikleri WebSource tarafından desteklenen özelliklerin listesi için bkz.

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

## <a name="get-index-of-a-table-in-an-html-page"></a>Bir HTML sayfasında bir tablo dizinini Al
1. Başlatma **Excel 2016** geçin **veri** sekmesi.  
2. Tıklayın **yeni sorgu** araç çubuğunda işaret **diğer kaynaklardan** tıklatıp **Web'den**.

    ![Power Query menüsü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. İçinde **Web'den** iletişim kutusuna **URL** bağlı hizmet JSON içinde kullanırsınız (örneğin: https://en.wikipedia.org/wiki/) yolu belirtmek için veri kümesi ile birlikte (örneğin: AFI % 27s_100_Years... 100_Movies) tıklayıp **Tamam**.

    ![Web iletişim kutusundan](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL'si: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Görürseniz **erişim Web içeriği** iletişim kutusunda, sağdaki seçin **URL**, **kimlik doğrulaması**, tıklatıp **Connect**.

   ![Web içerik iletişim kutusuna erişin](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. ' A tıklayın bir **tablo** öğesi içeriği tablosundan görebilirsiniz ve ardından ağaç görünümünde **Düzenle** altındaki düğmesini.  

   ![Gezgin iletişim kutusu](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. İçinde **sorgu Düzenleyicisi** penceresinde tıklayın **Gelişmiş Düzenleyici** araç çubuğunda.

    ![Gelişmiş Düzenleyici düğmesi](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusuna dizin "Kaynak" yanındaki sayıdır.

    ![Gelişmiş Düzenleyici - dizin](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, [Excel için Microsoft Power Query](https://www.microsoft.com/download/details.aspx?id=39379) dizin alınamıyor. Bkz: [bir web sayfasına bağlanma](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) makale Ayrıntılar için. Kullanıyorsanız benzer adımlarla [Microsoft Power BI Desktop için](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
