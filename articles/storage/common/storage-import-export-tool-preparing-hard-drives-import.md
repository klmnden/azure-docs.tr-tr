---
title: Sabit sürücüleri için Azure içeri/dışarı aktarma içeri aktarma işine hazırlama | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için bir içeri aktarma işi oluşturma WAImportExport Aracı'nı kullanarak sabit sürücüler hazırlamayı öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 777e0aac46dbffb1e491874b5889667a888aadf5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478524"
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlama

İle kullanabileceğiniz sürücü hazırlama ve Onarım aracını WAImportExport araçtır [Microsoft Azure içeri/dışarı aktarma hizmeti](../storage-import-export-service.md). Bir Azure veri merkezine gönderin olacak sabit sürücüleri veri kopyalamak için bu aracı kullanabilirsiniz. İçeri aktarma işi tamamlandıktan sonra bozuldu eksik olduğu ya da çakışan tüm blobların diğer bloblarla onarmak için bu aracı kullanabilirsiniz. Tamamlanan dışarı aktarma işleminden sürücüleri aldıktan sonra bozuk veya eksik sürücülerindeki dosyaları onarmak için bu aracı kullanabilirsiniz. Bu makalede, biz bu aracın kullanımı gidin.

## <a name="prerequisites"></a>Önkoşullar

### <a name="requirements-for-waimportexportexe"></a>WAImportExport.exe gereksinimleri

