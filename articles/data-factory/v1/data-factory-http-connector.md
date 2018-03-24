---
title: Bir HTTP kaynağı - Azure veri taşıma | Microsoft Docs
description: Şirket içi veya Azure Data Factory kullanarak bulut HTTP kaynağından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9820ed9b4c0abbb79c6f92e62f294fb7fbd4c87e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Azure Data Factory kullanarak bir HTTP kaynağından veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-http-connector.md)
> * [Sürüm 2 - Önizleme](../connector-http.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 HTTP Bağlayıcısı](../connector-http.md).


Bu makalede kopya etkinliği Azure Data Factory'de verileri bir şirket içi/bulut HTTP uç noktasından bir desteklenen havuz veri deposuna taşımak için nasıl kullanılacağı açıklanmaktadır. Bu makalede derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği ve kaynakları/havuzlarını desteklenen veri depoları listesi ile veri taşıma için genel bir bakış sunar.

Veri Fabrikası şu anda taşıma veriler yalnızca bir HTTP kaynaktan diğer veri depolarına destekler, ancak verileri diğer veriler taşıma olmayan bir HTTP hedefe depolar.

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri
Bu HTTP bağlayıcı verileri almak için kullanabileceğiniz **Bulut ve şirket içi HTTP/s uç nokta** HTTP kullanarak **almak** veya **POST** yöntemi. Aşağıdaki kimlik doğrulama türleri desteklenir: **anonim**, **temel**, **Özet**, **Windows**, ve **ClientCertificate**. Bu bağlayıcı arasındaki farkı unutmayın ve [Web tablo Bağlayıcısı](data-factory-web-table-connector.md) olduğu: ikinci web HTML sayfasından tablo içeriği ayıklamak için kullanılır.

Bir şirket içi HTTP uç noktasından veri kopyalama işlemi sırasında şirket içi ortamına/Azure VM veri yönetimi ağ geçidi yüklemeniz gerekir. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale veri yönetimi ağ geçidi ve ağ geçidi kurun ayarlama hakkında adım adım yönergeleri hakkında bilgi edinin.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir HTTP kaynaktan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. JSON örnekleri verileri Azure Blob depolama alanına HTTP kaynağından kopyalamak, bkz: [JSON örnekler](#json-examples) Bu makaleler bölümü.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri için HTTP belirli açıklamasını bağlantılı hizmetinin sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type | Type özelliği ayarlanmalıdır: `Http`. | Evet |
| url | Web sunucusu için temel URL | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler: **anonim**, **temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Daha fazla özellikleri ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlerde sırasıyla bakın. | Evet |
| enableServerCertificateValidation | Sunucu SSL sertifika doğrulamasını kaynak HTTPS Web sunucusu ise etkinleştirilip etkinleştirilmeyeceğini belirtin | Hayır, varsayılan değer true şeklindedir |
| gatewayName | Bir şirket içi HTTP kaynağına bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi HTTP kaynaktan veri kopyalama, Evet. |
| encryptedCredential | HTTP uç noktasına erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim kimlik doğrulama bilgilerini yapılandırın. | Hayır. Yalnızca bir şirket içi HTTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

Bkz: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) şirket içi HTTP Bağlayıcısı veri kaynağı için kimlik bilgilerini ayarlama hakkında ayrıntılı bilgi için.

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulaması kullanma

Ayarlama `authenticationType` olarak `Basic`, `Digest`, veya `Windows`ve HTTP Bağlayıcısı yukarıda sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | HTTP uç noktasına erişmek için kullanıcı adı. | Evet |
| password | (Kullanıcı adı) kullanıcının parolası. | Evet |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Örnek: Temel, Özet veya Windows kimlik doğrulaması kullanma

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>ClientCertificate kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `ClientCertificate`ve HTTP Bağlayıcısı yukarıda sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | İkili veriler kişisel bilgi değişimi (PFX) dosyası Base64 ile kodlanmış içeriği. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| certThumbprint | Ağ geçidi makinenizin sertifika deposunda yüklü sertifika parmak izi. Yalnızca bir şirket içi HTTP kaynaktan veri kopyalama işlemi sırasında uygulanır. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| password | Sertifikayla ilişkili parola. | Hayır |

