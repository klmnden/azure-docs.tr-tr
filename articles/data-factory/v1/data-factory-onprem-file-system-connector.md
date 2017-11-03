---
title: Veri kopyalama/Azure Data Factory kullanarak bir dosya sisteminden | Microsoft Docs
description: "Azure Data Factory kullanarak ve bir şirket içi dosya sisteminden veri kopyalamak öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6e5fa3391d7acf93a3362533d0a65d600913eee3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veri için ve bir şirket içi dosya sisteminden kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-onprem-file-system-connector.md)
> * [Sürüm 2 - Önizleme](../connector-file-system.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 dosya sistemi Bağlayıcısı](../connector-file-system.md).


Bu makalede kopya etkinliği Azure Data Factory öğesine/öğesinden bir şirket içi dosya sistemi veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **bir şirket içi dosya sisteminden** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **bir şirket içi dosya sistemine**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Hedefe başarıyla kopyalandıktan sonra kopyalama etkinliği kaynak dosyasını silmez. Kaynak dosya sonra başarılı bir kopyasını silmeniz gerekirse, dosyayı silin ve ardışık düzeninde etkinlik kullanmak için özel bir aktivite oluşturun. 

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Veri Fabrikası destekler ve bir şirket içi dosya sisteminden bağlamak **veri yönetimi ağ geçidi**. Dosya sistemi dahil olmak üzere tüm desteklenen şirket içi veri deposuna bağlanmak Data Factory hizmetinin şirket içi ortamınızda veri yönetimi ağ geçidi yüklemeniz gerekir. Ağ geçidi ayarlama hakkında adım adım yönergeler için ve veri yönetimi ağ geçidi hakkında bilgi edinmek için bkz: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md). Veri Yönetimi ağ geçidi dışında başka bir ikili dosyalar için ve bir şirket içi dosya sisteminden iletişim kurmak için yüklü olması gerekir. Yüklemeniz ve Azure Iaas sanal dosya sistemi olsa bile, veri yönetimi ağ geçidi kullanmanız gerekir. Ağ geçidi hakkında ayrıntılı bilgi için bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).

Linux dosya paylaşımını kullanmak için yükleme [Samba](https://www.samba.org/) Linux sunucusu ve Windows Server yükleme veri yönetimi ağ geçidi. Veri Yönetimi ağ geçidi Linux sunucusu üzerinde yüklenmesi desteklenmez.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak verileri öğesine/öğesinden bir dosya sistemi taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri Azure blob depolama alanından bir şirket içi dosya sistemine kopyalıyorsanız Azure depolama hesabı ve şirket içi dosya sistemi veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Bir şirket içi dosya sistemine özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.
3. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte blob kapsayıcısı ve giriş verilerini içeren klasörü belirtmek için bir veri kümesi oluşturun. Ve klasör ve dosya adı (dosya sisteminizi isteğe bağlı) belirtmek için başka bir veri kümesi oluşturun. Şirket içi dosya sistemine özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve FileSystemSink havuzu olarak kopya etkinliği için kullanırsınız. Azure Blob depolama alanına şirket içi dosya sisteminden kopyalama, benzer şekilde, FileSystemSource ve BlobSink kopyalama etkinliği kullanın. Şirket içi dosya sistemine özgü kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir dosya sistemi/veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-file-system) bu makalenin.

