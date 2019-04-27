---
title: Azure Data Factory kullanarak ODBC kaynaklardan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına OData kaynaklardan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
ms.openlocfilehash: f14c8f8ef9f0e59ac35dd7346bf37cc07f2cfb19
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60711467"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Gelen ve ODBC veri depoları Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-odbc-connector.md)
> * [Geçerli sürüm](connector-odbc.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de ilk ve son bir ODBC veri deposuna veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

ODBC kaynağından tüm desteklenen havuz veri deposuna veri kopyalamak ya da ODBC havuz için herhangi bir desteklenen kaynak veri deposundan kopyalayın. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu ODBC bağlayıcı/BLOB'dan veri kopyalamayı destekler **herhangi bir ODBC uyumlu veri depoları** kullanarak **temel** veya **anonim** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Bu ODBC bağlayıcıyı kullanmak için yapmanız:

- Şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Veri deposu için ODBC sürücüsü, Integration Runtime makineye yükleyin.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli ODBC Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

ODBC bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **ODBC** | Evet |
| bağlantı dizesi | Kimlik bilgisi bölümü hariç olmak üzere bağlantı dizesi. Desen gibi ile bağlantı dizesini belirtin `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, veya sistem DSN ayarladığınız Integration Runtime makinede (veri kaynağı adı) kullanmak `"DSN=<name of the DSN on IR machine>;"` (hala kimlik bilgisi bölümü bağlı hizmette uygun şekilde belirttiğiniz).<br>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md).| Evet |
| authenticationType | ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **anonim**. | Evet |
| Kullanıcı adı | Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. | Hayır |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| kimlik bilgisi | Erişim kimlik bilgisi sürücüye özel özellik-değer biçiminde belirtilen bağlantı dizesi kısmı. Örnek: `"RefreshToken=<secret refresh token>;"`. Bu alanı bir SecureString işaretleyin. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek 1: Temel kimlik doğrulaması kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
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

**Örnek 2: Anonim kimlik doğrulamasını kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, ODBC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

/ ODBC uyumlu veri deposuna veri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | ODBC veri deposundaki tablosunun adı. | Hayır ("etkinlik kaynakta sorgulama" belirtilirse) kaynak için;<br/>Havuz için Evet |

**Örnek**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, ODBC kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="odbc-as-source"></a>ODBC kaynağı olarak

ODBC uyumlu veri deposundan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="odbc-as-sink"></a>ODBC havuz olarak

ODBC uyumlu veri deposuna veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **OdbcSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **OdbcSink** | Evet |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/>İzin verilen değerler: TimeSpan değeri. Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan değer 0 - otomatik algılanan) |
| preCopyScript |Her çalıştırma, veri deposuna veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |

> [!NOTE]
> (Otomatik olarak algılanan), ayarlanmamışsa "writeBatchSize" için kopyalama etkinliği ilk sürücü toplu işlemleri destekler ve varsa 10000'e ayarlayın veya bu gereksinimleri karşılamıyorsa 1 olarak ayarlayın. algılar. Açıkça 0 dışında bir değere ayarlarsanız, kopyalama etkinliği, değer geçerlidir ve sürücü toplu işlemleri desteklemeyen çalışma zamanında başarısız olur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="ibm-informix-source"></a>IBM Informix kaynak

Genel ODBC Bağlayıcısı'nı kullanarak IBM Informix veritabanından veri kopyalayabilirsiniz.

Data store erişimi olan bir makinede bir şirket içinde barındırılan tümleştirme çalışma zamanı ayarlayın. Integration Runtime, veri deposuna bağlanmak için Informix için ODBC sürücüsünü kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüsü yükleyin. Örneğin, sürücü "IBM INFORMİX ODBC sürücü (64-bit)" kullanabilirsiniz. Bkz: [önkoşulları](#prerequisites) ayrıntıları bölümü.

Data Factory çözümünde Informix kaynak kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri deposu bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir IBM Informix data store bir Azure veri fabrikanıza bağlamak üzere bir ODBC bağlı hizmet oluşturun:

```json
{
    "name": "InformixLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<Informix connection string or DSN>"
            },
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

Okuma ODBC veri kullanımının makaleyi baştan ayrıntılı bir genel bakış için bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar.

## <a name="microsoft-access-source"></a>Microsoft Access kaynağı

Genel ODBC Bağlayıcısı'nı kullanarak Microsoft Access veritabanından veri kopyalayabilirsiniz.

Data store erişimi olan bir makinede bir şirket içinde barındırılan tümleştirme çalışma zamanı ayarlayın. Integration Runtime, veri deposuna bağlanmak için Microsoft Access ODBC sürücüsünü kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüsü yükleyin. Bkz: [önkoşulları](#prerequisites) ayrıntıları bölümü.

Data Factory çözümünde Microsoft Access kaynağı kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri deposu bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir Microsoft Access veritabanına bir Azure data factory'ye bağlamak için bir ODBC bağlı hizmet oluşturun:

```json
{
    "name": "MicrosoftAccessLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={Microsoft Access Driver (*.mdb, *.accdb)};Dbq=<path to your DB file e.g. C:\\mydatabase.accdb>;"
            },
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

Okuma ODBC veri kullanımının makaleyi baştan ayrıntılı bir genel bakış için bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar.

## <a name="sap-hana-sink"></a>SAP HANA havuz

>[!NOTE]
>SAP HANA veri deposundan veri kopyalamak için doğal olarak başvuran [SAP HANA Bağlayıcısı](connector-sap-hana.md). SAP HANA veri kopyalamak için ODBC bağlayıcıyı kullanmak üzere bu yönergeyi izleyin. Bu nedenle Not SAP HANA Bağlayıcısı ve ODBC Bağlayıcısı için bağlı hizmetler farklı bir türle yeniden kullanılamaz.
>

Genel ODBC Bağlayıcısı'nı kullanarak SAP HANA veritabanı'na veri kopyalayabilirsiniz.

Data store erişimi olan bir makinede bir şirket içinde barındırılan tümleştirme çalışma zamanı ayarlayın. Integration Runtime, veri deposuna bağlanmak için SAP HANA ODBC sürücüsünü kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüsü yükleyin. Bkz: [önkoşulları](#prerequisites) ayrıntıları bölümü.

Data Factory çözümünde SAP HANA havuz kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri deposu bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir Azure veri fabrikası için bir SAP HANA veri deposuna bağlamak için bir ODBC bağlı hizmet oluşturun:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015"
            },
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

Okuma ODBC veri kullanımının makaleyi baştan ayrıntılı bir genel bakış için bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar.

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bağlantı sorunlarını gidermek için **tanılama** sekmesinde **Integration Runtime Yapılandırma Yöneticisi**.

1. Başlatma **Integration Runtime Yapılandırma Yöneticisi**.
2. Geçiş **tanılama** sekmesi.
3. "Bağlantıyı Sına" bölümü altında seçin **türü** (bağlı hizmet) verilerini depolar.
4. Belirtin **bağlantı dizesi** öğesini veri deposuna bağlanmak için kullanılan **kimlik doğrulaması** girin **kullanıcı adı**, **parola**, ve/veya **kimlik bilgilerini**.
5. Tıklayın **Bağlantıyı Sına** veri deposuna test edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
