---
title: Data Factory kullanarak verileri veya Oracle kopyalayın | Microsoft Docs
description: Azure Data Factory kullanarak ya da şirket içi Oracle veritabanından veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4ff7f92d1d13966be5d17f37210bef961f64faf2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462420"
---
# <a name="copy-data-to-or-from-oracle-on-premises-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Oracle şirket içi veri kopyalayın

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-oracle.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Azure Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Oracle Bağlayıcısı V2'de](../connector-oracle.md).


Bu makalede, kopyalama etkinliği Azure Data Factory'de veya bir şirket içi Oracle veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Makaleyi yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md), hangi sunar genel bir bakış veri taşıma, kopyalama etkinliği'ni kullanarak.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Veri kopyalayabilirsiniz *bir Oracle veritabanından* aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz *bir Oracle veritabanına*:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Önkoşullar

Data Factory, veri yönetimi ağ geçidi kullanarak şirket içi Oracle kaynaklarına bağlanmayı destekler. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında daha fazla bilgi edinmek için. Ağ geçidi veri işlem hattı, verileri taşımak için ayarlama konusunda adım adım yönergeler için bkz. [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).

Oracle Azure altyapısının hizmet (Iaas) sanal makine olarak barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı Iaas VM'de veya farklı bir VM ağ geçidi yükleyebilirsiniz.

> [!NOTE]
> Bağlantı ve ağ geçidi ilgili sorunları giderme ipuçları için bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues).

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme

Bu Oracle Bağlayıcısı sürücülerin iki sürümlerini destekler:

- **(Önerilen) Oracle için Microsoft sürücüsü**: Oracle için Microsoft sürücüsü, veri yönetimi ağ geçidi sürüm 2.7 başlayarak, ağ geçidi ile otomatik olarak yüklenir. Yükleme veya Oracle bağlantı kurmak için bir sürücü güncelleştirmeniz gerekmez. Bu sürücü kullanarak, daha iyi bir kopyalama performansı da oluşabilir. Oracle veritabanları bu sürümleri desteklenir:
  - Oracle 12c R1 (12,1)
  - Oracle 11g R1, R2 (11.1, 11.2)
  - Oracle 10g R1, R2 (10,1, 10.2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3'ü (8.1.7)

    > [!NOTE]
    > Oracle Ara sunucu desteklenmiyor.

    > [!IMPORTANT]
    > Şu anda Microsoft sürücüsü Oracle için Oracle yalnızca veri kopyalamayı destekler. Oracle sürücüsü desteklememektedir. Veri Yönetimi ağ geçidi üzerinde test bağlantı özelliği **tanılama** sekme, bu sürücüyü desteklemiyor. Alternatif olarak, bağlantıyı doğrulamak için kopyalama Sihirbazı'nı kullanabilirsiniz.
    >

- **.NET için Oracle veri sağlayıcısı**: Gelen veya Oracle veri kopyalamak için Oracle veri sağlayıcısı'nı kullanabilirsiniz. Bu bileşen yer aldığı [için Oracle veri erişim bileşenleri Windows](https://www.oracle.com/technetwork/topics/dotnet/downloads/). İlgili sürüm (32 bit veya 64-bit), ağ geçidinin yüklü olduğu bir makineye yükleyin. [Oracle veri sağlayıcısı .NET 12,1](https://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) Oracle veritabanı 10 g sürüm 2 ve üzeri sürümler erişebilirsiniz.

    Seçerseniz **XCopy yükleme**, readme.htm dosyasında açıklanan adımları izleyin. Kullanıcı arabirimini (XCopy yükleyici değil) olan yükleyici seçmenizi öneririz.

    Sağlayıcıyı yükledikten sonra hizmetler uygulamasını veya veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni kullanarak makinenize veri yönetimi ağ geçidi konak hizmetini yeniden başlatın.

Kopyalama Sihirbazı'nı kopyalama işlem hattını oluşturmak için kullandığınız sürücü autodetermined türüdür. Microsoft sürücüsü, ağ geçidi sürümü 2.7 sürümden daha eski veya havuz olarak Oracle seçtiğiniz sürece varsayılan olarak kullanılır.

## <a name="get-started"></a>başlarken

Kopyalama etkinliği içeren işlem hattı oluşturabilirsiniz. İşlem hattı farklı araçları veya API'leri kullanarak ya da şirket içi Oracle veritabanından veri taşır.

Bir işlem hattı oluşturmanın en kolay yolu kopyalama Sihirbazı'nı kullanmaktır. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca aşağıdaki araçlardan birini bir işlem hattı oluşturmak için kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**e **Azure Resource Manager Şablon**, **.NET API**, veya **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği içeren işlem hattı oluşturma konusunda adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları tamamlayın:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir.
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, Azure Blob depolama alanına bir Oracle veritabanından veri kopyalıyorsanız, Oracle database ve Azure depolama hesabına veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. Oracle için özel bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties).
3. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Örnekte önceki adımda, girdi verilerini içeren, Oracle veritabanında tablo belirtmek için bir veri kümesi oluşturun. Blob kapsayıcısını ve Oracle veritabanından kopyalanan verileri tutan klasörün belirtmek için başka bir veri kümesi oluşturursunuz. Oracle için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties).
4. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği vardır. Önceki örnekte, kullandığınız **OracleSource** kaynağı olarak ve **BlobSink** kopyalama etkinliği için bir havuz olarak. Benzer şekilde, Azure Blob depolamadaki verileri bir Oracle veritabanına kopyalıyorsanız kullanmanız **BlobSource** ve **OracleSink** kopyalama etkinliğindeki. Bir Oracle veritabanına özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties). Bir kaynak veya havuz olarak bir veri deposunu kullanma hakkında daha fazla bilgi için önceki bölümde veri deponuz için bağlantıyı seçin.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları için JSON tanımları otomatik olarak sizin için oluşturulur: bağlı hizmetler, veri kümeleri ve işlem hattı. Araç veya API'lerden (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Bir şirket içi Oracle veritabanına veri kopyalamak için kullandığınız Data Factory varlıkları için JSON tanımları sahip örnekler için JSON örneklere bakın.

