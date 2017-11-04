---
title: "Sabit sürücüler için bir Azure içeri/dışarı aktarma içe aktarma işi hazırlama | Microsoft Docs"
description: "Azure içeri/dışarı aktarma hizmeti için bir alma işi oluşturmak için WAImportExport aracını kullanarak sabit sürücüler hazırlama hakkında bilgi edinin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: 2854822907e818297c8d2f74cab48b0afa0d646c
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Sabit sürücüler için içeri aktarma işi hazırlama

İle birlikte kullanabileceğiniz sürücü hazırlama ve Onarım aracını WAImportExport araçtır [Microsoft Azure içeri/dışarı aktarma hizmeti](../storage-import-export-service.md). Bir Azure veri merkezine dağıtmayı kalacaklarını sabit sürücüler verileri kopyalamak için bu aracı kullanabilirsiniz. İçe aktarma işi tamamlandıktan sonra bozuldu, eksik olan veya çakışan BLOB diğer BLOB'lar ile onarmak için bu aracı kullanabilirsiniz. Tamamlanan dışa aktarma işleminden sürücüleri aldıktan sonra bozuk veya eksik sürücülerindeki dosyaları onarmak için bu aracı kullanabilirsiniz. Bu makalede, bu aracın kullanımı gidin.

## <a name="prerequisites"></a>Ön koşullar

### <a name="requirements-for-waimportexportexe"></a>WAImportExport.exe gereksinimleri

