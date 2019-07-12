---
title: Mongodb'deki Data Factory ile verileri taşıma | Microsoft Docs
description: MongoDB veritabanından Azure Data Factory ile veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e7a84d74e1bda6de8549c79dab1bec8c2515e213
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839074"
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB gelen veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-on-premises-mongodb-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-mongodb.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de MongoDB bağlayıcısını](../connector-mongodb.md).


Bu makalede, bir şirket içi MongoDB veritabanından verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi MongoDB veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca verileri bir MongoDB veri deposundaki verileri diğer veri depolarına bir MongoDB veri deposuna taşımak için değil ancak diğer veri depolarına destekler.

## <a name="prerequisites"></a>Önkoşullar
Azure Data Factory hizmetinin şirket içi MongoDB veritabanına bağlanabilmesi için aşağıdaki bileşenleri yüklemeniz gerekir:

- Desteklenen MongoDB sürümleri şunlardır: 2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6.
- Veri yönetimi için kaynaklar veritabanı ile rekabet önlemek için ağ geçidi veritabanını barındıran aynı makinede veya ayrı bir makine. Veri Yönetimi ağ geçidi, şirket içi veri kaynaklarına bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlayan bir yazılımdır. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında bilgi için makalenin. Bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale verileri taşımak ağ geçidini ayarlamadan bir veri işlem hattı adım adım yönergeler için.

    Ağ geçidini yüklerken, Mongodb'ye bağlanmak için kullanılan bir Microsoft MongoDB ODBC sürücüsü otomatik olarak yükler.

    > [!NOTE]
    > Azure Iaas Vm'lerinde barındırılıyor olsa bile, Mongodb'ye bağlanmak için ağ geçidi kullanmanız gerekir. Bulutta barındırılan bir MongoDB örneğine bağlanmaya çalışıyorsanız, ağ geçidi örneğini Iaas sanal Makineye yükleyebilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi MongoDB veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Şirket içi MongoDB veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Azure Blob Mongodb'den kopyalama](#json-example-copy-data-from-mongodb-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli MongoDB kaynağına tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda verilmiştir JSON öğelerinin özgü açıklama **OnPremisesMongoDB** bağlı hizmeti.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| türü |Type özelliği ayarlanmalıdır: **OnPremisesMongoDb** |Evet |
| server |IP adresi veya ana bilgisayar adı MongoDB sunucusunun. |Evet |
| port |MongoDB sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Temel veya anonim. |Evet |
| username |MongoDB erişmek için kullanıcı hesabı'nı tıklatın. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |(Temel kimlik doğrulaması kullanılıyorsa) isteğe bağlı. Varsayılan: yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanır. |
| databaseName |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| gatewayName |Veri deposu erişen bir ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |İsteğe Bağlı |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **MongoDbCollection** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| collectionName |MongoDB veritabanındaki koleksiyonun adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Bulunan özelliklerin **typeProperties** etkinlik bölümünü diğer yandan her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **MongoDbSource** typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |SQL 92 sorgu dizesi. Örneğin: seçin * MyTable öğesinden. |Hayır (varsa **collectionName** , **veri kümesi** belirtilir) |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a>JSON örneği: Azure Blob Mongodb'deki verileri kopyalama
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu, bir Azure Blob depolama alanına bir şirket içi Mongodb'deki verileri kopyalamak nasıl gösterir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesMongoDb](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [MongoDbCollection](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [MongoDbSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri MongoDB veritabanına bir sorgu sonucunda bir bloba saatte kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi içindeki yönergeler doğrultusunda Kurulum [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makalesi.

**MongoDB bağlı hizmeti:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

**Azure depolama bağlı hizmeti:**

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

**MongoDB girdi veri kümesi:** "Dış" ayarını: "true" bildirir Data Factory hizmetinin tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**MongoDB kaynak ve havuz Blob ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattı yukarıdaki Giriş ve çıkış veri kümesi için yapılandırılmış bir kopyalama etkinliği içeren ve saatte çalışacak şekilde zamanlanır. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **MongoDbSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması
Azure Data Factory hizmetinin en son 100 belgelerin koleksiyonu kullanarak bir MongoDB koleksiyonu şemasından çıkarır. 100 bu belgeler, tam şema içermiyorsa, kopyalama işlemi sırasında bazı sütunları yoksayılabilir.

