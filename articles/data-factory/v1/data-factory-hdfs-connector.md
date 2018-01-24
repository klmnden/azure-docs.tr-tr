---
title: "Şirket içi HDFS veri taşıma | Microsoft Docs"
description: "Azure Data Factory kullanarak şirket içi HDFS veri taşıma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 87acbe81d20e0f2b209565eace16de1b979b1d96
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Azure Data Factory kullanarak şirket içi HDFS taşıma verileri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-hdfs-connector.md)
> * [Sürüm 2 - Önizleme](../connector-hdfs.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 HDFS Bağlayıcısı](../connector-hdfs.md).

Bu makalede kopya etkinliği Azure Data Factory'de bir şirket içi HDFS verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Verileri HDFS tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca taşıma verilerden bir şirket içi HDFS diğer veri depolarına, ancak verileri diğer veri depolarına bir şirket içi HDFS taşıma değil destekler.

> [!NOTE]
> Hedefe başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosyasını silmez. Kaynak dosya sonra başarılı bir kopyasını silmeniz gerekirse, dosyayı silin ve ardışık düzeninde etkinlik kullanmak için özel bir aktivite oluşturun. 

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Data Factory hizmetinin şirket içi HDFS için veri yönetimi ağ geçidi kullanarak bağlanmayı desteklemektedir. Bkz: [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale veri yönetimi ağ geçidi ve ağ geçidi kurun ayarlama hakkında adım adım yönergeleri hakkında bilgi edinin. Bir Azure Iaas sanal barındırılan olsa bile HDFS için bağlanmak için ağ geçidi'ni kullanın.

> [!NOTE]
> Emin olun veri yönetimi ağ geçidi için erişebilir **tüm** [ad sunucusu düğümü]: [ad düğümü bağlantı noktası] ve [veri düğümü sunucuları]: [veri düğümü bağlantı noktası] Hadoop kümesi. [Adı düğümü bağlantı noktası] 50070 varsayılandır ve [veri düğümü bağlantı noktası] 50075 varsayılandır.

Aynı şirket içi makinede veya HDFS olarak Azure VM ağ geçidi yükleme sırasında bir ayrı makine/Azure Iaas VM ağ geçidi yüklemenizi öneririz. Ağ geçidi ayrı bir makinede sahip kaynak çekişmesini azaltır ve performansı artırır. Ağ geçidi ayrı bir makineye yüklediğinizde, makine HDFS makineyle erişebilir olması gerekir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak verileri HDFS kaynağından taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için.
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  HDFS veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama şirket içi HDFS Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) bu makalenin.

