---
title: İçin veya Oracle Data factory'yi kullanarak veri kopyalama | Microsoft Docs
description: Veritabanı Azure Data Factory kullanarak şirket içi Oracle/deposundan veri kopyalamayı öğreneceksiniz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 10535e75a32a9f95e759340cf14d693f43639473
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856850"
---
# <a name="copy-data-to-or-from-on-premises-oracle-using-azure-data-factory"></a>Ya da Azure Data Factory kullanarak şirket içi Oracle veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-oracle.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Oracle Bağlayıcısı V2'de](../connector-oracle.md).


Bu makalede, kopyalama etkinliği Azure Data Factory'de veri gönderip buralardan şirket içi Oracle veritabanına taşımak için nasıl kullanılacağı açıklanmaktadır. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalayabilirsiniz **bir Oracle veritabanından** aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz **bir Oracle veritabanına**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Önkoşullar
Data Factory, veri yönetimi ağ geçidi kullanarak şirket içi Oracle kaynaklarına bağlanmayı destekler. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında bilgi edinmek için makaleyi ve [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale için ağ geçidini ayarlamadan bir veri işlem hattı adım adım yönergeler Veri taşıma.

Bir Azure Iaas sanal Makinesinde Oracle barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı Iaas VM'de veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Bu Oracle Bağlayıcısı sürücülerin iki sürümlerini destekler:

- **(Önerilen) Oracle için Microsoft sürücüsü**: veri yönetimi ağ geçidi sürüm 2.7, sürücü, ayrıca gerekmez Oracle ağ geçidi ile birlikte otomatik olarak yüklenir için işlemek için sürücü Microsoft gelen başlatılıyor Oracle bağlantı kurmak ve bu sürücü kullanarak daha iyi bir kopyalama performansı da oluşabilir. Veritabanları Oracle'nın altındaki sürümleri desteklenir:
    - Oracle 12c R1 (12,1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10,1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3'ü (8.1.7)

> [!NOTE]
> Oracle Ara sunucu desteklenmiyor.

> [!IMPORTANT]
> Şu anda Microsoft sürücüsü Oracle için yalnızca Oracle ancak Oracle'a yazılmıyor veri kopyalamayı destekler. Ve veri yönetimi ağ geçidi Tanılama sekmesi test bağlantı özelliği bu sürücüyü desteklemiyor dikkat edin. Alternatif olarak, bağlantıyı doğrulamak için kopyalama Sihirbazı'nı kullanabilirsiniz.
>

- **.NET için Oracle veri sağlayıcısı:** veri kopyalama/Oracle için Oracle veri sağlayıcısı kullanmayı tercih edebilirsiniz. Bu bileşen yer aldığı [için Oracle veri erişim bileşenleri Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Uygun sürüm (32/64 bit), ağ geçidinin yüklü olduğu bir makineye yükleyin. [Oracle veri sağlayıcısı .NET 12,1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) 10 g, 2 veya üzeri bir sürüm Oracle veritabanına erişebilir.

    "XCopy yüklemeyi" seçerseniz readme.htm adımları izleyin. Yükleyici kullanıcı Arabirimi (olmayan-XCopy biri) ile seçtiğiniz öneririz.

    Sağlayıcı yüklendikten sonra **yeniden** Hizmetleri uygulaması (veya) veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni kullanarak makinenize veri yönetimi ağ geçidi konak hizmeti.  

Kopyalama işlem hattını oluşturmak için kopyalama Sihirbazı'nı kullanırsanız, otomatik olarak belirlenen sürücü türü olacaktır. Microsoft sürücüsü, ağ geçidi sürümü 2.7 daha küçük veya havuz olarak Oracle seçtiğiniz sürece varsayılan olarak kullanılır.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak veri gönderip buralardan şirket içi Oracle veritabanına taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu** , **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir. 
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, bir Azure blob depolama alanına bir Oralce veritabanından veri kopyalıyorsanız, Oracle database ve Azure depolama hesabına veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. Oracle için özel bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
3. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Son adımda bahsedilen örnekte, girdi verilerini içeren, Oracle veritabanında tablo belirtmek için bir veri kümesi oluşturun. Ve blob kapsayıcısını ve Oracle veritabanından kopyalanan verileri tutan klasörün belirtmek için başka bir veri kümesi oluşturun. Oracle için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte OracleSource bir kaynak ve BlobSink havuz olarak kopyalama etkinliği için kullanırsınız. Azure Blob depolama alanından Oracle veritabanı kopyalıyorsanız benzer şekilde, BlobSource ve OracleSink kopyalama etkinliği kullanırsınız. Oracle veritabanına özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir kaynak veya havuz bir veri deposunu kullanma hakkında daha fazla ayrıntı için önceki bölümde veri deponuz için bağlantıya tıklayın. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Veri gönderip buralardan şirket içi Oracle veritabanına kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples-for-copying-data-to-and-from-oracle-database) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, JSON öğeleri Oracle bağlantılı hizmete özgü açıklama belirler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesOracle** |Evet |
| driverType | / Oracle veritabanına veri kopyalamak için kullanılacak sürücüyü belirtin. İzin verilen değerler **Microsoft** veya **ODP** (varsayılan). Bkz: [desteklenen sürümü ve yükleme](#supported-versions-and-installation) sürücü ayrıntıları bölümü. | Hayır |
| bağlantı dizesi | ConnectionString özelliği için Oracle veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| gatewayName | Şirket içi Oracle sunucusuna bağlanmak için kullanılan ağ geçidinin adı |Evet |

**Örnek: Microsoft sürücüsü kullanarak:**

>[!TIP]
>Hata bildiren ulaşırsanız "ORA-01025: UPI parametre aralık dışında" ve Oracle Sürüm 8i, ekleme `WireProtocolMode=1` bağlantı dizesi ve yeniden deneyin.

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

Başvurmak [bu site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) için izin verilen biçimler.

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
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için (Oracle, Azure blob, Azure tablo, vs.) benzerdir.

TypeProperties bölümünün her tür veri kümesi için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümünün veri kümesi için tür OracleTable aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetini ifade eder Oracle veritabanı tablosunun adı. |Hayır (varsa **oracleReaderQuery** , **OracleSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği, tek bir girdi alır ve tek bir çıktı üretir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

### <a name="oraclesource"></a>OracleSource
Kopya etkinlikteki kaynak türünde olduğunda **OracleSource** aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * MyTable gelen <br/><br/>Belirtilmezse, yürütülen SQL deyimi: seçin * MyTable gelen |Hayır (varsa **tableName** , **veri kümesi** belirtilir) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |zaman aralığı<br/><br/> Örnek: 00:30:00 (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılan otomatik dilim tanımlayıcısı ile doldurmak için sütun adı belirtin. |Bir sütunun veri türüyle binary(32) sütun adı. |Hayır |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>JSON örnekler ve Oracle veritabanından veri kopyalamak için
Aşağıdaki örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, / için Azure Blob Depolama içine/dışına bir Oracle veritabanına veri kopyalama işlemini göstermektedir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Örnek: verileri Oracle'dan Azure Blob kopyalama

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) kaynağı olarak ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri şirket içi Oracle veritabanında tablodan bir bloba saatlik kopyalar. Örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri aşağıdaki bölümlerde belgelerine bakın.

**Oracle bağlı hizmeti:**

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

**Azure Blob Depolama bağlı hizmeti:**

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

**Oracle giriş veri kümesi:**

Örnek, Oracle "MyTable" bir tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

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

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

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

**Kopyalama etkinliği ile işlem hattı:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **OracleSource** ve **havuz** türü ayarlandığında **BlobSink**.  Belirtilen SQL sorgusu **oracleReaderQuery** özelliği veri kopyalamak için son bir saat içinde seçer.

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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Örnek: verileri Azure Blobundan Oracle'a kopyalama
Bu örnek, bir şirket içi Oracle veritabanına bir Azure Blob depolama alanından verileri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** belirtilen kaynaklardan birinden [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.  

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) kaynağı olarak [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri bir blobun bir şirket içi Oracle veritabanı tablosunda her saat kopyalar. Örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri aşağıdaki bölümlerde belgelerine bakın.

**Oracle bağlı hizmeti:**
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

**Azure Blob Depolama bağlı hizmeti:**
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

Veri alındığından yeni blobundan her saat (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı, bu tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil Data Factory hizmetinin bildirir.

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

Örnek, bir tablo "MyTable" Oracle oluşturmuş olduğunuzu varsayar. Blob CSV dosyasını içerecek şekilde beklediğiniz gibi aynı sayıda sütun ile Oracle tablo oluşturun. Yeni satırlar saatte tablosuna eklenir.

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

**Kopyalama etkinliği ile işlem hattı:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **OracleSink**.  

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
### <a name="problem-1-net-framework-data-provider"></a>1. sorun: .NET Framework veri sağlayıcısı

Aşağıda gördüğünüz **hata iletisi**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Olası nedenler:**

1. Oracle için .NET Framework veri sağlayıcısı yüklenmedi.
2. Oracle için .NET Framework veri sağlayıcısı için .NET Framework 2.0 yüklendi ve .NET Framework 4.0 klasörler bulunamadı.

**Çözüm/geçici çözüm:**

1. Oracle için .NET sağlayıcısı yüklemediyseniz [yüklemek](http://www.oracle.com/technetwork/topics/dotnet/downloads/) ve senaryoyu yeniden deneyin.
2. Sağlayıcı yüklendikten sonra bile hata iletisi alırsanız, aşağıdaki adımları uygulayın:
   1. .NET 2.0 makine yapılandırmasının klasöründen açın: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Arama **.NET için Oracle veri sağlayıcısı**, ve altında aşağıdaki örnekte gösterildiği gibi bir giriş bulunacak erişebileceğinizi **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description=".NET için oracle veri sağlayıcısı" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Şu v4.0 klasörü machine.config dosyasında bu girdi kopyalayın: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config ve değişiklik 4.xxx.x.x sürümü.
4. "< ODP.NET yüklü yolu > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" genel derleme önbelleğine (GAC) çalıştırarak yükleyin `gacutil /i [provider path]`. ## sorun giderme ipuçları

### <a name="problem-2-datetime-formatting"></a>Sorun 2: datetime biçimlendirme

Aşağıda gördüğünüz **hata iletisi**:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Çözüm/geçici çözüm:**

Tarihler, Oracle veritabanında nasıl yapılandırılır (to_date işlevi kullanılarak) aşağıdaki örnekte gösterildiği gibi temel bir kopyalama etkinliği, sorgu dizesinde ayarlamanız gerekebilir:

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Oracle için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği kaynak türünden aşağıdaki 2 adımlı yaklaşım türleriyle havuz otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Verileri Oracle'dan taşırken, aşağıdaki eşlemeler Oracle veri türünden .NET türü ve bunun tersi de kullanılır.

| Oracle veri türü | .NET framework veri türü |
| --- | --- |
| BDOSYA |Bayt] |
| BLOB |Bayt]<br/>(yalnızca Oracle 10 g ve daha yüksek olduğunda desteklenen Microsoft sürücüsü kullanarak) |
| CHAR |Dize |
| CLOB |Dize |
| DATE |DateTime |
| KAYAN NOKTA |Ondalık, dize (olursa hassasiyet > 28) |
| TAMSAYI |Ondalık, dize (olursa hassasiyet > 28) |
| YIL AY ARALIĞI |Int32 |
| İKİNCİ GÜN ARALIĞI |Zaman aralığı |
| UZUN |Dize |
| LONG RAW |Bayt] |
| NCHAR |Dize |
| NCLOB |Dize |
| SAYI |Ondalık, dize (olursa hassasiyet > 28) |
| NVARCHAR2 |Dize |
| HAM |Bayt] |
| SATIR KİMLİĞİ |Dize |
| ZAMAN DAMGASI |DateTime |
| YEREL SAAT DİLİMİ İLE ZAMAN DAMGASI |DateTime |
| SAAT DİLİMİ İLE ZAMAN DAMGASI |DateTime |
| İŞARETSİZ TAMSAYI |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türü **ARALIĞI Yıl Bitiş ayı** ve **ARALIĞINI gün için ikinci** Microsoft sürücüsü kullanılırken desteklenmez.

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
