---
title: "Azure Data Factory kullanarak SFTP sunucudan veri taşıma | Microsoft Docs"
description: "Şirket içi veya Azure Data Factory kullanarak bulut SFTP sunucusundan veri taşıma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 48c56e2de40a3166f22db88850c6df2489a35e8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Azure Data Factory kullanarak bir SFTP sunucudan veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-sftp-connector.md)
> * [Sürüm 2 - Önizleme](../connector-sftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 SFTPconnector](../connector-sftp.md).

Bu makalede kopya etkinliği Azure Data Factory'de desteklenen havuz veri deposu için bir şirket içi/bulut SFTP sunucusundan veri taşımak için nasıl kullanılacağı açıklanmaktadır. Bu makalede derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği ve kaynakları/havuzlarını desteklenen veri depoları listesi ile veri taşıma için genel bir bakış sunar.

Veri Fabrikası şu anda yalnızca veri taşımayı SFTP sunucusundan diğer veri depolarına, ancak verileri diğer veri depolarına bir SFTP sunucuya taşımak için değil de destekler. Her iki şirket içi destekler ve SFTP sunucuları bulut.

> [!NOTE]
> Hedefe başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosyasını silmez. Kaynak dosya sonra başarılı bir kopyasını silmeniz gerekirse, dosyayı silin ve ardışık düzeninde etkinlik kullanmak için özel bir aktivite oluşturun. 

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri
Bu SFTP bağlayıcı veri kopyalamak için kullanabileceğiniz **SFTP sunucuları ve şirket içi SFTP sunucuları hem de bulut**. **Temel** ve **SshPublicKey** SFTP sunucuya bağlanırken kimlik doğrulama türleri desteklenir.

Bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında şirket içi ortamına/Azure VM veri yönetimi ağ geçidi yüklemeniz gerekir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) ağ geçidi hakkında ayrıntılı bilgi için. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale ağ geçidi kurun ayarlama ve kullanmaya ilişkin adım adım yönergeler.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir SFTP kaynaktan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. JSON örnekleri verileri Azure Blob depolama alanına SFTP sunucudan kopyalamak, bkz: [JSON örnek: veri kopyalama SFTP sunucusundan Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) bu makalenin.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri bağlantılı hizmet FTP belirli açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| type | Type özelliği ayarlamak `Sftp`. |Evet |
| ana bilgisayar | SFTP sunucunun adı veya IP adresi. |Evet |
| port |SFTP sunucunun dinlediği bağlantı noktası. Varsayılan değer: 21 |Hayır |
| authenticationType |Kimlik doğrulama türü belirtin. İzin verilen değerler: **temel**, **SshPublicKey**. <br><br> Başvurmak [kullanarak temel kimlik doğrulaması](#using-basic-authentication) ve [kullanarak SSH ortak anahtar kimlik doğrulaması](#using-ssh-public-key-authentication) daha fazla özellikleri ve JSON örnekleri sırasıyla bölümler. |Evet |
| skipHostKeyValidation | Ana anahtar doğrulama atlamak bu seçeneği belirtin. | Hayır. Varsayılan değeri: false |
| hostKeyFingerprint | Ana makine anahtarı, parmak izi belirtin. | Yanıt Evet ise `skipHostKeyValidation` false olarak ayarlayın.  |
| gatewayName |Bir şirket içi SFTP sunucusuna bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi SFTP sunucusundan veri kopyalama, Evet. |
| encryptedCredential | SFTP sunucuya erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim temel kimlik doğrulaması (kullanıcı adı + parola) veya SshPublicKey kimlik (kullanıcı adı + özel anahtar yolu veya içerik) belirttiğinizde. | Hayır. Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `Basic`ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | (Kullanıcı adı) kullanıcının parolası. | Evet |

#### <a name="example-basic-authentication"></a>Örnek: Temel kimlik doğrulaması
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: Temel kimlik doğrulaması ile şifrelenmiş kimlik bilgileri

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH ortak anahtar kimlik doğrulaması kullanma

SSH ortak anahtar kimlik doğrulaması kullanmak üzere ayarlanmış `authenticationType` olarak `SshPublicKey`ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- | --- |
| kullanıcı adı |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Belirtin özel anahtar dosyasının mutlak yolu, ağ geçidi erişebilir. | Belirtin `privateKeyPath` veya `privateKeyContent`. <br><br> Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |
| privateKeyContent | Özel anahtar içeriğinin seri hale getirilmiş bir dize. Kopyalama Sihirbazı'nı, özel anahtar dosyası okumak ve özel anahtar içeriği otomatik olarak ayıklayın. Tüm diğer aracı/SDK kullanıyorsanız, bunun yerine privateKeyPath özelliğini kullanın. | Belirtin `privateKeyPath` veya `privateKeyContent`. |
| Parola | Geçişi tümcecik/anahtar dosyası bir parola deyimi tarafından korunuyorsa, özel anahtarın şifresini çözmek için parola belirtin. | Evet, özel anahtar dosyası bir parola deyimi tarafından korunuyorsa. |

> [!NOTE]
> SFTP Bağlayıcısı yalnızca OpenSSH anahtarını destekler. Anahtar dosyası doğru biçimde olduğundan emin olun. .ppk OpenSSH biçimine dönüştürmek için Putty aracını kullanabilirsiniz.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Örnek: özel anahtar filePath kullanarak SshPublicKey kimlik doğrulaması

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: özel anahtar içeriği kullanarak SshPublicKey kimlik doğrulaması

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümü, her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. TypeProperties bölüm türü veri kümesi için **FileShare** veri kümesine aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasörün alt yolu. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik ile HDFS desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örneğin, her veri saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: True. İlişkili bağlantılı hizmet türü türü olduğunda bu özellik yalnızca kullanılabilir: Ftp_sunucusu. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi dinamik bir folderPath, zaman serisi veri partitionedBy ile dosya adını belirtebilirsiniz. Veri Fabrikası makroları ve sistem değişkeni SliceStart, verilen veri dilimi için mantıksal süre belirtmek SliceEnd bunu yapabilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında bilgi edinmek için [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [oluşturma ardışık düzen](data-factory-create-pipelines.md) makaleleri.

#### <a name="sample-1"></a>Örnek 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {dilim} belirtilen veri fabrikası sistem değişkenin değerini SliceStart (YYYYMMDDHH) biçiminde ile değiştirilir. Dilimin başlangıç SliceStart başvuruyor. FolderPath her dilim için farklıdır. Örnek: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

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
Bu örnekte, folderPath ve fileName özellikleri tarafından kullanılan ayrı değişkenleri içine yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için tür özellikleri türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

[!INCLUDE [data-factory-file-system-source](../../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md) makale ayrıntıları.

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a>JSON örnek: veri SFTP sunucusundan Azure blob kopyalama
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar veri SFTP kaynaktan Azure Blob depolama alanına kopyalama gösterilmektedir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

> [!IMPORTANT]
> Bu örnek, JSON parçacıklarını sağlar. Data factory oluşturmak için adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [sftp](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [FileShare](#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri saatte bir Azure blob bir SFTP sunucusundan kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**SFTP bağlı hizmet**

Bu örnekte, kullanıcı adı ve parola düz metin ile temel kimlik doğrulaması kullanır. Aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması
* SSH ortak anahtar kimlik doğrulaması

Bkz: [FTP bağlantılı hizmeti](#linked-service-properties) bölümü için farklı tür kimlik doğrulaması kullanabilirsiniz.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Azure Storage bağlı hizmeti**

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
**SFTP girdi veri kümesi**

Bu veri kümesi SFTP klasöre başvuruyor `mysharedfolder` ve dosya `test.csv`. Ardışık Düzen dosya hedefe kopyalar.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Azure Blob dataset çıktı**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopyalama etkinliği ile kanalı**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **FileSystemSource** ve **havuz** türü ayarlanmış **BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki makalelere bakın:

* [Kopya etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için.
