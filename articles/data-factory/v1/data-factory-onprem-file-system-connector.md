---
title: Azure Data Factory kullanarak bir dosya sistemi/veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak verileri için ve bir şirket içi dosya sisteminden kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 40086924731876dc44d9651ca46814149dba52f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122463"
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri için ve bir şirket içi dosya sisteminden kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-onprem-file-system-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-file-system.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de dosya sistemi Bağlayıcısı](../connector-file-system.md).


Bu makalede, kopyalama etkinliği Azure Data Factory'de veri gönderip buralardan veri bir şirket içi dosya sistemine kopyalamak için nasıl kullanılacağı açıklanmaktadır. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalayabilirsiniz **bir şirket içi dosya sisteminden** aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz **şirket içi dosya sistemine**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Hedefine başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosya silinmez. Kopyalama başarılı sonra kaynak dosyayı silmek için ihtiyacınız varsa dosyayı silin ve işlem hattı, etkinlik kullanmak için özel bir etkinlik oluşturun.

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Data Factory destekler ve bir şirket içi dosya sisteminden bağlamak **veri yönetimi ağ geçidi**. Dosya sistemi dahil olmak üzere tüm desteklenen şirket içi veri deposuna bağlanmak Data Factory hizmetinin şirket içi ortamınızda veri yönetimi ağ geçidi yüklemeniz gerekir. Ağ geçidini ayarlama hakkında adım adım yönergeler ve veri yönetimi ağ geçidi hakkında bilgi edinmek için [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md). Veri Yönetimi ağ geçidi dışında başka bir ikili dosyalar için ve bir şirket içi dosya sisteminden iletişim kurmak için yüklü olması gerekir. Yükleme ve Azure Iaas sanal dosya sistemi olsa bile, veri yönetimi ağ geçidi kullanmanız gerekir. Ağ geçidi hakkında ayrıntılı bilgi için bkz. [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).