Aşağıdaki bölümler, Data Factory varlıkları tanımlamak için kullandığınız JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki tabloda, Oracle bağlantılı hizmete özgü JSON öğeleri açıklanmıştır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |**Türü** özelliği ayarlanmalıdır **OnPremisesOracle**. |Evet |
| driverType | Ya da bir Oracle veritabanına veri kopyalamak için kullanılacak sürücüyü belirtin. İzin verilen değerler **Microsoft** ve **ODP** (varsayılan). Bkz: [desteklenen sürümü ve yükleme](#supported-versions-and-installation) için sürücü ayrıntıları. | Hayır |
| connectionString | Oracle veritabanı örneği için bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. | Evet |
| gatewayName | Şirket içi Oracle sunucusuna bağlanmak için kullanılan ağ geçidi adı. |Evet |

**Örnek: Microsoft sürücüsü kullanma**

> [!TIP]
> Bildiren bir hata görürseniz "ORA-01025: UPI parametre aralık dışında"ve Oracle sürümü 8i, ekleme `WireProtocolMode=1` bağlantı dizesi ve yeniden deneyin:

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Örnek: ODP sürücüsü kullanarak**

İzin verilen biçimler hakkında bilgi edinmek için [ODP .NET için Oracle veri sağlayıcısı](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/).

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>))); User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md).

Yapı, kullanılabilirlik ve ilke gibi bir veri kümesi JSON dosyası bölümlerini (örneğin, Oracle, Azure Blob Depolama ve Azure tablo depolama) tüm veri kümesi türleri için benzerdir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. **TypeProperties** türü için veri kümesi bölümünü **OracleTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tabloda bir Oracle veritabanına başvuran bağlı hizmetin adı. |Hayır (varsa **oracleReaderQuery** veya **OracleSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md).

İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği, tek bir girdi alır ve tek bir çıktı üretir.

Kullanılabilir özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği özellikleri, kaynak ve havuz türüne bağlı olarak değişir.

### <a name="oraclesource"></a>OracleSource

Kopya etkinlikteki kaynak olduğunda, **OracleSource** türü, aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorgu kullanın. |Bir SQL sorgu dizesi. Örneğin, "seçin \* gelen **MyTable**". <br/><br/>Belirtilmemişse, bu SQL deyimi yürütülür: "seçin \* gelen **MyTable**" |Hayır<br />(varsa **tableName** , **veri kümesi** belirtilir) |

