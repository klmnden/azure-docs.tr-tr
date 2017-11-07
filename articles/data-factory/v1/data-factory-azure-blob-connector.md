---
title: Veri kopyalama/Azure Blob depolama biriminden | Microsoft Docs
description: "Azure Data Factory'de BLOB verileri kopyalamak öğrenin. Bizim örnek kullanın: ve Azure Blob Storage ve Azure SQL veritabanına veri kopyalamak nasıl."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3f4e0c68542cc38e1c0d90c6589e97134c3845f8
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a>Veri ya da Azure Data Factory kullanarak Azure Blob depolama biriminden kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-azure-blob-connector.md)
> * [Sürüm 2 - Önizleme](../connector-azure-blob-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Azure Blob Storage Bağlayıcısı](../connector-azure-blob-storage.md).


Bu makalede kopya etkinliği Azure Data Factory'de ve Azure Blob depolama biriminden veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

## <a name="overview"></a>Genel Bakış
Verileri Azure Blob Storage için tüm desteklenen kaynak veri deposundan veya Azure Blob Storage tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Aşağıdaki tabloda tarafından kopyalama etkinliği iç havuzlar veya kaynakları olarak desteklenen veri depoları listesini sağlar. Örneğin, veri taşıyabilirsiniz **gelen** bir SQL Server veritabanı veya bir Azure SQL veritabanı **için** Azure blob depolama. Ve veri kopyalayabilirsiniz **gelen** Azure blob depolama **için** Azure SQL Data Warehouse veya bir Azure Cosmos DB koleksiyonu. 

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **Azure Blob depolama biriminden** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **Azure Blob depolama alanına**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> Kopyalama etkinliği başlangıç/bitiş hem genel amaçlı Azure depolama hesapları hem de etkin/Cool Blob Depolama veri kopyalamayı destekler. Etkinlik destekler **bloğundan okuma, ekleme veya sayfa blobları**, ancak destekleyen **yalnızca blok yazma**. Sayfa blobları tarafından yedeklenen olduğundan azure Premium depolama havuzu olarak desteklenmiyor.
> 
> Verileri başarıyla hedefe kopyalandıktan sonra kopyalama etkinliği kaynaktan verileri silmez. Sonra başarılı bir kopyasını kaynak verilerini silmek ihtiyacınız varsa oluşturma bir [özel etkinlik](data-factory-use-custom-activities.md) verileri silmek ve ardışık düzeninde etkinlik kullanın. Bir örnek için bkz: [Delete blob veya klasör örneği github'daki](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity). 

## <a name="get-started"></a>başlarken
Farklı araçlar/API'lerini kullanarak verileri Azure Blob Storage/gruptan taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bu makalede sahip bir [izlenecek](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) verileri bir Azure Blob Depolama Birimi konumundan başka bir Azure Blob depolama konumuna kopyalamak için bir işlem hattı oluşturmak için. Verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalamak için bir işlem hattı oluşturma bir öğretici için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri Azure blob depolama alanından Azure SQL veritabanına kopyalıyorsanız Azure storage hesabını ve Azure SQL veritabanını veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Azure Blob depolama alanına özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü. 
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte blob kapsayıcısı ve giriş verilerini içeren klasörü belirtmek için bir veri kümesi oluşturun. Ve blob depolama biriminden kopyalanan verilerini tutan Azure SQL veritabanında SQL tablosu belirtmek için başka bir veri kümesi oluşturun. Azure Blob depolama alanına özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve SqlSink havuzu olarak kopya etkinliği için kullanırsınız. Azure Blob depolama alanına Azure SQL veritabanından kopyalıyorsanız benzer şekilde, SqlSource ve BlobSink kopyalama etkinliği kullanırsınız. Azure Blob depolama alanına belirli kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın.  

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  / Azure Blob depolama biriminden veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-blob-storage  ) bu makalenin.

