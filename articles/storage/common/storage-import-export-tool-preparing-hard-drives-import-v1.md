---
title: Azure içeri/dışarı aktarma içeri aktarma işi için - v1 sabit sürücüleri hazırlama | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için bir içeri aktarma işi oluşturma WAImportExport v1 aracını kullanarak sabit sürücüler hazırlamayı öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 03b504524b2f489f1ee042c6e825ccffe0a60bb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478479"
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlama
Bir veya daha fazla sabit sürücüleri içeri aktarma işine hazırlamak için aşağıdaki adımları izleyin:

- Blob hizmetinde içeri aktarmak için veri belirle

- Hedef sanal dizinleri ve blobları Blob hizmetinde belirleyin

- Kaç tane sürücüleri ihtiyacınız olacak belirleme

- Her sabit sürücülerinizi veri kopyalama

  Örnek iş akışı için bkz: [içeri aktarma işi için sabit sürücüler hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-the-data-to-be-imported"></a>İçeri aktarılacak veri tanımlama
 İçeri aktarma işine oluşturmanın ilk adımı, hangi dizinlerin ve dosyaların içeri aktarmak için oluşturacağınız belirlemektir. Bu, bir dizin listesi, benzersiz dosyaların listesini veya bu iki bir birleşimi olabilir. Bir dizin eklendiğinde, dizinde ve alt dizinlerinde tüm dosyaları içeri aktarma işinin bir parçası olur.

> [!NOTE]
>  Bir üst dizin eklendiğinde birlikte yinelemeli olarak alt dizinler olduğundan, yalnızca üst dizini belirtin. Ayrıca alt dizinlerinde hiçbirini belirtmeyin.
>
>  Şu anda, Microsoft Azure içeri/dışarı aktarma aracı aşağıdaki sınırlamalara sahiptir: bir dizin bir sabit sürücü içerebilir daha fazla veri içeriyorsa, daha sonra dizini daha küçük dizinlere bozuk gerekir. Örneğin, bir dizin 2,5 TB veri içeriyor ve sabit sürücü kapasite yalnızca 2 TB ise, ardından, 2,5 TB dizin daha küçük dizinlere ayırmanız gerekir. Bu sınırlama, Aracı'nın sonraki bir sürümde ele alınacaktır.

## <a name="identify-the-destination-locations-in-the-blob-service"></a>Blob hizmetinde hedef konumları tanımlayın
 Her dizin veya içeri aktarılacak dosya için bir hedef sanal dizin veya Azure Blob hizmetindeki blob tanımlamak gerekir. Bu hedefler giriş Azure içeri/dışarı aktarma aracı olarak kullanır. Dizinleri eğik çizgi karakteri ile sınırlı olduğunu unutmayın. "/".

 Aşağıdaki tabloda, blob hedefleri bazı örnekler gösterilmektedir:

|Kaynak dosya veya dizin|Hedef blob veya sanal dizin|
|------------------------------|-------------------------------------------|
|H:\Video|https:\//mystorageaccount.blob.core.windows.net/video|
|H:\Photo|https:\//mystorageaccount.blob.core.windows.net/photo|
|K:\Temp\FavoriteVideo.ISO|https:\//mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https:\//mystorageaccount.blob.core.windows.net/music|

## <a name="determine-how-many-drives-are-needed"></a>Kaç tane sürücüleri belirlemek
 Ardından, belirlemeniz gerekir:

- Verileri depolamak için gereken sabit sürücü sayısı.

- Dizinleri ve/veya her sabit kopyalanacak tek başına dosyalar.

  Sabit sürücüler, aktardığınız verileri depolamak için gereken sayıda olduğundan emin olun.

## <a name="copy-data-to-your-hard-drive"></a>Sabit veri kopyalayın
 Bu bölümde, bir veya daha fazla sabit sürücüler için verilerinizi kopyalamak için Azure içeri/dışarı aktarma Aracı'nı çağırmak açıklar. Azure içeri/dışarı aktarma Aracı'nı arayın, her bir yeni oluşturduğunuz *kopyalama oturumu*. Veri kopyalama her sürücü için en az bir kopya oturumu oluşturduğunuz; Bazı durumlarda, birden fazla kopya oturumu tüm verilerinizi tek bir diske kopyalamak gerekebilir. Birden çok kopyasını oturumu gerekebilir bazı nedenler şunlardır:

-   Kopyaladığınız her sürücü için bir ayrı kopyasını oturumu oluşturmanız gerekir.

-   Bir kopya oturumu sürücüye tek bir dizin ya da tek bir blob kopyalayabilirsiniz. Birden çok dizin, birden çok BLOB'ları veya her ikisinin bir birleşiminde kopyalanıyorsa, birden çok kopyasını oturumları oluşturmak gerekir.

-   Özellikleri ve BLOB'ları içeri aktarma işi kapsamında alınan ayarlanır meta verileri belirtebilirsiniz. Özellikleri veya kopyalama oturum açmak için belirttiğiniz meta veriler bu kopya oturumu tarafından belirtilen tüm bloblara uygulanır. Farklı özellikler veya bazı BLOB meta verilerini belirtmek istiyorsanız, ayrı kopyasını oturumu oluşturmak gerekir. Bkz: [ayarı özellikleri ve meta verileri içeri aktarma işlemi sırasında](storage-import-export-tool-setting-properties-metadata-import-v1.md)daha fazla bilgi için.

> [!NOTE]
>  Bölümünde özetlenen gereksinimleri karşılayan birden çok makine varsa [ayarı oluşturan Azure içeri/dışarı aktarma aracı](storage-import-export-tool-setup-v1.md), her bir makinede bu aracı örneğini çalıştırarak birden çok sabit sürücülere paralel veri kopyalayabilirsiniz.

 Azure içeri/dışarı aktarma aracı ile hazırlama her sabit sürücü için aracı, tek bir günlük dosyası oluşturur. Günlük dosyaları içeri aktarma işi oluşturma sürücülerinizin tümünden gerekir. Günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlığını sürdürmek için de kullanılabilir.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>İçeri aktarma işi için Azure içeri/dışarı aktarma aracı söz dizimi
 Sürücüleri içeri aktarma işine hazırlamak için Azure içeri/dışarı aktarma aracı ile çağrı **PrepImport** komutu. Hangi parametreler dahil bağlı olarak bu ilk kopyalama oturumun olup olmadığını veya bir sonraki kopyalama oturumu.

 İlk kopyalama oturumun bir sürücü için depolama hesabı anahtarını belirtmek için bazı ek parametreler gerektirir; hedef sürücü harfini; Sürücü biçimlendirilmiş olması gerekir; Sürücü şifreli olup olmadığını ve bu durumda, BitLocker anahtarı; ve günlük dizini. Bir dizin veya tek bir dosyayı kopyalamak bir başlangıç kopyası oturumu için söz dizimi şu şekildedir:

 **İlk oturum tek bir dizine kopyalamak için kopyalama**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **İlk oturum tek bir dosya kopyalamak için kopyalama**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 Sonraki kopyalama oturumunda, başlangıç parametreleri belirtmek gerekmez. Bir dizin veya tek bir dosyayı kopyalamak bir sonraki kopyalama oturumu için söz dizimi şu şekildedir:

 **Tek bir dizine kopyalamak için kopyalama sonraki oturumları**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Tek bir dosya kopyalamak için kopyalama sonraki oturumları**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-the-first-copy-session-for-a-hard-drive"></a>İlk kopyalama oturum için bir sabit sürücü için parametreler
 Sabit sürücüye, dosyaları kopyalamak için Azure içeri/dışarı aktarma aracı her çalıştırdığınızda araç bir kopya oturumu oluşturur. Her kopya oturumu tek bir dizin veya tek bir dosyayı bir sabit sürücüye kopyalar. Kopyalama oturum durumunu günlük dosyasına yazılır. (Örneğin, bir sistem güç kaybı nedeniyle) bir kopyasını oturumu kesintiye uğrarsa, Aracı'nı yeniden çalıştırmak ve günlük dosyası komut satırında belirterek sürdürülebilir.

> [!WARNING]
>  Belirtirseniz **/biçimlendirme** ilk kopyalama oturumun parametresi, sürücü biçimlendirilir ve sürücüdeki tüm veriler silinir. Boş sürücüleri yalnızca kopya oturumunuz için kullanmak önerilir.

 Her sürücü için ilk kopyalama oturumu için kullanılan komut sonraki kopyalama oturumları için komutları değerinden farklı parametreler gerektirir. İlk kopyalama oturumu için ek parametreler aşağıdaki tabloda listelenmektedir:

|Komut satırı parametresi|Açıklama|
|-----------------------------|-----------------|
|**/sk:** <StorageAccountKey\>|`Optional.` Verileri içeri aktarılacak depolama hesabı için depolama hesabı anahtarı. Ya da içermelidir **/sk:** < StorageAccountKey\> veya **/csas:** < ContainerSas\> komutu.|
|**/csas:** <ContainerSas\>|`Optional`. Kapsayıcı SAS depolama hesabına veri alma için kullanılacak. Ya da içermelidir **/sk:** < StorageAccountKey\> veya **/csas:** < ContainerSas\> komutu.<br /><br /> Bu parametre için değer, ardından bir soru işareti (?) ve SAS belirteci, bir kapsayıcı adı ile başlamalıdır. Örneğin:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> İzinleri URL ya da depolanmış erişim ilkesini belirtilen, okuma, içermelidir olup olmadığını yazma ve dışarı aktarma işleri için içeri aktarma işlerinde ve okuma ve yazma için liste silin.<br /><br /> Bu parametre belirtildiğinde içeri veya dışarı aktarılacak tüm BLOB paylaşılan erişim imzasını belirtilen kapsayıcı içinde olması gerekir.|
|**/ t:** < TargetDriveLetter\>|`Required.` Sürücü harfini izleyen iki nokta üst üste olmadan geçerli kopyalama oturumu için hedef sabit sürücünün.|
|**Özetteki**|`Optional.` Sürücünün biçimlendirilmesi gerektiğinde bu parametreyi belirtin; Aksi takdirde, atlayın. Aracı sürücüyü biçimlendiren önce konsolundan bir onay ister. Onay bastırmak için /silentmode parametre belirtin.|
|**/silentmode**|`Optional.` Hedef sürücüyü biçimlendirmek için onay gizlemek için bu parametreyi belirtin.|
|**/ Şifreleme**|`Optional.` Sürücü henüz BitLocker ile şifrelenmiş değil ve aracı tarafından şifrelenmiş gerektiğinde bu parametre belirtildi. Sürücünün BitLocker ile şifrelenmiş, bu parametreyi atlayın ve belirtin `/bk` parametre, var olan BitLocker anahtarı sağlama.<br /><br /> Belirtirseniz `/format` parametresi, sonra da belirtmeniz gerekir `/encrypt` parametresi.|
|**/BK:** < BitLockerKey\>|`Optional.` Varsa `/encrypt` olduğundan, belirtilen bu parametreyi atlayın. Varsa `/encrypt` olan atlanırsa ihtiyacınız zaten sürücünün BitLocker ile şifrelenmiş. BitLocker anahtarı belirtmek için bu parametreyi kullanın. İçeri aktarma işleri için tüm sabit sürücüleri BitLocker şifrelemesi gereklidir.|
|**/ LOGDIR:** < LogDirectory\>|`Optional.` Günlük dizini ayrıntılı günlükleri gibi geçici dosyaları depolamak için kullanılacak bir dizini belirtir. Belirtilmezse, geçerli dizin günlük dizini kullanılır.|

### <a name="parameters-required-for-all-copy-sessions"></a>Tüm kopyalama oturumları için gerekli Parametreler
 Günlük dosyası, bir sabit sürücü için tüm kopyalama oturumlarını durumunu içerir. Ayrıca, içeri aktarma işi oluşturmak için gereken bilgileri içerir. Her zaman, bir kopya oturum kimliği yanı sıra, Azure içeri/dışarı aktarma aracı çalıştırıldığı sırada bir günlük dosyası belirtmeniz gerekir:

|||
|-|-|
|Komut satırı parametresi|Açıklama|
|**/j:** < JournalFile\>|`Required.` Günlük dosyası yolu. Her bir sürücü, tam olarak bir günlük dosyası olmalıdır. Günlük dosyası hedef sürücüde olması değil gerektiğini unutmayın. Günlük dosyası uzantısıdır `.jrn`.|
|**/id:** < SessionID\>|`Required.` Oturum kimliği, bir kopyalama oturumu tanımlar. Bu, kesintiye kopyalama oturumunun doğru kurtarma sağlamak için kullanılır. Bir kopya oturumda kopyalanan dosyalar sonra hedef sürücüde oturum kimliği adlı bir dizinde depolanır.|

### <a name="parameters-for-copying-a-single-directory"></a>Tek bir dizin kopyalama için parametreler
 Tek bir dizin kopyalama yapılırken, aşağıdaki gerekli ve isteğe bağlı parametreleri geçerlidir:

|Komut satırı parametresi|Açıklama|
|----------------------------|-----------------|
|**/srcdir:** < SourceDirectory\>|`Required.` Hedef sürücüye kopyalanacak dosyaları içeren kaynak dizin. Dizin yolu mutlak bir yol (göreli bir yol değil) olmalıdır.|
|**/dstdir:** < DestinationBlobVirtualDirectory\>|`Required.` Windows Azure depolama hesabınızdaki hedef sanal dizin yolu. Sanal dizin zaten mevcut veya olabilir.<br /><br /> Bir kapsayıcı belirtebilir veya bir blob ön eki ister `music/70s/`. Hedef dizin kapsayıcı adını, ardından eğik çizgi ile başlamalıdır. "/" ve isteğe bağlı olarak ile biten bir sanal blob dizin içerebilir "/".<br /><br /> Hedef kapsayıcı kök kapsayıcı olduğunda, eğik çizgi olarak dahil, bir kök kapsayıcı açıkça belirtmeniz gerekir `$root/`. Hedef dizin kök kapsayıcı olduğunda kök kapsayıcıdaki blobları içeremez beri "/" adlarında, ve kaynak dizinde tüm alt dizinler kopyalanmaz.<br /><br /> Hedef sanal dizinler veya BLOB'ları belirtilirken geçerli kapsayıcı adları kullandığınızdan emin olun. Kapsayıcı adları küçük harf olması gerektiğini unutmayın. Kapsayıcı adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta verileri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Eğilimi:** < Yeniden Adlandır&#124;Hayır üzerine&#124;üzerine >|`Optional.` Belirtilen adres blob'u zaten mevcut olduğunda davranışı belirtir. Bu parametre için geçerli değerler: `rename`, `no-overwrite` ve `overwrite`. Bu değerler büyük küçük harfe duyarlı olduğunu unutmayın. Hiçbir değer belirtilmemişse, varsayılan değer `rename`.<br /><br /> Bu parametre tarafından belirtilen dizindeki tüm dosyaları etkiler için belirtilen değer `/srcdir` parametresi.|
|**/ BlobType:** < BlockBlob&#124;PageBlob >|`Optional.` Hedef BLOB'ları için blob türünü belirtir. Geçerli değerler: `BlockBlob` ve `PageBlob`. Bu değerler büyük küçük harfe duyarlı olduğunu unutmayın. Hiçbir değer belirtilmemişse, varsayılan değer `BlockBlob`.<br /><br /> Çoğu durumda `BlockBlob` önerilir. Belirtirseniz `PageBlob`, dizindeki her dosya uzunluğu 512, sayfa blobları için bir sayfa boyutunun katı olmalıdır.|
|**/ PropertyFile:** < PropertyFile\>|`Optional.` Hedef BLOB'ları için özellik dosyası yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|
|**/ MetadataFile:** < MetadataFile\>|`Optional.` Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|

### <a name="parameters-for-copying-a-single-file"></a>Tek bir dosya kopyalama için parametreler
 Tek bir dosyaya kopyalama yaparken aşağıdaki gerekli ve isteğe bağlı parametreleri geçerlidir:

|Komut satırı parametresi|Açıklama|
|----------------------------|-----------------|
|**/srcfile:** < Kaynakdosya\>|`Required.` Kopyalanacak dosyanın tam yolu. Dizin yolu mutlak bir yol (göreli bir yol değil) olmalıdır.|
|**/dstblob:** < DestinationBlobPath\>|`Required.` Windows Azure depolama hesabınızdaki hedef blob yolu. Blob olabilir veya zaten mevcut olmayabilir.<br /><br /> Kapsayıcı adı ile blob adı belirtin. Blob adı ile başlayamaz "/" ya da depolama hesabı adı. BLOB adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta verileri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Hedef kapsayıcı kök kapsayıcı olduğunda, açıkça belirtmeniz gerekir `$root` kapsayıcısı gibi `$root/sample.txt`. Kök kapsayıcı altına blobları Not içeremez "/" adlarında.|
|**/ Eğilimi:** < Yeniden Adlandır&#124;Hayır üzerine&#124;üzerine >|`Optional.` Belirtilen adres blob'u zaten mevcut olduğunda davranışı belirtir. Bu parametre için geçerli değerler: `rename`, `no-overwrite` ve `overwrite`. Bu değerler büyük küçük harfe duyarlı olduğunu unutmayın. Hiçbir değer belirtilmemişse, varsayılan değer `rename`.|
|**/ BlobType:** < BlockBlob&#124;PageBlob >|`Optional.` Hedef BLOB'ları için blob türünü belirtir. Geçerli değerler: `BlockBlob` ve `PageBlob`. Bu değerler büyük küçük harfe duyarlı olduğunu unutmayın. Hiçbir değer belirtilmemişse, varsayılan değer `BlockBlob`.<br /><br /> Çoğu durumda `BlockBlob` önerilir. Belirtirseniz `PageBlob`, dizindeki her dosya uzunluğu 512, sayfa blobları için bir sayfa boyutunun katı olmalıdır.|
|**/ PropertyFile:** < PropertyFile\>|`Optional.` Hedef BLOB'ları için özellik dosyası yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|
|**/ MetadataFile:** < MetadataFile\>|`Optional.` Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|

### <a name="resuming-an-interrupted-copy-session"></a>Bir kesintiye kopyalama oturumu sürdürme
 Herhangi bir nedenle bir kopya oturumu kesintiye uğrarsa, yalnızca belirtilen günlük dosyası ile aracını çalıştırarak devam edebilir:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Yalnızca en son kopyasını oturumu, varsa, anormal devam ettirilebilir.

> [!IMPORTANT]
>  Bir kopya oturumu sürdürdüğünüzde, kaynak veri dosyaları ve dizinleri ekleyerek veya kaldırarak dosyaları değiştirmeyin.

### <a name="aborting-an-interrupted-copy-session"></a>Kesintiye uğramış kopyalama oturumu iptal ediliyor
 Kopyalama oturumu kesintiye ve (örneğin, bir kaynak dizin erişilemez kanıtlandı varsa) devam etmek mümkün değildir, böylece geri alınabilir ve yeni kopya oturumları başlatılabilir geçerli oturumu iptal gerekir:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Yalnızca son kopyalama oturumu, anormal olursa iptal. Bir sürücü için ilk kopyalama oturumu durdurulamıyor unutmayın. Bunun yerine yeni bir günlük dosyasıyla kopyalama oturumunu yeniden başlatmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma Aracı'nı ayarlama](storage-import-export-tool-setup-v1.md)
* [İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Sık kullanılan komutlar için hızlı başvuru](storage-import-export-tool-quick-reference-v1.md) 
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Bir dışarı aktarma işini onarma](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