### <a name="oraclesink"></a>OracleSink

**OracleSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Batch için bekleme süresi, işlemin zaman aşımına uğramadan önce tamamlanması ekleyin. |**Zaman aralığı**<br/><br/> Örnek: 00:30:00 (30 dakika) |Hayır |
| writeBatchSize |Arabellek boyutu değerini ulaştığında veri SQL tablosuna ekler **writeBatchSize**. |Tamsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Böylece belirli bir dilimle verilerinin temizlenmesini yürütmek kopyalama etkinliği için bir sorguyu belirtir. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin bir otomatik olarak oluşturulan dilim tanımlayıcı ile doldurmak için sütun adı belirtir. Değeri **Sliceıdentifiercolumnname** ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılır. |Bir sütunun veri türüne sahip sütun adı **binary(32)**. |Hayır |

## <a name="json-examples-for-copying-data-to-and-from-the-oracle-database"></a>JSON örnekler ve Oracle veritabanından veri kopyalamak için

Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnekler öğesinden veya bir Oracle veritabanına ya da Azure Blob depolamadan/depolamaya veri kopyalama işlemini göstermektedir. Ancak, veriler herhangi bir listelenen havuzlarını kopyalanabilir [desteklenen veri depoları ve biçimler](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure veri fabrikasında kopyalama etkinliği kullanarak.

**Örnek: Oracle'dan Azure Blob depolama alanına veri kopyalama**

Örnek, aşağıdaki Data Factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) kaynağı olarak ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri şirket içi Oracle veritabanında tablodan bir bloba saatlik kopyalar. Örnekleri aşağıdaki bölümlerde örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için bkz.

**Oracle bağlı hizmeti**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob Depolama bağlı hizmeti**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**Oracle giriş veri kümesi**

Örnek adlı bir tablo oluşturdunuz varsayılır **MyTable** Oracle'da bulunan. Adlı bir sütun içerdiği **timestampcolumn** zaman serisi verilerinin.

Ayarı **dış**: **true** Data Factory hizmetinin veri kümesi veri fabrikasında bir etkinliği tarafından üretilen değildir ve veri kümesini veri fabrikasına dış olduğunu bildirir.

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

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (**sıklığı**: **saat**, **aralığı**: **1**). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu, yıl, ay, gün ve başlangıç zamanın saat parçası kullanır.

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

**Kopyalama etkinliği ile işlem hattı**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **OracleSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusunu kullanarak belirttiğiniz **oracleReaderQuery** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for a copy activity",
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

**Örnek: Azure Blob depolamadan/depolamaya veri Oracle'a kopyalama**

