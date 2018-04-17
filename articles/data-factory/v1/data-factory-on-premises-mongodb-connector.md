---
title: Data Factory kullanarak MongoDB veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak MongoDB veritabanından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 0afdfb7b7d1f74d3df40b22bb97afc0f39bcc6d1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB gelen veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-on-premises-mongodb-connector.md)
> * [Sürüm 2 - Önizleme](../connector-mongodb.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 MongoDB Bağlayıcısı](../connector-mongodb.md).


Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi MongoDB veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir şirket içi MongoDB veri deposundan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca veri taşımayı MongoDB veri deposundan diğer veri depolarına, ancak verileri diğer veri depolarına bir MongoDB veri deposu taşıma değil destekler. 

## <a name="prerequisites"></a>Önkoşullar
Azure Data Factory hizmetinin şirket içi MongoDB veritabanınıza bağlanmak aşağıdaki bileşenleri yüklemeniz gerekir:

- Desteklenmeyen MongoDB sürümler: 2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6.
- Veri yönetimi veritabanı ile kaynakları için rekabete önlemek için ağ geçidi veritabanını barındıran aynı makine üzerindeki veya ayrı bir makine. Veri Yönetimi ağ geçidi, şirket içi veri kaynakları bulut hizmetlerine güvenli ve yönetilen bir şekilde birbirine bağlayan bir yazılımdır. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale veri yönetimi ağ geçidi hakkında ayrıntılı bilgi için. Bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale verileri taşımak veri ardışık ağ geçidi kurun ayarı ilişkin adım adım yönergeler.

    Ağ geçidi yüklediğinizde, MongoDB için bağlanmak için kullanılan bir Microsoft MongoDB ODBC sürücüsü otomatik olarak yükler.

    > [!NOTE]
    > Azure Iaas Vm'lerde barındırılan olsa bile MongoDB için bağlanmak için ağ geçidi kullanmanız gerekir. Bulutta barındırılan MongoDB örneği bağlanmaya çalışıyorsanız, ağ geçidi örneği Iaas VM'yi yükleyebilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi MongoDB veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir şirket içi MongoDB veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama adresinden Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, belirli Data Factory varlıklarını MongoDB kaynağına tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda JSON öğeleri için belirli bir açıklamasını sağlar **OnPremisesMongoDB** bağlı hizmeti.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesMongoDb** |Evet |
| sunucu |IP adresi veya ana bilgisayar MongoDB sunucunun adıdır. |Evet |
| port |İstemci bağlantılarını dinlemek için MongoDB sunucusunun kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Basic veya Anonymous. |Evet |
| kullanıcı adı |MongoDB erişmek için kullanıcı hesabı. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanı adı. |(Temel kimlik doğrulaması kullanılıyorsa) isteğe bağlı. Varsayılan: yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanır. |
| databaseName |Erişmek istediğiniz MongoDB veritabanı adı. |Evet |
| gatewayName |Veri deposu erişen ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |İsteğe bağlı |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **MongoDbCollection** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| collectionName |MongoDB veritabanı koleksiyonunda adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Kullanılabilir özellikler **typeProperties** etkinlik bölümünü diğer yandan her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **MongoDbSource** typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL-92 sorgu dizesi. Örneğin: seçin * from MyTable. |Hayır (varsa **collectionName** , **dataset** belirtilir) |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a>JSON örnek: veri kopyalama adresinden Azure Blob
Bu örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bir Azure Blob Depolama bir şirket içi MongoDB verileri kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

