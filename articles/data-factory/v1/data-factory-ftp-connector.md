---
title: Azure Data Factory kullanarak verileri bir FTP sunucusundan taşıma | Microsoft Docs
description: Azure Data Factory kullanarak bir FTP sunucusundan veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4aba7aadbe92b6c4f0ab417785e230bb6a6823df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486592"
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir FTP sunucusundan veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-ftp-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-ftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de FTP Bağlayıcısı](../connector-ftp.md).

Bu makalede, bir FTP sunucusundan verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Bir FTP sunucusundan tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data Factory şu anda yalnızca veri taşımayı bir FTP sunucusundan diğer veri depolarına destekler, ancak FTP sunucusuna verilerini diğer verilerden taşıma değil depolar. Hem şirket içi destekler ve FTP sunucuları bulut.

> [!NOTE]
> Hedefine başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosya silinmez. Başarılı kopyalamadan sonra kaynak dosyasını silmeniz gerekirse, dosyayı silmek için özel bir etkinlik oluşturma ve işlem hattı etkinliğini kullanın.

## <a name="enable-connectivity"></a>Bağlantı etkinleştir
Veri geçiş yapıyorsanız, bir **şirket içi** FTP sunucusuna bir bulut veri depolayın (örneğin, Azure Blob depolamaya) yükleyin ve veri yönetimi ağ geçidi kullanın. Veri Yönetimi ağ geçidi, şirket içi makinenizde yüklü bir istemci aracısıdır ve bir şirket içi kaynağa bağlanmak bulut hizmetleri sağlar. Ayrıntılar için bkz [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). Ayar adım adım yönergeler ağ geçidi ayarlama ve kullanma, bkz [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md). Sunucunun Azure altyapısının üzerinde bir hizmet (Iaas) sanal makine (VM) olsa bile bir FTP sunucusuna bağlanmak için ağ geçidi kullanın.

Ağ geçidi aynı şirket içi makinede veya Iaas VM FTP sunucusu olarak yüklemek mümkündür. Ancak, ayrı bir makine ya da kaynak çekişmesinden kaçınmak için Iaas VM ve daha iyi performans için ağ geçidi yüklemeniz önerilir. Ağ geçidi ayrı bir makineye yüklediğinizde, makine FTP sunucusuna erişmek için olmalıdır.

## <a name="get-started"></a>başlarken
Farklı araçlar veya API'leri kullanarak bir FTP kaynaktan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Data Factory Kopyalama Sihirbazı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araç veya API'lerden (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Bir FTP veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Azure blob için FTP sunucusundan kopyalama](#json-example-copy-data-from-ftp-server-to-azure-blob) bu makalenin.

> [!NOTE]
> Kullanmak için desteklenen dosya ve sıkıştırma biçimleri hakkında daha fazla ayrıntı için bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md).

Aşağıdaki bölümler, Data Factory varlıklarını belirli FTP tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, bir FTP bağlantılı hizmete özgü JSON öğeleri açıklar.

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| type |Bu işlem için Ftp_sunucusu ayarlayın. |Evet |&nbsp; |
| konak |Adını veya FTP sunucusunun IP adresini belirtin. |Evet |&nbsp; |
| authenticationType |Kimlik doğrulama türünü belirtin. |Evet |Temel, anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcının belirtin. |Hayır |&nbsp; |
| password |(Kullanıcı adı) kullanıcının parolasını belirtin. |Hayır |&nbsp; |
| encryptedCredential |FTP sunucusuna erişmek için şifrelenmiş kimlik bilgilerini belirtin. |Hayır |&nbsp; |
| gatewayName |Veri Yönetimi ağ geçidi şirket içi FTP sunucusuna bağlanmak için ağ geçidi adını belirtin. |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktasını belirtin. |Hayır |21 |
| enableSsl |Bir SSL/TLS kanalı üzerinden FTP kullanılıp kullanılmayacağını belirtin. |Hayır |true |
| enableServerCertificateValidation |FTP SSL/TLS kanalı üzerinden kullandığınızda, sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin. |Hayır |true |