Bu örnek bir Azure Blob Depolama hesabındaki bir şirket içi Oracle veritabanına veri kopyalama işlemi gösterilmektedir. Ancak, veri kopyalayabilirsiniz *doğrudan* herhangi listelenen kaynakları [desteklenen veri depoları ve biçimler](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure veri fabrikasında kopyalama etkinliği kullanarak.

Örnek, aşağıdaki Data Factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği olan [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) kaynağı olarak [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) havuz olarak.

Örnek verileri bir blobun bir şirket içi Oracle veritabanı tablosunda her saat kopyalar. Örnekleri aşağıdaki bölümlerde örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için bkz.

**Oracle bağlı hizmeti**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob Depolama bağlı hizmeti**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**Azure Blob girdi veri kümesi**

Veri alındığından yeni blobundan saatte (**sıklığı**: **saat**, **aralığı**: **1**). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu, yıl, ay ve gün kısmını başlangıç saatini kullanır. Dosya adı, başlangıç zamanı saat bölümünü kullanır. Ayar **dış**: **true** Data Factory hizmetinin bu tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

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

**Oracle çıktı veri kümesi**

Örnek adlı bir tablo oluşturdunuz varsayılır **MyTable** Oracle'da bulunan. Tablo Oracle ile aynı sayıda blob CSV dosyasını içerecek şekilde beklediğiniz bir sütun oluşturun. Yeni satırlar saatte tablosuna eklenir.

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

**Kopyalama etkinliği ile işlem hattı**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış ve saatte çalışacak şekilde zamanlanan bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **OracleSink**.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with a copy activity",
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

**hata iletisi**

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .NET Framework Data Provider. It may not be installed.

**Olası nedenler**

* Oracle için .NET Framework veri sağlayıcısı yüklü değildi.
* Oracle için .NET Framework veri sağlayıcısı için .NET Framework 2.0 yüklendi ve .NET Framework 4.0 klasörlerinde bulunan değil.

**Çözümleme**

* Oracle için .NET sağlayıcısı yüklemediyseniz [yüklemek](https://www.oracle.com/technetwork/topics/dotnet/downloads/)ve senaryoyu yeniden deneyin.
* Sağlayıcıyı yükledikten sonra bile hata iletisini görürseniz, aşağıdaki adımları tamamlayın:
    1. .NET 2.0 klasöründen makine yapılandırma dosyasını aç < sistem diski\>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Arama **.NET için Oracle veri sağlayıcısı**. Altında aşağıdaki örnekte gösterildiği gibi bir giriş bulunacak erişebileceğinizi **system.data** > **DbProviderFactories**: `<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />`
* Aşağıdaki .NET 4.0 klasörü machine.config dosyasında bu girdi kopyalayın: < sistem diski\>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config. Ardından, sürüm için 4.xxx.x.x değiştirin.
* Yükleme < ODP.NET yüklü yolu\>çalıştırarak genel derleme önbelleğinde (GAC) \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll **gacutil /i [sağlayıcı yolu]**.

### <a name="problem-2-datetime-formatting"></a>Sorun 2: Tarih/saat biçimlendirmesi

**hata iletisi**

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Çözümleme**

Tarihler, Oracle veritabanında nasıl yapılandırıldığına göre bir kopyalama etkinliği sorgu dizesinde ayarlamanız gerekebilir. Bir örnek aşağıda verilmiştir (kullanarak **to_date** işlevi):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Oracle için tür eşlemesi

Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md), kopyalama etkinliği, türleri, aşağıdaki iki adımlı yaklaşım kullanarak havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türleri için .NET türü dönüştürün.
2. .NET türünden yerel havuz türüne dönüştürün.

Verileri Oracle'dan taşıdığınızda, aşağıdaki eşlemeler Oracle veri türünden .NET türü ve tersi kullanılır:

| Oracle veri türü | .NET framework veri türü |
| --- | --- |
| BDOSYA |Byte[] |
| BLOB |Byte[]<br/>(Microsoft sürücüsü kullandığınızda yalnızca Oracle 10 g ve sonraki sürümlerde desteklenir) |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| KAYAN NOKTA |Ondalık, dize (olursa hassasiyet > 28) |
| INTEGER |Ondalık, dize (olursa hassasiyet > 28) |
| YIL AY ARALIĞI |Int32 |
| İKİNCİ GÜN ARALIĞI |TimeSpan |
| UZUN |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| SAYI |Ondalık, dize (olursa hassasiyet > 28) |
| NVARCHAR2 |String |
| HAM |Byte[] |
| SATIR KİMLİĞİ |String |
| ZAMAN DAMGASI |DateTime |
| YEREL SAAT DİLİMİ İLE ZAMAN DAMGASI |DateTime |
| SAAT DİLİMİ İLE ZAMAN DAMGASI |DateTime |
| İŞARETSİZ TAMSAYI |Sayı |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Veri türleri **ARALIĞI Yıl Bitiş ayı** ve **ARALIĞINI gün için ikinci** Microsoft sürücüsü kullandığınızda desteklenmez.

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi

Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında daha fazla bilgi için bkz: [Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma

İlişkisel veri depolarından veri kopyalamak, Yinelenebilirlik istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için bir yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim, ya da el ile yeniden çalıştırılır veya bir yeniden deneme ilkesi tarafından aynı verileri olduğundan emin olun, ne olursa olsun nasıl birden çok kez okuma bir dilim çalıştırılır. Daha fazla bilgi için [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayar

Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) Azure Data factory'deki veri taşıma (kopyalama etkinliği) performansını etkileyen önemli faktörlerin hakkında bilgi edinmek için. Ayrıca, en iyi duruma getirmek için çeşitli yollar hakkında bilgi edinebilirsiniz.
