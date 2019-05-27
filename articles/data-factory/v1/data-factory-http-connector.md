---
title: Veri taşıma bir HTTP kaynağı - Azure | Microsoft Docs
description: Bir şirket içi veri taşıma veya Azure Data Factory kullanarak HTTP kaynağı bulut öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f7e070788d2fc11addcafc30d9f232f194f44782
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318487"
---
# <a name="move-data-from-an-http-source-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir HTTP kaynaktan veri taşıma

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-http-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-http.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Azure Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de HTTP Bağlayıcısı](../connector-http.md).


Bu makalede, kopyalama etkinliği Azure Data Factory'de bir şirket içi veri taşıma veya desteklenen havuz veri deposu için HTTP uç noktası bulut için nasıl kullanılacağını özetlenmektedir. Bu makalede yapılar [kopyalama etkinliğiyle veri taşıma](data-factory-data-movement-activities.md), hangi sunar genel bir bakış veri taşıma, kopyalama etkinliği'ni kullanarak. Makalede ayrıca, kaynak ve havuz olarak kopyalama etkinliği destekleyen veri depolarını listelenir.

Data Factory şu anda yalnızca taşıma HTTP kaynak verileri diğer veri depolarına destekler. Bunu bir HTTP hedefe diğer veri depolarından veri taşımayı desteklemiyor.

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri

Bu HTTP Bağlayıcısı verileri almak için kullanabileceğiniz *hem bulut hem de şirket içi HTTP/S uç nokta* HTTP kullanarak **alma** veya **POST** yöntemleri. Aşağıdaki kimlik doğrulaması türleri desteklenir: **Anonim**, **temel**, **Özet**, **Windows**, ve **ClientCertificate**. Bu bağlayıcı arasındaki farka dikkat edin ve [Web tablo Bağlayıcısı](data-factory-web-table-connector.md). Web tablosu Bağlayıcısı, bir HTML Web sayfasından tablo içeriğini ayıklar.

Bir şirket içi HTTP uç noktasından veri kopyaladığınızda, şirket içi ortamda veya bir Azure sanal veri yönetimi ağ geçidi yüklemeniz gerekir. Ağ geçidini ayarlamak adım adım yönergeler ve veri yönetimi ağ geçidi hakkında bilgi edinmek için [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).

## <a name="get-started"></a>başlarken

Farklı araçlar veya API'leri kullanarak bir HTTP kaynaktan verileri taşımak için kopyalama etkinliği içeren işlem hattı oluşturabilirsiniz:

- Bir işlem hattı oluşturmanın en kolay yolu, veri kopyalama Sihirbazı'nı kullanmaktır. Veri Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı kılavuz için bkz. [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**e **Azure Resource Manager Şablon**, **.NET API**, veya **REST API**. Kopyalama etkinliği içeren işlem hattı oluşturma konusunda adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). JSON HTTP kaynağı bu kopya verileri Azure Blob Depolama örnekleri için bkz: [JSON örnekler](#json-examples).

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki tabloda bağlı HTTP hizmetine özel JSON öğeleri açıklanmıştır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type | **Türü** özelliği ayarlanmalıdır **Http**. | Evet |
| url | Web sunucusuna temel URL'si. | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler **anonim**, **temel**, **Özet**, **Windows**, ve **ClientCertificate**. <br><br> Bu makale için daha fazla özellik ve bu kimlik doğrulama türleri için JSON örnekleri sonraki bölümlerine bakın. | Evet |
| enableServerCertificateValidation | Kaynak bir HTTPS web sunucusu ise sunucu SSL sertifika doğrulamasını etkinleştirilip etkinleştirilmeyeceğini belirtir. HTTPS sunucunuzun otomatik olarak imzalanan bir sertifika kullandığında, bu ayar **false**. | Hayır<br /> (varsayılan değer **true**) |
| gatewayName | Bir şirket içi HTTP kaynağına bağlanmak için kullanılacak veri yönetimi ağ geçidi örneğinin adı. | Evet, bir şirket içi HTTP kaynaktan veri kopyalıyorsanız |
| encryptedCredential | HTTP uç noktasına erişmek için şifrelenmiş kimlik bilgileri. Kopyalama Sihirbazı'nı kullanarak veya kimlik doğrulama bilgileri yapılandırdığınızda otomatik olarak oluşturulan değerdir **ClickOnce** iletişim kutusu. | Hayır<br /> (geçerli bir şirket içi HTTP sunucusundan veri kopyalarken) |

