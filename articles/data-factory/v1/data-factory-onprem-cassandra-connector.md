---
title: Data Factory ile Cassandra ' veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak bir şirket içi Cassandra veritabanındaki verileri veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 5b098aaf2df5e04983aa53563d5e0203f3287b42
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839958"
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Azure Data Factory kullanarak şirket içi Cassandra veritabanındaki veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-onprem-cassandra-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-cassandra.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 Cassandra bağlayıcısında](../connector-cassandra.md).

Bu makalede, bir şirket içi Cassandra veritabanındaki verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Şirket içi Cassandra veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca veri taşımayı Cassandra veri deposundan verileri diğer veri depolarına bir Cassandra veri deposuna taşımak için değil ancak diğer veri depolarını destekler.

## <a name="supported-versions"></a>Desteklenen sürümler
Cassandra bağlayıcı Cassandra aşağıdaki sürümlerini destekler: 2.x ve 3.x. Şirket içinde barındırılan tümleştirme çalışma zamanı, Cassandra 3.x IR sürüm 3.7'den itibaren ve üstünde desteklenir üzerinde çalışan etkinlik için.

## <a name="prerequisites"></a>Önkoşullar
Azure Data Factory hizmetinin şirket içi Cassandra veritabanına bağlanabilmesi için bir veri yönetimi ağ geçidi veritabanını barındıran aynı makinede veya veritabanı ile kaynakları için rekabete önlemek için ayrı bir makineye yüklemeniz gerekir. Veri Yönetimi ağ geçidi, şirket içi veri kaynaklarına bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlayan bir bileşenidir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında bilgi için makalenin. Bkz: [buluta şirket içinden veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale verileri taşımak ağ geçidini ayarlamadan bir veri işlem hattı adım adım yönergeler için.

Veritabanı bulutta, örneğin, Azure Iaas sanal makinesine barındırılıyor olsa bile bir Cassandra veritabanına bağlanmak için ağ geçidi kullanmanız gerekir. Y, veritabanını barındıran aynı VM'de ağ geçidine sahip olabilir veya ağ geçidi olarak uzun ayrı bir VM üzerindeki veritabanına bağlanabilirsiniz.

Ağ geçidini yüklerken Cassandra veritabanına bağlanmak için kullanılan bir Microsoft Cassandra ODBC sürücüsü otomatik olarak yükler. Bu nedenle, Cassandra veritabanından veri kopyalama işlemi sırasında herhangi bir sürücü ağ geçidi makinesinde el ile yüklemeniz gerekmez.

> [!NOTE]
> Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir şirket içi Cassandra veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

- Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.
- Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Şirket içi Cassandra veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Veri Cassandra ' Azure Blob kopyalama](#json-example-copy-data-from-cassandra-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Cassandra veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, JSON öğeleri Cassandra bağlantılı hizmete özgü açıklama belirler.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| türü |Type özelliği ayarlanmalıdır: **OnPremisesCassandra** |Evet |
| host |Bir veya daha fazla IP adresleri veya Cassandra sunucusunun ana bilgisayar adını.<br/><br/>IP adreslerini veya aynı anda tüm sunuculara bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Temel veya anonim |Evet |
| username |Kullanıcı hesabının kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| gatewayName |Şirket içi Cassandra veritabanına bağlanmak için kullanılan ağ geçidi adı. |Evet |
| encryptedCredential |Ağ Geçidi tarafından şifrelenmiş kimlik bilgileri. |Hayır |

>[!NOTE]
>Cassandra SSL kullanarak bağlantı şu anda desteklenmiyor.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **CassandraTable** aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| keySpace |Anahtar alanı veya Cassandra veritabanındaki şema adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |
| tableName |Cassandra veritabanındaki tablonun adı. |Evet (varsa **sorgu** için **CassandraSource** tanımlı değil). |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kaynak türü olduğunda **CassandraSource**, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |92 SQL sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgu kullanarak belirtmeniz **anahtar alanı name.table adı** sorgulamak istediğiniz tablosunu temsil edecek. |Hayır (tableName ve veri kümesi üzerinde anahtar alanı tanımlanmışsa). |
| consistencyLevel |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce kaç çoğaltmalar için Okuma isteği yanıtlamalıdır belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. |BİR, İKİ, ÜÇ SANAL ÇEKİRDEK, TÜMÜ LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Bkz: [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için. |Hayır. Varsayılan değer biridir. |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a>JSON örneği: Azure Blob için veri cassanra'dan kopyalama
Bu örnekte kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu, verileri bir şirket içi Cassandra veritabanındaki verileri Azure Blob Depolama'ya kopyalamak nasıl gösterir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

> [!IMPORTANT]
> Bu örnek JSON parçacıklarını sağlar. Veri Fabrikası oluşturmaya yönelik adım adım yönergeler içermez. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale adım adım yönergeler için.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesCassandra](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [CassandraTable](#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [CassandraSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Cassandra bağlı hizmeti:**

Bu örnekte **Cassandra** bağlı hizmeti. Bkz: [Cassandra bağlı hizmet](#linked-service-properties) bu bağlı hizmeti tarafından desteklenen özellikler bölümü.

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

Ayarı **dış** için **true** Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1).

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

**Cassandra kaynak ve havuz Blob ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **CassandraSource** ve **havuz** türü ayarlandığında **BlobSink**.

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

### <a name="type-mapping-for-cassandra"></a>Cassandra için tür eşlemesi
| Cassandra türü | .NET türüne göre |
| --- | --- |
| ASCII |Dize |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAN |Boole değeri |
| DECIMAL |Decimal |
| DOUBLE |Double |
| FLOAT |Single |
| INET |Dize |
| INT |Int32 |
| TEXT |Dize |
| TIMESTAMP |Datetime |
| TIMEUUID |Guid |
| UUID |Guid |
| VARCHAR |Dize |
| VARINT |Decimal |

> [!NOTE]
> Türler (harita, set, list, vb.), başvurmak için koleksiyon [iş Cassandra koleksiyon türlerini kullanarak sanal bir tablo](#work-with-collections-using-virtual-table) bölümü.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> İkili bir sütunu ve dize sütunu uzunlukları uzunluğu 4000 ' büyük olamaz.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tablosunu kullanarak collections ile çalışma
Azure Data Factory, bağlanma ve Cassandra veritabanınızdan veri kopyalamak için yerleşik bir ODBC sürücüsünü kullanır. Harita, küme ve liste gibi koleksiyon türleri için karşılık gelen sanal tablolarına veri sürücü renormalizes. Özellikle, bir tablo koleksiyonu sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, aynı verileri toplama sütunları hariç gerçek tablosu içerir. Temel tablo adıyla temsil ettiği gerçek tablosu olarak kullanır.
* A **sanal tablo** her toplama sütunu için genişleyen iç içe geçmiş verileri. Gerçek bir ayırıcı tablonun adını kullanarak koleksiyonları temsil eden sanal tablolar adlı "*vt*" sütun adı.

Sanal tablolar normalleştirilmişlikten çıkarılmış verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Ayrıntılar için örnek bölümüne bakın. Cassandra koleksiyonları içeriğini sorgulama ve sanal tabloları birleştirme erişebilirsiniz.

Kullanabileceğiniz [Kopyalama Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) sezgisel sanal tablolar da dahil olmak üzere Cassandra veritabanındaki tabloların listesini görüntülemek ve içindeki verileri önizlemek için. Kopyalama Sihirbazı'nı bir sorgu oluşturun ve sonuçları görmek için doğrulayın.

### <a name="example"></a>Örnek
Örneğin, aşağıdaki "ExampleTable", "pk_int", değer adlı bir metin sütunu, bir liste sütunu, sütun eşleme ve ("StringSet" adlı) bir kümesi sütunu adlı bir tamsayı birincil anahtar sütunu içeren bir Cassandra veritabanı tablosu olur.

| pk_int | Value | List | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1\. |"örnek değeri 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü bu tek tabloda temsil etmek için birden çok sanal tablolar oluşturur. Sanal tablolarındaki yabancı anahtar sütunları gerçek tablosundaki birincil anahtar sütunlarını başvuru ve sanal tablo satırı karşılık gelen gerçek tablosu satırı belirtmenize.

İlk sanal "ExampleTable" adlı temel tablo aşağıdaki tabloda gösterilen bir tablodur. Temel tablo, bu tablodan atlanmış ve diğer sanal tablolarında genişletilmiş koleksiyonları dışında özgün veritabanı tablosunu aynı verileri içerir.

| pk_int | Value |
| --- | --- |
| 1\. |"örnek değeri 1" |
| 3 |"örnek value 3" |

Aşağıdaki tablolar, liste ve eşleme StringSet sütundaki verileri normalleştirebilir sanal tabloları gösterir. "_Index" veya "_anahtarı" ile biten sütunları verilerin özgün liste veya harita içinde konumunu belirtin. Koleksiyon genişletilmiş verileri "_Değeri" ile biten sütunları içerir.

#### <a name="table-exampletablevtlist"></a>Tablo "ExampleTable_vt_List":
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1\. |0 |1\. |
| 1\. |1\. |2 |
| 1\. |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>Tablo "ExampleTable_vt_Map":
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1\. |S1 |A |
| 1\. |S2 |b |
| 3 |S1 |t |

#### <a name="table-exampletablevtstringset"></a>Table “ExampleTable_vt_StringSet”:
| pk_int | StringSet_value |
| --- | --- |
| 1\. |A |
| 1\. |B |
| 1\. |C |
| 3 |A |
| 3 |E |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
