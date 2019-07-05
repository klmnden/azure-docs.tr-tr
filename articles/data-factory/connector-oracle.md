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
ms.date: 06/25/2019
ms.author: jingwang
ms.openlocfilehash: 04f623a889a87c325b1f53e3b39656ca4b703961
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509238"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri gelen ve Oracle'a kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-oracle-connector.md)
> * [Geçerli sürüm](connector-oracle.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de ilk ve son bir Oracle veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna bir Oracle veritabanından veri kopyalayabilirsiniz. Ayrıca, tüm desteklenen kaynak veri deposundan bir Oracle veritabanına veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Oracle Bağlayıcısı destekler:

- Bir Oracle veritabanına'nın şu sürümleri:
  - Oracle 12c R1 (12,1)
  - Oracle 11g R1, R2 (11.1, 11.2)
  - Oracle 10g R1, R2 (10,1, 10.2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3'ü (8.1.7)
- Kullanarak verileri kopyalama **temel** veya **OID** kimlik doğrulamaları.
- Oracle kaynak paralel Kopyala. Bkz: [paralel Oracle kopyadan](#parallel-copy-from-oracle) ayrıntıları bölümü.

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

> [!TIP]
>
> Daha fazla bilgi [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümünü verimli bir şekilde veri bölümleme kullanarak Oracle'dan verileri yüklenemedi.

Verileri Oracle'dan kopyalamak için kopyalama etkinliği kaynak türü ayarlayın. **OracleSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **OracleSource**. | Evet |
| oracleReaderQuery | Verileri okumak için özel bir SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir.<br>Bölümlenmiş yük etkinleştirdiğinizde, sorgunuzda karşılık gelen yerleşik bölüm parametreyle bağlamanız gerekir. Örneklere bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü. | Hayır |
| partitionOptions | Verileri bölümleme verileri Oracle'dan yüklemek için kullanılan seçenekleri belirtir. <br>Değerler: izin ver: **Hiçbiri** (varsayılan), **PhysicalPartitionsOfTable** ve **DynamicRange**.<br>Bölüm seçeneği etkin olduğunda (değil ' None'), lütfen de yapılandırmanız **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** kopyalama etkinliği örneğin 4 olarak ayarlama, belirleyen verileri Oracle'dan eşzamanlı olarak yüklemek için paralel derecesi Veritabanı. | Hayır |
| partitionSettings | Veri bölümleme için ayar grubu belirtin. <br>Bölüm seçenek olmadığı durumlarda uygulama `None`. | Hayır |
| partitionNames | Kopyalanması gereken fiziksel bölümler listesi. <br>Bölüm seçeneği olduğunda geçerli `PhysicalPartitionsOfTable`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfTabularPartitionName` WHERE yan tümcesinde. Örnekte bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü. | Hayır |
| partitionColumnName | Kaynak sütunun adını **Tamsayı türünde** kullanılacak aralığı için paralel bir kopya olarak bölümleyerek. Belirtilmezse, tablonun birincil anahtarı algılandı ve bölüm sütunu kullanılan otomatik olarak oluşturulacak. <br>Bölüm seçeneği olduğunda geçerli `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfRangePartitionColumnName` WHERE yan tümcesinde. Örnekte bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü. | Hayır |
| partitionUpperBound | Veri kopyalamak için bölüm sütunu en büyük değeri. <br>Bölüm seçeneği olduğunda geçerli `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfRangePartitionUpbound` WHERE yan tümcesinde. Örnekte bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü. | Hayır |
| partitionLowerBound | Veri kopyalamak için bölüm sütunu en küçük değeri. <br>Bölüm seçeneği olduğunda geçerli `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfRangePartitionLowbound` WHERE yan tümcesinde. Örnekte bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü. | Hayır |

**Örnek: temel sorgu bölümü olmadan'ni kullanarak veri kopyalama**

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

Daha fazla örneklere bakın [paralel Oracle kopyadan](#parallel-copy-from-oracle) bölümü.

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

## <a name="parallel-copy-from-oracle"></a>Oracle paralel Kopyala

Veri Fabrikası Oracle Bağlayıcısı muhteşem bir performans ile paralel Oracle'dan veri kopyalamak için bölümleme yerleşik veri sağlar. Bulabilirsiniz Oracle kaynak kopyalama etkinliği verileri bölümleme Seçenekler ->:

![Bölüm seçenekleri](./media/connector-oracle/connector-oracle-partition-options.png)

Veri Fabrikası paralel sorgular bölümlenmiş kopyalama etkinleştirdiğinizde, Oracle kaynak bölümler tarafından verileri yüklemek için karşı çalışır. Paralel derece yapılandırılır ve gösterirken **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** kopyalama etkinliğinde ayarlama. Örneğin, ayarlarsanız `parallelCopies` dört, eşzamanlı olarak veri fabrikası oluşturur ve dört sorgularını çalıştırır, Oracle veritabanından veri alınırken her bölümü, belirtilen bölüm seçenek ve ayarlar alınarak.

Oracle veritabanı'ndan büyük miktarda veri yüklediğinizde özellikle bölümleme verilerle paralel kopyasını etkinleştirmek için önerilir. Farklı senaryolar için önerilen yapılandırmaları şunlardır:

| Senaryo                                                     | Önerilen ayarları                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fiziksel bölümlerle büyük tablosundan tam yük          | **Bölüm seçeneği**: Tablonun fiziksel bölümler. <br><br/>Yürütme sırasında data Factory otomatik olarak fiziksel bölümler algılamak ve bölümler tarafından veri kopyalayın. |
| Veri bölümleme için bir tamsayı sütunuyla sırasında fiziksel bölümler olmadan büyük tablosundan tam yük | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütun belirtin. Aksi durumda belirtilen birincil anahtar sütunu kullanılır. |
| Büyük miktarda veri özel sorgu altındaki fiziksel bölümlerle kullanarak yükleme | **Bölüm seçeneği**: Tablonun fiziksel bölümler.<br>**Sorgu**: `SELECT * FROM <TABLENAME> PARTITION("?AdfTabularPartitionName") WHERE <your_additional_where_clause>`.<br>**Bölüm adı**: Verileri kopyalamak için bölüm adlarını belirtin. Belirtilmezse, ADF Oracle veri kümesinde belirtilen tablonun fiziksel bölümleri otomatik olarak algılar.<br><br>Yürütme, veri fabrikası değiştirme sırasında `?AdfTabularPartitionName` Oracle Gönder ve gerçek bölüm adı. |
| Büyük miktarda veri özel sorgu altındaki bir tamsayı sütunuyla sırasında fiziksel bölümler olmadan veri bölümleme için kullanarak yükleme | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütun belirtin. Tamsayı veri türü ile sütun karşı bölümleyebilirsiniz.<br>**Bölüm üst sınır** ve **bölüm alt sınır**: Yalnızca alt ve üst aralık arasında veri almak için bölüm sütunu karşı filtrelemek isteyip istemediğinizi belirtin.<br><br>Yürütme, veri fabrikası değiştirme sırasında `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound`, ve `?AdfRangePartitionLowbound` gerçek sütun adı ve değer aralıkları her bölüm ve Oracle için gönderin. <br>Örneğin, bölüm sütunu "ID" alt sınırı 1 ile 4 olarak paralel kopya kümesi ile 80'i olarak üst sınır olarak ayarlarsanız ADF veri alma [21, 40], kimliği [1,20] arasında 4 bölüm tarafından [41, 60] ve [61, 80]. |

**Örnek: sorgu ile fiziksel bölüm**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> PARTITION(\"?AdfTabularPartitionName\") WHERE <your_additional_where_clause>",
    "partitionOption": "PhysicalPartitionsOfTable",
    "partitionSettings": {
        "partitionNames": [
            "<partitionA_name>",
            "<partitionB_name>"
        ]
    }
}
```

**Örnek: Sorgu dinamik aralık bölümü**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
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