Bir şirket içi HTTP Bağlayıcısı veri kaynağı için kimlik bilgilerini ayarlama hakkında daha fazla ayrıntı için bkz [veri taşıma şirket içi kaynakları ile bulut arasında veri yönetimi ağ geçidi'ni kullanarak](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamasını kullanma

Ayarlama **authenticationType** için **temel**, **Özet**, veya **Windows**. Önceki bölümde açıklanan genel HTTP Bağlayıcısı özelliklerine ek olarak, aşağıdaki özellikleri ayarlayın:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| userName | HTTP uç noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının parolasını (**kullanıcıadı**). | Evet |

**Örnek: Temel, Özet veya Windows kimlik doğrulamasını kullanma**

```json
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

Temel kimlik doğrulaması kullanmak için ayarlanmış **authenticationType** için **ClientCertificate**. Önceki bölümde açıklanan genel HTTP Bağlayıcısı özelliklerine ek olarak, aşağıdaki özellikleri ayarlayın:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | PFX dosyasının ikili verileri Base64 ile kodlanmış içeriği. | Seçeneklerinden birini belirtin **embeddedCertData** veya **Certthumbprınt** |
| Certthumbprınt | Ağ geçidi makinenizin sertifika deposunda yüklü sertifika parmak izi. Bir şirket içi HTTP kaynaktan veri kopyalarken uygulayın. | Seçeneklerinden birini belirtin **embeddedCertData** veya **Certthumbprınt** |
| password | Sertifikayla ilişkili parola. | Hayır |

Kullanırsanız **Certthumbprınt** yerel bilgisayarın kişisel depoda kimlik doğrulaması ve sertifika yüklü için ağ geçidi hizmeti izinleri okuma izni ver:

1. Microsoft Yönetim Konsolu (MMC) açın. Ekleme **sertifikaları** hedefleyen eklentisini **yerel bilgisayar**.
2. Genişletin **sertifikaları** > **kişisel**ve ardından **sertifikaları**.
3. Kişisel deposundan sertifikaya sağ tıklayın ve ardından **tüm görevler** >**özel anahtarları Yönet**.
3. Üzerinde **güvenlik** sekmesinde, altında veri yönetimi ağ geçidi ana bilgisayar hizmeti çalışıyor, sertifika okuma erişimi olan kullanıcı hesabı ekleyin.  

**Örnek: Bir istemci sertifikası kullanarak**

Bu veri fabrikanıza şirket içi HTTP web sunucusuna bağlı hizmeti. Veri Yönetimi ağ geçidi yüklü olan bir makinede yüklü bir istemci sertifikası kullanır.

```json
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

**Örnek: Bir dosyayı bir istemci sertifikası kullanarak**

Bu veri fabrikanıza şirket içi HTTP web sunucusuna bağlı hizmeti. Veri Yönetimi ağ geçidi yüklü olan makine bir istemci sertifika dosyasını kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "Base64-encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bir veri kümesi JSON dosyasının yapısı, kullanılabilirlik ve ilke gibi bazı bölümler, tüm veri kümesi türleri (Azure SQL veritabanı, Azure Blob Depolama, Azure tablo depolama) için benzer.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md).

**TypeProperties** bölümünde her veri kümesi türü için farklıdır. **TypeProperties** bölüm veri deposundaki veriler konumu hakkında bilgi sağlar. **TypeProperties** bir veri kümesi için bölüm **Http** türü, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** veri kümesini ayarlanmalıdır **Http**. | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bağlı hizmet tanımında belirtilen URL yolu belirtilmemiş olduğunda kullanılır. <br><br> Dinamik bir URL oluşturmak için kullanabileceğiniz [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md). Örnek: **relativeUrl**: **$$Text.Format ('/ my/rapor? ay = {0: yyyy}-{0:MM} & fmt csv =', SliceStart)** . | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **alma** ve **POST**. | Hayır <br />(varsayılan değer **alma**) |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| Includesearchresults: true | HTTP isteğinin gövdesi. | Hayır |
| format | İsterseniz *bir HTTP uç noktasından veri alma-olan* ayrıştırma olmadan, atlama **biçimi** ayarı. <br><br> Kopyalama sırasında HTTP yanıt içeriği ayrıştırılamıyor istiyorsanız, şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format). |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

