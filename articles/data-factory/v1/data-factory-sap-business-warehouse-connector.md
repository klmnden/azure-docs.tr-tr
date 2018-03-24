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
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 38c1611c0404202be2e100d3059b4ba1ed1a9236
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Veri alanından SAP Business Azure Data Factory kullanarak ambar taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-sap-business-warehouse-connector.md)
> * [Sürüm 2 - Önizleme](../connector-sap-business-warehouse.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 SAP Business Warehouse Bağlayıcısı](../connector-sap-business-warehouse.md).


Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi SAP Business Warehouse (BW) gelen verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir şirket içi SAP Business Warehouse veri deposundan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca taşıma verilerden bir SAP Business Warehouse diğer veri depolarına, ancak verileri diğer veri depolarına bir SAP Business Warehouse taşıma değil destekler. 

## <a name="supported-versions-and-installation"></a>Desteklenen sürümleri ve yükleme
Bu bağlayıcı SAP Business Warehouse sürümünü destekleyen 7.x. MDX sorguları kullanarak veri kopyalamayı Infocubes ve QueryCubes (dahil olmak üzere BEx sorgular) destekler.

SAP BW örneği bağlantıyı etkinleştirmek için aşağıdaki bileşenleri yükleyin:
- **Veri Yönetimi ağ geçidi**: Data Factory hizmeti desteklediği şirket içi verilere bağlanma (SAP Business Warehouse dahil) depoları bir bileşeni kullanılarak veri yönetimi ağ geçidi çağrılır. Veri Yönetimi ağ geçidi ve adım adım yönergeler için ağ geçidi ayarlama hakkında bilgi edinmek için [şirket içi veri arasında taşıma verilerini depolamak veri deposu buluta](data-factory-move-data-between-onprem-and-cloud.md) makalesi. SAP Business Warehouse bir Azure Iaas sanal makine (VM) barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanıp sürece veri deposu olarak aynı VM veya farklı bir VM ağ geçidi yükleyebilirsiniz.
- **SAP NetWeaver Kitaplığı** ağ geçidi bilgisayarında. SAP Netweaver kitaplığı SAP yöneticinizden ya da doğrudan alabilirsiniz [SAP yazılım İndirme Merkezi](https://support.sap.com/swdc). Arama **SAP Not #1025361** en son sürümü karşıdan yükleme konumu alınamıyor. SAP NetWeaver kitaplığı (32 bit veya 64 bit) için Mimari, ağ geçidi yüklemenizi eşleştiğinden emin olun. Daha sonra SAP Not göre SAP NetWeaver RFC SDK'sı bulunan tüm dosyaları yükleyin. SAP NetWeaver kitaplığı SAP istemci araçlarını yükleme de dahil edilir.

> [!TIP]
> NetWeaver RFC SDK'dan system32 klasörüne ayıklanan DLL'leri yerleştirin.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun. 

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz. 
- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir şirket içi SAP Business Warehouse verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama SAP Business Warehouse Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir SAP BW veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri SAP Business Warehouse (BW) bağlantılı hizmete özgü açıklamasını sağlar.

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP BW örneği bulunduğu sunucunun adıdır. | string | Evet
systemNumber | SAP BW sisteminin sistem numarası. | Bir dize olarak gösterilen iki basamaklı ondalık sayı. | Evet
clientId | SAP W sistem istemcisinde istemci kimliği. | Bir dize olarak gösterilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı | string | Evet
password | Kullanıcının parolası. | string | Evet
gatewayName | Data Factory hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
encryptedCredential | Şifrelenmiş kimlik bilgileri dizesi. | string | Hayır

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. SAP BW veri kümesi türü için desteklenen türüne özgü özellikler yok **RelationalTable**. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları gibi özellikleri olan ilkeleri etkinlikleri tüm türleri için kullanılabilir.

Bulunan özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kopyalama etkinliği kaynağında türü olduğunda **RelationalSource** (içeren SAP BW), aşağıdaki özellikler typeProperties bölümünde kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP BW örneğinden verileri okumak için MDX Sorgusu belirtir. | MDX Sorgusu. | Evet |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a>JSON örnek: veri kopyalama SAP Business Warehouse Azure Blob
Aşağıdaki örneği kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu örnek, bir şirket içi SAP Business Warehouse bir Azure Blob depolama alanına veri kopyalama gösterilmektedir. Ancak, veriler kopyalanabilir **doğrudan** belirtildiği havuzlarını hiçbirine [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.  

> [!IMPORTANT]
> Bu örnek, JSON parçacıklarını sağlar. Data factory oluşturmak için adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [SapBw](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri bir SAP Business Warehouse örneğinden bir Azure blob saatlik kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidi ayarlayın. Yönergeler bulunan [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse hizmeti bağlı
Bu hizmet bağlantılar, SAP BW data factory bağlı örneği. Type özelliği ayarlamak **SapBw**. TypeProperties bölüm SAP BW örneği için bağlantı bilgilerini sağlar. 

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
Bu hizmeti Azure Storage hesabınızı data factory bağlantılı. Type özelliği ayarlamak **AzureStorage**. TypeProperties bölümünde Azure depolama hesabı için bağlantı bilgilerini sağlar.

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

### <a name="sap-bw-input-dataset"></a>SAP BW girdi veri kümesi
Bu veri kümesi SAP Business Warehouse veri kümesini tanımlamaktadır. Data Factory veri kümesi için türünü ayarlayın **RelationalTable**. Şu anda bir SAP BW veri kümesi için herhangi bir türe özgü özelliği belirtmeyin. Kopyalama etkinliği tanımı sorguda SAP BW örneğinden okumak için hangi verilerin belirtir. 

Dış özelliği true olarak ayarlanmasını Data Factory hizmetinin tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

Sıklık ve aralığı özelliklerini zamanlamayı tanımlar. Bu durumda, veriler SAP BW örneğinden saatlik okunur. 

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
Bu veri kümesini çıktı Azure Blob dataset tanımlar. Type özelliği AzureBlob olarak ayarlanmıştır. SAP BW örneğinden kopyalanan verilerin depolandığı typeProperties bölüm sağlar. Veriler her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

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


### <a name="pipeline-with-copy-activity"></a>Kopyalama etkinliği ile kanalı
Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **RelationalSource** (SAP BW kaynağı için) ve **havuz** türü ayarlanmış **BlobSink**. İçin belirtilen sorgu **sorgu** özelliği veri kopyalamak için son bir saat içindeki seçer.

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
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopyalama etkinliği, aşağıdaki iki aşamalı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

SAP BW verilerin taşınması, aşağıdaki eşlemelerini SAP BW türlerinden .NET türleri için kullanılır.

ABAP sözlükteki veri türü | .NET veri türü
-------------------------------- | --------------
ACCP |  Int
CHAR | Dize
CLNT | Dize
CURR | Ondalık
CUKY | Dize
DEC | Ondalık
FLTP | Çift
INT1 | Bayt
INT2 | Int16
INT4 | Int
DİL | Dize
LCHR | Dize
LRAW | Byte]
PREC | Int16
QUAN | Ondalık
RAW | Byte]
RAWSTRING | Byte]
DİZE | Dize
BİRİM | Dize
DATS | Dize
NUMC | Dize
TIMS | Dize

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).


## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
