---
title: Azure Data Factory kullanarak SFTP sunucusundan veri kopyalama | Microsoft Docs
description: "Bir havuz desteklenen bir veri deposu için bir SFTP sunucusundan veri kopyalamanıza olanak veren bir Azure veri fabrikası'nda MySQL Bağlayıcısı hakkında bilgi edinin."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 1d90bafebcd942454b31d0f62b0daf3f01ad6926
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-sftp-server-using-azure-data-factory"></a>Azure Data Factory kullanarak SFTP sunucusundan veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-sftp-connector.md)
> * [Sürüm 2 - Önizleme](connector-sftp.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir SFTP sunucusundan veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 SFTP Bağlayıcısı](v1/data-factory-sftp-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

SFTP sunucudan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SFTP bağlayıcı destekler:

- Dosya kopyalarken kullanarak **temel** veya **SshPublicKey** kimlik doğrulaması.
- Dosyaları olarak kopyalama- ya da dosyalarıyla ayrıştırma [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler için SFTP Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı SFTP hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Sftp**. |Evet |
| konak | SFTP sunucunun adı veya IP adresi. |Evet |
| port | SFTP sunucunun dinlediği bağlantı noktası.<br/>İzin verilen değerler: tamsayı, varsayılan değer: **22**. |Hayır |
| skipHostKeyValidation | Ana anahtar doğrulama atlamak bu seçeneği belirtin.<br/>İzin verilen değerler: **true**, **false** (varsayılan).  | Hayır |
| hostKeyFingerprint | Ana makine anahtarı, parmak izi belirtin. | "SkipHostKeyValidation" false olarak ayarlarsanız Evet.  |
| authenticationType | Kimlik doğrulama türü belirtin.<br/>İzin verilen değerler: **temel**, **SshPublicKey**. Başvurmak [kullanarak temel kimlik doğrulaması](#using-basic-authentication) ve [kullanarak SSH ortak anahtar kimlik doğrulaması](#using-ssh-public-key-authentication) daha fazla özellikleri ve JSON örnekleri sırasıyla bölümler. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulaması kullanma

Temel kimlik doğrulaması kullanmak için "authenticationType" özelliğini ayarlamak **temel**ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | (Kullanıcı adı) kullanıcının parolası. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |

**Örnek:**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH ortak anahtar kimlik doğrulaması kullanma

SSH ortak anahtar kimlik doğrulaması kullanmak üzere ayarlanmış "authenticationType" özelliği olarak **SshPublicKey**ve SFTP bağlayıcı son bölümde sunulan genel kaynakların yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Tümleştirme çalışma zamanı erişmek için özel anahtar dosyasının mutlak yolu belirtin. Yalnızca kendi kendini barındıran türü Integration zamanının "connectVia" belirtildiğinde geçerlidir. | Belirtin `privateKeyPath` veya `privateKeyContent`.  |
| privateKeyContent | Base64 olarak kodlanmış SSH özel anahtar içeriği. SSH özel anahtarı, OpenSSH biçiminde olmalıdır. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Belirtin `privateKeyPath` veya `privateKeyContent`. |
| Parola | Geçişi tümcecik/anahtar dosyası bir parola deyimi tarafından korunuyorsa, özel anahtarın şifresini çözmek için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet, özel anahtar dosyası bir parola deyimi tarafından korunuyorsa. |

> [!NOTE]
> SFTP Bağlayıcısı yalnızca OpenSSH anahtarını destekler. Anahtar dosyası doğru biçimde olduğundan emin olun. .ppk OpenSSH biçimine dönüştürmek için Putty aracını kullanabilirsiniz.

**Örnek 1: özel anahtar filePath kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: özel anahtar içeriği kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde SFTP veri kümesi tarafından desteklenen özellikler listesini sağlar.

SFTP verileri kopyalamak için kümesine tür özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **dosya paylaşımı** |Evet |
| folderPath | Klasör yolu. Örneğin: klasör/alt / |Evet |
| fileName | Dosya adını belirtin **folderPath** belirli bir dosyadan kopyalamak istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları kaynak olarak işaret eder. |Hayır |
| fileFilter | Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin. Yalnızca dosya adı değil belirtildiğinde geçerlidir. <br/><br/>Joker karakterler izin verilir: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/>-Örnek 1:`"fileFilter": "*.log"`<br/>-Örnek 2:`"fileFilter": 2017-09-??.txt"` |Hayır |
| Biçimi | İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>Belirli bir biçime sahip dosyaları ayrıştırma istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

**Örnek:**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SFTPDataset",
    "type": "Datasets",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde SFTP kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sftp-as-source"></a>Kaynak olarak SFTP

SFTP verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **FileSystemSource** |Evet |
| Özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanır ve oluşturulan olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