**Örnek: (Varsayılan) alma yöntemini kullanma**

```json
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

**Örnek: POST yöntemini kullanma**

```json
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

İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md). 

Kullanılabilir özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Bir kopyalama etkinliği için özellikler, kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Şu anda, kopyalama etkinliği kaynak olduğunda **HttpSource** türü, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Zaman aşımı ( **TimeSpan** değeri) bir yanıt almak HTTP isteği için. Yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olan. | Hayır<br />(varsayılan değer: **00:01:40**) |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri

Bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md) daha fazla bilgi için.

## <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnekler, bir HTTP kaynağından Azure Blob depolama alanına veri kopyalama işlemini göstermektedir. Ancak, veriler kopyalanabilir *doğrudan* herhangi birinden herhangi birine havuzlarını kaynakları [desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure veri fabrikasında kopyalama etkinliği kullanarak.

**Örnek: Bir HTTP kaynağından Azure Blob depolama alanına veri kopyalama**

Bu örnek Data Factory çözümü, aşağıdaki Data Factory varlıklarını içerir:

*   Bağlı hizmet türü [HTTP](#linked-service-properties).
*   Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
*   Girdi [veri kümesi](data-factory-create-datasets.md) türü [Http](#dataset-properties).
*   Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
*   A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği olan [HttpSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri saatte bir Azure blobuna bir HTTP kaynaktan kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="http-linked-service"></a>HTTP bağlı hizmeti

Bu örnek anonim kimlik doğrulaması ile bağlı HTTP hizmeti kullanır. Bkz: [HTTP bağlı hizmet](#linked-service-properties) için farklı türde kimlik doğrulaması kullanabilirsiniz.

```json
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

### <a name="azure-storage-linked-service"></a>Azure depolama bağlı hizmeti

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
    }
  }
}
```

### <a name="http-input-dataset"></a>HTTP giriş veri kümesi

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```json
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

### <a name="azure-blob-output-dataset"></a>Azure blob çıktı veri kümesi

Veriler her saat yeni bir bloba yazılır (**sıklığı**: **saat**, **aralığı**: **1**).

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

### <a name="pipeline-that-uses-a-copy-activity"></a>Kopyalama etkinliği kullanan işlem hattı

İşlem hattının kopyalama etkinliği girdi ve çıktı veri kümelerini kullanmak üzere yapılandırılmış içerir. Kopyalama etkinliği, saatte çalışacak şekilde zamanlanır. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **HttpSource** ve **havuz** türü ayarlandığında **BlobSink**.

Özelliklerin listesi için **HttpSource** bakın [HttpSource](#copy-activity-properties).

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with a copy activity",
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
> Sütunları havuz veri kümesi için bir kaynak veri kümesindeki sütunları eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar

Azure Data Factory ve bunu en iyi duruma çeşitli şekillerde veri taşıma (kopyalama etkinliği) performansını etkileyen önemli faktörlerin hakkında bilgi edinmek için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).