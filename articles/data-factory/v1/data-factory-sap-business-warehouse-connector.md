---
title: Azure Data Factory kullanarak SAP Business Warehouse veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak SAP Business Warehouse veri taşıma hakkında bilgi edinin.
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
ms.openlocfilehash: c928ad1fc9a8d6206c1b7e47591b17b6ae05ee4b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839878"
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Gelen SAP Business Azure Data Factory kullanarak Warehouse veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-sap-business-warehouse-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sap-business-warehouse.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de SAP Business Warehouse Bağlayıcısı](../connector-sap-business-warehouse.md).


Bu makalede, bir şirket içi SAP Business Warehouse (BW) gelen verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi SAP Business Warehouse veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca verileri bir SAP Business Warehouse verileri diğer veri depolarına bir SAP Business Warehouse için taşımak için değil ancak diğer veri depolarını destekler. 

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
SAP Business Warehouse sürümü bu bağlayıcıyı destekler 7.x. Bu, MDX sorguları kullanarak veri kopyalamayı Infocubes ve QueryCubes (dahil olmak üzere BEx sorguları) destekler.

SAP BW örneğine bağlantısını etkinleştirmek için aşağıdaki bileşenlerini yükleyin:
- **Veri Yönetimi ağ geçidi**: Veri Yönetimi ağ geçidi adlı bir bileşen kullanılarak bağlanmak için şirket içi veri depoları (SAP Business Warehouse gibi) veri fabrikası hizmeti destekler. Veri Yönetimi ağ geçidi ve adım adım yönergeler için ağ geçidini ayarlama hakkında bilgi edinmek için bkz. [şirket içi verileri arasında veri taşıma depolama bulut veri deposu](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Bir Azure Iaas sanal makine (VM) SAP Business Warehouse barındırılıyor olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabilir sürece veri deposu olarak aynı VM'de ya da farklı bir VM ağ geçidi yükleyebilirsiniz.
- **SAP NetWeaver Kitaplığı** ağ geçidi makinesinde. SAP Netweaver kitaplığını SAP yöneticinizden veya doğrudan alabilirsiniz [SAP Software Download Center](https://support.sap.com/swdc). Arama **SAP notu #1025361** en son sürümü için indirme konumunu almak için. Mimariyi (32 bit veya 64-bit) SAP NetWeaver kitaplığı için ağ geçidi yüklemenizi eşleştiğinden emin olun. Ardından tüm dosyaları SAP Note göre SAP NetWeaver RFC SDK'sı dahil yükleyin. SAP NetWeaver kitaplığı ayrıca SAP Client Tools yüklemesine de dahil edilir.

> [!TIP]
> NetWeaver RFC SDK'sından system32 klasörüne ayıklanan DLL'leri yerleştirin.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. 

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz. 
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. 
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Bir şirket içi SAP Business Warehouse verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Veri SAP Business Warehouse ' Azure Blob kopyalama](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir SAP BW veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, SAP Business Warehouse (BW) bağlantılı hizmete özgü JSON öğeleri için bir açıklama sağlar.

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
server | SAP BW örneği yer aldığı sunucunun adı. | dize | Evet
systemNumber | SAP BW sisteminin sistem numarası. | İki basamaklı ondalık sayı bir dize olarak temsil edilir. | Evet
clientId | SAP W sisteminde istemcinin istemci kimliği. | Bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | dize | Evet
password | Kullanıcının parolası. | dize | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidi adı. | dize | Evet
encryptedCredential | Şifrelenmiş kimlik bilgisi dizesi. | dize | Hayır

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. SAP BW veri kümesi türü için desteklenen türe özgü özellikler yoktur **RelationalTable**. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları gibi özellikleri olan ilkeleri, tüm etkinlik türleri için kullanılabilir.

Diğer yandan bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türünde olduğunda **RelationalSource** (SAP BW içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query | SAP BW örneğinden verileri okumak için MDX Sorgusu belirtir. | MDX Sorgusu. | Evet |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a>JSON örneği: SAP Business Warehouse kopya verilerinin Azure Blob
Aşağıdaki örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu örnek, bir şirket içi SAP Business Warehouse verileri bir Azure Blob depolama alanına kopyalamak gösterilmektedir. Ancak, veriler kopyalanabilir **doğrudan** herhangi bir belirtilen havuzlarını [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.  

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [SapBw](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri bir SAP Business Warehouse örneğinden Azure blobuna saatlik kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi'ni ayarlayın. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse bağlı hizmeti
Bu SAP BW örneğinizi veri fabrikasına bağlı hizmeti. Type özelliği ayarlamak **SapBw**. TypeProperties bölümünün SAP BW örneği için bağlantı bilgilerini sağlar. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
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

### <a name="sap-bw-input-dataset"></a>SAP BW giriş veri kümesi
Bu veri kümesi, SAP Business Warehouse veri kümesini tanımlar. Data Factory veri kümesi için tür ayarladığınız **RelationalTable**. Şu anda, herhangi bir SAP BW veri kümesi için türe özgü özellikleri belirtmeyin. Kopyalama etkinliği tanımındaki sorgu, SAP BW örneğinden okumak için hangi verilerin belirtir. 

Data Factory hizmeti, dış özelliğin true olarak ayarlanması tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

Frequency ve interval değerleriyle özelliklerini zamanlamayı tanımlar. Bu durumda, veriler, SAP BW örneğinden saatlik okunur. 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
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
Bu veri kümesi, çıktı Azure Blob veri kümesini tanımlar. Type özelliği AzureBlob olarak ayarlanmıştır. SAP BW örneğinden kopyalanan verilerin depolandığı typeProperties bölümünün sağlar. Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** (SAP BW kaynağındaki için) ve **havuz** türü ayarlandığında **BlobSink**. İçin belirtilen sorgu **sorgu** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>SAP BW için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Veri SAP BW taşırken, aşağıdaki eşlemeler SAP BW türlerinden .NET türleri için kullanılır.

ABAP sözlükteki veri türü | .NET veri türü
-------------------------------- | --------------
ACCP |  Int
CHAR | String
CLNT | String
CURR | Decimal
CUKY | String
DEC | Decimal
FLTP | Double
INT1 | Byte
INT2 | Int16
INT4 | Int
LANG | String
LCHR | String
LRAW | Byte[]
PREC | Int16
QUAN | Decimal
RAW | Byte[]
RAWSTRING | Byte[]
STRING | String
UNIT | String
DATS | String
NUMC | String
TIMS | Dize

> [!NOTE]
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).


## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
