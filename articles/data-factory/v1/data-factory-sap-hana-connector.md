---
title: Azure Data Factory kullanarak SAP HANA'dan veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak SAP HANA'dan veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 96d16552cfadca9b345d0f0cd0a344249897f571
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61258445"
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a>Gelen SAP, HANA Azure Data Factory ile veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-sap-hana-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sap-hana.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de SAP HANA Bağlayıcısı](../connector-sap-business-warehouse.md).

Bu makalede, bir şirket içi SAP HANA'dan verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi SAP HANA veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca verileri bir SAP HANA verileri diğer veri depolarına bir SAP HANA için taşımak için değil ancak diğer veri depolarını destekler.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Bu bağlayıcı, SAP HANA veritabanı herhangi bir sürümünü destekler. Bu, HANA bilgi modellerini (Analiz ve hesaplama görünümleri gibi) ve SQL sorgularını kullanarak satır/sütun tablolarından veri kopyalamayı destekler.

SAP HANA örneği bağlantıyı etkinleştirmek için aşağıdaki bileşenlerini yükleyin:
- **Veri Yönetimi ağ geçidi**: Data Factory hizmeti, veri yönetimi ağ geçidi adlı bir bileşen kullanarak şirket içi veri depolarına (dahil olmak üzere SAP HANA) bağlanmayı destekler. Veri Yönetimi ağ geçidi ve adım adım yönergeler için ağ geçidini ayarlama hakkında bilgi edinmek için bkz. [şirket içi verileri arasında veri taşıma depolama bulut veri deposu](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bir Azure Iaas sanal makine (VM) SAP HANA barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı VM'de ya da farklı bir VM ağ geçidi yükleyebilirsiniz.
- **SAP HANA ODBC sürücüsünü** ağ geçidi makinesinde. SAP HANA ODBC sürücüsünü yükleyebilirsiniz [SAP Software Download Center](https://support.sap.com/swdc). Anahtar sözcüğüyle arama **için SAP HANA istemci Windows**. 

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi SAP HANA veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. 

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz. 
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. 
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Bir şirket içi SAP HANA'dan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Azure Blob için SAP HANA'dan kopyalama](#json-example-copy-data-from-sap-hana-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir SAP HANA veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, SAP HANA bağlantılı hizmete özgü JSON öğeleri için bir açıklama sağlar.

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP HANA örneği yer aldığı sunucunun adı. Sunucunuz özelleştirilmiş bir bağlantı noktası kullanıyorsa, belirtin `server:port`. | string | Evet
authenticationType | Kimlik doğrulaması türü. | dize. "Temel" veya "Windows" | Evet 
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP HANA örneğine bağlanmak için kullanması gereken ağ geçidi adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. SAP HANA veri kümesi türü için desteklenen türe özgü özellikler yoktur **RelationalTable**. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları gibi özellikleri olan ilkeleri, tüm etkinlik türleri için kullanılabilir.

Diğer yandan bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türünde olduğunda **RelationalSource** (SAP HANA içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP HANA örneği verileri okumak için SQL sorgusu belirtir. | SQL sorgusu. | Evet |

## <a name="json-example-copy-data-from-sap-hana-to-azure-blob"></a>JSON örneği: Verileri Azure Blob için SAP HANA'dan kopyalama
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu örnek, bir şirket içi SAP HANA'dan bir Azure Blob depolama alanına veri kopyalamaya gösterilmektedir. Ancak, veriler kopyalanabilir **doğrudan** havuzlarını birine listelenen [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.  

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [SapHana](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri bir SAP HANA örneğinden Azure blobuna saatlik kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi'ni ayarlayın. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

### <a name="sap-hana-linked-service"></a>SAP HANA bağlı hizmeti
Bu, SAP HANA örneği veri fabrikasına bağlı hizmeti. Type özelliği ayarlamak **SapHana**. TypeProperties bölümünün SAP HANA örneği için bağlantı bilgilerini sağlar.

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
Bu, Azure depolama hesabınızı veri fabrikasına bağlı hizmeti. Type özelliği ayarlamak **AzureStorage**. TypeProperties bölümünün Azure depolama hesabı için bağlantı bilgilerini sağlar.

```json
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

### <a name="sap-hana-input-dataset"></a>SAP HANA giriş veri kümesi

Bu veri kümesi, SAP HANA veri kümesini tanımlar. Data Factory veri kümesi için tür ayarladığınız **RelationalTable**. Şu anda, herhangi bir SAP HANA veri kümesi için türe özgü özellikleri belirtmeyin. Kopyalama etkinliği tanımındaki sorgu, SAP HANA örneğinden okumak için hangi verilerin belirtir. 

Data Factory hizmeti, dış özelliğin true olarak ayarlanması tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

Frequency ve interval değerleriyle özelliklerini zamanlamayı tanımlar. Bu durumda, veriler, SAP HANA örneğinden saatlik okunur. 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi
Bu veri kümesi, çıktı Azure Blob veri kümesini tanımlar. Type özelliği AzureBlob olarak ayarlanmıştır. TypeProperties bölümünün, SAP HANA örneğinden kopyalanan verilerin depolandığı sağlar. Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Kopyalama etkinliği ile işlem hattı

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** (için SAP HANA kaynak) ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a>SAP HANA için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

SAP HANA'dan veri taşıma, aşağıdaki eşlemeler SAP HANA türlerinden .NET türleri için kullanılır.

SAP HANA türü | .NET türüne göre
------------- | ---------------
TINYINT | Byte
SMALLINT | Int16
INT | Int32
BIGINT | Int64
REAL | Single
DOUBLE | Single
DECIMAL | Decimal
BOOLEAN | Byte
VARCHAR | String
NVARCHAR | String
CLOB | Byte[]
ALPHANUM | String
BLOB | Byte[]
DATE | DateTime
TIME | TimeSpan
TIMESTAMP | DateTime
SECONDDATE | DateTime

## <a name="known-limitations"></a>Bilinen sınırlamalar
SAP HANA'dan veri kopyalama işlemi sırasında bazı bilinen sınırlamalar vardır:

- NVARCHAR dizeleri 4000 Unicode karakter maksimum uzunluğunu kesilir
- SMALLDECIMAL desteklenmez
- VARBINARY desteklenmez
- Geçerli tarihler 1899/12/30 arasında olan ve 9999/12/31

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
