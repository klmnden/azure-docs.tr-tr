---
title: Data Factory kullanarak Cassandra veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak bir şirket içi Cassandra veritabanından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6e6b9bf194da17ebd03389829ba594bf3fbf1e64
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34622110"
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Azure Data Factory kullanarak bir şirket içi Cassandra veritabanından veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-cassandra-connector.md)
> * [Sürüm 2 - Önizleme](../connector-cassandra.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Cassandra Bağlayıcısı](../connector-cassandra.md).

Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi Cassandra veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Bir şirket içi Cassandra veri deposundan verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca veri taşımayı Cassandra veri deposundan diğer veri depolarına, ancak verileri diğer veri depolarına Cassandra veri deposuna taşıma değil destekler. 

## <a name="supported-versions"></a>Desteklenen sürümler
Cassandra bağlayıcı Cassandra aşağıdaki sürümlerini destekler: 2.X.

## <a name="prerequisites"></a>Önkoşullar
Azure Data Factory hizmetinin şirket içi Cassandra veritabanınıza bağlanmak veritabanını barındıran aynı makine üzerindeki veya veritabanı ile kaynakları için rekabete önlemek için ayrı bir makine veri yönetimi ağ geçidi yüklemeniz gerekir. Veri Yönetimi ağ geçidi, şirket içi veri kaynakları bulut hizmetlerine güvenli ve yönetilen bir şekilde birbirine bağlayan bir bileşenidir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale veri yönetimi ağ geçidi hakkında ayrıntılı bilgi için. Bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale verileri taşımak veri ardışık ağ geçidi kurun ayarı ilişkin adım adım yönergeler.

Veritabanı bulutta, örneğin, bir Azure Iaas sanal bile barındırılıyorsa Cassandra veritabanına bağlanmak için ağ geçidi kullanmanız gerekir. Y, veritabanını barındıran aynı VM ağ geçidi olabilir veya ağ geçidi olarak uzunluğunda ayrı bir VM'de veritabanına bağlanabilirsiniz.  

Ağ geçidi yüklediğinizde, Cassandra veritabanına bağlanmak için kullanılan bir Microsoft Cassandra ODBC sürücüsü otomatik olarak yükler. Bu nedenle, Cassandra veritabanından veri kopyalama işlemi sırasında herhangi bir sürücüsü ağ geçidi makinesi üzerinde el ile yüklemeniz gerekmez. 

> [!NOTE]
> Bkz: [ağ geçidi sorunlarını giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) ilgili sorunlar bağlantı/ağ geçidi sorun giderme ipuçları için.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun. 

- Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz. 
- Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir şirket içi Cassandra veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama Cassandra Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) bu makalenin. 

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Cassandra veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, JSON öğeleri Cassandra bağlantılı hizmete özgü açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesCassandra** |Evet |
| konak |Bir veya daha fazla IP adresleri veya ana bilgisayar adlarını Cassandra sunucuları.<br/><br/>IP adreslerini veya aynı anda tüm sunucularına bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |İstemci bağlantılarını dinlemek için Cassandra sunucusunun kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Basic veya Anonymous |Evet |
| kullanıcı adı |Kullanıcı hesabının kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| gatewayName |Şirket içi Cassandra veritabanına bağlanmak için kullanılan ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |Hayır |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **CassandraTable** aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| keyspace |Keyspace veya Cassandra veritabanındaki şema adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |
| tableName |Cassandra veritabanı tablosunun adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **CassandraSource**, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL-92 sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgusu kullanırken belirtin **keyspace name.table adı** sorgulamak istediğiniz tabloyu temsil etmek için. |Hayır (tableName ve veri kümesi üzerinde keyspace tanımlanmışsa). |
| consistencyLevel |Tutarlılık düzeyi kaç çoğaltmaları Okuma isteği için veri istemci uygulamasına geri dönmeden önce yanıt vermesi gereken belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. |BİR, İKİ, ÜÇ, ÇEKİRDEK, TÜMÜ, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Bkz: [veri tutarlılığını yapılandırma](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için. |Hayır. Varsayılan değer biridir. |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a>JSON örnek: veri kopyalama Cassandra Azure Blob
Bu örnek kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bir Azure Blob depolama alanına bir şirket içi Cassandra veritabanından veri kopyalamak nasıl gösterir. Ancak, veri herhangi belirtildiği havuzlarını kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

> [!IMPORTANT]
> Bu örnek, JSON parçacıklarını sağlar. Data factory oluşturmak için adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesCassandra](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [CassandraTable](#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [CassandraSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Bağlantılı Cassandra hizmeti:**

Bu örnekte **Cassandra** bağlı hizmeti. Bkz: [Cassandra bağlantılı hizmeti](#linked-service-properties) bu bağlantılı hizmet tarafından desteklenen özellikler bölümü.  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
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

**Cassandra girdi veri kümesi:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Etkinlik Cassandra kaynak ve Blob havuz sahip işlem hattı kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **CassandraSource** ve **havuz** türü ayarlanmış **BlobSink**.

Bkz: [RelationalSource türü özellikleri](#copy-activity-properties) RelationalSource tarafından desteklenen özelliklerin listesi için.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

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

### <a name="type-mapping-for-cassandra"></a>Tür eşlemesi için Cassandra
| Cassandra türü | .NET türü temelinde |
| --- | --- |
| ASCII |Dize |
| BIGINT |Int64 |
| BLOB |Byte] |
| BOOLE DEĞERİ |Boole |
| ONDALIK |Ondalık |
| ÇİFT |Çift |
| KAYAN NOKTA |Tek |
| INET |Dize |
| INT |Int32 |
| METİN |Dize |
| ZAMAN DAMGASI |DateTime |
| TIMEUUID |Guid |
| UUID |Guid |
| VARCHAR |Dize |
| VARINT |Ondalık |

> [!NOTE]
> Türleri (harita, kümesi, liste, vb.), başvurmak için koleksiyonu [iş Cassandra koleksiyon türlerini sanal tablosunu kullanarak](#work-with-collections-using-virtual-table) bölümü.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> İkili sütun ve dize sütunu uzunluklarını uzunluğu 4000'den büyük olamaz.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tablosunu kullanarak koleksiyonları ile çalışma
Azure Data Factory bağlanmak ve Cassandra veritabanınızdan verileri kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Harita, kümesi ve listesi dahil olmak üzere koleksiyon türü için karşılık gelen sanal tablolara veri sürücüsü renormalizes. Özellikle, bir tablo tüm koleksiyon sütunları içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, koleksiyon sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo aynı adını temsil ettiği gerçek tablo olarak kullanır.
* A **sanal tablo** her koleksiyon sütun için hangi genişletir iç içe veri. Koleksiyonları temsil eden sanal tablolar ayırıcı gerçek tablosunun adı kullanılarak adlandırılmış "*vt*" ve sütunun adı.

Sanal tablolar Normalleştirilmemiş verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Ayrıntılar için örnek bölümüne bakın. Sorgulamak ve sanal tabloları birleştirme Cassandra koleksiyonların içeriğe erişebilir.

Kullanabileceğiniz [Kopyalama Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) sezgisel sanal tablolar dahil olmak üzere Cassandra veritabanındaki tabloların listesini görüntülemek ve içindeki verilerin önizlemesini görüntülemek için. Ayrıca, kopyalama Sihirbazı'nda bir sorgu oluşturun ve sonuçları görmek için Doğrula.

### <a name="example"></a>Örnek
Örneğin, aşağıdaki "ExampleTable", "pk_int", değer adlı bir text sütunu, bir liste sütunu, sütun eşleme ve ("StringSet" olarak adlandırılır) bir sütunu Ayarla adlı bir tamsayı birincil anahtar sütunu içeren bir Cassandra veritabanı tablosu verilmiştir.

| pk_int | Değer | Liste | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"örnek değeri 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü bu tek tablo göstermek için birden çok sanal tablo oluşturur. Sanal tablolardaki yabancı anahtar sütunları gerçek tablosundaki birincil anahtar sütunlarının başvuru ve sanal tablo satırı karşılık gelen gerçek tablo satırı belirtmenize.

İlk sanal "ExampleTable" adlı temel tablo aşağıdaki tabloda gösterilen tablodur. Temel tablo özgün veritabanı tablosunun bu tablodan atlanmış ve diğer sanal tablolarda genişletilmiş koleksiyonları dışında aynı verileri içerir.

| pk_int | Değer |
| --- | --- |
| 1 |"örnek değeri 1" |
| 3 |"örnek value 3" |

Aşağıdaki tablolarda listesi, eşleme ve StringSet sütunları verilerden renormalize sanal tablolar gösterilmektedir. Yalnızca "_index" veya "_anahtar" ile biten adlarını sütunlarla özgün listesi veya eşlemesi içinde veri konumunu belirtin. Yalnızca "_value" ile biten adlarını sütunlarla koleksiyondan genişletilmiş verileri içerir.

#### <a name="table-exampletablevtlist"></a>Tablo "ExampleTable_vt_List":
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>Tablo "ExampleTable_vt_Map":
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |T |

#### <a name="table-exampletablevtstringset"></a>Tablo "ExampleTable_vt_StringSet":
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