Kullanırsanız `certThumbprint` kimlik doğrulaması ve sertifika yüklü için yerel bilgisayarın kişisel deposunda, ağ geçidi hizmeti okuma izni vermesi gerekir:

1. Microsoft Yönetim Konsolu (MMC) başlatın. Ekleme **sertifikaları** hedefleyen eklentisi **yerel bilgisayar**.
2. Genişletme **sertifikaları**, **kişisel**, tıklatıp **Sertifikalar**.
3. Kişisel deposundaki sertifikayı sağ tıklatın ve seçin **tüm görevler**->**özel anahtarları Yönet...**
3. Üzerinde **güvenlik** sekmesinde, altında veri yönetimi ağ geçidi ana bilgisayar hizmeti çalıştığı okuma erişimi sertifikayı kullanıcı hesabını ekleyin.  

#### <a name="example-using-client-certificate"></a>Örnek: istemci sertifikası kullanarak
Bu hizmet bağlantılar, veri fabrikası bir şirket içi HTTP web sunucusuna bağlı. Veri Yönetimi ağ ile geçidi yüklü olduğu makinede yüklü bir istemci sertifikası kullanır.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Örnek: istemci sertifikası bir dosyada kullanma
Bu hizmet bağlantılar, veri fabrikası bir şirket içi HTTP web sunucusuna bağlı. Veri Yönetimi ağ ile geçidi yüklü olduğu makinedeki bir istemci sertifikası dosyası kullanır.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **Http** aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü belirttiniz. ayarlanmalıdır `Http`. | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bağlantılı hizmet tanımında belirtilen URL yolu belirtilmediğinde kullanılır. <br><br> Dinamik URL oluşturmak için kullanabileceğiniz [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md), örneğin "relativeUrl": "$$Text.Format ('/ my/rapor? ay = {0:yyyy}-{0:MM} & fmt csv =', SliceStart)". | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **almak** veya **POST**. | Hayır. `GET` varsayılan değerdir. |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| requestBody | HTTP istek gövdesi. | Hayır |
| Biçimi | Yalnızca istiyorsanız, **HTTP uç noktası olarak veri almak-olduğu** ayrıştırma olmadan, bu biçim ayarlarını atla. <br><br> HTTP yanıt içeriği kopyalama sırasında ayrıştırma istiyorsanız, aşağıdaki biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

### <a name="example-using-the-get-default-method"></a>Örnek: (varsayılan) GET yöntemini kullanma

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-the-post-method"></a>Örnek: POST yöntemini kullanma

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
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
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Kullanılabilir özellikler **typeProperties** etkinlik bölümünü diğer yandan her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Şu anda kopyalama etkinliği kaynağında olduğunda türü **HttpSource**, aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Zaman aşımı (TimeSpan) için bir yanıt almak HTTP isteği. Bu zaman aşımı yanıt verileri okumak için zaman aşımına bir yanıt elde etmektir. | Hayır. Varsayılan değer: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md) makale ayrıntıları.

## <a name="json-examples"></a>JSON örnekleri
Aşağıdaki örnek sağlamak kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, verileri Azure Blob depolama alanına HTTP kaynağından kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a>Örnek: veri HTTP kaynağından Azure Blob depolama alanına kopyalama
Bu örnek için veri fabrikası çözümü aşağıdaki Data Factory varlıklarını içerir:

1. Bağlı hizmet türü [HTTP](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [Http](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [HttpSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek saatte bir Azure blob için bir HTTP kaynaktan verileri kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="http-linked-service"></a>Bağlantılı HTTP hizmeti
Bu örnek anonim kimlik doğrulaması ile bağlantılı HTTP hizmeti kullanır. Bkz: [HTTP bağlantılı hizmeti](#linked-service-properties) bölümü için farklı tür kimlik doğrulaması kullanabilirsiniz.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

```JSON
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

### <a name="http-input-dataset"></a>HTTP girdi veri kümesi
Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1).

```JSON
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

### <a name="pipeline-with-copy-activity"></a>Kopyalama etkinliği ile kanalı

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **HttpSource** ve **havuz** türü ayarlanmış **BlobSink**.

Bkz: [HttpSource](#copy-activity-properties) HttpSource tarafından desteklenen özelliklerin listesi için.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
