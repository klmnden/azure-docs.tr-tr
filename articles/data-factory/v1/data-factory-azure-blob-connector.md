---
title: Azure Blob Depolama içine/dışına veri kopyalama | Microsoft Docs
description: "Azure Data Factory'de BLOB veri kopyalama hakkında bilgi edinin. Örneğimizi kullanın: Azure Blob Depolama ve Azure SQL veritabanı ve veri kopyalamak nasıl."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 85832abeb9908dd891e3f35a0368bc35c7816a6e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66167798"
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a>İçin veya Azure Blob Depolama, Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-azure-blob-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-blob-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de Azure Blob Depolama Bağlayıcısı](../connector-azure-blob-storage.md).


Bu makalede, kopyalama etkinliği Azure Data Factory ve Azure Blob depolamadan veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

## <a name="overview"></a>Genel Bakış
Tüm desteklenen kaynak veri deposundan Azure Blob depolama alanına veya herhangi bir desteklenen havuz veri deposu Azure Blob Depolama'ya veri kopyalayabilirsiniz. Aşağıdaki tabloda kaynakları olarak desteklenen veri depolarının bir listesini sağlar veya kopyalama etkinliği tarafından başlatır. Örneğin, veri taşıyabileceğinizi **gelen** bir SQL Server veritabanı veya bir Azure SQL veritabanı **için** Azure blob depolama. Ve veri kopyalayabilirsiniz **gelen** Azure blob depolama **için** bir Azure SQL veri ambarı veya bir Azure Cosmos DB koleksiyonu.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalayabilirsiniz **Azure Blob Depolama'dan** aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz **Azure Blob Depolama'ya**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!IMPORTANT]
> Kopyalama etkinliği, / için hem genel amaçlı Azure depolama hesapları hem de sık erişimli/seyrek erişimli Blob Depolama veri kopyalamayı destekler. Etkinlik destekler **bloğundan okuma ekleyin veya sayfa blobları**, ancak destekler **yalnızca blok blobları yazma**. Sayfa blobları tarafından yedeklenir çünkü azure Premium depolama, havuz olarak desteklenmiyor.
>
> Veri hedefine başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak sunucudan verileri silmez. Kopyalama başarılı sonra kaynak verilerini silmek gerekiyorsa, oluşturun bir [özel etkinlik](data-factory-use-custom-activities.md) verileri silmek ve işlem hattı etkinliğini kullanın. Bir örnek için bkz. [silme blob veya klasör örneği github'daki](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).

## <a name="get-started"></a>başlarken
Farklı araçlar/API'lerini kullanarak bir Azure Blob Depolama içine/dışına veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bu makalede sahip bir [izlenecek](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) verileri bir Azure Blob Depolama Birimi konumundan başka bir Azure Blob depolama konumuna kopyalamak için bir işlem hattı oluşturma. Verileri bir Azure Blob depolama alanından Azure SQL veritabanı'na kopyalamak için bir işlem hattı oluşturmaya ilişkin öğretici için bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir.
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, bir Azure SQL veritabanı için bir Azure blob depolamadan veri kopyalıyorsanız, Azure depolama hesabınızı ve Azure SQL veritabanına veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. Azure Blob depolama alanına özel bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Son adımda bahsedilen örnekte, bir veri kümesi blob kapsayıcıyı ve girdi verilerini içeren klasörü belirtin oluşturun. Ayrıca, blob depolama alanından kopyalanan verileri tutan Azure SQL veritabanındaki SQL tablosunu belirtirsiniz. başka bir veri kümesi oluşturursunuz. Azure Blob depolama alanına özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve SqlSink havuz olarak kopyalama etkinliği için kullanırsınız. Azure SQL veritabanı'ndan Azure Blob depolama alanına kopyalanıyorsa, benzer şekilde, SqlSource ve BlobSink kopyalama etkinliği kullanırsınız. Azure Blob depolama alanına özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir kaynak veya havuz bir veri deposunu kullanma hakkında daha fazla ayrıntı için önceki bölümde veri deponuz için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  Bir Azure Blob Depolama içine/dışına veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples-for-copying-data-to-and-from-blob-storage  ) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure Blob depolama alanına tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmet bir Azure depolama için bir Azure data factory'ye bağlamak için kullanabileceğiniz iki tür vardır. Bunlar: **AzureStorage** bağlı hizmet ve **AzureStorageSas** bağlı hizmeti. Azure depolama bağlı hizmeti Azure depolama data factory ile küresel erişim sağlar. Azure depolama SAS (paylaşılan erişim imzası) bağlı ise hizmet kısıtlı/süresi sınırlı erişimi olan data factory Azure depolama sağlar. Bu iki bağlı hizmet arasında başka hiçbir fark yoktur. Gereksinimlerinize uyan bağlı hizmeti seçin. Aşağıdaki bölümlerde, bu iki bağlı hizmet üzerinde daha fazla ayrıntı sağlar.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Azure Blob depolama alanındaki giriş veya çıkış verilerini temsil eden bir veri kümesi belirtmek için veri kümesine öğesinin type özelliği ayarlayın: **AzureBlob**. Ayarlama **linkedServiceName** Özelliği Azure depolama veya Azure depolama SAS adını veri kümesine bağlı hizmeti.  Veri kümesi türü özelliklerini belirtin **blob kapsayıcısı** ve **klasör** blob depolamada.

JSON bölümleri & veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

Data factory, Azure blob gibi şema okuma veri kaynakları için "yapı" tür bilgileri sağlamak için aşağıdaki temel CLS uyumlu .NET türü değerleri destekler: Int16, Int32, Int64, tek, Double, Decimal, Byte [], Bool, String, GUID, Datetime, Datetimeoffset, TimeSpan değeri. Veri fabrikası, verileri bir kaynak veri deposundan bir havuz veri deposuna taşırken tür dönüştürmeleri otomatik olarak gerçekleştirir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve bilgiler sağlar konumu hakkında vb., verilerin veri deposundaki biçimlendirin. TypeProperties bölümü için veri kümesi türü **AzureBlob** veri kümesi, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Kapsayıcı ve blob depolama alanında bir klasör yolu. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Blob adı. İsteğe bağlı ve büyük küçük harfe duyarlı dosya adıdır.<br/><br/>Etkinlik (kopyalama dahil), bir filename belirtirseniz, belirli bir blobu üzerinde çalışır.<br/><br/>Dosya adı belirtilmemişse, kopya tüm BLOB'ları folderPath için giriş veri kümesi içerir.<br/><br/>Zaman **fileName** bir çıktı veri kümesi için belirtilmemiş ve **preserveHierarchy** belirtilmezse etkinlik havuzunda oluşturulan dosya adı aşağıdaki olacak bu biçim: `Data.<Guid>.txt` (için Örnek:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| partitionedBy |partitionedBy isteğe bağlı bir özelliktir. Bir dinamik folderPath ve zaman serisi verileri için dosya adı belirtmek için kullanabilirsiniz. Örneğin, saatte veri folderPath parametreli olabilir. Bkz: [partitionedBy özellik bölümünü kullanarak](#using-partitionedby-property) Ayrıntılar ve örnekler. |Hayır |
| format | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath ve zaman serisi verileri ile dosya adını belirtebilirsiniz **partitionedBy** özelliği [Data Factory işlevleri ve sistem değişkenlerini](data-factory-functions-variables.md).

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla bilgi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md) ve [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makaleler.

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {dilim} belirtilen değeri (YYYYMMDDHH) biçiminde Data Factory sistem değişkeni SliceStart ile değiştirilir. Slicestart'taki saat diliminin başlangıç ifade eder. FolderPath için her bir dilimi farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104

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

Bu örnekte, folderPath ve dosya adı özellikleri tarafından kullanılan ayrı değişkenleri içine yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı veri kümeleri ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir. Diğer yandan bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir. Bir Azure Blob Depolama'dan veri taşıma, kaynak türü için kopyalama etkinliğindeki verilirse **BlobSource**. Bir Azure Blob depolama alanına veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğindeki ayarladığınız **BlobSink**. Bu bölümde BlobSource ve BlobSink tarafından desteklenen özelliklerin bir listesini sağlar.

**BlobSource** şu özelliklerde destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |(Varsayılan değer) true, False |Hayır |

**BlobSink** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |<b>PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><br/><b>FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik adına sahip. <br/><br/><b>MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Birleştirilmiş Dosya adı, dosya/Blob adı belirtilmezse, belirtilen adı olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

**BlobSource** da geriye dönük uyumluluk için bu iki özelliği destekler.

* **treatEmptyAsNull**: Null veya boş dize null değer olarak değerlendirilmesi belirtir.
* **skipHeaderLineCount** -kaç satır atlanması belirtir. Yalnızca giriş veri kümesi TextFormat kullandığında, geçerlidir.

Benzer şekilde, **BlobSink** geriye dönük uyumluluk için aşağıdaki özelliği destekler.

* **blobWriterAddHeader**: Sütun tanımları üstbilgisinin bir çıktı veri kümesi için yazılırken eklenip eklenmeyeceğini belirtir.

Veri kümeleri artık aynı işlevselliği uygulamak aşağıdaki özellikleri destekler: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

Aşağıdaki tabloda, bu blob kaynak/havuz özellikleri yerine yeni veri kümesi özellikleri kullanma hakkında yönergeler sağlar.

| Kopyalama etkinliği özelliği | Veri kümesi özelliği |
|:--- |:--- |
| BlobSource üzerinde skipHeaderLineCount |skipLineCount hem de firstrowasheader parametresi. Önce satırlar atlanır ve ardından ilk satırı üst bilgi olarak okunur. |
| BlobSource üzerinde treatEmptyAsNull |Giriş veri kümesi üzerinde treatEmptyAsNull |
| blobWriterAddHeader BlobSink üzerinde |Çıktı veri kümesi üzerinde firstRowAsHeader |

Bkz: [TextFormat belirtme](data-factory-supported-file-and-compression-formats.md#text-format) bölümü bu özellikler hakkında ayrıntılı bilgi için.

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, elde edilen davranışını özyinelemeli ve copyBehavior değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| recursive | copyBehavior | Sonuç davranış |
| --- | --- | --- |
| true |preserveHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 kaynak aynı yapıda ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| false |preserveHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |flattenHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |mergeFiles |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. Fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a>Çözüm: Blob Depolama içine/dışına veri kopyalamak için kopyalama Sihirbazı'nı kullanın
Hızla bir Azure blob depolama içine/dışına veri kopyalamak nasıl bakalım. Bu kılavuzda, kaynak ve hedef veri türünü depolar: Azure Blob Depolama. İşlem hattı Bu izlenecek yolda, verileri bir klasörden aynı blob kapsayıcısında başka bir klasöre kopyalar. Bu izlenecek yol, bir kaynak veya havuz olarak Blob Depolama kullanırken, ayarları veya özellikleri göstermek kasıtlı olarak basit bir işlemdir.

### <a name="prerequisites"></a>Önkoşullar
1. Genel amaçlı bir oluşturma **Azure depolama hesabı** zaten yoksa. Her ikisi de olarak blob depolama kullanma **kaynak** ve **hedef** veri depolayın, bu izlenecek yolda. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın.
2. Adlı bir blob kapsayıcısı oluşturursunuz **adfblobconnector** depolama hesabındaki.
4. Adlı bir klasör oluşturun **giriş** içinde **adfblobconnector** kapsayıcı.
5. Adlı bir dosya oluşturun **emp.txt** ile aşağıdaki içerik ve bunu **giriş** gibi araçları kullanarak klasör [Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```

### <a name="create-the-data-factory"></a>Veri Fabrikası oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklayın **kaynak Oluştur** sol üst köşesdeki **zeka + analiz**, tıklatıp **Data Factory**.
3. İçinde **yeni veri fabrikası** bölmesi:  
    1. Girin **ADFBlobConnectorDF** için **adı**. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: `*Data factory name “ADFBlobConnectorDF” is not available`(örneğin, yournameADFBlobConnectorDF) veri fabrikasının adını değiştirin ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    2. Azure **aboneliğinizi** seçin.
    3. Kaynak grubunu seçin **var olanı kullan** mevcut bir kaynak grubunu seçin (veya) seçmek için **Yeni Oluştur** için bir kaynak grubu için bir ad girin.
    4. Veri fabrikası için bir **konum** seçin.
    5. Dikey pencerenin alt kısmındaki **Panoya sabitle** onay kutusunu seçin.
    6. **Oluştur**’a tıklayın.
3. Oluşturma işlemi tamamlandıktan sonra, aşağıdaki görüntüde gösterildiği gibi **Data Factory** dikey penceresini görürsünüz:  ![Data factory giriş sayfası](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Kopyalama Sihirbazı
1. Data Factory giriş sayfasında tıklayın **veri kopyalama** başlatmak için **veri kopyalama Sihirbazı'nı** ayrı bir sekmede.  
    
    > [!NOTE]
    > Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz, devre dışı bırakın/işaretini kaldırın **ve site verilerini üçüncü taraf tanımlama bilgilerini engelle** ayarlama (veya) etkin durumda ve oluşturmak için bir özel durum **login.microsoftonline.com**ve ardından Sihirbazı yeniden başlatmayı deneyin.
2. **Özellikler** sayfasında:
    1. Girin **CopyPipeline** için **görev adı**. Görev adı, veri fabrikasındaki işlem hattı adıdır.
    2. Girin bir **açıklama** görevin (isteğe bağlı).
    3. İçin **görev temposu veya görev zamanlaması**, tutmak **düzenli olarak zamanlamaya göre çalıştırma** seçeneği. Bu görevi yalnızca bir kez art arda bir zamanlamaya göre çalıştır yerine çalıştırmak isteyip istemediğinizi seçin **şimdi bir kez çalıştır**. Seçerseniz, **şimdi bir kez çalıştır** seçeneği, bir [tek seferlik işlem hattı](data-factory-create-pipelines.md#onetime-pipeline) oluşturulur.
    4. Ayarlarını koruyabilirsiniz **yinelenen desen**. Bu görev, bir sonraki adımda belirttiğiniz her gün başlangıç ve bitiş saatleri arasında çalışır.
    5. Değişiklik **başlangıç tarihi ve saati** için **21/04/2017**.
    6. Değişiklik **bitiş tarihi ve saati** için **04/25/2017**. Takvim göz atma yerine tarihi yazın isteyebilirsiniz.
    8. **İleri**’ye tıklayın.
        ![Kopyalama aracı - Özellikler sayfası](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Storage** kutucuğuna tıklayın. Kopyalama görevine yönelik kaynak veri deposunu belirtmek için bu sayfayı kullanın. Yeni bir veri deposu belirtmek için mevcut bir veri deposu bağlı hizmetini kullanabilirsiniz (veya) yeni bir veri deposu belirtebilirsiniz. Mevcut bir bağlı hizmeti kullanmak için seçeceğiniz **mevcut bağlı hizmetlerden** ve doğru bağlı hizmeti seçin.
    ![Kopyalama aracı - kaynak veri deposu sayfası](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında:
    1. İçin otomatik olarak oluşturulan adı değiştirmeyin **bağlantı adı**. Bağlantı adı türündeki bağlı hizmetin adıdır: Azure depolama.
    2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
    3. Azure aboneliğinizi seçin ya da koruma **Tümünü Seç** için **Azure aboneliği**.
    4. Seçili abonelikte bulunan Azure depolama hesapları listesinden bir **Azure depolama hesabı** seçin. Ayrıca depolama hesabı ayarlarını el ile girmeyi seçebilirsiniz **el ile girmek** seçeneğini **hesap seçme yöntemi**.
    5. **İleri**’ye tıklayın.  
        ![Kopyalama aracı - Azure Blob Depolama hesabı belirtin](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. **Girdi dosyası veya klasörü seçin** sayfasında:
    1. Çift **adfblobcontainer**.
    2. Seçin **giriş**, tıklatıp **Seç**. Bu kılavuzda, giriş klasörü seçin. Siz de emp.txt dosyasını klasöründe yerine seçebilirsiniz.
        ![Kopyalama aracı - girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. Üzerinde **girdi dosyasını veya klasörünü seçin** sayfası:
    1. Onaylayın **dosya veya klasör** ayarlanır **adfblobconnector/input**. Alt klasörlerde bulunan dosyalar varsa 2017/04/01, 2017/04/02 ve bu şekilde adfblobconnector/input gibi girin / {year} / {month} / {day} dosya veya klasör için. Metin kutusu dışına SEKME tuşuna bastığınızda biçimleri (yyyy) yıl, ay (MM) ve günlük (gg) seçmek için üç açılan listeler bakın.
    2. Ayarlı değil **dosyasını yinelemeli olarak kopyalama**. Hedefe kopyalanacak dosyaları klasörlerde yinelemeli olarak çapraz geçiş için bu seçeneği belirleyin.
    3. Sağlamadığı **ikili kopya** seçeneği. Hedef kaynak dosyasını ikili bir kopyasını gerçekleştirmek için bu seçeneği belirleyin. Bu kılavuz için sonraki sayfalarda daha fazla seçenek görebilmeniz için seçmeyin.
    4. Onaylayın **sıkıştırma türü** ayarlanır **hiçbiri**. Desteklenen biçimlerden birinde sıkıştırılmış Kaynak dosyalarınız varsa bu seçeneği için bir değer seçin.
    5. **İleri**’ye tıklayın.
    ![Kopyalama aracı - girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)
7. **Dosya biçimi ayarları** sayfasında sınırlayıcıları ve sihirbaz tarafından dosya ayrıştırılarak otomatik olarak algılanan düzeni görürsünüz.
    1. Aşağıdaki seçenekler onaylayın:  
        a. **Dosya biçimi** ayarlanır **metin biçimi**. Tüm desteklenen biçimler aşağı açılan listesinde görebilirsiniz. Örneğin: JSON, Avro, ORC, Parquet.
       b. **Sütun sınırlayıcısı** ayarlanır `Comma (,)`. Aşağı açılan listeden Data Factory tarafından desteklenen diğer sütun sınırlayıcıları görebilirsiniz. Özel sınırlayıcı de belirtebilirsiniz.
       c. **Satır sınırlayıcısı** ayarlanır `Carriage Return + Line feed (\r\n)`. Aşağı açılan listeden Data Factory tarafından desteklenen diğer satır sınırlayıcıları görebilirsiniz. Özel sınırlayıcı de belirtebilirsiniz.
       d. **Satır sayısını atla** ayarlanır **0**. Birkaç satır kod dosyasının en üstüne atlanacak istiyorsanız, burada numarasını girin.
       e. **İlk veri satırı sütun adları içeren** ayarlı değil. İlk satırda sütun adı kaynak dosyaları içeriyorsa, bu seçeneği belirleyin.
       f. **Boş sütun değeri null olarak kabul** seçeneği ayarlanır.
    2. Genişletin **Gelişmiş ayarlar** Gelişmiş seçenek kullanılabilir görmek için.
    3. Sayfanın en altında bkz **Önizleme** emp.txt dosyasındaki verilerin.
    4. Tıklayın **şema** altındaki kaynak dosyasındaki verilerin bakarak Kopyalama Sihirbazı'nı ortaya çıkan şemasını görmek için sekmesinde.
    5. Sınırlayıcıları gözden geçirin verilerin önizlemesini gördükten sonra **İleri**’ye tıklayın.
    ![Kopyalama aracı - dosya biçimi ayarları](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)
8. Üzerinde **hedef veri deposu sayfası**seçin **Azure Blob Depolama**, tıklatıp **sonraki**. Azure Blob Depolama, kaynak ve hedef veri depoları bu kılavuzda olarak kullanıyor.  
    ![Kopyalama aracı - select hedef veri deposu](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. Üzerinde **Azure Blob Depolama hesabı belirtin** sayfası:  
    1. Girin **AzureStorageLinkedService** için **bağlantı adı** alan.
    2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
    3. Azure **aboneliğinizi** seçin.
    4. Azure depolama hesabınızı seçin.
    5. **İleri**’ye tıklayın.
10. Üzerinde **çıktı dosyasını veya klasörünü seçin** sayfası:  
    1. belirtin **klasör yolu** olarak **adfblobconnector/output / {year} / {month} / {day}** . Girin **sekmesini**.
    1. İçin **yıl**seçin **yyyy**.
    1. İçin **ay**, kümesine olduğunu onaylayın **MM**.
    1. İçin **gün**, kümesine olduğunu onaylayın **GG**.
    1. Onaylayın **sıkıştırma türü** ayarlanır **hiçbiri**.
    1. Onaylayın **kopyalama davranışı** ayarlanır **dosyaları Birleştir**. Aynı ada sahip çıkış dosyası zaten varsa, yeni içerik aynı dosyanın sonuna eklenir.
    1. **İleri**’ye tıklayın.
       ![Kopyalama aracı - çıktı dosyasını veya klasörünü seçin](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. Üzerinde **dosya biçimi ayarları** sayfasında, ayarları gözden geçirin ve tıklayın **sonraki**. Ek seçenekler burada bir üst bilgi çıkış dosyasına eklemek için biridir. Bu seçeneği belirlerseniz, bir üst bilgi satırı kaynak şemasından sütunların adlarıyla eklenir. Varsayılan sütun adları kaynağı için şema görüntülerken yeniden adlandırabilirsiniz. Örneğin, ilk sütun, ad ve Soyadı ikinci sütuna değiştirebilir. Ardından, çıktı dosyası şu adlara sahip bir üst bilgisiyle sütun adları olarak oluşturulur.
    ![Kopyalama aracı - hedef için dosya biçimi ayarları](media/data-factory-azure-blob-connector/file-format-destination.png)
12. Üzerinde **performans ayarları** sayfasında, onaylayın **bulut birimleri** ve **paralel kopya** ayarlandığından **otomatik**, İleri'ye tıklayın. Bu ayarlar hakkında daha fazla ayrıntı için bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md#parallel-copy).
    ![Kopyalama aracı - performans ayarları](media/data-factory-azure-blob-connector/copy-performance-settings.png)
14. Üzerinde **özeti** sayfasında (görev özellikleri, kaynak ve hedef ayarları ve kopyalama ayarları) tüm ayarları gözden geçirin ve tıklayın **sonraki**.
    ![Kopyalama aracı - Özet sayfası](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. **Özet** sayfasındaki bilgileri gözden geçirin ve **Son**’a tıklayın. Sihirbaz, veri fabrikasında (Kopyalama Sihirbazı’nı başlattığınız yer) iki bağlı hizmet, iki veri kümesi (girdi ve çıktı) ve bir işlem hattı oluşturur.
    ![Kopyalama aracı - dağıtım sayfası](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-the-pipeline-copy-task"></a>(Kopyalama görevi) işlem hattını izleme

1. Bağlantıya tıklayın `Click here to monitor copy pipeline` üzerinde **dağıtım** sayfası.
2. Görmelisiniz **izleme ve yönetme uygulamasını** ayrı bir sekmede.  ![İzleme ve yönetme uygulaması](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Değişiklik **Başlat** zaman en üstte `04/19/2017` ve **son** zamanı `04/27/2017`ve ardından **Uygula**.
4. Beş etkinlik pencerelerini görmelisiniz **etkinlik PENCERELERİ** listesi. **WindowStart** süreleri, işlem hattı baştan bitiş zamanlarını işlem hattı her gün kapsamalıdır.
5. Tıklayın **Yenile** için düğme **etkinlik PENCERELERİ** birkaç kez tüm etkinlik pencerelerini durumunu görene kadar listeyi hazır ayarlayın.
6. Şimdi, çıktı dosyaları adfblobconnector kapsayıcı çıkış klasöründe oluşturulan doğrulayın. Çıktı klasöründe aşağıdaki klasör yapısına görmeniz gerekir:
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25
    ```
   İzleme ve veri fabrikaları yönetme hakkında ayrıntılı bilgi için bkz: [İzleyici ve Data Factory işlem hattı yönetme](data-factory-monitor-manage-app.md) makalesi.

### <a name="data-factory-entities"></a>Data Factory varlıkları
Şimdi, Data Factory giriş sayfasını içeren sekmeye dönün. İki bağlı hizmet, iki veri kümesi ve, data factory'de bir işlem hattı artık olduğuna dikkat edin.

![Varlıklarla Data Factory giriş sayfası](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Tıklayın **yazar ve dağıtma** Data Factory Düzenleyicisi'ni başlatmak için.

![Data Factory Düzenleyicisi](media/data-factory-azure-blob-connector/data-factory-editor.png)

Veri fabrikanızın aşağıdaki Data Factory varlıklarını görmeniz gerekir:

- İki bağlı hizmet. Bir kaynak ve hedef için bir tane. İki bağlı hizmet, bu kılavuzda aynı Azure depolama hesabına bakın.
- İki veri kümesi. Girdi veri kümesi ve çıktı veri kümesi. Bu izlenecek yolda, her ikisi de aynı blob kapsayıcısını kullanın ancak farklı klasörlere (girdi ve çıktı) ifade eder.
- Bir işlem hattı. İşlem hattı, verileri Azure blob bir konumdan başka bir Azure blob konumuna kopyalamak için bir blob kaynağı ve havuz blob kullanan bir kopyalama etkinliği içeriyor.

Aşağıdaki bölümler bu varlıkları hakkında daha fazla bilgi sağlar.

#### <a name="linked-services"></a>Bağlı hizmetler
İki bağlı hizmet görmeniz gerekir. Bir kaynak ve hedef için bir tane. Bu izlenecek yolda, her iki tanımları aynı adları dışında bakın. **Türü** bağlı hizmetin adı kümesine **AzureStorage**. Bağlı hizmet tanımının en önemli özelliği **connectionString**, çalışma zamanında Azure depolama hesabınıza bağlanmak için Data Factory tarafından kullanılır. Tanımındaki hubName özelliği yoksayın.

##### <a name="source-blob-storage-linked-service"></a>Kaynak blob depolama bağlı hizmeti
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

Azure depolama bağlı hizmeti hakkında daha fazla bilgi için bkz. [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.

#### <a name="datasets"></a>Veri kümeleri
İki veri kümesi vardır: bir giriş veri kümesi ve çıktı veri kümesi. Veri kümesi türü **AzureBlob** hem de.

Girdi veri kümesini işaret **giriş** klasörü **adfblobconnector** blob kapsayıcısı. **Dış** özelliği **true** bu veri kümesi için verileri girdi olarak bu veri kümesini alan kopyalama etkinliği ile işlem hattı tarafından üretilen değil olarak.

Çıktı veri kümesini işaret **çıkış** aynı blob kapsayıcısı, klasör. Çıktı veri kümesi yıl, ay ve günü kullanan **SliceStart** sistem değişkeni çıkış dosyasının yolunu dinamik olarak değerlendirilecek. İşlevler ve Data Factory tarafından desteklenen sistem değişkenleri listesi için bkz. [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md). **Dış** özelliği **false** (varsayılan değer) bu veri kümesi, işlem hattı tarafından oluşturulduğundan.

Azure Blob veri kümesi tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz. [veri kümesi özellikleri](#dataset-properties) bölümü.

##### <a name="input-dataset"></a>Giriş veri kümesi

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
İşlem hattı yalnızca bir etkinlik içerir. **Türü** etkinliğini kümesine **kopyalama**. Etkinliğin tür özelliklerini iki bölüm, bir kaynak için ve havuz için bir tane vardır. Kaynak türü **BlobSource** etkinlik blob depolama alanından verileri kopyalama gibi. Havuz türü **BlobSink** olarak bir blob depolama alanına veri kopyalama etkinliği. Kopyalama etkinliği, girdi olarak Inputdataset z4y alır ve çıktı olarak OutputDataset z4y.

BlobSource ve BlobSink tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz. [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü.

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

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a>JSON örnekler ve Blob depolamadan/depolamaya veri kopyalamak için
Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar için ve Azure Blob Depolama ve Azure SQL veritabanına veri kopyalama işlemini göstermektedir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtilen havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a>JSON örneği: Verileri Blob depolama alanından SQL veritabanına kopyalama
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [BlobSource](#copy-activity-properties) ve [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

Zaman serisi, Azure SQL verileri Azure blob örnek kopya saatlik tablo. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

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
**Azure depolama bağlı hizmeti:**

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
Azure Data Factory, Azure depolama bağlı hizmetlerini iki türlerini destekler: **AzureStorage** ve **AzureStorageSas**. Birinci hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.

**Azure Blob girdi veri kümesi:**

Veri alındığından yeni blobundan her saat (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı Data Factory tablosu dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

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

Örnek kopya verileri bir tablo için bir Azure SQL veritabanı'nda "MyTable" adlı. Blob CSV dosyasını içerecek şekilde beklediğiniz gibi aynı sayıda sütun ile Azure SQL veritabanı tablosu oluşturun. Yeni satırlar saatte tablosuna eklenir.

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
**Blob kaynağı ve SQL ile bir işlem hattındaki kopyalama etkinliği havuzu:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **SqlSink**.

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
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a>JSON örneği: Azure Blob Azure SQL veri kopyalayın
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](#dataset-properties).
5. A [işlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) ve [BlobSink](#copy-activity-properties).

Örnek zaman serisi verileri bir Azure SQL tablosundan bir Azure blobuna saatlik kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

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
**Azure depolama bağlı hizmeti:**

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
Azure Data Factory, Azure depolama bağlı hizmetlerini iki türlerini destekler: **AzureStorage** ve **AzureStorageSas**. Birinci hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.

**Azure SQL giriş veri kümesi:**

Örnek, "MyTable" Azure SQL'de bir tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

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

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

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
          "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
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

**SQL kaynak ve havuz Blob ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **SqlSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içinde seçer.

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
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