Aşağıdaki bölümler, Azure Blob depolama alanına Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlı hizmetler Azure Storage bir Azure data factory'ye bağlamak için kullanabileceğiniz iki tür vardır. Bunlar: **AzureStorage** bağlantılı hizmeti ve **AzureStorageSas** bağlı hizmeti. Azure Storage bağlı hizmeti Azure Storage data factory ile genel erişim sağlar. Azure depolama SAS (paylaşılan erişim imzası) bağlı ise hizmeti Azure Storage veri fabrikası kısıtlanmış/zaman sınırlı erişimi olan sağlar. Bu iki bağlı hizmet arasındaki herhangi bir fark vardır. Gereksinimlerinize uygun bağlı hizmeti seçin. Aşağıdaki bölümler bu iki bağlı hizmet üzerinde daha fazla ayrıntı sağlar.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Giriş veya çıkış verileri Azure Blob Depolama göstermek için bir veri kümesi belirtmek için veri kümesine tür özelliği ayarlayın: **AzureBlob**. Ayarlama **linkedServiceName** bağlı hizmeti Azure Storage veya Azure depolama SAS adını kümesinin özellik.  Veri kümesi türü özelliklerini belirtin **blob kapsayıcısı** ve **klasörü** blob depolamada.

JSON bölümleri & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

Veri Fabrikası yapısındaki"" şema üzerinde okuma veri kaynakları için Azure blob gibi tür bilgilerini sağlamaktan aşağıdaki temel CLS uyumlu .NET türü değerleri destekler: Int16, Int32, Int64, tek, Double, Decimal, Byte [], Bool, dize, GUID, Datetime, Datetimeoffset, Timespan. Veri Fabrikası tür dönüşümleri veri bir havuz veri deposu için bir kaynak veri deposundan taşırken otomatik olarak gerçekleştirir.