- **Makine Yapılandırması**
  - Windows 7, Windows Server 2008 R2 veya daha yeni Windows işletim sistemi
  - .NET framework 4 yüklü olması gerekir. Bkz: [SSS](#faq) .Net Framework olup olmadığını denetlemek nasıl makinede yüklü.
- **Depolama hesabı anahtarı** -en az bir hesabı anahtarları için depolama hesabı gerekir.

### <a name="preparing-disk-for-import-job"></a>Disk için içeri aktarma işi hazırlama

- **BitLocker -** BitLocker WAImportExport aracın çalıştığı makinede etkinleştirilmesi gerekir. Bkz: [SSS](#faq) nasıl BitLocker'ı etkinleştirmek.
- **Diskleri** WAImportExport aracın çalıştığı makineden erişilebilir. Bkz: [SSS](#faq) disk belirtimi için.
- **Kaynak dosyaları** -bir ağ paylaşımına veya yerel bir sabit sürücü üzerinde olup planladığınız içeri aktarmak için dosya kopyalama makinesinden erişilebilir olması gerekir.

### <a name="repairing-a-partially-failed-import-job"></a>Kısmen başarısız alma işi onarma

- **Günlük Dosya Kopyala** Azure içeri/dışarı aktarma hizmeti depolama hesabı ve Disk arasında veri kopyalarken oluşturulur. Hedef depolama hesabınızdaki bulunur.

### <a name="repairing-a-partially-failed-export-job"></a>Kısmen başarısız dışarı aktarma işini onarma

- **Günlük Dosya Kopyala** Azure içeri/dışarı aktarma hizmeti depolama hesabı ve Disk arasında veri kopyalarken oluşturulur. Kaynak depolama hesabınızdaki bulunur.
- **Bildirim dosyası** -Microsoft tarafından döndürülen dışarı aktarılan sürücüde [isteğe bağlı] Located.

## <a name="download-and-install-waimportexport"></a>WAImportExport yükleyip

Karşıdan [WAImportExport.exe en son sürümünü](https://www.microsoft.com/download/details.aspx?id=55280). Sıkıştırılmış içerik bilgisayarınızdaki bir dizine ayıklayın.

CSV dosyaları için bir sonraki göreviniz oluşturulmasıdır.

## <a name="prepare-the-dataset-csv-file"></a>Veri kümesi CSV dosyasını hazırlama

### <a name="what-is-dataset-csv"></a>Veri kümesi CSV nedir

Veri kümesi CSV dizinlerin listesini ve/veya hedef sürücülere kopyalanacak dosyaların bir listesini içeren bir CSV dosyası /dataset bayrak değeri dosyasıdır. İlk adım bir alma işi oluşturmak için hangi dizinleri ve dosyaları almak için kalacaklarını belirlemektir. Bu dizinlerin listesini, benzersiz dosyaların listesini veya bu iki bir birleşimi olabilir. Bir dizin dahil edildiğinde, dizinde ve alt dizinlerinde tüm dosyaları alma işinin bir parçası olur.

Her dizin veya içeri aktarılacak dosya için bir hedef sanal dizin veya Azure Blob hizmetinde blob tanımlamanız gerekir. Bu hedeflerde giriş WAImportExport aracı olarak kullanır. Dizinleri eğik çizgi karakteriyle ayrılmış "/".

Aşağıdaki tabloda blob hedefleri bazı örnekler gösterilmektedir:

| Kaynak dosya veya dizin | Hedef blob veya sanal dizin |
| --- | --- |
| H:\Video | https://mystorageaccount.BLOB.Core.Windows.NET/video |
| H:\Photo | https://mystorageaccount.BLOB.Core.Windows.NET/Photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.BLOB.Core.Windows.NET/Music |

### <a name="sample-datasetcsv"></a>Örnek dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Veri kümesi CSV dosyası alanları

| Alan | Açıklama |
| --- | --- |
| Ana yolu | **[Gerekli]**<br/>Bu parametrenin değeri alınacak verilerin bulunduğu kaynağını temsil eder. Aracı, bu yolu altında bulunan tüm veriler yinelemeli olarak kopyalama olur.<br><br/>**İzin verilen değerler**: Bu yerel bilgisayardaki geçerli bir yol veya geçerli bir paylaşım yolu olmalıdır ve kullanıcı tarafından erişilebilir olması gerekir. Dizin yolu mutlak bir yol (göreli bir yol değil) olması gerekir. Yol ile bitiyorsa "\\", başka bir dizin olmadan bitiş yolu temsil eder"\\" bir dosyayı temsil eder.<br/>Hiçbir regex bu alana izin verilir. Yol boşluk içeriyorsa, içine "".<br><br/>**Örnek**: "c:\Directory\c\Directory\File.txt"<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Gerekli]**<br/> Windows Azure depolama hesabınızdaki hedef sanal dizin yolu. Sanal dizin olabilir veya zaten mevcut olmayabilir. Yoksa, içeri/dışarı aktarma hizmeti bir oluşturacak.<br/><br/>Hedef sanal dizinleri ve blobları belirtirken geçerli kapsayıcı adları kullandığınızdan emin olun. Kapsayıcı adları küçük harfli olması gerektiğini unutmayın. Kapsayıcı adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Yalnızca kök belirtilen kaynak dizin yapısını hedef blob kapsayıcısında çoğaltılır. Kaynağındaki eşleme CSV'ye birden çok satır olandan farklı bir dizin yapısı isterseniz<br/><br/>Bir kapsayıcı veya müzik/70s gibi bir blob öneki belirtebilirsiniz /. Hedef dizin eğik tarafından izlenen kapsayıcı adıyla başlaması gerekir "/" ve isteğe bağlı olarak ile biten bir sanal blob dizin içerebilir "/".<br/><br/>Hedef kapsayıcı kök kapsayıcı olduğunda, $root olarak eğik dahil olmak üzere kök kapsayıcı açıkça belirtmelisiniz /. Hedef dizin kök kapsayıcı olduğunda kök kapsayıcısı altında BLOB'lar içeremez bu yana "/" adlarında, kaynak dizin hiçbir dizinlerde kopyalanmaz.<br/><br/>**Örnek**<br/>Hedef blob yolu https://mystorageaccount.blob.core.windows.net/video ise, bu alanın değeri video olabilir /  |
| BlobType | **[İsteğe bağlı]**  Blok &#124; sayfası<br/>Şu anda içeri/dışarı aktarma hizmeti BLOB'lar 2 türlerini destekler. Sayfa blobları ve blok BlobsBy varsayılan tüm dosyaları blok Blobları içeri aktarılacak. Ve \*.vhd ve \*sayfa BlobsThere blok blobu ve sayfa blob boyutu izin verilen üst sınırı olarak .vhdx alınır. Bkz: [depolama ölçeklenebilirlik hedefleri](storage-scalability-targets.md) daha fazla bilgi için.  |
| Değerlendirme | **[İsteğe bağlı]**  yeniden adlandırmak &#124; Hayır üzerine &#124; üzerine yaz <br/> Bu alan kopyalama davranışını alma yani sırasında belirtir. ne zaman veri depolama hesabına diskten karşıya yükleniyor. Kullanılabilir seçenekler şunlardır: yeniden adlandırma &#124; üzerine yazılsın mı &#124; Hayır üzerine. "Hiçbir şey, belirtilen yeniden adlandırmak için" varsayılan olarak. <br/><br/>**Yeniden Adlandır**: aynı ada sahip bir nesne varsa, hedef bir kopyasını oluşturur.<br/>Üzerine yaz: yeni dosyasıyla dosyasının üzerine yazar. Son değiştirilen WINS dosyasıyla.<br/>**Hayır üzerine**: dosya zaten mevcutsa yazılırken atlar.|
| MetadataFile | **[İsteğe bağlı]** <br/>Bu alan için bir meta veri nesnelerinin korumak veya özel meta verilerini sağlamak gerekiyorsa, sağlanabilir meta veri dosyası değeridir. Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için |
| PropertiesFile | **[İsteğe bağlı]** <br/>Hedef BLOB'ları için özellik dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>InitialDriveSet veya AdditionalDriveSet CSV dosyasını hazırlama

### <a name="what-is-driveset-csv"></a>Driveset CSV nedir

Böylece aracı doğru hazırlanması için disk listesi seçebilirsiniz sürücü harfleri eşlenmiş disklerin listesini içeren bir CSV dosyası /InitialDriveSet veya /AdditionalDriveSet bayrağı değerdir. Veri boyutu tek disk boyutundan daha büyükse WAImportExport aracı bu CSV dosyasında en iyi duruma getirilmiş bir şekilde kayıtlı birden çok disk üzerinde veri dağıtır.

Verileri tek bir oturumda yazılabilir disklerin sayısına bir sınır yoktur. Aracı disk boyutu ve klasör boyutunu temel alan veri dağıtır. En çok disk seçer nesne boyutu için en iyi duruma getirilmiş. Depolama hesabı karşıya verilerinin geri dataset dosyasında belirtilen dizin yapısına Yakınsanan. Driveset CSV oluşturmak için aşağıdaki adımları izleyin.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Temel birim oluşturun ve sürücü harfi atama

Temel birim oluşturmak ve "daha basit bölüm oluşturma sırasında verilen" yönergelerini izleyerek bir sürücü harfi atamak için [Disk Yönetimi'ne genel bakış](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Örnek InitialDriveSet ve AdditionalDriveSet CSV dosyası

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Driveset CSV dosyası alanları

| Alanları | Değer |
| --- | --- |
| SürücüHarfi | **[Gerekli]**<br/> Hedef ve kendisine atanmış bir sürücü harfi basit bir NTFS birimde olması şekilde araç sağlanan her sürücü.<br/> <br/>**Örnek**: R veya r |
| FormatOption | **[Gerekli]**  Biçimi &#124; AlreadyFormatted<br/><br/> **Biçim**: Bu belirleme diskteki tüm verilerin biçimlendirecek. <br/>**AlreadyFormatted**: aracı, bu değeri belirtildiğinde biçimlendirme atlar. |
| SilentOrPromptOnFormat | **[Gerekli]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: Bu değer sağlama aracı sessiz modda çalıştırmak kullanıcı etkinleştirir. <br/>**PromptOnFormat**: aracı eylemi her biçiminde gerçekten yönelik olduğunu onaylamak için kullanıcıyı uyarır.<br/><br/>Ayarlanmadı, komut iptal etmek ve hata iletisini görüntüler: "SilentOrPromptOnFormat için geçersiz değer: yok" |
| Şifreleme | **[Gerekli]**  Şifrelemek &#124; AlreadyEncrypted<br/> Bu alanın değeri, şifrelemek için hangi disk ve hangi değil karar verir. <br/><br/>**Şifreleme**: aracı, sürücü biçimlendirecek. "FormatOption" alanının değerini "Format" ise, bu değer, "Şifrele" olması için gereklidir. "AlreadyEncrypted" Bu durumda belirtilirse, "Biçimi belirtildiğinde şifrele de belirtilmesi gerekir" bir hatayla sonuçlanır.<br/>**AlreadyEncrypted**: aracı "ExistingBitLockerKey" alanında sağlanan BitLockerKey kullanarak sürücü şifresini. "FormatOption" alanının değerini "AlreadyFormatted" ise, ardından bu değer, "Şifrele" veya "AlreadyEncrypted" olabilir |
| ExistingBitLockerKey | **[Gerekli]**  "Şifreleme" alanının değerini "AlreadyEncrypted" ise<br/> Bu alanın değerini belirli bir disk ile ilişkili BitLocker anahtardır. <br/><br/>Bu alan, "Şifrele" "Şifreleme" alanının değerini ise, boş bırakılmalıdır.  BitLocker anahtar bu durumda belirtilirse, "Bitlocker anahtarını belirtilmemesi gerekir" bir hatayla sonuçlanır.<br/>  **Örnek**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Disk için içeri aktarma işi hazırlama

Sürücüleri içeri aktarma işi için hazırlamak üzere WAImportExport aracıyla çağrısı **PrepImport** komutu. Eklediğiniz hangi parametreleri bağlı olarak bu ilk kopya oturumu olup olmadığını veya bir sonraki kopyalama oturumu.

### <a name="first-session"></a>İlk oturumun

Tek/birden çok Disk (bağlı olarak hangi CSV dosyasında belirtilir) WAImportExport aracı dizinler ve/veya yeni bir kopya oturum dosyaları kopyalamak ilk kopya oturumu için PrepImport komutu Single/Multiple dizin kopyalamak için ilk kopyalama oturum:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Sonraki oturumunda veri ekleme

Sonraki kopyalama oturumlarında ilk parametrelerini belirtmek gerekmez. Önceki oturumda kaldığı anımsaması aynı günlük dosyası Aracı'nı kullanmanız gerekir. Kopya oturum durumu günlük dosyasına yazılır. Ek dizinler ve/veya dosyaları kopyalamak bir sonraki kopya oturumu sözdizimi şöyledir:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-to-latest-session"></a>En son oturumuna sürücü ekleme

Veri InitialDriveset belirtilen sürücüleri sığmadı, bir aracı ek sürücüler aynı kopyasını oturumuna eklemek için kullanabilirsiniz. 

>[!NOTE] 
>Oturum kimliği önceki oturum kimliği ile eşleşmesi gerekir. Günlük dosyası önceki oturumunda belirtilen hesapla eşleşmelidir.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-the-latest-session"></a>Son oturum iptal:

Kopya oturumu kesintiye ve (örneğin, bir kaynak dizin erişilemez oluyor uygulamasına yol açıyordu varsa) sürdürmek mümkün değilse, böylece geri alınabilir ve yeni kopya oturumları başlatılabilir geçerli oturum iptal gerekir:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Yalnızca son kopyalama oturumu, anormal olursa iptal. Bir sürücü için ilk kopyalama oturum iptal edilemiyor unutmayın. Bunun yerine yeni bir günlük dosyasıyla kopyalama oturumlarını yeniden başlatmalısınız.

### <a name="resume-a-latest-interrupted-session"></a>Bir son kesintiye uğramış oturum sürdürme

Herhangi bir nedenle bir kopya oturumu kesintiye uğrarsa, yalnızca belirtilen günlük dosyası ile aracını çalıştırarak devam edebilirsiniz:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> Kopya oturumu çağırdığınızda, kaynak veri dosyaları ve dizinleri ekleyerek veya kaldırarak dosyaları değiştirmeyin.

## <a name="waimportexport-parameters"></a>WAImportExport parametreleri

| Parametreler | Açıklama |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Gerekli**<br/> Günlük dosyası yolu. Bir günlük dosyası sürücüleri kümesini izler ve bu sürücüler hazırlarken ilerleme durumunu kaydeder. Günlük dosyası her zaman belirtilmesi gerekir.  |
|     / LOGDIR:&lt;LogDirectory&gt;  | **İsteğe bağlı**. Günlük dosyası dizini.<br/> Bu dizin için bazı geçici dosyaları yanı sıra, ayrıntılı günlük dosyalarına yazılır. Aksi durumda belirtilmezse, geçerli dizin günlük dizini olarak kullanılır. Günlük dizini için aynı günlük dosyası yalnızca bir kez belirtilebilir.  |
|     /ID:&lt;SessionID&gt;  | **Gerekli**<br/> Kopya oturumunu tanımlamak için kullanılan kimliği oturumu. Kesintiye uğramış kopyalama oturumu doğru kurtarma sağlamak için kullanılır.  |
|     / ResumeSession  | İsteğe bağlı. Son kopya oturumu anormal varsa, bu parametre oturum sürdürmek için belirtilebilir.   |
|     / AbortSession  | İsteğe bağlı. Son kopya oturumu anormal varsa, bu parametre oturum iptal etmek için belirtilebilir.  |
|     /sn:&lt;StorageAccountName&gt;  | **Gerekli**<br/> Yalnızca, RepairImport ve RepairExport için de geçerlidir. Depolama hesabı adı.  |
|     /SK:&lt;StorageAccountKey&gt;  | **Gerekli**<br/> Depolama hesabı anahtarı. |
|     / InitialDriveSet:&lt;driveset.csv&gt;  | **Gerekli** ilk kopya oturumu çalışırken<br/> Hazırlamak için sürücülerin listesini içeren bir CSV dosyası.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Gerekli**. Sürücüleri geçerli kopyalama oturuma eklerken. <br/> Eklenecek ek sürücüler listesini içeren bir CSV dosyası.  |
|      / r:&lt;RepairFile&gt; | **Gerekli** yalnızca RepairImport ve RepairExport için geçerlidir.<br/> Onarım ilerleme durumunu izlemek için dosya yolu. Her bir sürücü bir ve yalnızca bir onarım dosyası olması gerekir.  |
|     / d:&lt;TargetDirectories&gt; | **Gerekli**. Yalnızca, RepairImport ve RepairExport için de geçerlidir. RepairImport için onarmak için; bir veya daha çok noktalı virgülle ayrılmış dizinleri Onarmak için bir dizin RepairExport için örneğin dizin sürücünün kök.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Gerekli** yalnızca RepairImport ve RepairExport için geçerlidir. Sürücüyü Kopyala günlük dosyası yolu (ayrıntılı veya hata).  |
|     / Manıfestfıle:&lt;DriveManifestFile&gt; | **Gerekli** RepairExport yalnızca uygulanabilir.<br/> Sürücü bildirim dosyasının yolu.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **İsteğe bağlı**. Yalnızca RepairImport için geçerlidir.<br/> (Sekmeyle sınırlı) gerçek dosya konumlarını sürücüsünün köküne göreli dosya yolları eşlemelerini içeren dosyanın yolu. İlk belirtildiğinde, bu dosya yolları boş hedefler ile TargetDirectories, erişim reddedildi, geçersiz bir ada içinde bulunmayan ya da birden fazla dizinde mevcut yani doldurulur. Yol Haritası el ile doğru hedef yollarını içerecek şekilde düzenlenmesi ve yeniden aracın doğru dosya yolları çözümlemek için belirtilen.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Gerekli**. Yalnızca PreviewExport için geçerlidir.<br/> XML yolu içeren blob yollar listesi dosya veya yol önekleri verilecek BLOB'ları için blob. Dosya biçimi içeri/dışarı aktarma hizmeti REST API'si, iş Put işlemini blob listesi blob biçiminde ile aynıdır.  |
|     / DriveSize:&lt;DriveSize&gt; | **Gerekli**. Yalnızca PreviewExport için geçerlidir.<br/>  Dışarı aktarma için kullanılacak sürücüleri boyutu. Örneğin, 500 GB 1,5 TB. Not: 1 GB = 1.000.000.000 bytes1 TB = 1,000,000,000,000 bayt  |
|     / DataSet:&lt;dataset.csv&gt; | **Gerekli**<br/> Dizinlerin listesini ve/veya hedef sürücülere kopyalanacak dosyaların bir listesini içeren bir CSV dosyası.  |
|     /silentmode  | **İsteğe bağlı**.<br/> Belirtilmezse, sürücüler gereksinim şu aralıklarla uyar ve devam etmek için onay gerekiyor.  |

## <a name="tool-output"></a>Araç çıktısı

### <a name="sample-drive-manifest-file"></a>Örnek sürücü bildirim dosyası

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DriveManifest Version="2011-MM-DD">
   <Drive>
      <DriveId>drive-id</DriveId>
      <StorageAccountKey>storage-account-key</StorageAccountKey>
      <ClientCreator>client-creator</ClientCreator>
      <!-- First Blob List -->
      <BlobList Id="session#1-0">
         <!-- Global properties and metadata that applies to all blobs -->
         <MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>
         <PropertiesPath Hash="md5-hash">global-properties-file-path</PropertiesPath>
         <!-- First Blob -->
         <Blob>
            <BlobPath>blob-path-relative-to-account</BlobPath>
            <FilePath>file-path-relative-to-transfer-disk</FilePath>
            <ClientData>client-data</ClientData>
            <Length>content-length</Length>
            <ImportDisposition>import-disposition</ImportDisposition>
            <!-- page-range-list-or-block-list -->
            <!-- page-range-list -->
            <PageRangeList>
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
            </PageRangeList>
            <!-- block-list -->
            <BlockList>
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
            </BlockList>
            <MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>
            <PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>
         </Blob>
      </BlobList>
   </Drive>
</DriveManifest>
```

### <a name="sample-journal-file-xml-for-each-drive"></a>Her sürücü için örnek günlük dosyası (XML)

```xml
[BeginUpdateRecord][2016/11/01 21:22:25.379][Type:ActivityRecord]
ActivityId: DriveInfo
DriveState: [BeginValue]
<?xml version="1.0" encoding="UTF-8"?>
<Drive>
   <DriveId>drive-id</DriveId>
   <BitLockerKey>*******</BitLockerKey>
   <ManifestFile>\DriveManifest.xml</ManifestFile>
   <ManifestHash>D863FE44F861AE0DA4DCEAEEFFCCCE68</ManifestHash> </Drive>
[EndValue]
SaveCommandOutput: Completed
[EndUpdateRecord]
```

### <a name="sample-journal-file-jrn-for-session-that-records-the-trail-of-sessions"></a>Oturumları izi kayıtları oturumu için örnek günlük dosyası (JRN)

```
[BeginUpdateRecord][2016/11/02 18:24:14.735][Type:NewJournalFile]
VocabularyVersion: 2013-02-01
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.749][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
LogDirectory: F:\logs
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.754][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
StorageAccountKey: *******
[EndUpdateRecord]
```

## <a name="faq"></a>SSS

### <a name="general"></a>Genel

#### <a name="what-is-waimportexport-tool"></a>WAImportExport Aracı nedir?

İle Microsoft Azure içeri/dışarı aktarma hizmetini kullanabilirsiniz sürücü hazırlama ve Onarım aracını WAImportExport aracıdır. Bir Azure veri merkezine dağıtmayı kalacaklarını sabit sürücüler verileri kopyalamak için bu aracı kullanabilirsiniz. İçe aktarma işi tamamlandıktan sonra bozuldu, eksik olan veya çakışan BLOB diğer BLOB'lar ile onarmak için bu aracı kullanabilirsiniz. Tamamlanan dışa aktarma işleminden sürücüleri aldıktan sonra bozuk veya eksik sürücülerindeki dosyaları onarmak için bu aracı kullanabilirsiniz.

#### <a name="how-does-the-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>WAImportExport aracı, birden çok kaynak dizini ve diskleri nasıl çalışır?

Veri boyutu disk boyutundan daha büyükse WAImportExport aracı disklerde veri iyileştirilmiş bir şekilde dağıtın. Birden fazla diske veri kopyalama, paralel veya sıralı olarak yapılabilir. Verileri aynı anda yazılabilir disklerin sayısına bir sınır yoktur. Aracı disk boyutu ve klasör boyutunu temel alan veri dağıtır. En çok disk seçer nesne boyutu için en iyi duruma getirilmiş. Depolama hesabı karşıya verilerinin için belirtilen dizin yapısını Yakınsanan.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>WAImportExport aracının önceki sürümünü nereden bulabilirim?

WAImportExport aracı WAImportExport V1 aracı olan tüm işlevler içerir. WAImportExport aracı, birden fazla kaynağını belirtin ve birden çok sürücü yazmak kullanıcıların sağlar. Ayrıca, bir içinden verileri tek bir CSV dosyası olarak kopyalanması gereken birden çok kaynak konumlarını kolayca yönetebilirsiniz. Ancak, durumda SAS desteği veya tek bir disk için [yükleyebilir WAImportExport V1 aracı] tek kaynak kopyalamak istiyorsanız gerekir (http://go.microsoft.com/fwlink/?LinkID=301900&amp;clcid = 0x409) ve başvurulacak [WAImportExport V1 başvuru](storage-import-export-tool-how-to-v1.md) WAImportExport V1 kullanımı ile ilgili Yardım.

#### <a name="what-is-a-session-id"></a>Bir oturum kimliği nedir?

Aracı kopya oturumu bekliyor (/ kimliği) hedefi verileri birden çok diskte yaymak için aynı olması için parametre. Kopya oturumu aynı adı koruma verileri bir veya birden çok kaynak konumlarını bir veya birden çok hedef diskleri/dizinler içine kopyalamak kullanıcıyı etkinleştirir. Aynı oturum kimliği bakımı burada en son ne zaman bırakıldı dosyaların kopyasını seçmek araç sağlar.

Ancak, aynı kopya oturumu farklı depolama hesaplarına veri almak için kullanılamaz.

Kopya oturum adı olduğunda aynı aracı, günlük dosyası birden çok çalıştırmaları arasında (/ logdir) ve depolama hesabı anahtarı (/ sk) de aynı olması bekleniyor.

SessionID harfler, 0 oluşabilir ~ 9, understore (\_), tire (-) ya da karma (#), ve uzunluğu 3 olmalıdır ~ 30.

Örneğin oturum-1 veya oturum #1 veya oturum\_1

#### <a name="what-is-a-journal-file"></a>Bir günlük dosyası nedir?

Dosyaları sabit sürücüsüne kopyalamak için WAImportExport aracını çalıştırın her aracı bir kopya oturumu oluşturur. Kopya oturum durumu günlük dosyasına yazılır. (Örneğin, bir sistem güç kaybı nedeniyle) bir kopyası oturumu kesintiye uğrarsa, Aracı'nı yeniden çalıştırmak ve günlük dosyası komut satırında belirterek ettirilebilir.

Azure içeri/dışarı aktarma aracı ile hazırlama her sabit sürücü için aracı adla bir tek günlük dosyası oluşturulur "&lt;DriveID&gt;.xml" diskten aracı okur sürücüye ilişkili seri numarası olduğu sürücü kimliği. Günlük dosyalarını içeri aktarma işi oluşturmak için sürücülerinizin tümünden gerekir. Günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlama sürdürmek için de kullanılabilir.

#### <a name="what-is-a-log-directory"></a>Bir günlük dizini nedir?

Günlük dizinini ayrıntılı günlükleri gibi geçici dosyaları depolamak için kullanılacak dizini belirtir. Belirtilmezse, geçerli dizin günlük dizini olarak kullanılır. Ayrıntılı günlükleri günlüklerin.

### <a name="prerequisites"></a>Ön koşullar

#### <a name="what-are-the-specifications-of-my-disk"></a>My disk özellikleri nelerdir?

Kopya makineye bağlı bir veya daha fazla boş 2,5 inç veya 3,5 inç SATAII veya III veya SSD sabit disk sürücüleri.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>My makinede BitLocker nasıl etkinleştirebilirim?

Basit şekilde sistem sürücüsünde sağ tıklayarak denetleyebilirsiniz. Özelliği açıksa, Bitlocker seçeneklerini gösterir. Kapalı ise, görmezsiniz.

![BitLocker'ı denetleyin](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Üzerinde bir makale işte [BitLocker'ı etkinleştirme](https://technet.microsoft.com/library/cc766295.aspx)

Bu, makinenize TPM yongası yok mümkündür. Tpm.msc kullanarak bir çıktı alamazsanız sonraki SSS Bölümüne bakın.

#### <a name="how-to-disable-trusted-platform-module-tpm-in-bitlocker"></a>Güvenilir Platform Modülü (TPM) BitLocker'ın devre dışı bırakma?
> [!NOTE]
> Yalnızca varsa hiçbir TPM kendi sunucuları, TPM ilke devre dışı bırakmanız gerekir. Kullanıcının Server'da güvenilir bir TPM varsa TPM devre dışı bırakmak gerekli değildir. 
> 

BitLocker TPM'de devre dışı bırakmak için aşağıdaki adımları gidin:<br/>
1. Başlatma **Grup İlkesi Düzenleyicisi'ni** bir komut istemi gpedit.msc yazarak. Varsa **Grup İlkesi Düzenleyicisi'ni** önce BitLocker'ı etkinleştirmek için kullanılamaz gibi görünüyor. Önceki SSS bakın.
2. Açık **yerel bilgisayar ilkesi &gt; Bilgisayar Yapılandırması &gt; Yönetim Şablonları &gt; Windows bileşenleri&gt; BitLocker Sürücü Şifrelemesi &gt; işletim sistemi sürücüleri**.
3. Düzen **başlangıçta ek kimlik doğrulamasını gerektiren** ilkesi.
4. İlke kümesine **etkin** ve emin olun **izin olmadan BitLocker'ı uyumlu bir TPM** denetlenir.

####  <a name="how-to-check-if-net-4-or-higher-version-is-installed-on-my-machine"></a>.NET 4 veya daha yeni bir sürüm benim makinede yüklü olup olmadığını denetlemek nasıl?

Tüm Microsoft .NET Framework sürümleri şu dizine yüklenir: %windir%\Microsoft.NET\Framework\

Burada aracını çalıştırmak için gerek duyduğu hedef makinedeki yukarıda sözü edilen bölümüne gidin. Klasör adı "v4" ile başlayan arayın. Böyle bir dizin olmaması anlamına gelir, makinenizde .NET 4 yüklü değil. .Net 4, makine kullanarak indirebilirsiniz [Microsoft .NET Framework 4 (Web Yükleyicisi)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Sınırlar

#### <a name="how-many-drives-can-i-preparesend-at-the-same-time"></a>Kaç tane sürücüleri ı hazırlama/aynı anda gönderme?

Aracı hazırlayabilirsiniz disklerin sayısına bir sınır yoktur. Ancak, aracı sürücü harflerini girdi olarak bekliyor. Bu 25 eşzamanlı disk hazırlık sınırlar. Tek bir işi aynı anda en fazla 10 diskleri işleyebilir. 10'dan fazla diskleri aynı depolama hesabındaki hedefleme hazırlıyorsanız, diskler arasında birden çok iş dağıtılabilir.

#### <a name="can-i-target-more-than-one-storage-account"></a>Birden fazla depolama hesabı hedefleyebilir miyim?

Yalnızca bir depolama hesabı, iş başına ve tek bir kopya oturumunda gönderilebilir.

### <a name="capabilities"></a>Özellikler

#### <a name="does-waimportexportexe-encrypt-my-data"></a>WAImportExport.exe verilerimi şifreleyebilir mi?

Evet. BitLocker şifrelemesi etkin ve bu işlemi için gereklidir.

#### <a name="what-will-be-the-hierarchy-of-my-data-when-it-appears-in-the-storage-account"></a>Depolama hesabında görüntülendiğinde ne verilerimi hiyerarşisini olacak?

Veri disklerde dağıtılmış rağmen depolama hesabına karşıya verilerinin veri kümesi CSV dosyasında belirtilen dizin yapısına Yakınsanan.

#### <a name="how-many-of-the-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Kaç tane giriş diskleri kopyalama devam ederken etkin GÇ paralel olarak gerekir?

Aracı verileri girdi dosyaları boyutuna göre giriş diskler arasında dağıtır. Bu, paralel etkin disk sayısı tam giriş veri yapısı üzerinde delends belirtti. Girdi veri kümesi içinde tek tek dosyaların boyutuna bağlı olarak, bir veya daha fazla diski etkin GÇ paralel olarak gösterebilir. Daha fazla ayrıntı için sonraki soruya bakın.

#### <a name="how-does-the-tool-distribute-the-files-across-the-disks"></a>Nasıl aracı dosyaları dağıtmak ve bu da bu disklere?

WAImportExport aracı okur ve tek bir toplu 100000 dosyaların maksimum içeren dosyaları toplu toplu yazar. Bu paralel max 100000 dosyalar yazılabilir anlamına gelir. Birden çok disk 100000 bu dosyaları çoklu sürücülere dağıtılmışsa için aynı anda yazılır. Ancak mi aracı birden çok disk aynı anda yazar veya tek bir disk toplu toplu boyutuna bağlıdır. 10,0000 dosyaların tümünü tek bir sürücüye uyacak şekilde tamamlayabilirseniz örneği için daha küçük dosyalar halinde, aracı için yalnızca bir disk bu toplu işleme sırasında yazacaksınız.

### <a name="waimportexport-output"></a>WAImportExport çıkış

#### <a name="there-are-two-journal-files-which-one-should-i-upload-to-azure-portal"></a>İki günlük dosyası, hangisinin ı Azure portalına yüklemeniz gerekir?

**.xml** -WAImportExport aracıyla hazırlama her sabit sürücü için aracı adla bir tek günlük dosyası oluşturulur `<DriveID>.xml` DriveID aracı diskten okur sürücüye ilişkili seri numarası olduğu. Azure portalında alma işi oluşturmak için sürücülerinizin tüm günlük dosyalarını gerekir. Bu günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlama sürdürmek için de kullanılabilir.

**.jrn** -sonekiyle günlük dosyası `.jrn` bir sabit sürücü için tüm kopyalama oturumlarını durumunu içerir. Ayrıca, içe aktarma işi oluşturmak için gereken bilgileri içerir. Her zaman bir kopya oturum kimliği yanı sıra WAImportExport Aracı'nı çalıştırırken bir günlük dosyası belirtmelisiniz

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma aracı ayarlama](storage-import-export-tool-setup.md)
* [İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama](storage-import-export-tool-setting-properties-metadata-import.md)
* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Sık kullanılan komutlar için hızlı başvuru](storage-import-export-tool-quick-reference.md) 
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Bir dışarı aktarma işini onarma](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
