---
title: ODBC veri depolarından veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak ODBC veri depolarından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 0db301109da657083d121cc8b986372adcb54171
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389036"
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Azure Data Factory ile veri öğesinden ODBC veri depoları Taşı
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-odbc-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-odbc.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [ODBC Bağlayıcısı V2'de](../connector-odbc.md).


Bu makalede, şirket içi ODBC veri deposundan veri taşımanız Azure veri fabrikasında kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Bir ODBC veri deposundan desteklenen bir havuz veri deposuna veri kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data factory şu anda yalnızca veri taşımayı bir ODBC veri deposundan verileri diğer veri depolarına bir ODBC veri deposuna taşımak için değil ancak diğer veri depolarını destekler.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Data Factory hizmeti, veri yönetimi ağ geçidi kullanarak şirket içi ODBC kaynağına bağlanmayı destekler. Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalenin veri yönetimi ağ geçidi ve ağ geçidini ayarlamadan adım adım yönergeleri hakkında bilgi edinin. Bile bir Azure Iaas VM'de barındırılan bir ODBC veri deposuna bağlanmak için ağ geçidi'ni kullanın.

ODBC veri deposu olarak aynı şirket içi makine ya da Azure sanal makinesine bir ağ geçidi yükleyebilirsiniz. Ancak, ayrı makine/Azure Iaas VM kaynak çekişmesini önlemek ve daha iyi performans için ağ geçidi yüklemeniz önerilir. Ağ geçidi ayrı bir makineye yüklediğinizde, makine ODBC veri deposu olan makine erişebilir olmalıdır.

Veri Yönetimi ağ geçidi dışında da ağ geçidi makinesinde veri deposu için ODBC sürücüsünden yüklemeniz gerekir.

> [!NOTE]
> Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir ODBC veri deposundan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Bir ODBC veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: ODBC veri deposundan Azure Blob veri kopyalamak](#json-example-copy-data-from-odbc-data-store-to-azure-blob) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli ODBC veri deposuna tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Aşağıdaki tabloda, JSON öğeleri için ODBC belirli için açıklama bağlı hizmeti sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesOdbc** |Evet |
| connectionString |Bağlantı dizesini ve isteğe bağlı bir şifrelenmiş kimlik bilgileri olmayan erişim kimlik bilgileri bölümü. Aşağıdaki bölümlerde örneklere bakın. <br/><br/>Desen gibi ile bağlantı dizesini belirtin `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, veya sistem DSN'si ayarladığınız ile ağ geçidi makinesinde (veri kaynağı adı) kullanmak `"DSN=<name of the DSN>;"` (hala kimlik bilgisi bölümü bağlı hizmette uygun şekilde belirttiğiniz). |Evet |
| credential |Erişim kimlik bilgisi sürücüye özel özellik-değer biçiminde belirtilen bağlantı dizesi kısmı. Örnek: `"Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;"`. |Hayır |
| authenticationType |ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü. Olası değerler şunlardır: Anonim ve temel. |Evet |
| userName |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |
| gatewayName |Data Factory hizmetinin ODBC veri deposuna bağlanmak için kullanması gereken ağ geçidi adı. |Evet |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulaması kullanma

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Temel kimlik doğrulaması ile şifrelenmiş kimlik bilgilerini kullanma
Kimlik bilgilerini kullanarak şifreleyebilirsiniz [yeni AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) (Azure PowerShell 1.0 sürümü) cmdlet'ini veya [yeni AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 veya önceki bir sürümünü Azure PowerShell).

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulaması

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. TypeProperties bölümü için veri kümesi türü **RelationalTable** (ODBC veri kümesini içeren) aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |ODBC veri deposundaki tablosunun adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Bulunan özelliklerin **typeProperties** etkinlik bölümünü diğer yandan her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

Kopya etkinlikteki kaynak türü olduğunda, **RelationalSource** (ODBC içeren), typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| query |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * MyTable öğesinden. |Evet |


## <a name="json-example-copy-data-from-odbc-data-store-to-azure-blob"></a>JSON örneği: ODBC veri deposundan Azure Blob veri kopyalamak
Bu örnek, kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz JSON tanımları sağlar, [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bu, bir Azure Blob depolama alanına bir ODBC kaynaktan veri kopyalamak nasıl gösterir. Ancak, veriler belirtilen havuzlarını birine kopyalanabilir [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

1. Bağlı hizmet türü [OnPremisesOdbc](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri bir ODBC veri deposuna bir sorgu sonucunda bir bloba saatte kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi ayarlama. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**ODBC bağlı hizmet** Bu örnek, temel kimlik doğrulaması kullanır. Bkz: [ODBC bağlı hizmet](#linked-service-properties) bölüm için farklı türde kimlik doğrulaması kullanabilirsiniz.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Storage bağlı hizmeti**

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

**ODBC girdi veri kümesi**

Örnek, bir ODBC veritabanı "MyTable" Tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
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

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**ODBC kaynağını (RelationalSource) ve Blob havuz (BlobSink) ile bir işlem hattındaki kopyalama etkinliği**

İşlem hattı, bu girdi ve çıktı veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
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
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>ODBC için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

ODBC veri depolarından veri taşıma, ODBC veri türleri için .NET türlerini belirtildiği gibi eşleştirildikleri [ODBC veri türü eşlemeleri](https://msdn.microsoft.com/library/cc668763.aspx) konu.

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi için eşleme sütunları hakkında bilgi edinmek için bkz. [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme
Bağlantı sorunlarını gidermek için **tanılama** sekmesinde **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni**.

1. Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni**. Çalıştırabilir ya da "C:\Program Files\Microsoft veri yönetimi Gateway\1.0\Shared\ConfigManager.exe" doğrudan (veya) arama için **ağ geçidi** bağlantısını bulmak için **Microsoft Veri Yönetimi ağ geçidi** Aşağıdaki görüntüde gösterildiği gibi uygulama.

    ![Arama ağ geçidi](./media/data-factory-odbc-connector/search-gateway.png)
2. Geçiş **tanılama** sekmesi.

    ![Ağ geçidi tanılama](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Seçin **türü** (bağlı hizmet) verilerini depolar.
4. Belirtin **kimlik doğrulaması** girin **kimlik bilgilerini** (veya) girin **bağlantı dizesi** veri deposuna bağlanmak için kullanılır.
5. Tıklayın **Bağlantıyı Sına** veri deposuna test edin.

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