Linux dosya paylaşımını kullanmak için yükleme [Samba](https://www.samba.org/) Linux sunucusu ve bir Windows sunucusundaki veri yönetimi ağ geçidi yükleyin. Veri Yönetimi ağ geçidi yükleme Linux sunucusu üzerinde desteklenmiyor.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak veri gönderip buralardan veri bir dosya sistemi taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir.
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri bir Azure blob depolama alanından bir şirket içi dosya sistemine kopyalıyorsanız kullanarak şirket içi dosya sistemi ve Azure depolama hesabınızı veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. Bir şirket içi dosya sistemine özgü bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
3. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Son adımda bahsedilen örnekte, bir veri kümesi blob kapsayıcıyı ve girdi verilerini içeren klasörü belirtin oluşturun. Ayrıca, klasör ve dosya adı (dosya sisteminizde isteğe bağlı) belirtmek için başka bir veri kümesi oluşturursunuz. Şirket içi dosya sistemine özgü veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve FileSystemSink havuz olarak kopyalama etkinliği için kullanırsınız. Azure Blob Depolama'ya şirket içi dosya sisteminden kopyalıyorsanız benzer şekilde, FileSystemSource ve BlobSink kopyalama etkinliği kullanırsınız. Şirket içi dosya sistemine özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir kaynak veya havuz bir veri deposunu kullanma hakkında daha fazla ayrıntı için önceki bölümde veri deponuz için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın.  / Dosya sisteminden veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples-for-copying-data-to-and-from-file-system) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli dosya sistemine tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bir Azure data factory ile bir şirket içi dosya sistemine bağlanabilirsiniz **şirket içi dosya sunucusu** bağlı hizmeti. Aşağıdaki tabloda, şirket içi dosya sunucusuna bağlı hizmete özgü JSON öğelerinin açıklamaları verilmiştir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlandığından emin olun **OnPremisesFileServer**. |Evet |
| host |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Çıkış karakterini kullanma ' \ ' dizesinde özel karakterler için. Bkz: [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekler. |Evet |
| userid |Sunucu erişimi olan kullanıcının kimliği belirtin. |Hayır (encryptedCredential seçerseniz) |
| password |(Kullanıcı kimliği) kullanıcının parolasını belirtin. |Hayır (encryptedCredential seçin |
| encryptedCredential |New-AzDataFactoryEncryptValue cmdlet çalıştırılarak elde edebilirsiniz şifrelenmiş kimlik bilgilerini belirtin. |Hayır (kullanıcı kimliği ve parola düz metin olarak belirtmek isterseniz varsa) |
| gatewayName |Data Factory şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidi adını belirtir. |Evet |


### <a name="sample-linked-service-and-dataset-definitions"></a>Bağlı hizmet ve veri kümesi tanımları örneği
| Senaryo | Bağlı hizmet tanımında barındırın | veri kümesi tanımında folderPath |
| --- | --- | --- |
| Veri Yönetimi ağ geçidi makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |D:\\ \\ (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/> localhost (daha önceki sürümler için veri yönetimi ağ geçidi 2.0) |. \\ \\ veya klasör\\\\alt klasör (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/>D:\\ \\ veya D:\\\\klasör\\\\alt klasör (için ağ geçidi sürüm 2.0 altında) |
| Paylaşılan uzak klasör: <br/><br/>Örnekler: \\ \\myserver\\paylaşmak\\ \* veya \\ \\myserver\\paylaşmak\\klasör\\alt\\* |\\\\\\\\myserver\\\\paylaşın |. \\ \\ veya klasör\\\\alt |

>[!NOTE]
>Kullanıcı Arabirimi yazılırken çift ters eğik çizgi giriş gerekmez (`\\`) gibi JSON erişmek için tek bir ters eğik çizgi belirtin.

### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: Kullanıcı adı ve parola düz metin olarak kullanma

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a>Örnek: Encryptedcredential kullanma

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri için benzerdir.

TypeProperties bölümünün her tür veri kümesi için farklıdır. Bu, veri deposundaki veri biçimi ve konumu gibi bilgiler sağlar. TypeProperties bölüm türü için veri kümesi **FileShare** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Yükleme kökü klasörüne belirtir. Çıkış karakterini kullanma '\' dizedeki özel karakterleri. Joker karakter filtresi desteklenmez. Bkz: [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekler.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün yol tabanlı slice başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adı belirtin **folderPath** klasördeki belirli bir dosyaya başvurmak için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tabloda bir klasördeki tüm dosyaları işaret eder.<br/><br/>Zaman **fileName** için bir çıktı veri kümesi belirtilmedi ve **preserveHierarchy** belirtilmemiş etkinlik havuzunda oluşturulan dosya adı şu biçimde: <br/><br/>`Data.<Guid>.txt` (Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: "fileFilter": "* .log"<br/>Örnek 2: "fileFilter": 2014-1-?.txt"<br/><br/>Bu fileFilter girdi FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| partitionedBy |PartitionedBy dinamik bir folderPath/fileName için zaman serisi verilerini belirtmek için kullanabilirsiniz. FolderPath için verileri saatte parametreli bir örnektir. |Hayır |
| format | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanamazsınız.

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath ve zaman serisi verileri ile dosya adını belirtebilirsiniz **partitionedBy** özelliği [Data Factory işlevleri ve sistem değişkenlerini](data-factory-functions-variables.md).

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla ayrıntı anlamak için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [komut zincirleri oluşturma](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {dilim} biçiminde (YYYYMMDDHH) Data Factory sistem değişkeni SliceStart değeriyle değiştirilir. Başlangıç saati dilimi SliceStart başvuruyor. FolderPath için her bir dilimi farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

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

Bu örnekte, folderPath ve dosya özelliklerini kullanan ayrı değişkenlere yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı veri kümeleri ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir. Diğer yandan bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir.

Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir. Bir şirket içi dosya sisteminden veri taşıyorsanız, kaynak türü için kopyalama etkinliğindeki ayarladığınız **FileSystemSource**. Bir şirket içi dosya sistemine veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğindeki ayarladığınız **FileSystemSink**. Bu bölümde FileSystemSource ve FileSystemSink tarafından desteklenen özelliklerin bir listesini sağlar.

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |TRUE, False (varsayılan) |Hayır |

**FileSystemSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |**PreserveHierarchy:** Hedef klasördeki ise dosya hiyerarşisini korur. Diğer bir deyişle, kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><br/>**FlattenHierarchy:** Tüm dosyaları kaynak klasörden hedef klasöre ilk düzeyde yer oluşturulur. Hedef dosyalar bir otomatik olarak oluşturulan adıyla oluşturulur.<br/><br/>**MergeFiles:** Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı/blob adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior özellikleri için değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| özyinelemeli değeri | copyBehavior değeri | Sonuç davranış |
| --- | --- | --- |
| true |preserveHierarchy |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1, kaynağı olarak aynı yapıya sahip oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + 5 dosya içeriği, bir otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. |
| false |preserveHierarchy |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |flattenHierarchy |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |mergeFiles |Kaynak klasör Klasör1 aşağıdaki yapıya sahip<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği tek bir otomatik olarak oluşturulan dosya adına sahip bir dosyada birleştirilir.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md) ilişkin ayrıntıları.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>Sistem ve ondan veri kopyalamak için örnek JSON dosyası
Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve bir şirket içi dosya sistemi ve Azure Blob depolama alanına veri kopyalamak nasıl gösterir. Ancak, veri kopyalayabilirsiniz *doğrudan* herhangi birinden herhangi birine listelenen havuzlarını kaynakları [desteklenen kaynaklar ve havuzlar](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure veri fabrikasında kopyalama etkinliği kullanarak.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Örnek: Verileri Azure Blob depolama alanına bir şirket içi dosya sisteminden kopyalama
Bu örnek, verileri Azure Blob depolama alanına bir şirket içi dosya sisteminden kopyalama işlemi gösterilmektedir. Örnek, aşağıdaki Data Factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesFileServer](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [FileShare](#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Aşağıdaki örnek zaman serisi verilerinin bir şirket içi dosya sisteminden saatte Azure Blob depolama alanına kopyalar. Bu örneklerde kullanılan JSON özellikleri sonra örnekleri bölümlerde açıklanmıştır.

İçindeki yönergeler doğrultusunda veri yönetimi ağ geçidi ilk adım, ayarlama [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).

**Şirket içi dosya sunucusuna bağlı hizmeti:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Kullanmanızı öneririz **encryptedCredential** özelliği bunun yerine **UserID** ve **parola** özellikleri. Bkz: [dosya sunucusuna bağlı hizmet](#linked-service-properties) bu hakkındaki ayrıntılar için bağlı hizmeti.

**Azure depolama bağlı hizmeti:**

```JSON
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

**Şirket içi sistem girdi veri kümesi dosyası:**

Her saat yeni bir dosyadan veri seçilir. FolderPath ve dosya özelliklerini dilimin başlangıç zamanı temel alınarak belirlenir.

Ayar `"external": "true"` Data Factory veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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

**Azure Blob Depolama çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Dosya sistemi kaynak ve havuz Blob ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **FileSystemSource**, ve **havuz** türü ayarlandığında **BlobSink**.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
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

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Örnek: Verileri Azure SQL veritabanı'ndan bir şirket içi dosya sistemine kopyalayın.
Aşağıdaki örnek, gösterir:

* Bağlı hizmet türü [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* Bağlı hizmet türü [OnPremisesFileServer](#linked-service-properties).
* Girdi veri kümesi türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* Bir çıktı veri kümesi türü [FileShare](#dataset-properties).
* Kullanan bir kopyalama etkinlikli bir işlem hattı [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) ve [FileSystemSink](#copy-activity-properties).

Örnek zaman serisi verileri bir Azure SQL tablosundan saatte bir şirket içi dosya sistemine kopyalar. Bu örneklerde kullanılan JSON özellikleri sonra örnekleri bölümlerde açıklanmıştır.

**Azure SQL veritabanı bağlı hizmeti:**

```JSON
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

**Şirket içi dosya sunucusuna bağlı hizmeti:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Kullanmanızı öneririz **encryptedCredential** kullanmak yerine özellik **UserID** ve **parola** özellikleri. Bkz: [dosya sistemi bağlantılı hizmet](#linked-service-properties) bu hakkındaki ayrıntılar için bağlı hizmeti.

**Azure SQL giriş veri kümesi:**

Örnek, "MyTable" Azure SQL'de bir tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır.

Ayar ``“external”: ”true”`` Data Factory veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```JSON
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

**Şirket içi sistem çıktı veri kümesi dosyası:**

Veriler her saat yeni bir dosyaya kopyalanır. Blob dosya adı ve folderPath dilimin başlangıç zamanı temel alınarak belirlenir.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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

**SQL kaynak ve havuz dosya sistemi ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **SqlSource**ve **havuz** türü ayarlandığında **FileSystemSink**. İçin belirtilen SQL sorgusu **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içinde seçer.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```

Ayrıca, kaynak veri kümesi sütunları havuz veri kümesi kopyalama etkinliği tanımındaki sütunlarından yerine eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
 Veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar performansını etkileyen önemli faktörlerin hakkında bilgi edinmek için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