Örnek aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesMongoDb](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [MongoDbCollection](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [MongoDbSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek veri MongoDB veritabanı bir sorgu sonucunda bir blobu saatte kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

Veri Yönetimi ağ geçidi içindeki yönergeler doğrultusunda ilk adım olarak, Kurulum [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makalesi.

**MongoDB bağlantılı hizmeti:**

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

**Azure Storage bağlı hizmeti:**

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

**MongoDB girdi veri kümesi:** "dış" ayarı: "true" bildirir Data Factory hizmetinin tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```json
{
     "name":  "MongoDbInputDataset",
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

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

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

**MongoDB kaynak ve Blob havuz ile ardışık düzeninde etkinlik kopyalayın:**

Yukarıdaki giriş ve çıktı veri kümeleri için yapılandırılmış bir kopyalama etkinliği ardışık düzen içerir ve saatte çalışacak şekilde zamanlanır. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **MongoDbSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içindeki seçer.

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
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
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


## <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası
Azure Data Factory hizmeti MongoDB koleksiyonundan şema koleksiyonunda en son 100 belgeler kullanarak oluşturur. Bu 100 belgeleri tam şema içermiyorsa kopyalama işlemi sırasında bazı sütunları yoksayılabilir.

## <a name="type-mapping-for-mongodb"></a>MongoDB için tür eşlemesi
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopya etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Veri MongoDB için taşırken aşağıdaki eşlemelerini MongoDB türlerinden .NET türleri için kullanılır.

| MongoDB türü | .NET framework türü |
| --- | --- |
| İkili |Byte] |
| Boole |Boole |
| Tarih |DateTime |
| NumberDouble |Çift |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Dize |
| Dize |Dize |
| UUID |Guid |
| Nesne |Renormalized içine iç içe geçmiş ayırıcı olarak "_" sütunlarla düzleştirme |

> [!NOTE]
> Sanal tablolar kullanarak dizileri desteği hakkında bilgi edinmek için bkz [sanal tablolar kullanarak karmaşık türleri için destek](#support-for-complex-types-using-virtual-tables) bölümüne bakın.

Şu anda, aşağıdaki MongoDB veri türleri desteklenmiyor: DBPointer, JavaScript, en fazla/dak anahtar, normal ifade, simge, zaman damgası, tanımlanmamış

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tablolar kullanarak karmaşık türleri için destek
Azure Data Factory bağlanmak ve MongoDB veritabanınızdan verileri kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Belgeler arasında farklı türleriyle dizileri veya nesneleri gibi karmaşık türler için sürücü veri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, karmaşık tür sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo aynı adını temsil ettiği gerçek tablo olarak kullanır.
* A **sanal tablo** her karmaşık tür sütun için hangi genişletir iç içe veri. Sanal tablolar gerçek tablo, ayırıcı "_" ve dizi veya nesne adı adını kullanarak adlandırılır.

Sanal tablolar Normalleştirilmemiş verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Ayrıntılar aşağıda örnek bölümüne bakın. Sorgulamak ve sanal tabloları birleştirme MongoDB dizi içeriğe erişebilir.

Kullanabileceğiniz [Kopyalama Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) sezgisel sanal tablolar dahil olmak üzere MongoDB veritabanındaki tabloların listesini görüntülemek ve içindeki verilerin önizlemesini görüntülemek için. Ayrıca, kopyalama Sihirbazı'nda bir sorgu oluşturun ve sonuçları görmek için Doğrula.

### <a name="example"></a>Örnek
Örneğin, aşağıda "ExampleTable" her hücresinde – faturaları ve bir dizi skaler türler – derecelendirmeleri bir sütunla nesnelerinin bir dizisi ile bir sütunu bulunan bir MongoDB tablodur.

| _ıd | Müşteri adı | Faturalar | Hizmet Düzeyi | Derecelendirme |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123" öğesi: "toaster", fiyat: "456", indirim: "0.2"}, {invoice_id: "124" öğesi: "fırın", fiyat: "1235" indirim: "0.2"}] |Gümüş |[5,6] |
| 2222 |XYZ |[{invoice_id: "135" öğesi: "fridge", fiyat: "12543", indirim: "0,0"}] |Altın |[1,2] |

Sürücü bu tek tablo göstermek için birden çok sanal tablo oluşturur. İlk sanal "aşağıda gösterilen ExampleTable" adlı temel tablo tablodur. Temel tablo özgün tablonun tüm verileri içerir, ancak diziler verilerden çıkarıldı ve sanal tablolarda genişletilir.

| _ıd | Müşteri adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Gümüş |
| 2222 |XYZ |Altın |

Aşağıdaki tablolarda örnek özgün dizilerde temsil eden sanal tablolar gösterilmektedir. Bu tablolar aşağıdakileri içerir:

* (Aracılığıyla _ıd sütun) özgün dizi satır karşılık gelen geri özgün birincil anahtar sütunu başvurusu
* Göstergesidir özgün dizinin içindeki verilerin konumu
* Dizi içinde her öğe için genişletilmiş veri

Tablo "ExampleTable_Invoices":

| _ıd | ExampleTable_Invoices_dim1_idx | invoice_id | Öğesi | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |fridge |12543 |0.0 |

Tablo "ExampleTable_Ratings":

| _ıd | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki Adımlar
Bkz: [şirket içi ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) verileri bir şirket içi veri deposundan bir Azure veri deposuna taşır veri işlem hattı oluşturmak için adım adım yönergeler için makalesi.