## <a name="type-mapping-for-mongodb"></a>MongoDB için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki 2 adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Verileri, Mongodb'ye taşırken şu eşlemeler MongoDB türlerinden .NET türleri için kullanılır.

| MongoDB türü | .NET framework türü |
| --- | --- |
| Binary |Byte[] |
| Boole değeri |Boole değeri |
| Date |Datetime |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| Nesne Kimliği |Dize |
| String |Dize |
| UUID |Guid |
| Object |Renormalized içine sütunları içeren iç içe geçmiş ayırıcı olarak "_" düzleştirme |

> [!NOTE]
> Sanal tablolar kullanarak dizileri için destek hakkında bilgi edinmek için başvurmak [sanal tabloları kullanarak karmaşık türler için destek](#support-for-complex-types-using-virtual-tables) bölümüne bakın.

Şu anda aşağıdaki MongoDB veri türleri desteklenmez: Normal ifade, sembol, zaman damgası, tanımlanmamış DBPointer, JavaScript, en yüksek/dak anahtar

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tablolar'ı kullanarak karmaşık türler için destek
Azure Data Factory, bağlanma ve MongoDB veritabanından veri kopyalamak için yerleşik bir ODBC sürücüsünü kullanır. Belgeler arasında farklı tür ile diziler veya nesneler gibi karmaşık türler için sürücü veri karşılık gelen sanal tablolarına yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, karmaşık tür sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo adıyla temsil ettiği gerçek tablosu olarak kullanır.
* A **sanal tablo** her bir karmaşık türü sütun için genişleyen iç içe veri. Sanal tablolar, gerçek tablosu, "_" ayırıcı ve dizi veya nesne adı adını kullanarak yeniden adlandırılır.

Sanal tablolar normalleştirilmişlikten çıkarılmış verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Aşağıdaki ayrıntıları örnek bölümüne bakın. MongoDB diziler içeriğini sorgulama ve sanal tabloları birleştirme erişebilirsiniz.

Kullanabileceğiniz [Kopyalama Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) sezgisel sanal tablolar da dahil olmak üzere MongoDB veritabanındaki tabloların listesini görüntülemek ve içindeki verileri önizlemek için. Kopyalama Sihirbazı'nı bir sorgu oluşturun ve sonuçları görmek için doğrulayın.

### <a name="example"></a>Örnek
Örneğin, aşağıdaki "ExampleTable" nesneleri içeren bir dizi içeren bir sütun her hücre – faturaları ve skaler türler – derecelendirmeleri bir dizi içeren bir sütun içeren bir MongoDB tablodur.

| _id | Müşteri adı | Faturalar | Hizmet Düzeyi | Derecelendirme |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123" öğesi: "toaster", price: "456" indirim: "0.2"}, {invoice_id: "124" öğesi: "fırın", price: indirim "1235": "0.2"}] |Silver |[5,6] |
| 2222 |XYZ |[{invoice_id: "135" öğesi: "fridge", price: "12543" indirim: "0.0"}] |Gold |[1,2] |

Sürücü bu tek tabloda temsil etmek için birden çok sanal tablolar oluşturur. İlk sanal "aşağıda gösterilen ExampleTable" adlı temel tablo tablosudur. Temel tablo özgün tablonun tüm verileri içerir, ancak dizileri verilerden çıkarıldı ve sanal tablolarında genişletilir.

| _id | Müşteri adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Silver |
| 2222 |XYZ |Gold |

Aşağıdaki tablolar, özgün diziler örnekte temsil eden sanal tablolar gösterir. Bu tablolar arasında aşağıdakiler yer alır:

* Özgün birincil anahtar sütunu satır (aracılığıyla _kimliği sütun) özgün dizinin karşılık gelen bir başvuru dön
* Verileri özgün dizi içinde konumunu bir göstergesi
* Dizideki her öğe için genişletilmiş verileri

Tablo "ExampleTable_Invoices":

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | Öğesi | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1\. |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |fridge |12543 |0.0 |

Tablo "ExampleTable_Ratings":

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1\. |6 |
| 2222 |0 |1\. |
| 2222 |1\. |2 |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki Adımlar
Bkz: [şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) veri bir şirket içi veri deposundan bir Azure veri deposuna taşıyan bir veri işlem hattı oluşturmak için adım adım yönergeler için makalesi.
