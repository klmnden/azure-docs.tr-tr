---
title: Azure Data Factory kullanarak ODBC kaynaklardan veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak OData kaynaklardan desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.openlocfilehash: 14f654979f004186e81b2f18578ced3c9aab3815
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>İlk ve son Azure Data Factory kullanarak ODBC veri depolarını veri Kopyala
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-odbc-connector.md)
> * [Sürüm 2 - Önizleme](connector-odbc.md)

Bu makalede kopya etkinliği Azure Data Factory'de verileri ilk ve son bir ODBC veri deposuna kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 ODBC Bağlayıcısı](v1/data-factory-odata-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna ODBC kaynağından veri kopyalama veya ODBC havuz için tüm desteklenen kaynak veri deposundan kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu ODBC bağlayıcı/için veri kopyalamayı destekler **herhangi ODBC uyumlu veri depolarına** kullanarak **temel** veya **anonim** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Bu ODBC bağlayıcıyı kullanmak için aktarmanız gerekir:

- Self-hosted tümleştirme çalışma zamanı ayarlayın. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) Ayrıntılar için makale.
- Veri deposu için ODBC sürücüsü tümleştirmesi çalışma zamanı makineye yükleyin.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını ODBC bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler ODBC bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Odbc** | Evet |
| connectionString | Kimlik bilgisi bölümü hariç bağlantı dizesi. LIKE desen ile bağlantı dizesini belirtin `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, veya sistem ayarladığınız tümleştirmesi çalışma zamanı makinede DSN (veri kaynağı adı) kullanmak `"DSN=<name of the DSN on IR machine>;"` (hala kimlik bilgisi bölümü bağlantılı hizmetteki buna uygun olarak belirttiğiniz).| Evet |
| authenticationType | ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel** ve **anonim**. | Evet |
| Kullanıcı adı | Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. | Hayır |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan bir SecureString işaretleyin. | Hayır |
| kimlik bilgisi | Sürücü özgü özellik değer biçiminde belirtilen bağlantı dizesi erişim kimlik bilgisi bölümü. Örnek: `"RefreshToken=<secret refresh token>;"`. Bu alan bir SecureString işaretleyin. | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Bölümünde belirtildiği gibi bir Self-hosted tümleştirmesi çalışma zamanı gereklidir [Önkoşullar](#prerequisites). |Evet |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde bir ODBC veri kümesi tarafından desteklenen özellikler listesini sağlar.

Verileri/ODBC uyumlu veri deposuna kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | ODBC veri deposundaki tablonun adı. | Hayır ("etkinlik kaynağında sorgu" belirtilirse) kaynağı için;<br/>Havuz için Evet |

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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde ODBC kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="odbc-as-source"></a>ODBC kaynağı olarak

ODBC uyumlu veri deposundan verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | (Veri kümesinde "tableName" belirtilmişse) yok |

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

ODBC uyumlu veri deposuna verileri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **OdbcSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **OdbcSink** | Evet |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin.<br/>İzin verilen değerler: timespan. Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan: 0 - otomatik algılanan) |
| preCopyScript |Her çalıştırmada veri deposuna veri yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş veriyi temizlemek için bu özelliği kullanın. |Hayır |

> [!NOTE]
> (Otomatik olarak algılanır), ayarlanmamışsa "writeBatchSize" için kopyalama etkinliği ilk sürücü toplu işlemleri destekler ve varsa 10000'e ayarlayın veya içermiyorsa 1 olarak ayarlayın algılar. Açıkça 0 dışında bir değer ayarlarsanız, kopyalama etkinliği değeri geliştirir ve sürücü toplu işlemlerini desteklemediğinden çalışma zamanında başarısız olur.

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

Self-hosted tümleştirme çalışma zamanı, veri deposuna erişimi olan bir makinede ayarlayın. Tümleştirme çalışma zamanı Informix için ODBC sürücüsü veri deposuna bağlanmak için kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüyü yükleyin. Örneğin, sürücü "IBM INFORMİX ODBC sürücüsü (64 bit)" kullanabilirsiniz. Bkz: [Önkoşullar](#prerequisites) ayrıntıları bölümü.

Veri Fabrikası çözüm Informix kaynağında kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri depolama alanı bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir IBM Informix veri deposu için bir Azure data factory bağlamak için bir ODBC bağlantılı hizmet oluşturun:

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

ODBC veri kullanarak makale ayrıntılı bir genel bakış için en başından bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar okuyun.

## <a name="microsoft-access-source"></a>Microsoft Access kaynağı

Genel ODBC Bağlayıcısı'nı kullanarak Microsoft Access veritabanından veri kopyalayabilirsiniz.

Self-hosted tümleştirme çalışma zamanı, veri deposuna erişimi olan bir makinede ayarlayın. Tümleştirme çalışma zamanı Microsoft Access için ODBC sürücüsü veri deposuna bağlanmak için kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüyü yükleyin. Bkz: [Önkoşullar](#prerequisites) ayrıntıları bölümü.

Microsoft Access kaynağı bir veri fabrikası çözümde kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri depolama alanı bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir Azure data factory bir Microsoft Access veritabanına bağlanmak için bir ODBC bağlantılı hizmet oluşturun:

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

ODBC veri kullanarak makale ayrıntılı bir genel bakış için en başından bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar okuyun.

## <a name="ge-historian-source"></a>GE Historian kaynağı

GE genel ODBC Bağlayıcısı'nı kullanarak Historian veri kopyalayabilirsiniz.

Self-hosted tümleştirme çalışma zamanı, veri deposuna erişimi olan bir makinede ayarlayın. Tümleştirme çalışma zamanı GE Historian için ODBC sürücüsü veri deposuna bağlanmak için kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüyü yükleyin. Bkz: [Önkoşullar](#prerequisites) ayrıntıları bölümü.

Veri Fabrikası çözüm GE Historian kaynağında kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri depolama alanı bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir Azure data factory bir Microsoft Access veritabanına bağlanmak için bir ODBC bağlantılı hizmet oluşturun:

```json
{
    "name": "HistorianLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<GE Historian store connection string or DSN>"
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

ODBC veri kullanarak makale ayrıntılı bir genel bakış için en başından bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar okuyun.

## <a name="sap-hana-sink"></a>SAP HANA havuz

>[!NOTE]
>SAP HANA veri deposundan verileri kopyalamak için doğal olarak başvuran [SAP HANA bağlayıcı](connector-sap-hana.md). SAP HANA veri kopyalamak için lütfen ODBC bağlayıcısını kullanmak için bu yönergeleri izleyin. SAP HANA bağlayıcı ve ODBC bağlayıcı için bağlantılı Hizmetleri farklı türde Not böylece yeniden kullanılamaz.
>

Genel ODBC Bağlayıcısı'nı kullanarak SAP HANA veritabanına veri kopyalayabilirsiniz.

Self-hosted tümleştirme çalışma zamanı, veri deposuna erişimi olan bir makinede ayarlayın. Tümleştirme çalışma zamanı SAP HANA için ODBC sürücüsü veri deposuna bağlanmak için kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüyü yükleyin. Bkz: [Önkoşullar](#prerequisites) ayrıntıları bölümü.

SAP HANA havuz bir veri fabrikası çözümde kullanmadan önce tümleştirme çalışma zamanı'ndaki yönergeleri kullanarak veri depolama alanı bağlanıp bağlanamadığını doğrulayın [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümü.

Aşağıdaki örnekte gösterildiği gibi bir SAP HANA veri deposu için bir Azure data factory bağlamak için bir ODBC bağlantılı hizmet oluşturun:

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

ODBC veri kullanarak makale ayrıntılı bir genel bakış için en başından bir kopyalama işleminde kaynak/havuz veri depoları olarak depolar okuyun.

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bağlantı sorunlarını gidermek için kullanmak **tanılama** sekmesinde **tümleştirmesi çalışma zamanı Configuration Manager**.

1. Başlatma **tümleştirmesi çalışma zamanı Configuration Manager**.
2. Geçiş **tanılama** sekmesi.
3. "Test Bağlantısı" bölümü altında seçin **türü** (bağlantılı hizmeti) verilerini depolar.
4. Belirtin **bağlantı dizesi** seçin, veri deposuna bağlanmak için kullanılan **kimlik doğrulaması** ve girin **kullanıcı adı**, **parola**, ve/veya **kimlik bilgileri**.
5. Tıklatın **Bağlantıyı Sına** veri deposu test edin.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).