>[!NOTE]
>FTP Bağlayıcısı yok şifreleme veya açık SSL/TLS şifrelemesi ile FTP sunucuya erişirken destekler. Bunu yapmak için SSL/TLS örtülü şifreleme desteklemiyor.

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

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Bağlantı noktası, enableSsl enableServerCertificateValidation kullanın

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
Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. **TypeProperties** türü için bir veri kümesi bölümünü **FileShare** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasörünün yolu. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasör yolları tabanlı slice başlama üzerinde olması ve bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Zaman **fileName** belirtilmemiş bir çıktı veri kümesi için oluşturulan dosya adı şu biçimde: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Bir alt dosya seçmek için kullanılacak bir filtre belirtin **folderPath**, tüm dosyalar yerine.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** girdi FileShare veri kümesi için geçerlidir. Bu özellik, Hadoop dağıtılmış dosya sistemi (HDFS ile) desteklenmiyor. |Hayır |
| partitionedBy |Dinamik belirtmek için kullanılan **folderPath** ve **fileName** zaman serisi verilerinin. Örneğin, belirtebileceğiniz bir **folderPath** veri her saat için parametreli. |Hayır |
| biçim | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> Dosyaları (ikili kopya) depoları arasında dosya tabanlı oldukları gibi kopyalamak istiyorsanız, hem girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**, ve desteklenen düzeyleri **Optimal** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağınızı belirtin. Değerler (Bu, varsayılan değer) ikili mod için true ve false ASCII için. Bu özellik, yalnızca ilişkili bağlantılı hizmet türü türü olduğunda kullanılabilir: FtpServer. |Hayır |

> [!NOTE]
> **fileName** ve **fileFilter** aynı anda kullanılamaz.

### <a name="use-the-partionedby-property"></a>PartionedBy özelliğini kullanın
Önceki bölümde belirtildiği gibi bir dinamik belirtebilirsiniz **folderPath** ve **fileName** ile zaman serisi verilerinin **partitionedBy** özelliği.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında bilgi edinmek için [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [komut zincirleri oluşturma](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte, {dilim} değişkeninin değeri Data Factory sistem SliceStart, belirtilen biçimde (YYYYMMDDHH) ile değiştirilir. Slicestart'taki saat diliminin başlangıç ifade eder. Klasör yolu için her bir dilimi farklıdır. (Örneğin, wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.)

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
Bu örnekte, yıl, ay, gün ve saati SliceStart tarafından kullanılan ayrı değişkenleri içine ayıklanan **folderPath** ve **fileName** özellikleri.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md). Ad, açıklama, girdi ve çıktı tabloları ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Bulunan özelliklerin **typeProperties** etkinlik bölümünü Öte yandan, her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için tür özellikleri, kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türünde olduğunda **FileSystemSource**, aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Alt klasörleri veya yalnızca belirtilen klasöre veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a>JSON örneği: Azure Blob için FTP sunucusundan veri kopyalama
Bu örnek, bir FTP sunucusundan Azure Blob depolama alanına veri kopyalama işlemi gösterilmektedir. Ancak, verileri doğrudan belirtilen havuzlarını hiçbirini kopyalanabilir [desteklenen veri depoları ve biçimler](data-factory-data-movement-activities.md#supported-data-stores-and-formats), veri fabrikasında kopyalama etkinliği kullanarak.

Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* Bağlı hizmet türü [Ftp_sunucusu](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [dosya paylaşımı](#dataset-properties)
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [işlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

Örnek verileri saatte bir Azure blobuna bir FTP sunucusundan kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="ftp-linked-service"></a>Bağlı FTP hizmeti

Bu örnek, kullanıcı adı ve parola düz metin ile temel kimlik doğrulaması kullanır. Aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Anonim kimlik doğrulaması
* Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması
* FTP (FTPS) SSL/TLS üzerinden

Bkz: [FTP bağlı hizmet](#linked-service-properties) bölüm için farklı türde kimlik doğrulaması kullanabilirsiniz.

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

Bu veri kümesi FTP klasörüne başvurur `mysharedfolder` ve dosya `test.csv`. İşlem hattı, dosyayı hedefe kopyalar.

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

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

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu dinamik olarak, işlenmekte olan dilimin başlangıç zamanı temel alınarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Dosya sistemi kaynak ve blob havuz ile bir işlem hattındaki kopyalama etkinliği

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **FileSystemSource**ve **havuz** türü ayarlandığında **BlobSink**.

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
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Veri taşıma (kopyalama etkinliği) Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

* Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