**TypeProperties** bölüm bilgileri sağlar ve her veri kümesi türü için farklı konumu hakkında vb., verilerin veri deposundaki biçimlendirin. TypeProperties bölüm türü veri kümesi için **AzureBlob** veri kümesine aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Kapsayıcı ve klasöre blob depolamada yolu. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Blob adı. isteğe bağlıdır ve büyük küçük harfe duyarlı dosya adıdır.<br/><br/>Bir dosya adı belirtirseniz, (kopyalama dahil) etkinlik belirli Blob üzerinde çalışır.<br/><br/>Dosya adı belirtilmediğinde kopyalama tüm BLOB'lar girdi veri kümesi için folderPath içerir.<br/><br/>Zaman **fileName** bir çıkış veri kümesi için belirtilmemiş ve **preserveHierarchy** belirtilmedi etkinlik havuzunda oluşturulmuş dosya adı aşağıdaki olacaktır Bu biçim: veri.<Guid>. txt (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath için verileri saatte parametreli olabilir. Bkz: [partitionedBy özellik bölümünü kullanarak](#using-partitionedBy-property) ayrıntı ve örnekler için. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath ve zaman serisi verilerle dosya adını belirtebilirsiniz **partitionedBy** özelliği, [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md).

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla bilgi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) ve [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makaleleri.

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {dilim} belirtilen veri fabrikası sistem değişkenin değerini SliceStart (YYYYMMDDHH) biçiminde ile değiştirilir. Dilimin başlangıç SliceStart başvuruyor. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104

#### <a name="sample-2"></a>Örnek 2

```json
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

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış veri kümeleri ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir. Bulunan özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir. Bir Azure Blob depolama alanından veri taşıyorsanız, kaynak türü için kopyalama etkinliğinde ayarladığınız **BlobSource**. Bir Azure Blob depolama alanına veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğinde ayarladığınız **BlobSink**. Bu bölümde BlobSource ve BlobSink tarafından desteklenen özellikler listesini sağlar.

**BlobSource** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |(Varsayılan değer) false değerini true |Hayır |

**BlobSink** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |<b>PreserveHierarchy</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><br/><b>FlattenHierarchy</b>: tüm kaynak klasörü hedef klasör ilk düzeyi dosyalarıdır. Hedef dosyalar otomatik adına sahip. <br/><br/><b>MergeFiles</b>: bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilirse, belirtilen ad olur; Aksi takdirde otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

**BlobSource** geriye dönük uyumluluk için bu iki özellik de destekler.

* **treatEmptyAsNull**: null veya boş dize null değeri olarak kabul edilip edilmeyeceğini belirtir.
* **skipHeaderLineCount** -kaç satır atlandı belirtir. Yalnızca girdi veri kümesi TextFormat kullandığında geçerli olduğu.

Benzer şekilde, **BlobSink** geriye dönük uyumluluk için aşağıdaki özelliği destekler.

* **blobWriterAddHeader**: bir çıkış veri kümesi yazılırken sütun tanımları üstbilgisinin eklenip eklenmeyeceğini belirler.

Veri kümeleri artık aynı işlevselliği uygulamak aşağıdaki özellikleri destekler: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

Aşağıdaki tabloda, bu blob kaynak/havuz özellikleri yerine yeni veri kümesi özellikleri kullanma hakkında yönergeler sunmaktadır.

| Kopya etkinliği özelliği | Veri kümesi özelliği |
|:--- |:--- |
| BlobSource üzerinde skipHeaderLineCount |skipLineCount ve firstRowAsHeader. Satırlar ilk atlanır ve ilk satırın üstbilgi olarak sonra okuyun. |
| BlobSource üzerinde treatEmptyAsNull |girdi veri kümesi üzerinde treatEmptyAsNull |
| BlobSink üzerinde blobWriterAddHeader |Çıktı veri kümesi üzerinde firstRowAsHeader |

Bkz: [belirtme TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) bu özellikleri hakkında ayrıntılı bilgi için bölüm.    

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior değerleri farklı birleşimlerini kopyalama işlemi açıklanmaktadır.

| Özyinelemeli | copyBehavior | Bunun sonucunda oluşan davranışı |
| --- | --- | --- |
| TRUE |preserveHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 kaynak aynı yapısını oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| TRUE |flattenHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan adı |
| TRUE |mergeFiles |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| False |preserveHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| False |flattenHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/><br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| False |mergeFiles |Bir kaynak klasörü için Klasör1 şu yapıda:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. dosya1 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a>İzlenecek yol: Kullanım Kopyalama Sihirbazı ' / Blob depolama biriminden verileri kopyalamak için
Hızlı bir şekilde grafikten bir Azure blob depolama veri kopyalamak nasıl bakalım. Bu kılavuzda, hem kaynak hem de hedef veri türünü depolar: Azure Blob Storage. Bu kılavuzda ardışık düzen veri bir klasörden aynı blob kapsayıcısında başka bir klasöre kopyalar. Bu kılavuz, Blob Depolama kaynağı veya havuz olarak kullanırken ayarları veya özellikleri göstermek kasıtlı olarak basit bir işlemdir. 

### <a name="prerequisites"></a>Ön koşullar
1. Bir genel amaçlı oluşturma **Azure depolama hesabı** zaten yoksa. Her iki biçimde blob storage kullanma **kaynak** ve **hedef** verileri depolamak bu kılavuzda. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
2. Adlı bir blob kapsayıcı oluşturun **adfblobconnector** depolama hesabındaki. 
4. Adlı bir klasör oluşturun **giriş** içinde **adfblobconnector** kapsayıcı.
5. Adlı bir dosya oluşturun **emp.txt** ile aşağıdaki içerik ve kendisine karşıya **giriş** gibi araçları kullanarak klasör [Azure Storage Gezgini](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-the-data-factory"></a>Veri Fabrikası oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **+ yeni** sol üst köşeden tıklatın **Intelligence + analiz**, tıklatıp **Data Factory**.
3. **Yeni data factory** dikey penceresinde:   
    1. Girin **ADFBlobConnectorDF** için **adı**. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: `*Data factory name “ADFBlobConnectorDF” is not available`, veri fabrikası (örneğin, yournameADFBlobConnectorDF) adını değiştirin ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    2. Azure **aboneliğinizi** seçin.
    3. Kaynak grubu için seçin **kullanım varolan** varolan bir kaynak grubu seçin (veya) seçmek için **Yeni Oluştur** bir kaynak grubu için bir ad girmelisiniz.
    4. Veri fabrikası için bir **konum** seçin.
    5. Dikey pencerenin alt kısmındaki **Panoya sabitle** onay kutusunu seçin.
    6. **Oluştur**'a tıklayın.
3. Oluşturma işlemi tamamlandıktan sonra gördüğünüz **Data Factory** dikey penceresini aşağıdaki görüntüde gösterildiği gibi: ![Data factory giriş sayfası](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Kopyalama Sihirbazı
1. Data Factory giriş sayfasında, tıklatın **kopyalama [Önizleme] verileri** başlatmak için döşeme **kopyalama veri Sihirbazı** ayrı bir sekmede.    
    
    > [!NOTE]
    >    Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz, devre dışı bırakın/işaretini **üçüncü taraf tanımlama bilgilerini engelleyebilir ve site verileri** ayarlama (veya) etkin ve oluşturmak için bir özel durum **login.microsoftonline.com** ve ardından Sihirbazı yeniden başlatmayı deneyin.
2. **Özellikler** sayfasında:
    1. Girin **CopyPipeline** için **görev adı**. Görev adı, veri fabrikasında ardışık düzeni adıdır.
    2. Girin bir **açıklama** görevin (isteğe bağlı).
    3. İçin **görev tempoyla veya görev zamanlama**, tutmak **düzenli olarak zamanında** seçeneği. Bu görevi yalnızca bir kez art arda bir zamanlamaya göre çalıştır yerine çalıştırmak isteyip istemediğinizi seçin **kez Şimdi Çalıştır**. Öğesini seçerseniz, **kez Şimdi Çalıştır** seçeneği, bir [tek seferlik ardışık düzen](data-factory-create-pipelines.md#onetime-pipeline) oluşturulur. 
    4. Ayarlarını koruyabilirsiniz **yinelenen desen**. Bu görev, sonraki adımda belirttiğiniz her gün başlangıç ve bitiş zamanları arasında çalışır.
    5. Değişiklik **başlangıç tarihi ve saati** için **21/04/2017**. 
    6. Değişiklik **bitiş tarihi ve saati** için **25/04/2017**. Takvim gözatma yerine tarihi yazın isteyebilirsiniz.     
    8. **İleri**’ye tıklayın.
      ![Kopyalama aracı - Özellikler sayfası](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png) 
3. **Kaynak veri deposu** sayfasında **Azure Blob Storage** kutucuğuna tıklayın. Kopyalama görevine yönelik kaynak veri deposunu belirtmek için bu sayfayı kullanın. Yeni bir veri deposu belirtmek için mevcut bir veri deposu bağlı hizmetini kullanabilirsiniz (veya) yeni bir veri deposu belirtebilirsiniz. Mevcut bir bağlı hizmeti kullanmak için seçeceğiniz **mevcut bağlı hizmetlerden** ve doğru bağlı hizmeti seçin. 
    ![Kopyalama aracı - kaynak veri deposu sayfası](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında:
   1. İçin otomatik olarak oluşturulan adı bırakın **bağlantı adı**. Bağlantı adı türündeki bağlı hizmetin adıdır: Azure depolama. 
   2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
   3. Azure aboneliğinizi seçin ya da koruma **Tümünü Seç** için **Azure aboneliği**.   
   4. Seçili abonelikte bulunan Azure depolama hesapları listesinden bir **Azure depolama hesabı** seçin. Ayrıca depolama hesabı ayarlarını el ile girmeyi seçebilirsiniz **el ile girmeniz** seçenek için **hesap seçme yöntemi**.
   5. **İleri**’ye tıklayın. 
      ![Kopyalama aracı - Azure Blob Depolama hesabı belirtin](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. **Girdi dosyası veya klasörü seçin** sayfasında:
   1. Çift **adfblobcontainer**.
   2. Seçin **giriş**, tıklatıp **Seç**. Bu kılavuzda, giriş klasörü seçin. Ayrıca emp.txt dosya klasöründe yerine seçebilirsiniz. 
      ![Kopyalama aracı - girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. Üzerinde **girdi dosyası veya klasörü seçin** sayfa:
    1. Onaylayın **dosya veya klasör** ayarlanır **adfblobconnector/giriş**. Dosyaları alt klasörlerde, örneğin, 04/2017/01, 04/2017/02 ve vb. adfblobconnector/giriş girin / {year} / {month} / {day} dosya veya klasör için. Metin kutusu dışında SEKME tuşuna bastığınızda biçimlerini (yyyy) yıl, ay (MM) ve gün (dd) seçmek için üç açılan listeleri bakın. 
    2. Ayarlamayın **kopyalayın dosyayı yinelemeli olarak**. Hedefe kopyalanacak dosyaları yinelemeli olarak çapraz klasörlerde için bu seçeneği belirleyin. 
    3. Sağlamadığı **ikili kopyalama** seçeneği. Hedef kaynak dosyaya ikili bir kopyasını gerçekleştirmek için bu seçeneği belirleyin. Daha fazla seçenek sonraki sayfalarda görebilmeniz için bu kılavuzda seçmeyin. 
    4. Onaylayın **sıkıştırma türünü** ayarlanır **hiçbiri**. Kaynak dosyalar desteklenen biçimlerden birinde sıkıştırılmış bu seçenek için bir değer seçin. 
    5. **İleri**’ye tıklayın.
    ![Kopyalama aracı - girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/chose-input-file-folder.png) 
7. **Dosya biçimi ayarları** sayfasında sınırlayıcıları ve sihirbaz tarafından dosya ayrıştırılarak otomatik olarak algılanan düzeni görürsünüz. 
    1. Aşağıdaki seçenekler onaylayın: bir. **Dosya biçimi** ayarlanır **metin biçimi**. Aşağı açılan listesinde tüm desteklenen biçimler görebilirsiniz. Örneğin: JSON, Avro, ORC, Parquet.
        b. **Sütun sınırlayıcı** ayarlanır `Comma (,)`. Aşağı açılan listesinde Data Factory ile desteklenen diğer sütun sınırlayıcıları görebilirsiniz. Özel bir sınırlayıcı de belirtebilirsiniz.
        c. **Satır ayırıcı** ayarlanır `Carriage Return + Line feed (\r\n)`. Aşağı açılan listesinde Data Factory ile desteklenen diğer satır sınırlayıcıları görebilirsiniz. Özel bir sınırlayıcı de belirtebilirsiniz.
        d. **Satır sayısı atla** ayarlanır **0**. Dosyanın üst kısmında atlanacak birkaç satır numarasını buraya girin.
        e.  **İlk veri satırı sütun adları içeren** ayarlı değil. İlk satırı sütun adları kaynak dosyaları içeriyorsa, bu seçeneği belirleyin.
        f. **Boş sütun değeri null olarak davran** seçeneği ayarlanmış.
    2. Genişletme **Gelişmiş ayarları** Gelişmiş seçeneği kullanılabilir görmek için.
    3. Sayfanın alt kısmındaki bkz **Önizleme** emp.txt dosya verileri.
    4. Tıklatın **şema** kaynak dosyasında veri bakarak Kopyalama Sihirbazı'nı çıkarımı yapılan şema görmek için alt sekmesi.
    5. Sınırlayıcıları gözden geçirin verilerin önizlemesini gördükten sonra **İleri**’ye tıklayın.
    ![Kopyalama aracı - dosya biçimi ayarları](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)  
8. Üzerinde **hedef veri deposu sayfası**seçin **Azure Blob Storage**, tıklatıp **sonraki**. Kaynak ve hedef veri depolarına bu kılavuzda olarak Azure Blob Storage kullanıyor.    
    ![Kopyalama aracı - select hedef veri deposu](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. Üzerinde **Azure Blob Depolama hesabı belirtin** sayfa:
   1. Girin **AzureStorageLinkedService** için **bağlantı adı** alan.
   2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
   3. Azure **aboneliğinizi** seçin.  
   4. Azure depolama hesabınızı seçin. 
   5. **İleri**’ye tıklayın.     
10. Üzerinde **çıktı dosyası veya klasörü seçin** sayfa: 
    6. belirtin **klasör yolu** olarak **adfblobconnector/çıkış / {year} / {month} / {day}**. Girin **sekmesini**.
    7. İçin **yıl**seçin **yyyy**.
    8. İçin **ay**, kümesine olduğunu onaylayın **MM**.
    9. İçin **gün**, kümesine olduğunu onaylayın **GG**.
    10. Onaylayın **sıkıştırma türünü** ayarlanır **hiçbiri**.
    11. Onaylayın **kopyalama davranışı** ayarlanır **dosyaları Birleştir**. Aynı ada sahip çıktı dosyası zaten varsa, yeni içerik aynı dosyanın sonuna eklenir.
    12. **İleri**’ye tıklayın.
    ![Kopyalama aracı - çıktı dosyası veya klasörü seçin](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. Üzerinde **dosya biçimi ayarları** sayfasında, ayarları gözden geçirin ve tıklayın **sonraki**. Ek seçenekler burada ve çıktı dosyası için bir başlık eklemek için biridir. Bu seçeneği belirlerseniz, bir başlık satırı sütunlarının adlarını kaynak şemadan eklenir. Varsayılan sütun adları kaynağı için şema görüntülerken yeniden adlandırabilirsiniz. Örneğin, ad ve Soyadı ikinci sütuna ilk sütun değiştirebilir. Ardından, çıktı dosyası bu adları içeren bir başlık sütun adları olarak üretilir. 
    ![Kopyalama aracı - hedef için dosya biçimi ayarları](media/data-factory-azure-blob-connector/file-format-destination.png)
12. Üzerinde **performans ayarlarını** sayfasında, onaylayın **bulut birimleri** ve **paralel kopyaları** ayarlanır **otomatik**, İleri'yi tıklatın. Bu ayarlar hakkında daha fazla ayrıntı için bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md#parallel-copy).
    ![Kopyalama aracı - performans ayarları](media/data-factory-azure-blob-connector/copy-performance-settings.png) 
14. Üzerinde **Özet** sayfasında (görev özellikleri, kaynak ve hedef ayarları ve kopya ayarlarını) tüm ayarları gözden geçirin ve tıklayın **sonraki**.
    ![Kopyalama aracı - Özet sayfası](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. **Özet** sayfasındaki bilgileri gözden geçirin ve **Son**’a tıklayın. Sihirbaz, veri fabrikasında (Kopyalama Sihirbazı’nı başlattığınız yer) iki bağlı hizmet, iki veri kümesi (girdi ve çıktı) ve bir işlem hattı oluşturur.
    ![Kopyalama aracı - dağıtım sayfası](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-the-pipeline-copy-task"></a>(Kopyalama görevi) işlem hattını izleme

1. Bağlantıya tıklayın `Click here to monitor copy pipeline` üzerinde **dağıtım** sayfası. 
2. Görmeniz gerekir **izleme ve yönetme uygulaması** ayrı bir sekmede.  ![İzleme ve uygulama yönetme](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Değişiklik **Başlat** en üstte zaman `04/19/2017` ve **son** için zaman `04/27/2017`ve ardından **Uygula**. 
4. Beş etkinlik Windows'da görmelisiniz **etkinlik WINDOWS** listesi. **WindowStart** kez ardışık düzen baştan bitiş zamanları kanal tüm günler kapak. 
5. Tıklatın **yenileme** için düğmesini **etkinlik WINDOWS** birkaç kez tüm etkinlik windows durumunu görene kadar listesini hazır ayarlayın. 
6. Şimdi, çıktı dosyaları adfblobconnector kapsayıcı çıkış klasöründe oluşturulan doğrulayın. Çıkış klasöründe aşağıdaki klasör yapısını görmeniz gerekir: 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
İzleme ve veri fabrikaları yönetme hakkında ayrıntılı bilgi için bkz: [İzleyici ve Data Factory işlem hattı yönetmek](data-factory-monitor-manage-app.md) makalesi. 
 
### <a name="data-factory-entities"></a>Data Factory varlıklarını
Data Factory giriş sayfası sekmesi için şimdi dönün. İki bağlı hizmet, iki veri kümesi ve bir ardışık düzeni, veri fabrikası'nda artık dikkat edin. 

![Varlıklarla Data Factory giriş sayfası](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Tıklatın **yazar ve dağıtma** Data Factory Düzenleyici başlatmak için. 

![Data Factory Düzenleyicisi](media/data-factory-azure-blob-connector/data-factory-editor.png)

Veri fabrikanızın aşağıdaki Data Factory varlıklarını görmeniz gerekir: 

 - İki bağlı hizmet. Bir kaynak ve hedef için başka bir. Bu kılavuzda aynı Azure depolama hesabı her iki bağlı hizmet bakın. 
 - İki veri kümesi. Bir giriş veri kümesi ve bir çıkış veri kümesi. Bu kılavuzda, her ikisi de aynı blob kapsayıcısı kullanır ancak farklı klasörlere (girdi ve çıktı) bakın.
 - Ardışık Düzen. Ardışık düzen, verileri bir Azure blob konumundan başka bir Azure blob konumuna kopyalamak için bir blob kaynağı ve blob havuz kullanan kopyalama etkinliği içerir. 

Aşağıdaki bölümler bu varlıkları hakkında daha fazla bilgi sağlar. 

#### <a name="linked-services"></a>Bağlı hizmetler
İki bağlı hizmet görmeniz gerekir. Bir kaynak ve hedef için başka bir. Bu kılavuzda, her iki tanımları aynı adları dışında arayın. **Türü** bağlı hizmetin adı ayarlamak **AzureStorage**. Bağlantılı hizmet tanımını en önemli özelliği **connectionString**, hangi veri fabrikası tarafından çalışma zamanında Azure Storage hesabınıza bağlanmak için kullanılır. HubName özelliği tanımı'ndaki yoksay. 

##### <a name="source-blob-storage-linked-service"></a>Kaynak blob storage bağlı hizmeti
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Hedef blob depolama bağlı hizmeti

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Azure Storage bağlı hizmeti hakkında daha fazla bilgi için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü. 

#### <a name="datasets"></a>Veri kümeleri
İki veri kümesi vardır: bir giriş veri kümesi ve bir çıkış veri kümesi. Veri kümesi türü kümesine **AzureBlob** her ikisi için de. 

Girdi veri kümesi işaret **giriş** klasöründe **adfblobconnector** blob kapsayıcısı. **Dış** özelliği ayarlanmış **true** bu veri kümesi için verileri bu veri kümesine bir girdi olarak alır kopyalama etkinliği ile ardışık düzen tarafından üretilen değil olarak. 

Çıktı veri kümesi işaret **çıkış** aynı blob kapsayıcısının klasör. Çıktı veri kümesi yıl, ay ve günün kullanan **SliceStart** çıktı dosyası yolu dinamik olarak değerlendirilecek sistem değişkeni. İşlevler ve Data Factory ile desteklenen sistem değişkenleri listesi için bkz: [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md). **Dış** özelliği ayarlanmış **false** (varsayılan değer) bu veri kümesi ardışık düzen tarafından üretilen olduğundan. 

Azure Blob veri kümesi tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.

##### <a name="input-dataset"></a>Girdi veri kümesi

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Çıktı veri kümesi

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>İşlem hattı
Ardışık Düzen yalnızca bir etkinlik vardır. **Türü** etkinliğini kümesine **kopya**.  Etkinlik türü özelliklerini iki bölümü, kaynak için bir ve havuz için başka bir vardır. Kaynak türü kümesine **BlobSource** etkinlik bir blob depolama alanından veri kopyalama gibi. Havuz türü kümesine **BlobSink** bir blob depolama alanına veri kopyalama etkinlik olarak. Kopya etkinliği InputDataset z4y giriş olarak alır ve çıktı olarak OutputDataset z4y. 

BlobSource ve BlobSink tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a>Depolamaya ve Blob depolamadan veri kopyalamak için JSON örnekleri  
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve Azure Blob Storage ve Azure SQL veritabanına veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a>JSON örnek: verileri Blob depolama alanından SQL veritabanına kopyalamak
Aşağıdaki örnek gösterilmektedir:

1. Bağlı hizmet türü [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](#copy-activity-properties) ve [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

Zaman serisi bir Azure SQL verileri Azure blob örnek kopyaları saatlik tablo. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure SQL bağlı hizmeti:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Storage bağlı hizmeti:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Azure Data Factory iki tür Azure Storage bağlı hizmeti destekler: **AzureStorage** ve **AzureStorageSas**. İlk biri için hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI'sini belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.  

**Azure Blob girdi veri kümesi:**

Veri toplanma yeni blob üzerinden saatte (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı bildirir Data Factory tablosu data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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
**Azure SQL çıktı veri kümesi:**

Kopya veri örneklemek için bir tablo bir Azure SQL veritabanında "MyTable" adlı. Blob CSV dosyasında içerecek şekilde beklediğiniz gibi Azure SQL veritabanınızda aynı sayıda sütuna sahip tablo oluşturun. Yeni satırlar tabloya saatte eklenir.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Blob kaynağı ve SQL havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a>JSON örnek: Verilerini Azure SQL Azure Blob
Aşağıdaki örnek gösterilmektedir:

1. Bağlı hizmet türü [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) ve [BlobSink](#copy-activity-properties).

Örnek zaman serisi veri bir Azure SQL tablosundan bir Azure blob saatlik kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure SQL bağlı hizmeti:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Storage bağlı hizmeti:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Azure Data Factory iki tür Azure Storage bağlı hizmeti destekler: **AzureStorage** ve **AzureStorageSas**. İlk biri için hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI'sini belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.  

**Azure SQL girdi veri kümesi:**

Örnek, Azure SQL tablosu "MyTable" oluşturulur ve zaman serisi veri için "timestampcolumn" adlı bir sütun içerdiği varsayar.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**SQL kaynak ve Blob havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **SqlSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