Aşağıdaki bölümler, Data Factory varlıklarını belirli dosya sistemine tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bir Azure data factory ile bir şirket içi dosya sistemi bağlayabilirsiniz **şirket içi dosya sunucusu** bağlı hizmeti. Aşağıdaki tabloda şirket içi dosya sunucusu bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlandığından emin olun **OnPremisesFileServer**. |Evet |
| ana bilgisayar |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Kaçış karakteri kullanmak ' \ ' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için. |Evet |
| Kullanıcı Kimliği |Sunucusuna erişimi olan kullanıcı Kimliğini belirtin. |Hayır (encryptedCredential seçerseniz) |
| password |(UserID) kullanıcının parolasını belirtin. |Hayır (encryptedCredential seçerseniz |
| encryptedCredential |Yeni AzureRmDataFactoryEncryptValue cmdlet'ini çalıştırarak alabilirsiniz şifreli kimlik bilgilerini belirtin. |Hayır (kullanıcı kimliği ve parola düz metin olarak belirtmek isterseniz) |
| gatewayName |Veri Fabrikası şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidi adını belirtir. |Evet |


### <a name="sample-linked-service-and-dataset-definitions"></a>Örnek bağlantılı hizmet ve veri kümesi tanımları
| Senaryo | Bağlantılı hizmet tanımında ana bilgisayar | veri kümesi tanımında folderPath |
| --- | --- | --- |
| Veri Yönetimi ağ geçidi makine üzerinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |D:\\ \\ (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/> localhost (daha önceki sürümler için veri yönetimi ağ geçidi 2. 0) |. \\ \\ veya klasör\\\\alt klasör (için veri yönetimi ağ geçidi 2.0 ve sonraki sürümler) <br/><br/>D:\\ \\ veya D:\\\\klasörü\\\\alt klasör (için ağ geçidi sürüm 2.0 altında) |
| Uzak paylaşılan klasör: <br/><br/>Örnekler: \\ \\myserver\\paylaşmak\\ \* veya \\ \\myserver\\paylaşmak\\klasörü\\alt klasör\\* |\\\\\\\\myserver\\\\paylaşma |. \\ \\ veya klasör\\\\alt klasör |


### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: kullanıcı adı ve parola düz metin olarak kullanma

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

### <a name="example-using-encryptedcredential"></a>Örnek: encryptedcredential kullanma

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
Bölümleri ve veri kümelerini tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir.

Veri kümesi her tür için farklı typeProperties bölümüdür. Konum ve verilerin veri deposunda biçimi gibi bilgiler sağlar. TypeProperties bölüm türü veri kümesi için **FileShare** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Alt klasöre belirtir. Kaçış karakteri kullanmak ' \' dize özel karakter. Bkz: [örnek bağlantılı hizmeti ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekleri için.<br/><br/>Bu özellik ile birleştirebilirsiniz **partitionBy** klasörün dilimine dayalı yol başlangıç/bitiş tarih saatleri. |Evet |
| fileName |Dosya adını belirtin **folderPath** klasöründeki belirli bir dosya belirtmek için tablo istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, tablonun klasördeki tüm dosyaları işaret eder.<br/><br/>Zaman **fileName** bir çıkış veri kümesi için belirtilmemiş ve **preserveHierarchy** belirtilmemiş etkinlik havuzunda oluşturulmuş dosya adı şu biçimde: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath dosyaları kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen değerler: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/><br/>Örnek 1: "fileFilter": "* .log"<br/>Örnek 2: "fileFilter": 2014 - 1-? txt"<br/><br/>Bu fileFilter bir giriş FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| partitionedBy |PartitionedBy dinamik folderPath/için bir dosya adı time series verilerini belirtmek için kullanabilirsiniz. İçin verileri saatte parametreli folderPath örneğidir. |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter aynı anda kullanamazsınız.

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi bir dinamik folderPath ve zaman serisi verilerle dosya adını belirtebilirsiniz **partitionedBy** özelliği, [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md).

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla ayrıntı anlamak için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md), ve [ardışık düzen oluşturma](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {dilim} biçiminde (YYYYMMDDHH) veri fabrikası sistem değişkeni SliceStart değeriyle değiştirilir. Dilimin başlangıç SliceStart başvuruyor. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

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

Bu örnekte, folderPath ve dosya adı özellikleri kullanan ayrı değişkenlere yıl, ay, gün ve saat SliceStart ayıklanır.

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış veri kümeleri ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir. Bulunan özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile değişir.

Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir. Bir şirket içi dosya sisteminden veri taşıyorsanız, kaynak türü için kopyalama etkinliğinde ayarladığınız **FileSystemSource**. Bir şirket içi dosya sistemi veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğinde ayarladığınız **FileSystemSink**. Bu bölümde FileSystemSource ve FileSystemSink tarafından desteklenen özellikler listesini sağlar.

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |TRUE, False (varsayılan) |Hayır |

**FileSystemSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya dosya sistemi olduğunda kopyalama davranışını tanımlar. |**PreserveHierarchy:** dosya hiyerarşisi hedef klasördeki korur. Diğer bir deyişle, kaynak dosyanın kaynak klasöre göreli yol hedef dosya hedef klasöre göreli yol aynıdır.<br/><br/>**FlattenHierarchy:** tüm dosyaları kaynak klasörden hedef klasöre ilk düzeyi oluşturulur. Hedef dosyalar otomatik olarak oluşturulur adıyla oluşturulur.<br/><br/>**MergeFiles:** bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Dosya adı/blob adı belirtilirse, birleştirilmiş dosya adı belirtilen addır. Aksi halde, bir otomatik olarak oluşturulan dosya adı değil. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior özelliklerine ilişkin değerleri farklı birleşimlerini kopyalama işlemi açıklanmaktadır.

| özyinelemeli değeri | copyBehavior değeri | Bunun sonucunda oluşan davranışı |
| --- | --- | --- |
| TRUE |preserveHierarchy |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 kaynak aynı yapısını oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| TRUE |flattenHierarchy |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan adı |
| TRUE |mergeFiles |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adına sahip bir dosya halinde birleştirilir. |
| False |preserveHierarchy |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |
| False |flattenHierarchy |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |
| False |mergeFiles |Kaynak klasör Klasör1 şu yapıda<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 içeriği otomatik olarak oluşturulan dosya adına sahip bir dosya halinde birleştirilir.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 toplanma değil. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md) makale ayrıntıları.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>JSON örnekleri ve ondan veri kopyalamak için dosya sistemi
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve bir şirket içi dosya sistemi ve Azure Blob depolama alanından veri kopyalamak nasıl gösterir. Ancak, veri kopyalayabilirsiniz *doğrudan* herhangi birinden herhangi birine listelenen havuzlarını kaynakları [desteklenen kaynakları ve havuzlarını](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Örnek: verileri Azure Blob depolama alanına bir şirket içi dosya sisteminden kopyalama
Bu örnek, bir şirket içi dosya sisteminden Azure Blob depolama alanına veri kopyalama gösterilmektedir. Örnek aşağıdaki Data Factory varlıklarını sahiptir:

* Bağlı hizmet türü [OnPremisesFileServer](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [FileShare](#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Aşağıdaki örnek time series verilerini bir şirket içi dosya sisteminden saatte Azure Blob Depolama birimine kopyalar. Bu örnekler kullanılan JSON özellikleri sonra örnekleri bölümlerde açıklanmıştır.

İçindeki yönergeler doğrultusunda ilk adım olarak, veri yönetimi ağ geçidi kurun ayarlamak [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).

**Şirket içi dosya sunucusu hizmeti bağlı:**

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

Kullanmanızı öneririz **encryptedCredential** özelliği yerine **UserID** ve **parola** özellikleri. Bkz: [dosya sunucusuna bağlı hizmetinin](#linked-service-properties) bu hakkındaki ayrıntılar için bağlantılı hizmeti.

**Azure Storage bağlı hizmeti:**

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

**Şirket içi sistem girdi veri kümesi dosya:**

Verileri yeni bir dosyadan saatte kayıt. FolderPath ve dosya adı özellikleri, dilim başlangıç zamanı temel alınarak belirlenir.  

Ayarı `"external": "true"` Data Factory veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

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

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saati başlangıç saatinden bölümlerini kullanır.

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

**Dosya sistemi kaynak ve Blob havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **FileSystemSource**, ve **havuz** türü ayarlanmış **BlobSink**.

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

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Örnek: Azure SQL veritabanından veri, bir şirket içi dosya sistemine kopyalayın.
Aşağıdaki örnek gösterilmektedir:

* Bağlı hizmet türü [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* Bağlı hizmet türü [OnPremisesFileServer](#linked-service-properties).
* Bir giriş veri kümesi türü [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* Bir çıkış veri kümesi türü [FileShare](#dataset-properties).
* Kullanan kopyalama etkinliği ile işlem hattı [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) ve [FileSystemSink](#copy-activity-properties).

Örnek zaman serisi veri saatte bir şirket içi dosya sistemine bir Azure SQL tablosundan kopyalar. Bu örnekler kullanılan JSON özellikleri sonra örnekleri bölümlerde açıklanmıştır.

**Azure SQL Database hizmeti bağlı:**

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

**Şirket içi dosya sunucusu hizmeti bağlı:**

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

Kullanmanızı öneririz **encryptedCredential** kullanmak yerine özelliği **UserID** ve **parola** özellikleri. Bkz: [dosya sistemi bağlantılı hizmet](#linked-service-properties) bu hakkındaki ayrıntılar için bağlı hizmeti.

**Azure SQL girdi veri kümesi:**

Örnek bir tablo "MyTable" Azure SQL oluşturduğunuz ve için zaman serisi veri "timestampcolumn" adlı bir sütun içerdiği varsayar.

Ayarı ``“external”: ”true”`` Data Factory veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

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

**Şirket içi sistem çıktı veri kümesi dosya:**

Veriler her saat yeni bir dosyaya kopyalanır. FolderPath ve blob fileName dilimin başlangıç zamanı temel alınarak belirlenir.

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

**SQL kaynak ve dosya sistemi havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **SqlSource**ve **havuz** türü ayarlanmış **FileSystemSink**. İçin belirtilen SQL sorgusu **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

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


Ayrıca havuz dataset kopyalama etkinliği tanımında sütunları kaynak kümesinden sütunları eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
 Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar veri taşıma (kopyalama etkinliği) performansını etkileyip anahtar Etkenler hakkında bilgi edinmek için [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
