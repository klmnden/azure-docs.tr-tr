---
title: SFTP sunucusundan Azure Data Factory ile verileri taşıma | Microsoft Docs
description: Şirket içi veya Bulut SFTP sunucusundan Azure Data Factory ile veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: fe253feca6a22ee0177082e178f897c5b634bb3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61257206"
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Azure Data Factory kullanarak bir SFTP sunucusundan veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-sftp-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de SFTPconnector](../connector-sftp.md).

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir desteklenen havuz veri deposu için bir şirket içi/bulut SFTP sunucusundan verileri taşımak için nasıl kullanılacağını özetlenmektedir. Bu makalede yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ve kaynakları/havuz desteklenen veri depolarının listesi ile veri taşıma genel bir bakış sunan makalesi.

Data factory şu anda yalnızca veri taşımayı bir SFTP sunucusundan verileri diğer veri depolarına bir SFTP sunucusuna taşımak için değil ancak diğer veri depolarını destekler. Hem şirket içi destekler ve SFTP sunucuları bulut.

> [!NOTE]
> Hedefine başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosya silinmez. Kopyalama başarılı sonra kaynak dosyayı silmek için ihtiyacınız varsa dosyayı silin ve işlem hattı, etkinlik kullanmak için özel bir etkinlik oluşturun.

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri
Bu SFTP Bağlayıcısı veri kopyalamak için kullanabileceğiniz **SFTP sunucusu ve şirket içi SFTP sunucusu hem de bulut**. **Temel** ve **SshPublicKey** SFTP sunucusuna bağlanırken kimlik doğrulaması türleri desteklenir.

