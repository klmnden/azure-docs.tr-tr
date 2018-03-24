---
title: Azure Data Factory kullanarak bir FTP sunucusundan veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak bir FTP sunucusundan veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 17dea2d1106a57aa678a88db6647c71048d8c38f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir FTP sunucusundan veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-ftp-connector.md)
> * [Sürüm 2 - Önizleme](../connector-ftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 FTP Bağlayıcısı](../connector-ftp.md).

Bu makalede kopya etkinliği Azure Data Factory'de bir FTP sunucusundan veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir FTP sunucusundan veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca taşıma verilerden bir FTP sunucusu diğer veri depolarına destekler, ancak FTP sunucusuna verileri diğer veriler taşıma değil depolar. Her iki şirket içi destekler ve FTP sunucuları bulut.

> [!NOTE]
> Hedefe başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosyasını silmez. Sonra başarılı bir kopyasını kaynak dosyasını silmeniz gerekirse, dosyayı silmek için özel bir etkinlik oluşturmak ve ardışık düzeninde etkinlik kullanın. 

## <a name="enable-connectivity"></a>Bağlantıyı etkinleştirmek
Verileri taşıyorsanız, bir **şirket içi** FTP sunucusuna Bulutu veri depolamak (örneğin,. Azure Blob depolama alanına) yükleyin ve veri yönetimi ağ geçidi kullanın. Veri Yönetimi ağ geçidi, şirket içi makinenizde yüklü bir istemci aracısıdır ve bir şirket içi kaynağa bağlanmak bulut hizmetleri sağlar. Ayrıntılar için bkz [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). Adım adım yönergeler ayarı yukarı ağ geçidi ve, bkz [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md). Sunucu üzerinde bir Azure altyapı (ıaas) sanal makine (VM) olarak olsa bile, bir FTP sunucusuna bağlanmak için ağ geçidi'ni kullanın.

Aynı şirket içi makinede veya FTP sunucusu olarak Iaas VM ağ geçidi yüklemek mümkündür. Ancak, ayrı makinede veya Iaas kaynak çekişmesini önlemek için VM ve daha iyi performans için ağ geçidi yüklemenizi öneririz. Ağ geçidi ayrı bir makineye yüklediğinizde, makine FTP sunucusunun erişebilir olması gerekir.

## <a name="get-started"></a>başlarken
Farklı araçlar veya API'lerini kullanarak bir FTP kaynaktan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Data Factory Kopyalama Sihirbazı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) hızlı kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için.
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları veya API'ler (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın. Bir FTP veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama FTP sunucusundan Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) bu makalenin.

> [!NOTE]
> Kullanmak için desteklenen dosya ve sıkıştırma biçimleri hakkında daha fazla ayrıntı için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md).

Aşağıdaki bölümler, Data Factory varlıklarını belirli FTP tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda bir FTP bağlantılı hizmete özgü JSON öğelerini açıklar.

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| type |Bu Ftp_sunucusu için ayarlayın. |Evet |&nbsp; |
| konak |Adını veya FTP sunucusunun IP adresini belirtin. |Evet |&nbsp; |
| authenticationType |Kimlik doğrulama türünü belirtin. |Evet |Temel, anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcı belirtin. |Hayır |&nbsp; |
| password |(Kullanıcı adı) kullanıcının parolasını belirtin. |Hayır |&nbsp; |
| encryptedCredential |FTP sunucusuna erişmek için şifreli kimlik bilgilerini belirtin. |Hayır |&nbsp; |
| gatewayName |Veri Yönetimi ağ geçidi şirket içi FTP sunucusuna bağlanmak için ağ geçidi adını belirtin. |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktasını belirtin. |Hayır |21 |
| enableSsl |FTP SSL/TLS kanalı üzerinden kullanılıp kullanılmayacağını belirtin. |Hayır |true |
| enableServerCertificateValidation |FTP SSL/TLS kanalı üzerinden kullanırken sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin. |Hayır |true |