Aşağıdaki bölümler için HDFS Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlı hizmet, veri fabrikası için bir veri deposu bağlar. Bağlı hizmet türü oluşturma **Hdfs** bir şirket içi HDFS veri fabrikanıza bağlamak için. Aşağıdaki tabloda, bağlantılı hizmetinin JSON öğeleri HDFS için belirli bir açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **Hdfs** |Evet |
| Url |HDFS URL'si |Evet |
| authenticationType |Anonim veya Windows. <br><br> Kullanılacak **Kerberos kimlik doğrulaması** HDFS bağlayıcı için başvurmak [Bu bölümde](#use-kerberos-authentication-for-hdfs-connector) şirket içi ortamınıza uygun şekilde ayarlamak için. |Evet |
| Kullanıcı adı |Kullanıcı adı için Windows kimlik doğrulaması. Kerberos kimlik doğrulaması için belirtmek `<username>@<domain>.com`. |Evet (Windows kimlik doğrulaması için) |
| password |Windows kimlik doğrulaması için parola. |Evet (Windows kimlik doğrulaması için) |
| gatewayName |Data Factory hizmetinin HDFS bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| encryptedCredential |[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential. |Hayır |

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Windows kimlik doğrulaması kullanma

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **FileShare** (HDFS dataset içerir) aşağıdaki özelliklere sahip

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasör yolu. Örnek:`myfolder`<br/><br/>Kaçış karakteri kullanmak ' \ ' dize özel karakter. Örneğin: folder\subfolder için klasör belirtin\\\\alt ve d:\samplefolder için d: belirtin\\\\ÖrnekKlasör.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>FileName bir çıkış veri kümesi için belirtilmediğinde oluşturulan dosya adı aşağıdaki olacaktır bu biçimi: <br/><br/>Veriler. <Guid>.txt (örnek:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy filename zaman serisi veri için dinamik bir folderPath belirtmek için kullanılabilir. Örnek: veri her saat için parametreli folderPath. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanılamaz.

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath ve zaman serisi verilerle dosya adını belirtebilirsiniz **partitionedBy** özelliği, [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md).

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla bilgi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [oluşturma ardışık düzen](data-factory-create-pipelines.md) makaleleri.

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {dilim} belirtilen veri fabrikası sistem değişkenin değerini SliceStart (YYYYMMDDHH) biçiminde ile değiştirilir. Dilimin başlangıç SliceStart başvuruyor. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Bu örnekte, folderPath ve fileName özellikleri tarafından kullanılan ayrı değişkenleri içine yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kopyalama kaynağı türü olduğunda etkinliği için **FileSystemSource** typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md) makale ayrıntıları.

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a>JSON örnek: veri kopyalama şirket içi HDFS Azure Blob
Bu örnek, bir şirket içi HDFS Azure Blob depolama alanına veri kopyalama gösterilmektedir. Ancak, veriler kopyalanabilir **doğrudan** belirtildiği havuzlarını hiçbirine [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.  

Örnek aşağıdaki Data Factory varlıkları için JSON tanımları sağlar. Bu tanımları verileri HDFS kullanarak Azure Blob depolama alanına kopyalamak için bir işlem hattı oluşturmak için kullanabileceğiniz [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. Bağlı hizmet türü [OnPremisesHdfs](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [FileShare](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri şirket içi HDFS her saat bir Azure blob kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidi kurun ayarlayın. ' Ndaki yönergeleri [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**HDFS bağlantılı hizmeti:** Bu örneği, Windows kimlik doğrulaması kullanır. Bkz: [HDFS bağlantılı hizmeti](#linked-service-properties) bölümü için farklı tür kimlik doğrulaması kullanabilirsiniz.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Storage bağlı hizmeti:**

```JSON
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

**HDFS giriş veri kümesi:** HDFS klasörüne DataTransfer/UnitTest bu veri kümesine başvuruyor /. Ardışık Düzen tüm dosyaları bu klasörde hedefe kopyalar.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Dosya sistemi kaynak ve Blob havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen bu girdi ve çıktı veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **FileSystemSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği veri kopyalamak için son bir saat içindeki seçer.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS Bağlayıcısı için Kerberos kimlik doğrulamasını kullan
HDFS Bağlayıcısı Kerberos kimlik doğrulamasını kullanmak amacıyla şirket içi ortamını ayarlamak için iki seçenek vardır. Bir durumunuz daha iyi uyduğunu seçebilirsiniz.
* Seçenek 1: [birleştirme ağ geçidi makinesiyle Kerberos alanı](#kerberos-join-realm)
* Seçenek 2: [Windows etki alanı Kerberos alanı arasında karşılıklı güven etkinleştir](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Seçenek 1: ağ geçidi makinesi Kerberos bölgesinde katılın.

#### <a name="requirement"></a>Gereksinim:

* Ağ geçidi makinesi Kerberos alanı katılmak gereken ve herhangi bir Windows etki alanına katılamıyor.

#### <a name="how-to-configure"></a>Nasıl yapılandırılır:

**Ağ geçidi makinede:**

1.  Çalıştırma **Ksetup** Kerberos KDC sunucu ve bölge yapılandırma yardımcı programı.

    Kerberos alanı bir Windows etki alanından farklı olduğundan makine bir çalışma grubunun bir üyesi yapılandırılmalıdır. Bu işlem, Kerberos alanı ayarlama ve şu şekilde bir KDC sunucusu eklemeyi de yararlanılabilir. Değiştir *REALM.COM* gerektiği gibi ilgili kendi bölge ile.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Yeniden** makineyi 2 Bu komutları yürütülürken sonra.

2.  Yapılandırmayla doğrulayın **Ksetup** komutu. Çıktısı gibi olmalıdır:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Data Factory'de:**

* HDFS Bağlayıcısı'nı kullanarak yapılandırma **Windows kimlik doğrulaması** Kerberos asıl adı ve HDFS veri kaynağına bağlanmak için parola ile birlikte. Denetleme [HDFS bağlantılı hizmet özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.

### <a name="kerberos-mutual-trust"></a>Seçenek 2: Windows etki alanı Kerberos alanı arasında karşılıklı güven etkinleştirme

#### <a name="requirement"></a>Gereksinim:
*   Ağ geçidi makinesi bir Windows etki alanına katılması gerekir.
*   Etki alanı denetleyicisinin ayarlarını güncelleştirmek için izniniz olmalıdır.

#### <a name="how-to-configure"></a>Nasıl yapılandırılır:

> [!NOTE]
> REALM.COM ve AD.COM aşağıdaki öğreticide kendi ilgili bölge ve gerektiği gibi etki alanı denetleyicisi ile değiştirin.

**KDC sunucusunda:**

1.  KDC yapılandırmasında Düzenle **krb5.conf** dosya KDC izin vermek için aşağıdaki yapılandırma şablonuna başvuran Windows etki alanı güven. Varsayılan olarak, yapılandırmanın bulunur **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Yeniden** yapılandırmadan sonra KDC Hizmeti.

2.  Adlı bir asıl hazırlama  **krbtgt/REALM.COM@AD.COM**  aşağıdaki komutla KDC Server'daki:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  İçinde **hadoop.security.auth_to_local** HDFS hizmet yapılandırma dosyası, ekleme `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Etki alanı denetleyicisinde:**

1.  Aşağıdaki komutu çalıştırarak **Ksetup** komutları bir bölge girdisi eklemek için:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Kerberos alanı için Windows etki alanından güven oluşturun. [parola] olan asıl parola  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  İçinde Kerberos kullanılan şifreleme algoritmasını seçin.

    1. Sunucu Yöneticisi gidin > Grup İlkesi Yönetimi > etki alanı > Grup İlkesi nesneleri > Varsayılan veya etkin etki alanı ilkesi ve düzenleyin.

    2. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** açılan pencerede, bilgisayar yapılandırması gidin > ilkeler > Windows Ayarları > Güvenlik Ayarları > yerel ilkeler > güvenlik seçenekleri ve yapılandırma **ağ güvenliği: yapılandırma şifreleme türleri için Kerberos izin**.

    3. Seçin, kullanmak istediğiniz şifreleme algoritması için KDC bağlayın. Genellikle, yalnızca tüm seçeneklerini belirleyebilirsiniz.

        ![Kerberos için yapılandırma şifreleme türleri](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Kullanım **Ksetup** üzerinde belirli bölge kullanılacak şifreleme algoritmasını belirtmek için komutu.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows etki alanında Kerberos asıl adı kullanmasını için etki alanı hesabını ve Kerberos asıl arasında eşleme oluşturun.

    1. Yönetimsel Araçları başlatmak > **Active Directory Kullanıcıları ve Bilgisayarları**.

    2. Tıklayarak Gelişmiş özellikleri yapılandırma **Görünüm** > **Gelişmiş Özellikler**.

    3. Hangi eşlemeleri oluşturmak istediğiniz ve görüntülemek için sağ hesabını bulun **adı eşlemeleri** > tıklatın **Kerberos adları** sekmesi.

    4. Sorumlu bölge ekleyin.

        ![Güvenlik kimliğine eşleyin](media/data-factory-hdfs-connector/map-security-identity.png)

**Ağ geçidi makinede:**

* Aşağıdaki komutu çalıştırarak **Ksetup** bir bölge girdisi eklemek için komutları.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Data Factory'de:**

* HDFS Bağlayıcısı'nı kullanarak yapılandırma **Windows kimlik doğrulaması** , etki alanı hesabı veya Kerberos asıl HDFS veri kaynağına bağlanmak için birlikte. Denetleme [HDFS bağlantılı hizmet özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
