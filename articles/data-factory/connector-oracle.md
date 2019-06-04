---
title: Azure Data Factory kullanarak verileri ve Oracle kopyalamak | Microsoft Docs
description: Data Factory kullanarak bir Oracle veritabanına desteklenen kaynak depolardan gelen veya desteklenen havuz depoları için Oracle veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: 3fa7612b9e4cd8a714e60879229bd0d39349494f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405945"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri gelen ve Oracle'a kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-onprem-oracle-connector.md)
> * [Geçerli sürüm](connector-oracle.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de ilk ve son bir Oracle veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna bir Oracle veritabanından veri kopyalayabilirsiniz. Ayrıca, tüm desteklenen kaynak veri deposundan bir Oracle veritabanına veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Oracle Bağlayıcısı bir Oracle veritabanına aşağıdaki sürümlerini destekler. Ayrıca, temel veya OID kimlik doğrulamaları destekler:

- Oracle 12c R1 (12,1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10,1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3'ü (8.1.7)

> [!Note]
> Oracle Ara sunucu desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Gelen ve genel olarak erişilebilir olmayan bir Oracle veritabanına veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Tümleştirme çalışma zamanı hakkında daha fazla bilgi için bkz. [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md). Tümleştirme çalışma zamanı, yerleşik bir Oracle sürücüsü sağlar. Bu nedenle, ilk ve son Oracle veri kopyaladığınızda, bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Oracle Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler Oracle bağlı hizmeti için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **Oracle**. | Evet |
| connectionString | Oracle veritabanına bağlanmak için gereken bilgileri belirtir. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Parola Azure anahtar kasası ve çekme koyabilirsiniz `password` yapılandırma bağlantı dizesini dışında. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. <br><br>**Bağlantı türü desteklenen**: Kullanabileceğiniz **Oracle SID** veya **Oracle hizmet adı** veritabanınızı tanımlamak için:<br>-SID'ı kullanıyorsanız: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Hizmet adı kullanıyorsanız: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!TIP]
>Hata bildiren ulaşırsanız "ORA-01025: UPI parametre aralık dışında"ve Oracle Sürüm 8i, ekleme `WireProtocolMode=1` bağlantı dizesi ve yeniden deneyin.

**Oracle bağlantı şifrelemesini etkinleştirmek için**, iki seçeneğiniz vardır:

1.  Kullanılacak **Üçlü DES şifrelemesi (3DES) ve Gelişmiş Şifreleme Standardı (AES)** , Oracle Gelişmiş Güvenlik (OAS) için Oracle sunucu tarafında gidin ve şifreleme ayarlarını yapılandırın, ayrıntılara bakın [burada](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). ADF Oracle Bağlayıcısı otomatik olarak Oracle bağlantı kurulurken OAS yapılandırma birini kullanmak için şifreleme yöntemini görüşür.

2.  Kullanılacak **SSL**, aşağıdaki adımları izleyin:

    1.  SSL sertifika bilgileri alın. DER ile kodlanmış sertifika bilgileri, SSL sertifikası alın ve çıkış kaydedin (---sertifika başlayın... Son sertifika---) bir metin dosyası olarak.

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        **Örnek:** DERcert.cer sertifika bilgileri ayıklayın; daha sonra çıktı cert.txt için kaydedin

        ```
        openssl x509 -inform DER -in DERcert.cer -text
        Output:
        -----BEGIN CERTIFICATE-----
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXX
        -----END CERTIFICATE-----
        ```
    
    2.  Anahtar deposu veya truststore oluşturun. Aşağıdaki komutu truststore dosyayı içeren veya içermeyen PKCS-12 biçiminde bir parola oluşturur.

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        **Örnek:** bir parolayla MyTrustStoreFile adlı PKCS12 truststore dosyası oluşturur

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  Makinede şirket içinde barındırılan IR, örneğin C:\MyTrustStoreFile truststore dosyası yerleştirin.
    4.  Oracle bağlantı dizesiyle ADF içinde yapılandırma `EncryptionMethod=1` ve karşılık gelen `TrustStore` / `TrustStorePassword`değer, örneğin `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`.

**Örnek:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: parola Azure Key Vault'ta depolama**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Oracle veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

İlk ve son Oracle veri kopyalamak için dataset öğesinin type özelliği ayarlamak **OracleTable**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **OracleTable**. | Evet |
| tableName |Tabloda bir Oracle veritabanına başvuran bağlı hizmetin adı. | Evet |

**Örnek:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Oracle kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="oracle-as-a-source-type"></a>Oracle kaynak türü

Verileri Oracle'dan kopyalamak için kopyalama etkinliği kaynak türü ayarlayın. **OracleSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **OracleSource**. | Evet |
| oracleReaderQuery | Verileri okumak için özel bir SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Hayır |

"OracleReaderQuery" belirtmezseniz, sütunları veri kümesi "yapı" bölümünde tanımlanan bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) Oracle veritabanına karşı çalıştırılacak. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Bir havuz türü olarak Oracle

Oracle için veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **OracleSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **OracleSink**. | Evet |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler şunlardır: tamsayı (satır sayısı). |Hayır (varsayılan: 10.000) |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/>Zaman aralığı izin verilen değerler. 00:30:00 (30 dakika) bir örnektir. | Hayır |
| preCopyScript | Her bir çalıştırmada Oracle veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanabilirsiniz. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Eşleme için Oracle veri türü

İlk ve son Oracle veri kopyaladığınızda, aşağıdaki eşlemeler Oracle veri türlerinden veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemelerini nasıl hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Oracle veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>(yalnızca Oracle 10 g desteklenir ve üzeri) |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimal, String (olursa hassasiyet > 28) |
| INTEGER |Decimal, String (olursa hassasiyet > 28) |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Decimal, String (olursa hassasiyet > 28) |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |String |
| TIMESTAMP WITH TIME ZONE |String |
| UNSIGNED INTEGER |Number |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Veri türleri ARALIĞI Yıl Bitiş ayı ve günü bitiş ARALIĞI ikinci desteklenmiyor.


## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