Bir şirket içi SFTP sunucusundan veri kopyalama yapılırken, şirket içi ortam/Azure VM veri yönetimi ağ geçidi yüklemeniz gerekir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) ağ geçidi hakkında ayrıntılı bilgi için. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale ağ geçidini ayarlamadan ve kullanmaya ilişkin adım adım yönergeler.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir SFTP kaynaktan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. SFTP sunucusundan verileri Azure Blob Depolama'ya kopyalamak JSON örnekleri için bkz [JSON örneği: Verileri Azure blob için SFTP sunucusundan kopyalama](#json-example-copy-data-from-sftp-server-to-azure-blob) bu makalenin.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, bağlı hizmet FTP özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type | Type özelliği ayarlanmalıdır `Sftp`. |Evet |
| host | SFTP sunucusunun adı veya IP adresi. |Evet |
| port |SFTP sunucusunun dinlediği bağlantı noktası. Varsayılan değerdir: 21 |Hayır |
| authenticationType |Kimlik doğrulama türünü belirtin. İzin verilen değerler: **Temel**, **SshPublicKey**. <br><br> Başvurmak [kullanarak temel kimlik doğrulaması](#using-basic-authentication) ve [kullanarak SSH ortak anahtarı kimlik doğrulaması](#using-ssh-public-key-authentication) daha fazla özellik ve JSON örneklerini sırasıyla bölümler. |Evet |
| skipHostKeyValidation | Konak anahtar doğrulamasını atlayın verilip verilmeyeceğini belirtin. | Hayır. Varsayılan değer: false |
| hostKeyFingerprint | Konak anahtarı parmak izi belirtin. | Yanıt Evet ise `skipHostKeyValidation` false olarak ayarlanır.  |
| gatewayName |Şirket içi SFTP sunucusuna bağlanmak için veri yönetimi ağ geçidi adı. | Bir şirket içi SFTP sunucusundan veri kopyalama, Evet. |
| encryptedCredential | SFTP sunucusuna erişmek için şifrelenmiş kimlik bilgileri'ı seçin. Otomatik olarak oluşturulan temel kimlik doğrulaması (kullanıcı adı ve parola) ya da SshPublicKey kimlik doğrulaması (kullanıcı adı ve özel anahtar yolu veya içerik) Kopyalama Sihirbazı'nı veya ClickOnce açılan iletişim kutusunda belirttiğiniz zaman. | Hayır. Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak için ayarlanmış `authenticationType` olarak `Basic`ve SFTP Bağlayıcısı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| username | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | (Kullanıcı adı) kullanıcı parolası. | Evet |

#### <a name="example-basic-authentication"></a>Örnek: Temel kimlik doğrulama
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması

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

### <a name="using-ssh-public-key-authentication"></a>SSH ortak anahtarı kimlik doğrulaması kullanma

SSH ortak anahtar kimlik doğrulamasını kullanmak için ayarlanmış `authenticationType` olarak `SshPublicKey`ve SFTP Bağlayıcısı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| username |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Belirtin özel anahtar dosyasının mutlak yolu, ağ geçidinin erişebilir. | Seçeneklerinden birini belirtin `privateKeyPath` veya `privateKeyContent`. <br><br> Yalnızca bir şirket içi SFTP sunucusundan veri kopyalama işlemi sırasında uygulanır. |
| privateKeyContent | Özel anahtar içeriği seri hale getirilmiş bir dize. Kopyalama Sihirbazı'nı, özel anahtar dosyasını okuma ve özel anahtar içeriği otomatik olarak ayıklayın. Herhangi diğer araca/SDK'ya kullanıyorsanız privateKeyPath özelliğini kullanın. | Seçeneklerinden birini belirtin `privateKeyPath` veya `privateKeyContent`. |
| passPhrase | Geçişi tümcecik/anahtar dosyası bir parola deyimi tarafından korunuyorsa, özel anahtarın şifresini çözmek için parola belirtin. | Özel anahtar dosyasını bir parola deyimi tarafından korunuyorsa, Evet. |

> [!NOTE]
> SFTP Bağlayıcısı RSA/DSA OpenSSH anahtarını destekler. "---Başlangıç RSA/DSA özel ANAHTARA sahip---" anahtar dosyası içeriğinizi başlar emin olun. Özel anahtar dosyası ppk-format dosyası ise, .ppk OpenSSH biçimine dönüştürmek için Putty aracını kullanın.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Örnek: Özel anahtar dosya yolu kullanarak SshPublicKey kimlik doğrulaması

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: Özel anahtar içeriğini kullanarak SshPublicKey kimlik doğrulaması

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
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. TypeProperties bölüm türü için bir veri kümesi **FileShare** veri kümesi, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasörünün yolu. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bağlı örnek hizmet ve veri kümesi tanımları örnekler için bkz.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Oluşturulan dosyanın adını bir çıktı veri kümesi için dosya adı belirtilmediği durumlarda, aşağıdaki olacaktır bu biçimi: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>1. örnekler: `"fileFilter": "*.log"`<br/>Örnek 2: `"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter girdi FileShare veri kümesi için geçerlidir. Bu özellik, HDFS ile desteklenmiyor. |Hayır |
| partitionedBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, verilerin her saat için parametreli folderPath. |Hayır |
| format | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |
| useBinaryTransfer |Belirtin olup ikili aktarım modunu kullanın. İkili mod ve false ASCII için true. Varsayılan değer: TRUE. Bu özellik, yalnızca ilişkili bağlantılı hizmet türü türü olduğunda kullanılabilir: FtpServer. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath, partitionedBy ile zaman serisi verileri için dosya adı belirtebilirsiniz. Data Factory makroları ve sistem değişkeni SliceStart, mantıksal bir zaman aralığı için belirtilen veri dilimi belirtmek SliceEnd ile bunu yapabilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında bilgi edinmek için [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [işlem hatları oluşturma](data-factory-create-pipelines.md) makaleler.

#### <a name="sample-1"></a>Örnek 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {dilim} belirtilen değeri (YYYYMMDDHH) biçiminde Data Factory sistem değişkeni SliceStart ile değiştirilir. Slicestart'taki saat diliminin başlangıç ifade eder. FolderPath için her bir dilimi farklıdır. Örnek: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

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
Bu örnekte, folderPath ve dosya adı özellikleri tarafından kullanılan ayrı değişkenleri içine yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için tür özellikleri, kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

[!INCLUDE [data-factory-file-system-source](../../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md) ilişkin ayrıntıları.

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a>JSON örneği: Azure blob SFTP sunucusundan veri kopyalayın
Aşağıdaki örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar Azure Blob depolama alanına SFTP kaynaktan veri kopyalama işlemini göstermektedir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtilen havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [sftp](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [FileShare](#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri saatte bir Azure blobuna bir SFTP sunucusundan kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**SFTP bağlı hizmeti**

Bu örnek, kullanıcı adı ve parola düz metin ile temel kimlik doğrulaması kullanır. Aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Şifrelenmiş kimlik bilgileri ile temel kimlik doğrulaması
* SSH ortak anahtarı kimlik doğrulaması

Bkz: [FTP bağlı hizmet](#linked-service-properties) bölüm için farklı türde kimlik doğrulaması kullanabilirsiniz.

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

Bu veri kümesi SFTP klasörüne başvurur `mysharedfolder` ve dosya `test.csv`. İşlem hattı, dosyayı hedefe kopyalar.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

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

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

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

**Kopyalama etkinliği ile işlem hattı**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **FileSystemSource** ve **havuz** türü ayarlandığında **BlobSink**.

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
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki makalelere bakın:

* [Kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmaya yönelik adım adım yönergeler için.
