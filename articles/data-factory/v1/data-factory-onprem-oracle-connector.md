---
title: Veri kopyalama/Data Factory kullanarak Oracle | Microsoft Docs
description: "Öğesine/öğesinden veritabanını Azure Data Factory kullanarak şirket içi Oracle veri kopyalama öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 09850217018321f67e2e20270aadd054258c90a2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Öğesine/öğesinden Azure Data Factory kullanarak şirket içi Oracle veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 - Önizleme](../connector-oracle.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Oracle Bağlayıcısı](../connector-oracle.md).


Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi Oracle veritabanından/gelen verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **bir Oracle veritabanından** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **bir Oracle veritabanına**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Ön koşullar
Data Factory veri yönetimi ağ geçidi kullanarak şirket içi Oracle kaynaklarına bağlanma destekler. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında bilgi edinmek için makale ve [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) ağ geçidi kurun veri ardışık ayarlamak adım adım yönergeler için makalenin verileri taşıyın.

Bir Azure Iaas sanal Oracle barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanıp sürece veri deposu olarak aynı Iaas VM veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) ilgili sorunlar bağlantı/ağ geçidi sorun giderme ipuçları için.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Bu Oracle bağlayıcı sürücülerin iki sürümlerini destekler:

- **(Önerilen) Oracle için Microsoft sürücüsü**: veri yönetimi ağ geçidi sürümü 2.7, sürücü Oracle otomatik olarak ağ geçidi ile birlikte yüklenir, ayrıca gerek kalmaması için işlemek için sürücü Microsoft başlayarak Oracle bağlantı kurar ve bu sürücü kullanarak daha iyi kopyalama performansını da karşılaşabilirsiniz. Oracle sürümleri veritabanları desteklenir:
    - Oracle 12c R1 (12,1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10,1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!IMPORTANT]
> Şu anda Oracle için Microsoft sürücüsü yalnızca Oracle ancak için Oracle yazılamıyor veri kopyalamayı destekler. Ve veri yönetimi ağ geçidi tanılama sekmesinde test bağlantısı özelliği bu sürücüyü desteklemiyor not edin. Alternatif olarak, bağlantıyı doğrulamak için kopyalama Sihirbazı'nı kullanabilirsiniz.
>

- **.NET için Oracle veri sağlayıcısı:**  /Oracle veri kopyalamak için Oracle veri sağlayıcısı kullanmayı da seçebilirsiniz. Bu bileşen dahil [için Oracle veri erişim bileşenleri Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Uygun sürüm (32/64 bit), ağ geçidinin yüklü olduğu makineye yükleyin. [Oracle veri sağlayıcısı .NET 12,1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) Oracle veritabanına 10 g sürüm 2 veya sonrasını erişebilirsiniz.

    "XCopy yükleme" seçerseniz, readme.htm adımları izleyin. Kullanıcı Arabirimi (olmayan-XCopy biri) ile yükleyici seçtiğiniz öneririz.

    Sağlayıcı yüklendikten sonra **yeniden** Hizmetleri uygulaması (veya) veri yönetimi ağ geçidi Yapılandırma Yöneticisi kullanarak makinenizde veri yönetimi ağ geçidi ana bilgisayar hizmeti.  

Kopyalama işlem hattını yazmak için kopyalama Sihirbazı'nı kullanırsanız sürücü türünü otomatik olarak belirlenir. Microsoft sürücüsü, ağ geçidi sürümü 2.7 düşük olduğu veya Oracle havuz olarak seçtiğiniz sürece varsayılan olarak kullanılır.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Oracle veritabanından/gelen verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, bir Azure blob depolama alanına bir Oralce veritabanından veri kopyalıyorsanız, Oracle veritabanı ve Azure depolama hesabı veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Oracle için özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.
3. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte, Oracle veritabanınız giriş verileri içeren tablo belirtmek için bir veri kümesi oluşturun. Ve blob kapsayıcısında ve Oracle veritabanından kopyalanan verileri tutan klasör belirtmek için başka bir veri kümesi oluşturun. Oracle için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte OracleSource bir kaynak ve BlobSink havuzu olarak kopya etkinliği için kullanırsınız. Oracle veritabanına Azure Blob depolama alanından kopyalıyorsanız benzer şekilde, BlobSource ve OracleSink kopyalama etkinliği kullanırsınız. Oracle veritabanına belirli kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  / Bir şirket içi Oracle veritabanından veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-oracle-database) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri Oracle bağlantılı hizmete özgü açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesOracle** |Evet |
| driverType | Hangi sürücünün/Oracle veritabanına veri kopyalamak için kullanılacağını belirtin. İzin verilen değerler **Microsoft** veya **ODP** (varsayılan). Bkz: [desteklenen sürümü ve yükleme](#supported-versions-and-installation) sürücü ayrıntıları bölümü. | Hayır |
| connectionString | ConnectionString özelliği için Oracle veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| gatewayName | Ağ geçidinin adı, şirket içi Oracle sunucusuna bağlanmak için kullanılır |Evet |

**Örnek: Microsoft sürücüsü kullanma:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Örnek: ODP sürücü kullanma**

Başvurmak [bu site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) izin verilen biçimler için.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Oracle, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. Veri kümesi için typeProperties bölüm türü OracleTable aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Oracle veritabanında bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **oracleReaderQuery** , **OracleSource** belirtilir) |

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği yalnızca bir girdi alır ve tek bir çıktı üretir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