### <a name="use-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullan

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Kullanıcı adı ve parola düz metin olarak temel kimlik doğrulaması için kullanın.

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Bağlantı noktası, enableSsl, enableServerCertificateValidation kullanın

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>Kimlik doğrulama ve ağ geçidi için encryptedCredential kullanın

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümü, her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. **TypeProperties** bir veri kümesi için bir bölüm türü **FileShare** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasöre. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** dilim başlangıç bağlı klasör yoluna sahip ve bitiş tarihi saatlerini. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>Zaman **fileName** belirtilmemiş bir çıkış veri kümesi için oluşturulan dosya adı şu biçimde: <br/><br/>Veriler. <Guid>.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Bir alt kümesini dosyalarında seçmek için kullanılacak bir filtre belirtin **folderPath**, tüm dosyalar yerine.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** bir giriş FileShare veri kümesi için geçerlidir. Bu özellik, Hadoop dağıtılmış dosya sistemi (HDFS ile) desteklenmiyor. |Hayır |
| partitionedBy |Dinamik belirtmek için kullanılan **folderPath** ve **fileName** zaman serisi veriler için. Örneğin, belirleyebileceğiniz bir **folderPath** için verileri saatte parametreli. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> Depoları arasında (ikili kopya), dosya tabanlı olarak dosyaları kopyalamak istiyorsanız, her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**, ve desteklenen düzeyler **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağınızı belirtin. Değerleri (Bu değer varsayılan değer) ikili mod için doğru olduğundan ve ASCII için false. İlişkili bağlantılı hizmet türü türü olduğunda bu özellik yalnızca kullanılabilir: Ftp_sunucusu. |Hayır |

> [!NOTE]
> **Dosya adı** ve **fileFilter** eşzamanlı olarak kullanılamaz.

### <a name="use-the-partionedby-property"></a>PartionedBy özelliğini kullanın
Önceki bölümde belirtildiği gibi bir dinamik belirtebilirsiniz **folderPath** ve **fileName** zaman serisi verilerle için **partitionedBy** özelliği.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında bilgi edinmek için [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [ardışık düzen oluşturma](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte, {dilim} değişkeninin değeri veri fabrikası sistem SliceStart, belirtilen biçimde (YYYYMMDDHH) ile değiştirilir. Dilimin başlangıç SliceStart başvuruyor. Klasör yolu, her dilim için farklıdır. (Örneğin, wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.)

#### <a name="sample-2"></a>Örnek 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Bu örnekte, tarafından kullanılan ayrı değişkenleri içine ayıklanır yıl, ay, gün ve saati SliceStart, **folderPath** ve **fileName** özellikleri.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md). Ad, açıklama, giriş ve çıkış tabloları ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Kullanılabilir özellikler **typeProperties** etkinlik bölümünü Öte yandan, her etkinlik türü ile değişir. Kopya etkinliği için tür özellikleri türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kopyalama etkinliğinde kaynak türü olduğunda **FileSystemSource**, aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Alt klasörleri veya yalnızca belirtilen klasöre verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a>JSON örnek: veri kopyalama FTP sunucusundan Azure Blob
Bu örnek, bir FTP sunucusundan Azure Blob depolama alanına veri kopyalama gösterilmektedir. Ancak, verileri doğrudan belirtilen havuzlarını hiçbirini kopyalanabilir [desteklenen veri depoları ve biçimleri](data-factory-data-movement-activities.md#supported-data-stores-and-formats), Data Factory kopyalama etkinliği kullanarak.  

Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* Bağlı hizmet türü [Ftp_sunucusu](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Bir giriş [dataset](data-factory-create-datasets.md) türü [dosya paylaşımı](#dataset-properties)
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

Örnek verileri saatte bir Azure blob bir FTP sunucusundan kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="ftp-linked-service"></a>Bağlı FTP hizmeti

Bu örnek temel kimlik doğrulaması, kullanıcı adı ve parola düz metin kullanır. Aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Anonim kimlik doğrulaması
* Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması
* SSL/TLS (FTPS) üzerinden FTP

Bkz: [FTP bağlantılı hizmeti](#linked-service-properties) bölümü için farklı tür kimlik doğrulaması kullanabilirsiniz.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
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
### <a name="ftp-input-dataset"></a>FTP giriş veri kümesi

Bu veri kümesi FTP klasöre başvuruyor `mysharedfolder` ve dosya `test.csv`. Ardışık Düzen dosya hedefe kopyalar.

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Klasör yolu blob'a dinamik olarak hesaplanan işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay, gün ve saatleri bölümlerini başlangıç saatini kullanır.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Dosya sistemi kaynak ve blob havuz sahip işlem hattı kopyalama etkinliği

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **FileSystemSource**ve **havuz** türü ayarlanmış **BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Bu veri fabrikası ve onu en iyi duruma getirmek için çeşitli yollar veri taşıma (kopyalama etkinliği) etkisi performansını anahtar Etkenler hakkında bilgi için bkz [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

* Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