- **Makine Yapılandırması**
  - Windows 7, Windows Server 2008 R2 veya daha yeni bir Windows işletim sistemi
  - .NET framework 4 yüklü olması gerekir. Bkz: [SSS](#faq) nasıl .NET Framework makinede yüklü olup olmadığını denetleyin.
- **Depolama hesabı anahtarı** -depolama hesabı için hesap anahtarları en az biri gerekir.

### <a name="preparing-disk-for-import-job"></a>Disk içeri aktarma işine hazırlama

- **BitLocker -** BitLocker WAImportExport aracın çalıştığı makinede etkinleştirilmesi gerekir. Bkz: [SSS](#faq) için BitLocker'ı etkinleştirme.
- **Diskleri** WAImportExport aracın çalıştığı makineden erişilebilir. Bkz: [SSS](#faq) disk belirtimi için.
- **Kaynak dosyaları** -bir ağ paylaşımına veya yerel bir sabit sürücü üzerinde olup almayı planladığınız dosyaların kopyasını makineden erişilebilir olması gerekir.

### <a name="repairing-a-partially-failed-import-job"></a>Kısmen başarısız olan içeri aktarma işini onarma

- **Kopyalama günlük dosyası** Azure içeri/dışarı aktarma hizmeti, depolama hesabı ve Disk arasında veri kopyalar. olduğunda oluşturulur. Bu, hedef depolama hesabında yer alır.

### <a name="repairing-a-partially-failed-export-job"></a>Kısmen başarısız dışarı aktarma işini onarma

- **Kopyalama günlük dosyası** Azure içeri/dışarı aktarma hizmeti, depolama hesabı ve Disk arasında veri kopyalar. olduğunda oluşturulur. Kaynak depolama hesabınızda bulunan.
- **Bildirim dosyası** -Microsoft tarafından döndürülen dışarı aktarılan sürücüde [isteğe bağlı] Located.

## <a name="download-and-install-waimportexport"></a>WAImportExport yükleyip

İndirme [WAImportExport.exe en son sürümünü](https://www.microsoft.com/download/details.aspx?id=55280). Sıkıştırılmış içerik bilgisayarınızdaki bir dizine ayıklayın.

Sonraki göreviniz, CSV dosyaları oluşturmaktır.

## <a name="prepare-the-dataset-csv-file"></a>Veri kümesi CSV dosyasını hazırlama

### <a name="what-is-dataset-csv"></a>Veri kümesi CSV nedir

Veri kümesi CSV dosyası/DataSet bayrak değeri bir dizin listesi ve/veya hedef sürücülere kopyalanacak dosyaların bir listesini içeren bir CSV dosyası şeklindedir. İçeri aktarma işine oluşturmanın ilk adımı, hangi dizinlerin ve dosyaların içeri aktarmak için oluşturacağınız belirlemektir. Bu, bir dizin listesi, benzersiz dosyaların listesini veya bu iki bir birleşimi olabilir. Bir dizin eklendiğinde, dizinde ve alt dizinlerinde tüm dosyaları içeri aktarma işinin bir parçası olur.

Her dizin veya içeri aktarılacak dosya için bir hedef sanal dizin veya Azure Blob hizmetindeki blob belirlemeniz gerekir. Bu hedefler giriş WAImportExport aracı olarak kullanır. Dizinleri eğik çizgi karakteriyle ayrılmış "/".

Aşağıdaki tabloda, blob hedefleri bazı örnekler gösterilmektedir:

| Kaynak dosya veya dizin | Hedef blob veya sanal dizin |
| --- | --- |
| H:\Video | https://mystorageaccount.blob.core.windows.net/video |
| H:\Photo | https://mystorageaccount.blob.core.windows.net/photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.blob.core.windows.net/music |

### <a name="sample-datasetcsv"></a>Örnek dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Veri kümesi CSV dosyası alanları

| Alan | Açıklama |
| --- | --- |
| BasePath | **[Gerekli]**<br/>Bu parametrenin değeri, içeri aktarılacak verilerin bulunduğu kaynak temsil eder. Aracı, bu yolu altında bulunan tüm veriler yinelemeli olarak kopyalama olur.<br><br/>**İzin verilen değerler**: Bu, yerel bilgisayardaki geçerli bir yol veya geçerli bir paylaşım yolu olmalıdır ve kullanıcı tarafından erişilebilir olması gerekir. Dizin yolu mutlak bir yol (göreli bir yol değil) olmalıdır. Yol ile bitiyorsa "\\", başka bir dizin olmadan biten bir yolunu temsil ettiği"\\" bir dosyayı temsil eder.<br/>Hiçbir regex, bu alana izin verilir. Yol boşluklar içeriyorsa, yerleştirecek "".<br><br/>**Örnek**: "c:\Directory\c\Directory\File.txt"<br>"\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Gerekli]**<br/> Windows Azure depolama hesabınızdaki hedef sanal dizin yolu. Sanal dizin zaten mevcut veya olabilir. Yoksa, içeri/dışarı aktarma hizmeti oluşturur.<br/><br/>Hedef sanal dizinler veya BLOB'ları belirtilirken geçerli kapsayıcı adları kullandığınızdan emin olun. Kapsayıcı adları küçük harf olması gerektiğini unutmayın. Kapsayıcı adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta verileri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Yalnızca bir kök belirtilen, kaynak dizin yapısı hedef blob kapsayıcısında çoğaltılır. Farklı bir dizin yapısı bir kaynakta, birden çok csv eşleme satırlarını isteniyorsa<br/><br/>Bir kapsayıcı veya bir müzik/70s gibi blob öneki belirtebilirsiniz /. Hedef dizin kapsayıcı adını, ardından eğik çizgi ile başlamalıdır. "/" ve isteğe bağlı olarak ile biten bir sanal blob dizin içerebilir "/".<br/><br/>Hedef kapsayıcı kök kapsayıcı olduğunda $root olarak eğik çizgi dahil kök kapsayıcı açıkça belirtmeniz gerekir /. Hedef dizin kök kapsayıcı olduğunda kök kapsayıcıdaki blobları içeremez beri "/" adlarında, ve kaynak dizinde tüm alt dizinler kopyalanmaz.<br/><br/>**Örnek**<br/>Hedef blob yolu ise https://mystorageaccount.blob.core.windows.net/video, bu alanın değeri video /  |
| BlobType | **[İsteğe bağlı]**  blok &#124; sayfası<br/>Şu anda içeri/dışarı aktarma hizmeti, BLOB'ları 2 tür destekler. Sayfa blobları ve blok BlobsBy varsayılan tüm dosyaları blok Blobları olarak içeri aktarılacak. Ve \*.vhd ve \*sayfası BlobsThere blok blobu ve sayfa blob boyutu izin verilen üst sınırı olarak .vhdx içeri aktarılacak. Bkz: [depolama ölçeklenebilirlik hedefleri](storage-scalability-targets.md) daha fazla bilgi için.  |
| Değerlendirme | **[İsteğe bağlı]**  Yeniden Adlandır &#124; Hayır üzerine &#124; üzerine yaz <br/> Bu alan kopyalama davranışı sırasında içeri aktarma yani belirtir. ne zaman veri depolama hesabına diskten karşıya yükleniyor. Kullanılabilir seçenekler şunlardır: yeniden adlandırma&#124;üzerine&#124;Hayır üzerine yazın. "Hiçbir şey, belirtilen yeniden adlandırmak için" varsayılan olarak. <br/><br/>**Yeniden adlandırma**: Aynı ada sahip bir nesne varsa, hedef olarak bir kopyasını oluşturur.<br/>Üzerine yaz: dosyanın daha yeni bir dosya ile üzerine yazar. Son değiştirilme WINS içeren dosya.<br/>**Hayır üzerine**: Dosya zaten varsa yazılırken atlamalar var.|
| MetadataFile | **[İsteğe bağlı]** <br/>Bu alan için değer bir nesne meta verilerini korumak ya da özel bir meta veri sağlamak gerekiyorsa, sağlanabilir meta veri dosyasıdır. Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için |
| PropertiesFile | **[İsteğe bağlı]** <br/>Hedef BLOB'ları için özellik dosyası yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>InitialDriveSet veya AdditionalDriveSet CSV dosyasını hazırlama

### <a name="what-is-driveset-csv"></a>CSV driveset nedir

Aracı düzgün listesi disklerin hazırlanması seçebilir böylece sürücü harflerini eşlenmiş disklerin listesini içeren bir CSV dosyası /InitialDriveSet veya /AdditionalDriveSet bayrağı değeridir. Veri boyutu tek disk boyutundan daha büyükse WAImportExport aracın birden çok disklerde bulunan en iyi duruma getirilmiş bir yolla bu CSV dosyası olarak kayıtlı verileri dağıtacak.

Verileri tek bir oturumda yazılabilir disk sayısı sınırlı değildir. Aracı veri disk boyutu ve klasör boyutu göre dağıtır. En çok disk seçer nesne boyutu için en iyi duruma getirilmiş. Depolama hesabına yüklediniz verilerinin geri veri kümesi dosyası içinde belirtilen dizin yapısına yakınsanmış. Bir CSV driveset oluşturmak için aşağıdaki adımları izleyin.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Temel birim oluşturun ve sürücü harfini ata

Temel birim oluşturmak ve "daha basit bölüm oluşturma sırasında" yönergelerini izleyerek bir sürücü harfi atamak için [Disk Yönetimi'ne genel bakış](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Örnek InitialDriveSet ve AdditionalDriveSet CSV dosyası

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Driveset CSV dosyası alanları

| Alanlar | Değer |
| --- | --- |
| DriveLetter | **[Gerekli]**<br/> Hedef ve bir sürücü harfi atanmış basit bir NTFS biriminde olduğundan araç sağlanan her sürücü.<br/> <br/>**Örnek**: R veya r |
| FormatOption | **[Gerekli]**  Biçimi &#124; AlreadyFormatted<br/><br/> **Biçim**: Bunun belirtilmesi diskteki tüm verilerin biçimlendirir. <br/>**AlreadyFormatted**: Aracı, bu değeri belirtildiğinde biçimlendirme atlar. |
| SilentOrPromptOnFormat | **[Gerekli]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: Bu değer sağlama aracı sessiz modda çalıştırmak kullanıcıyı etkinleştirir. <br/>**PromptOnFormat**: Aracı eylem her biçimi gerçekten yönelik olduğunu onaylamak için girmesini ister.<br/><br/>Ayarlı değil, komut iptal etmek ve hata iletisini görüntüler varsa: "SilentOrPromptOnFormat için geçersiz değer: yok" |
| Şifreleme | **[Gerekli]**  Şifrelemek &#124; AlreadyEncrypted<br/> Bu alanın değeri, şifrelemek için hangi disk ve hangi değil karar verir. <br/><br/>**şifreleme**: Aracı sürücüyü biçimlendirir. "FormatOption" alanının değeri "Format" ise bu değer, "Şifrele" olması gereklidir. Bu durumda "AlreadyEncrypted" belirtilirse, "Biçimi belirtildiğinde şifrele de belirtilmelidir" bir hatayla sonuçlanır.<br/>**AlreadyEncrypted**: Aracı "ExistingBitLockerKey" alanında sağlanan BitLockerKey kullanarak sürücü şifresini çözer. "FormatOption" alanının değeri "AlreadyFormatted" ise, ardından bu değeri "Şifrele" veya "AlreadyEncrypted" olabilir |
| ExistingBitLockerKey | **[Gerekli]**  "Şifreleme" alanının değeri "AlreadyEncrypted" ise<br/> Bu alan belirli bir diskle ilişkili BitLocker anahtarı değerdir. <br/><br/>Bu alan, "Şifrele" "Şifreleme" alanının değeri ise, boş bırakılmalıdır.  Bu durumda BitLocker anahtarı belirtilmezse, "BitLocker anahtarı belirtilmemesi gerekir" bir hatayla sonuçlanır.<br/>  **Örnek**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Disk içeri aktarma işine hazırlama

Sürücüleri içeri aktarma işine hazırlamak için WAImportExport aracıyla çağrı **PrepImport** komutu. Hangi parametreler dahil bağlı olarak bu ilk kopyalama oturumun olup olmadığını veya bir sonraki kopyalama oturumu.

### <a name="first-session"></a>İlk oturum

Tek/birden çok Disk (bağlı olarak hangi CSV dosyasında belirtilir) WAImportExport aracına PrepImport komut dizinleri ve/veya yeni bir kopya oturumu dosyaları kopyalamak ilk kopyalama oturumu için bir Single/Multiple dizine kopyalamak için ilk kopya oturumu:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] /DataSet:<dataset.csv>
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Sonraki oturumunda veri ekleme

Sonraki kopyalama oturumunda, başlangıç parametreleri belirtmek gerekmez. Önceki oturumu kaldığı hatırlamak aracı için sırayla aynı günlük dosyası kullanmanız gerekir. Kopyalama oturum durumunu günlük dosyasına yazılır. Ek dizinleri ve/veya dosyaları kopyalamak bir sonraki kopyalama oturumu için söz dizimi şu şekildedir:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-to-latest-session"></a>En son oturumuna sürücü ekleme

Veri InitialDriveset içinde belirtilen sürücülere değil sığıyorsa, ek sürücüler aynı kopyasını oturumuna eklemek için bir araç kullanabilirsiniz. 

> [!NOTE]
> Oturum kimliği, önceki bir oturum kimliği eşleşmesi gerekir. Günlük dosyası önceki oturumu belirtilen hesapla eşleşmelidir.
> 
> ```
> WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
> ```

**Örnek:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-the-latest-session"></a>Son oturumu durdur:

Kopyalama oturumu kesintiye ve (örneğin, bir kaynak dizin erişilemez kanıtlandı varsa) devam etmek mümkün değildir, böylece geri alınabilir ve yeni kopya oturumları başlatılabilir geçerli oturumu iptal gerekir:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Yalnızca son kopyalama oturumu, anormal olursa iptal. Bir sürücü için ilk kopyalama oturumu durdurulamıyor unutmayın. Bunun yerine yeni bir günlük dosyasıyla kopyalama oturumunu yeniden başlatmalısınız.

### <a name="resume-a-latest-interrupted-session"></a>Bir son kesintiye oturumu Sürdür

Herhangi bir nedenle bir kopya oturumu kesintiye uğrarsa, yalnızca belirtilen günlük dosyası ile aracını çalıştırarak devam edebilir:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Örnek:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> Bir kopya oturumu sürdürdüğünüzde, kaynak veri dosyaları ve dizinleri ekleyerek veya kaldırarak dosyaları değiştirmeyin.

## <a name="waimportexport-parameters"></a>WAImportExport parametreleri

| Parametreler | Açıklama |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Gerekli**<br/> Günlük dosyası yolu. Günlük dosyası sürücüleri kümesini izler ve bu sürücüler hazırladığınız sırada ilerleme durumunu kaydeder. Günlük dosyası her zaman belirtilmesi gerekir.  |
|     / LOGDIR:&lt;LogDirectory&gt;  | **İsteğe bağlı**. Günlük dizini.<br/> Ayrıntılı günlük dosyası, hem de bazı geçici dosyaları bu dizine yazılır. Aksi durumda belirtilmezse, geçerli dizin günlük dizini kullanılır. Günlük dizini için aynı günlük dosyası yalnızca bir kez belirtilebilir.  |
|     /id:&lt;oturum kimliği&gt;  | **Gerekli**<br/> Oturum kimliği bir kopya oturumunu tanımlamak için kullanılır. Bu, kesintiye kopyalama oturumunun doğru kurtarma sağlamak için kullanılır.  |
|     / ResumeSession  | İsteğe bağlı. Son kopyalama oturumu anormal, bu parametre, oturumu sürdürme belirtilebilir.   |
|     / AbortSession  | İsteğe bağlı. Son kopyalama oturumu anormal, bu parametre oturumunu durdurmak için belirtilebilir.  |
|     /sn:&lt;StorageAccountName&gt;  | **Gerekli**<br/> Yalnızca, RepairImport ve RepairExport için de geçerlidir. Depolama hesabı adı.  |
|     /sk:&lt;StorageAccountKey&gt;  | **Gerekli**<br/> Depolama hesabı anahtarı. |
|     /InitialDriveSet:&lt;driveset.csv&gt;  | **Gerekli** ilk kopyalama oturumun çalışırken<br/> Hazırlamak için sürücülerin listesini içeren bir CSV dosyası.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Gerekli**. Sürücüleri geçerli kopyalama oturuma eklerken. <br/> Eklenecek ek sürücüler listesini içeren bir CSV dosyası.  |
|      / r:&lt;RepairFile&gt; | **Gerekli** RepairImport ve RepairExport için yalnızca geçerlidir.<br/> Onarım ilerleme durumunu izlemek için dosyanın yolu. Her sürücü bir ve yalnızca bir onarım dosyası olması gerekir.  |
|     / d:&lt;TargetDirectories&gt; | **Gerekli**. Yalnızca, RepairImport ve RepairExport için de geçerlidir. RepairImport için onarma; bir veya daha fazla noktalı virgülle ayrılmış dizinler RepairExport, onarmak için bir dizin için örneğin directory sürücünün kök.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Gerekli** RepairImport ve RepairExport için yalnızca geçerlidir. Sürücüyü Kopyala günlük dosyası yolu (ayrıntılı veya hata).  |
|     / Manıfestfıle:&lt;DriveManifestFile&gt; | **Gerekli** RepairExport yalnızca uygulanabilir.<br/> Sürücü bildirim dosyasının yolu.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **İsteğe bağlı**. Yalnızca, RepairImport için de geçerlidir.<br/> Dosya yolları (sekmeyle sınırlı) gerçek dosya konumları için sürücünün köküne eşlemelerini içeren dosyanın yolu. İlk olarak belirtildiğinde, dosya yolları boş hedefleri ile TargetDirectories, erişim reddedildi, geçersiz bir ada olarak bulunmayan veya birden fazla dizinde oldukları anlamına gelir doldurulur. Yol Haritası el ile doğru hedef yolları içerecek şekilde düzenlendi ve doğru dosya yollarını çözmek için aracı yeniden belirtildi.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Gerekli**. Yalnızca, PreviewExport için de geçerlidir.<br/> XML yolu içeren blob yollarının listesini dosya veya yol önekleri dışarı aktarılacak bloblar için blob. Dosya biçimi, işi Put işlemi içeri/dışarı aktarma hizmeti REST API blob listesi blob biçiminde ile aynıdır.  |
|     / DriveSize:&lt;DriveSize&gt; | **Gerekli**. Yalnızca, PreviewExport için de geçerlidir.<br/>  Dışarı aktarma için kullanılacak sürücüleri boyutu. Örneğin, 500 GB, 1,5 TB. Not: 1 GB = 1.000.000.000 bytes1 TB 1,000,000,000,000 bayt =  |
|     /DataSet:&lt;dataset.csv&gt; | **Gerekli**<br/> Bir dizin listesi ve/veya hedef sürücülere kopyalanacak dosyaların bir listesini içeren bir CSV dosyası.  |
|     /silentmode  | **İsteğe bağlı**.<br/> Belirtilmezse, sürücüler gereksinimi anımsatmak ve devam etmek için onay gerekir.  |

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

### <a name="sample-journal-file-jrn-for-session-that-records-the-trail-of-sessions"></a>Oturumlarının izleme kayıtları oturumu için örnek günlük dosyası (JRN)

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

Microsoft Azure içeri/dışarı aktarma hizmeti ile kullanabilir sürücü hazırlık ve Onarım aracını WAImportExport aracıdır. Bir Azure veri merkezine gönderin olacak sabit sürücüleri veri kopyalamak için bu aracı kullanabilirsiniz. İçeri aktarma işi tamamlandıktan sonra bozuldu eksik olduğu ya da çakışan tüm blobların diğer bloblarla onarmak için bu aracı kullanabilirsiniz. Tamamlanan dışarı aktarma işleminden sürücüleri aldıktan sonra bozuk veya eksik sürücülerindeki dosyaları onarmak için bu aracı kullanabilirsiniz.

#### <a name="how-does-the-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Birden çok kaynak dizini ve diskleri WAImportExport Aracı'nı nasıl çalışır?

Veri boyutu disk boyutundan daha büyükse WAImportExport aracı disklerde veri en iyi duruma getirilmiş bir şekilde dağıtın. Veri kopyalama işlemini birden çok disk paralel veya sıralı olarak yapılabilir. Verileri aynı anda yazılıp disk sayısı sınırlı değildir. Aracı veri disk boyutu ve klasör boyutu göre dağıtır. En çok disk seçer nesne boyutu için en iyi duruma getirilmiş. Depolama hesabına yüklediniz verilerinin belirtilen dizin yapısına yakınsanmış.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>WAImportExport Aracı'nın önceki bir sürümü nereden edinebilirim?

WAImportExport aracı WAImportExport V1 aracı olan tüm işlevleri içerir. Kullanıcıların birden çok kaynaktan belirtin ve birden çok sürücülerine yazma WAImportExport aracı sağlar. Ayrıca, bir kolayca içinden verileri tek bir CSV dosyası olarak kopyalanması gereken birden çok kaynak konumları yönetebilirsiniz. Ancak, SAS desteği veya tek bir disk için tek kaynak kopyalamak istediğiniz gerektiği durumlarda, yapabilecekleriniz [WAImportExport V1 Aracı'nı indirme](https://go.microsoft.com/fwlink/?LinkID=301900&amp;clcid=0x409) başvurun [WAImportExport V1 başvuru](storage-import-export-tool-how-to-v1.md) WAImportExport V1 kullanımı ile ilgili Yardım için .

#### <a name="what-is-a-session-id"></a>Oturum Kimliği nedir?

Kopya oturumu aracı bekliyor (/ kimliği) amacı, verileri birden çok diskte yaymak için ise aynı parametre. Kopyalama oturumun aynı adı Bakımı bir veya birden çok hedef diskler/dizin bir veya birden çok kaynak konumlara veri kopyalamak kullanıcıyı etkinleştirir. Aynı oturum kimliğine koruma, son kaldığı dosyaların kopyasını alması araç sağlar.

Ancak, aynı kopya oturumu farklı depolama hesaplarına veri almak için kullanılamaz.

Kopyalama oturum adı olduğunda aynı günlük dosyası aracı birden fazla çalıştırma sonucunda (/ logdir) ve depolama hesabı anahtarı (/ sk) aynı olması beklenir.

SessionID harfler, 0 oluşabilir ~ 9, alt çizgi (\_), tire (-) ya da karma (#), ve uzunluğu 3 olmalıdır yaklaşık 30.

Örneğin oturum-1 veya oturumu #1 veya oturumu\_1

#### <a name="what-is-a-journal-file"></a>Bir günlük dosyası nedir?

Sabit sürücüye, dosyaları kopyalamak için WAImportExport aracı her çalıştırdığınızda araç bir kopya oturumu oluşturur. Kopyalama oturum durumunu günlük dosyasına yazılır. (Örneğin, bir sistem güç kaybı nedeniyle) bir kopyasını oturumu kesintiye uğrarsa, Aracı'nı yeniden çalıştırmak ve günlük dosyası komut satırında belirterek sürdürülebilir.

Azure içeri/dışarı aktarma aracı ile hazırlama her sabit sürücü için aracı ada sahip bir tek bir günlük dosyası oluşturur "&lt;DriveID&gt;.xml" seri numarasını sürücünün diskten aracı okur ile ilişkili olduğu sürücü kimliği. Günlük dosyaları içeri aktarma işi oluşturma sürücülerinizin tümünden gerekir. Günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlığını sürdürmek için de kullanılabilir.

#### <a name="what-is-a-log-directory"></a>Bir günlük dizini nedir?

Günlük dizini ayrıntılı günlükleri gibi geçici dosyaları depolamak için kullanılacak bir dizini belirtir. Belirtilmezse, geçerli dizin günlük dizini kullanılır. Ayrıntılı günlükler günlüklerdir.

### <a name="prerequisites"></a>Önkoşullar

#### <a name="what-are-the-specifications-of-my-disk"></a>Diskimi özellikleri nelerdir?

Kopyalama makineye bağlı bir veya daha fazla boş 2.5 inç veya 3,5 inçlik SATAII veya III veya SSD sabit disk sürücüleri.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>BitLocker makinemde nasıl etkinleştirebilirim?

Denetlenecek basit sistem sürücüsünde sağ tıklayarak yoludur. Özelliği etkinleştirilmişse, BitLocker seçeneklerini gösterir. Kapalı ise, görmezsiniz.

![BitLocker'ı denetleme](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Üzerinde bir makale işte [BitLocker'ı etkinleştirme](https://technet.microsoft.com/library/cc766295.aspx)

Makinenizde TPM yongasına sahip olmadığını mümkündür. Tpm.msc kullanarak bir çıktı alamazsanız İleri SSS bakın.

#### <a name="how-to-disable-trusted-platform-module-tpm-in-bitlocker"></a>Güvenilir Platform Modülü (TPM), BitLocker'ı devre dışı bırakma?
> [!NOTE]
> Yalnızca ise hiçbir TPM, sunucuları, TPM ilke devre dışı bırakmanız gerekir. Kullanıcının Server'da güvenilen TPM yoksa TPM devre dışı bırakmak gerekli değildir. 
> 

BitLocker TPM'de devre dışı bırakmak için aşağıdaki adımları izleyerek gidin:<br/>
1. Başlatma **Grup İlkesi Düzenleyicisi** gpedit.msc yazarak bir komut istemi. Varsa **Grup İlkesi Düzenleyicisi** önce BitLocker'ı etkinleştirmek için kullanılamıyor gibi görünüyor. Önceki SSS Bölümüne bakın.
2. Açık **yerel bilgisayar ilkesi &gt; Bilgisayar Yapılandırması &gt; Yönetim Şablonları &gt; Windows bileşenleri&gt; BitLocker Sürücü Şifrelemesi &gt; işletim sistemi sürücüleri**.
3. Düzen **başlangıçta ek kimlik doğrulaması gerektiren** ilkesi.
4. İlke kümesine **etkin** emin **uyumlu TPM'siz BitLocker izin** denetlenir.

####  <a name="how-to-check-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Benim makinemde .NET 4 veya üzeri bir sürüm yüklü olup olmadığını denetlemek nasıl?

Tüm Microsoft .NET Framework sürümleri aşağıdaki dizine yüklenir: %windir%\Microsoft.NET\Framework\

Aracı çalıştırmak için gereken yere, hedef makinenizde yukarıda belirtilen bölümüne gidin. "V4" ile başlayan klasör adı arayın. Böyle bir dizin olmaması, .NET 4, makinenizde yüklü anlamına gelir. .NET 4 kullanarak makinenize indirebileceğiniz [Microsoft .NET Framework 4 (Web Yükleyicisi)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Limits

#### <a name="how-many-drives-can-i-preparesend-at-the-same-time"></a>Kaç sürücüleri miyim hazırlama/aynı anda gönderme?

Aracı hazırlayabilirsiniz disk sayısı sınırlı değildir. Ancak, aracı sürücü harflerini girdi olarak bekler. Bu, 25 eşzamanlı disk hazırlama için kısıtlar. Tek bir iş, aynı anda en fazla 10 disk işleyebilir. 10'dan fazla diskleri aynı depolama hesabı hedef belirleme hazırlıyorsanız, diskleri birden çok iş arasında dağıtılabilir.

#### <a name="can-i-target-more-than-one-storage-account"></a>Birden fazla depolama hesabı hedefleyebilirim?

Yalnızca bir depolama hesabı başına iş ve tek kopyası oturumunda gönderilebilir.

### <a name="capabilities"></a>Özellikler

#### <a name="does-waimportexportexe-encrypt-my-data"></a>WAImportExport.exe verilerimi şifreleyebilir mi?

Evet. BitLocker şifrelemesi etkin ve bu işlemi için gereklidir.

#### <a name="what-will-be-the-hierarchy-of-my-data-when-it-appears-in-the-storage-account"></a>Depolama hesabında göründüğünde ne verilerimi hiyerarşisini olacaktır?

Veri diskleri arasında dağıtıldığı olsa da, depolama hesabına yüklediniz verilerinin veri kümesi CSV dosyasında belirtilen dizin yapısına yakınsanmış.

#### <a name="how-many-of-the-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Kaç tane giriş diskleri kopyalama devam ederken, etkin GÇ paralel olarak gerekir?

Aracı verileri girdi dosyalarının boyutuna göre giriş diskler arasında dağıtır. Bu, paralel etkin disk sayısı, giriş veri yapısı üzerinde tamamen bağlıdır belirtti. Giriş veri kümesinde tek tek dosyaların boyutuna bağlı olarak, bir veya daha fazla disk, paralel olarak etkin GÇ'de gösterebilir. Daha fazla ayrıntı için bir sonraki soruya bakın.

#### <a name="how-does-the-tool-distribute-the-files-across-the-disks"></a>Nasıl araç dosyaları dağıtmak ve bu da disklerde?

WAImportExport aracı okur ve tek bir toplu 100000 dosyaların maksimum içeren dosyaları toplu batch yazar. Bu, paralel max 100000 dosyaları yazılabilir anlamına gelir. Bu 100000 dosyaları çoklu sürücülere dağıtılmışsa birden fazla disk aynı anda yazılır. Ancak mi aracı için birden fazla disk aynı anda yazar veya tek bir diske toplu işin toplam boyutuna bağlıdır. 10,0000 dosyalarının tümünü tek bir sürücü uygun olanağınız varsa örneği için daha küçük dosyalar, aracı için yalnızca bir disk bu toplu işlenmesi sırasında yazılacaktır.

### <a name="waimportexport-output"></a>WAImportExport çıkış

#### <a name="there-are-two-journal-files-which-one-should-i-upload-to-azure-portal"></a>İki günlük dosyası, hangi Azure portalına karşıya?

**.xml** -WAImportExport aracıyla hazırlama her sabit sürücü için aracı ada sahip bir tek bir günlük dosyası oluşturur `<DriveID>.xml` DriveID sürücünün diskten aracı okur ile ilişkili seri numarası olduğu. Azure portalında içeri aktarma işi oluşturma sürücülerinizin tüm günlük dosyaları gerekir. Bu günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlığını sürdürmek için de kullanılabilir.

**.jrn** -sonekine sahip günlük dosyası `.jrn` sabit sürücü için tüm kopyalama oturumlarını durumunu içerir. Ayrıca, içeri aktarma işi oluşturmak için gereken bilgileri içerir. Her zaman bir günlük dosyası kopyalama oturum kimliği yanı sıra WAImportExport aracını çalıştırırken belirtmeniz gerekir

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma Aracı'nı ayarlama](storage-import-export-tool-setup.md)
* [İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama](storage-import-export-tool-setting-properties-metadata-import.md)
* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Sık kullanılan komutlar için hızlı başvuru](storage-import-export-tool-quick-reference.md) 
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Bir dışarı aktarma işini onarma](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