### <a name="oraclesource"></a>OracleSource
Kopyalama etkinliğinde kaynak türü olduğunda **OracleSource** aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * from MyTable <br/><br/>Belirtilmezse, yürütülen SQL deyimi: seçin * from MyTable |Hayır (varsa **tableName** , **dataset** belirtilir) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: 00:30:00 (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Sorgu bildirimi. |Hayır |
| Sliceıdentifiercolumnname |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilim verileri temizlemek için kullanılan otomatik dilim tanımlayıcı doldurmak için sütun adı belirtin. |Binary(32) veri türüne sahip bir sütunun sütun adı. |Hayır |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>JSON örnekleri ve Oracle veritabanından veri kopyalama
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, Oracle veritabanından için/Azure Blob Depolama / için verileri kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Örnek: verileri Oracle'dan Azure Blob kopyalama

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) kaynağı olarak ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri şirket içi Oracle veritabanına bir tablodaki bir blobu saatlik kopyalar. Aşağıdaki örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri aşağıdaki bölümlerde belgelerine bakın.

**Oracle bağlantılı hizmeti:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage bağlı hizmeti:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle girdi veri kümesi:**

Örnek, Oracle tablo "MyTable" oluşturulur ve zaman serisi veri için "timestampcolumn" adlı bir sütun içerdiği varsayar.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği ile kanal:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **OracleSource** ve **havuz** türü ayarlanmış **BlobSink**.  Belirtilen SQL sorgusu **oracleReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Örnek: verileri Azure Blob'tan için Oracle kopyalayın
Bu örnek bir Azure Blob depolama alanından bir şirket içi Oracle veritabanına veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi belirtildiği kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.  

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) kaynağı olarak [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri blob üzerinden şirket içi Oracle veritabanına tablosunda her saat kopyalar. Aşağıdaki örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri aşağıdaki bölümlerde belgelerine bakın.

**Oracle bağlantılı hizmeti:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage bağlı hizmeti:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure Blob girdi veri kümesi**

Veri toplanma yeni blob üzerinden saatte (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı Bu tablo veri fabrikası dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin sizi bilgilendirir.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Oracle çıktı veri kümesi:**

Örnek bir tablo "MyTable" Oracle oluşturdunuz varsayılır. Blob CSV dosyasında içerecek şekilde beklediğiniz gibi tablo Oracle ile aynı sayıda sütun oluşturun. Yeni satırlar tabloya saatte eklenir.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Kopyalama etkinliği ile kanal:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
### <a name="problem-1-net-framework-data-provider"></a>Sorun 1: .NET Framework veri sağlayıcısı

Aşağıdaki bakın **hata iletisi**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Olası nedenler:**

1. Oracle için .NET Framework veri sağlayıcısı yüklü değil.
2. Oracle için .NET Framework veri sağlayıcısı için .NET Framework 2.0 yüklendi ve .NET Framework 4.0 klasörlerde bulunamadı.

**Çözümleme/geçici çözüm:**

1. Oracle, .NET sağlayıcısı yüklemediyseniz [yüklemek](http://www.oracle.com/technetwork/topics/dotnet/downloads/) ve senaryo yeniden deneyin.
2. Sağlayıcı yüklendikten sonra bile hata iletisi alırsanız, aşağıdaki adımları uygulayın:
   1. Makine yapılandırma .NET 2.0 klasöründen açın: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Arama **.NET için Oracle veri sağlayıcısı**, ve altında aşağıdaki örnekte gösterildiği gibi bir giriş bulamadı olmalıdır **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description=".NET için oracle veri sağlayıcısı" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Machine.config dosyasının aşağıdaki v4.0 klasörde bu girdiyi kopyalayın: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config ve değişiklik 4.xxx.x.x sürüme.
4. "< ODP.NET yüklü yolu > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" Genel Derleme Önbelleği'ne (GAC) çalıştırarak yükleyin `gacutil /i [provider path]`. ## sorun giderme ipuçları

### <a name="problem-2-datetime-formatting"></a>Sorun 2: datetime biçimlendirme

Aşağıdaki bakın **hata iletisi**:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Çözümleme/geçici çözüm:**

Sorgu dizesi (to_date işlevi kullanılarak) aşağıdaki örnekte gösterildiği gibi tarihleri, Oracle veritabanınızı nasıl yapılandırıldığına göre kopyalama etkinliğinde ayarlamanız gerekebilir:

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Oracle için tür eşlemesi
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Verileri Oracle'dan taşırken, aşağıdaki eşlemelerini Oracle veri türünden .NET türü ve tersi yönde kullanılır.

| Oracle veri türü | .NET framework veri türü |
| --- | --- |
| BDOSYA |Byte] |
| BLOB |Byte] |
| CHAR |Dize |
| CLOB |Dize |
| TARİH |Tarih saat |
| KAYAN NOKTA |Ondalık, dize (varsa precision > 28) |
| TAMSAYI |Ondalık, dize (varsa precision > 28) |
| ARALIĞI YIL AY İÇİN |Int32 |
| İKİNCİ GÜN ARALIĞI |TimeSpan |
| UZUN |Dize |
| UZUN HAM |Byte] |
| NCHAR |Dize |
| NCLOB |Dize |
| SAYI |Ondalık, dize (varsa precision > 28) |
| NVARCHAR2 |Dize |
| HAM |Byte] |
| SATIR KİMLİĞİ |Dize |
| ZAMAN DAMGASI |Tarih saat |
| YEREL SAAT DİLİMİ ZAMAN DAMGASI |Tarih saat |
| SAAT DİLİMİ ZAMAN DAMGASI |Tarih saat |
| İŞARETSİZ TAMSAYI |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türü **ARALIĞI yıl Kime ay** ve **ARALIĞI gün için ikinci** Microsoft sürücüsü kullanılırken desteklenmez.

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
