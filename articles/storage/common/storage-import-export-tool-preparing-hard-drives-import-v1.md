---
title: Bir Azure içeri/dışarı aktarma içe aktarma işi için - v1 sabit sürücüler hazırlama | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için bir alma işi oluşturmak için WAImportExport v1 aracını kullanarak sabit sürücüler hazırlama hakkında bilgi edinin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 3d818245-8b1b-4435-a41f-8e5ec1f194b2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 361e16262e528c7dea1bab4b9d945a28af8be399
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874097"
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Sabit sürücüleri içeri aktarma işine hazırlama
Bir veya daha fazla sabit disk sürücüler içeri aktarma işi için hazırlamak için aşağıdaki adımları izleyin:

-   Blob hizmetinde alınacak verileri tanımlayın

-   Hedef sanal dizinleri ve blobları Blob hizmetinde tanımlayın

-   Kaç tane sürücüler, gerekir belirleme

-   Her sabit sürücüler için veri kopyalama

 Örnek iş akışı için bkz: [bir içeri aktarma işi için sabit sürücüler hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-the-data-to-be-imported"></a>İçeri aktarılacak veri tanımlayın
 İlk adım bir alma işi oluşturmak için hangi dizinleri ve dosyaları almak için kalacaklarını belirlemektir. Bu dizinlerin listesini, benzersiz dosyaların listesini veya bu iki bir birleşimi olabilir. Bir dizin dahil edildiğinde, dizinde ve alt dizinlerinde tüm dosyaları alma işinin bir parçası olur.

> [!NOTE]
>  Bir üst dizine dahil edildiğinde alt dizinleri birlikte yinelemeli olarak olduğundan, yalnızca üst dizini belirtin. Ayrıca dizinlerinden hiçbirini belirtmeyin.
>
>  Şu anda Microsoft Azure içeri/dışarı aktarma Aracı'nı aşağıdaki sınırlamalara sahiptir: bir dizin bir sabit sürücü içeren daha fazla veri içeriyorsa, ardından dizin küçük dizinlere bozuk gerekir. Örneğin, bir dizin 2,5 TB'lık veriyi içerir ve sabit diskin kapasitesini yalnızca 2 TB ise, daha sonra daha küçük dizinlere 2,5 TB dizin bölün gerekir. Bu sınırlama aracının daha sonraki bir sürümde ele alınacaktır.

## <a name="identify-the-destination-locations-in-the-blob-service"></a>Blob hizmetinde hedef konumları tanımlayın
 Her dizin veya içeri aktarılacak dosya için bir hedef sanal dizin veya Azure Blob hizmetinde blob tanımlayan gerekir. Bu hedeflerde giriş Azure içeri/dışarı aktarma aracı olarak kullanır. Dizinleri eğik çizgi karakteri ile sınırlı olduğunu unutmayın "/".

 Aşağıdaki tabloda blob hedefleri bazı örnekler gösterilmektedir:

|Kaynak dosya veya dizin|Hedef blob veya sanal dizin|
|------------------------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|
|K:\Temp\FavoriteVideo.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|

## <a name="determine-how-many-drives-are-needed"></a>Kaç tane sürücüler gerekli belirleme
 Ardından, belirlemeniz gerekir:

-   Verileri depolamak için gereken sabit sürücü sayısı.

-   Dizinler ve/veya her sabit kopyalanacak bağımsız dosyalar.

 Sabit sürücü, aktardığınız verileri depolamak için gerekir olduğundan emin olun.

## <a name="copy-data-to-your-hard-drive"></a>Verileri sabit sürücünüze kopyalayın
 Bu bölümde, bir veya daha fazla sabit sürücüler, verileri kopyalamak üzere Azure içeri/dışarı aktarma aracı çağrı açıklar. Azure içeri/dışarı aktarma aracı çağrısı, her bir yeni oluşturduğunuz *kopyalama oturum*. Veri kopyalama her sürücü için en az bir kopya oturumu oluşturun; Bazı durumlarda, tüm verilerinizi tek bir diske kopyalamak için birden fazla kopya oturumu gerekebilir. Birden çok kopya oturumu gerekebilir bazı nedenler şunlardır:

-   Kopyaladığınız her sürücü için ayrı kopya oturumu oluşturmanız gerekir.

-   Kopya oturumu tek bir dizin veya tek bir blob diske kopyalayabilirsiniz. Birden çok dizin, birden çok BLOB veya her ikisinin birleşimini kopyalıyorsanız birden çok kopya oturumlar oluşturmanız gerekir.

-   Özellikler ve alma işinin bir parçası olarak alınan BLOB'ları üzerinde ayarlanacak meta veriler belirtebilirsiniz. Özellikleri veya kopya oturum açmak için belirttiğiniz meta veriler bu kopya oturumu tarafından belirtilen tüm BLOB'lar için geçerli olur. Farklı özellikleri veya bazı BLOB'lar için meta veriler belirtmek istiyorsanız, ayrı kopya oturumu oluşturmanız gerekir. Bkz: [özelliklerini ayarlama ve meta verileri içeri aktarma işlemi sırasında](storage-import-export-tool-setting-properties-metadata-import-v1.md)daha fazla bilgi için.

> [!NOTE]
>  Bölümünde özetlenen gereksinimleri karşılayan birden çok makine varsa [ayarı yukarı Azure içeri/dışarı aktarma aracı](storage-import-export-tool-setup-v1.md), bu aracı örneği her makinede çalıştırarak paralel birden çok sabit sürücüler için veri kopyalayabilirsiniz.

 Azure içeri/dışarı aktarma aracı ile hazırlama her sabit sürücü için aracı, tek bir günlük dosyası oluşturur. Günlük dosyalarını içeri aktarma işi oluşturmak için sürücülerinizin tümünden gerekir. Günlük dosyası, aracı kesintiye uğrarsa sürücü hazırlama sürdürmek için de kullanılabilir.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>İçe aktarma işi için Azure içeri/dışarı aktarma aracı söz dizimi
 Sürücüleri içeri aktarma işi için hazırlamak için Azure içeri/dışarı aktarma aracı ile çağrı **PrepImport** komutu. Eklediğiniz hangi parametreleri bağlı olarak bu ilk kopya oturumu olup olmadığını veya bir sonraki kopyalama oturumu.

 İlk kopyalama oturum bir sürücü için depolama hesabı anahtarı belirtmek üzere bazı ek parametreler gerektirir; hedef sürücü harfi; Sürücü biçimlendirilmiş olması gerekir; sürücünün şifrelenmesi gerekip gerekmediğini ve bu durumda, BitLocker anahtar; ve günlük dizini. Bir dizin veya tek bir dosya kopyalamak bir başlangıç kopyası oturumu sözdizimi şöyledir:

 **İlk oturum tek bir dizin kopyalamak için Kopyala**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **İlk oturum tek bir dosya kopyalamak için Kopyala**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 Sonraki kopyalama oturumlarında ilk parametrelerini belirtmek gerekmez. Bir dizin veya tek bir dosya kopyalamak bir sonraki kopya oturumu sözdizimi şöyledir:

 **Tek bir dizin kopyalamak için sonraki kopyalama oturumları**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Tek bir dosya kopyalamak için sonraki kopyalama oturumları**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-the-first-copy-session-for-a-hard-drive"></a>Bir sabit sürücü için ilk kopyalama oturumu için parametreler
 Dosyaları sabit diske kopyalamak için Azure içeri/dışarı aktarma aracı her çalıştırdığınızda araç kopya oturumu oluşturur. Her bir kopya oturumu tek bir dizin veya tek bir dosyayı bir sabit sürücüye kopyalar. Kopya oturum durumu günlük dosyasına yazılır. (Örneğin, bir sistem güç kaybı nedeniyle) bir kopyası oturumu kesintiye uğrarsa, Aracı'nı yeniden çalıştırmak ve günlük dosyası komut satırında belirterek ettirilebilir.

> [!WARNING]
>  Belirtirseniz **/format** parametresi ilk kopya oturumu için sürücü biçimlendirilecek ve sürücüdeki tüm veriler silinecek. Boş sürücüleri yalnızca kopya oturumunuz için kullanmak önerilir.

 Her sürücü için ilk kopyalama oturumu için kullanılan komut sonraki kopyalama oturumları için komutları sayısından farklı parametre gerektirir. Aşağıdaki tabloda ilk kopyalama oturumu için ek parametreler listelenmektedir:

|Komut satırı parametresi|Açıklama|
|-----------------------------|-----------------|
|**/SK:**< StorageAccountKey\>|`Optional.`Verileri içeri aktarılacak depolama hesabı için depolama hesabı anahtarı. Ya da içermelidir **/sk:**< StorageAccountKey\> veya **/csas:**< ContainerSas\> komutu.|
|**/csas:**< ContainerSas\>|`Optional`. Kapsayıcı SAS depolama hesabına verilerini almak için kullanın. Ya da içermelidir **/sk:**< StorageAccountKey\> veya **/csas:**< ContainerSas\> komutu.<br /><br /> Bu parametre için değer, ardından bir soru işareti (?) ve SAS belirteci kapsayıcı adıyla başlaması gerekir. Örneğin:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> İzinleri URL veya bir saklı erişim ilkesinde belirtilen, okuma, içermelidir olup olmadığını yazma ve içeri aktarma işi ve okuma, yazma ve liste için dışarı aktarma işleri silin.<br /><br /> Bu parametre belirtilmediğinde, içe veya dışa aktarılacak tüm BLOB paylaşılan erişim imzası belirtilen kapsayıcı içinde olmalıdır.|
|**/ t:**< TargetDriveLetter\>|`Required.`Sürücü harfini izleyen iki nokta üst üste olmadan geçerli kopyalama oturumu için hedef sabit disk.|
|**Format**|`Optional.`Sürücünün biçimlendirilmesi gerektiğinde bu parametreyi belirtin; Aksi takdirde yok sayın. Aracı sürücü biçimleri önce konsolundan onay ister. Onay gizlemek için /silentmode parametresini belirtin.|
|**/silentmode**|`Optional.`Targert sürücüyü biçimlendirmek için onay gizlemek için bu parametreyi belirtin.|
|**/ şifrele**|`Optional.`Sürücü henüz BitLocker ile şifrelenmiş değil ve aracı tarafından şifrelenmiş gerektiğinde, bu parametre belirtildi. Sürücünün BitLocker ile şifrelenmiş durumunda bu parametreyi atlayın ve belirtin `/bk` parametresi, varolan BitLocker anahtar sağlama.<br /><br /> Belirtirseniz `/format` parametresi, sonra da belirtmeniz gerekir `/encrypt` parametresi.|
|**/BK:**< BitLockerKey\>|`Optional.`Varsa `/encrypt` olduğu belirtilen, bu parametreyi atlayın. Varsa `/encrypt` olan atlanırsa, şunlara sahip olmanız gerekir zaten sürücüsü BitLocker ile şifrelenmiş. BitLocker anahtarını belirtmek için bu parametreyi kullanın. İçeri aktarma işi için tüm sabit sürücülere yönelik BitLocker şifrelemesi gereklidir.|
|**/ LOGDIR:**< LogDirectory\>|`Optional.`Günlük dizinini ayrıntılı günlükleri gibi geçici dosyaları depolamak için kullanılacak dizini belirtir. Belirtilmezse, geçerli dizin günlük dizini olarak kullanılır.|

### <a name="parameters-required-for-all-copy-sessions"></a>Tüm kopyalama oturumları için gerekli parametreleri
 Günlük dosyası, bir sabit sürücü için tüm kopyalama oturumlarını durumunu içerir. Ayrıca, içe aktarma işi oluşturmak için gereken bilgileri içerir. Her zaman, bir kopya oturum kimliği yanı sıra Azure içeri/dışarı aktarma Aracı çalıştırırken bir günlük dosyası belirtmeniz gerekir:

|||
|-|-|
|Komut satırı parametresi|Açıklama|
|**/j:**< JournalFile\>|`Required.`Günlük dosyası yolu. Her bir sürücü tam olarak bir günlük dosyası olmalıdır. Günlük dosyası hedef sürücüde bulunan değil unutmayın. Günlük dosya uzantısı `.jrn`.|
|**/ID:**< SessionID\>|`Required.`Oturum kimliği kopyalama oturumu tanımlar. Kesintiye uğramış kopyalama oturumu doğru kurtarma sağlamak için kullanılır. Bir kopyalama oturumda kopyalanır dosyaları hedef sürücüde oturum kimliği sonra adlı bir dizinde depolanır.|

### <a name="parameters-for-copying-a-single-directory"></a>Tek bir dizin kopyalama için parametreler
 Tek bir dizin kopyalama işlemi sırasında aşağıdaki gerekli ve isteğe bağlı parametreleri geçerlidir:

|Komut satırı parametresi|Açıklama|
|----------------------------|-----------------|
|**/srcdir:**< SourceDirectory\>|`Required.`Hedef sürücüye kopyalanması için dosyaları içeren kaynak dizini. Dizin yolu mutlak bir yol (göreli bir yol değil) olması gerekir.|
|**/dstdir:**< DestinationBlobVirtualDirectory\>|`Required.`Windows Azure depolama hesabınızdaki hedef sanal dizin yolu. Sanal dizin olabilir veya zaten mevcut olmayabilir.<br /><br /> Bir kapsayıcı belirtebilir veya bir blob öneki ister `music/70s/`. Hedef dizin eğik tarafından izlenen kapsayıcı adıyla başlaması gerekir "/" ve isteğe bağlı olarak ile biten bir sanal blob dizin içerebilir "/".<br /><br /> Hedef kapsayıcı kök kapsayıcı olduğunda, eğik dahil olmak üzere kök kapsayıcı açıkça belirtmelisiniz `$root/`. Hedef dizin kök kapsayıcı olduğunda kök kapsayıcısı altında BLOB'lar içeremez bu yana "/" adlarında, kaynak dizin hiçbir dizinlerde kopyalanmaz.<br /><br /> Hedef sanal dizinleri ve blobları belirtirken geçerli kapsayıcı adları kullandığınızdan emin olun. Kapsayıcı adları küçük harfli olması gerektiğini unutmayın. Kapsayıcı adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Değerlendirme:**< yeniden adlandırmak &#124; Hayır üzerine &#124; üzerine >|`Optional.`Belirtilen adres bir blob zaten mevcut olduğunda davranışı belirtir. Bu parametre için geçerli değerler şunlardır: `rename`, `no-overwrite` ve `overwrite`. Bu değerleri büyük küçük harfe duyarlı olduğunu unutmayın. Herhangi bir değer belirtilirse, varsayılan değer `rename`.<br /><br /> Bu parametre tarafından belirtilen dizindeki tüm dosyaları etkiler için belirtilen değer `/srcdir` parametresi.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hedef BLOB'ları için blob türünü belirtir. Geçerli değerler: `BlockBlob` ve `PageBlob`. Bu değerleri büyük küçük harfe duyarlı olduğunu unutmayın. Herhangi bir değer belirtilirse, varsayılan değer `BlockBlob`.<br /><br /> Çoğu durumda, `BlockBlob` önerilir. Belirtirseniz `PageBlob`, her dosya dizininde uzunluğu 512, sayfa bloblarını için bir sayfa boyutunu katları olmalıdır.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Hedef BLOB'ları için özellik dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|

### <a name="parameters-for-copying-a-single-file"></a>Tek bir dosya kopyalama için parametreler
 Tek bir dosya kopyalama işlemi sırasında aşağıdaki gerekli ve isteğe bağlı parametreleri geçerlidir:

|Komut satırı parametresi|Açıklama|
|----------------------------|-----------------|
|**/srcfile:**< SourceFile\>|`Required.`Kopyalanacak dosyanın tam yolu. Dizin yolu mutlak bir yol (göreli bir yol değil) olması gerekir.|
|**/dstblob:**< DestinationBlobPath\>|`Required.`Windows Azure depolama hesabınızdaki hedef blob yolu. Blob olabilir veya zaten mevcut olmayabilir.<br /><br /> Kapsayıcı adı ile başlayan blob adı belirtin. Blob adı ile başlayamaz "/" veya depolama hesabı adı. BLOB adlandırma kuralları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> Hedef kapsayıcı kök kapsayıcı olduğunda, açıkça belirtmeniz gerekir `$root` kapsayıcı olarak gibi `$root/sample.txt`. Kök kapsayıcısı altında BLOB'ların Not içeremez "/" adlarında.|
|**/ Değerlendirme:**< yeniden adlandırmak &#124; Hayır üzerine &#124; üzerine >|`Optional.`Belirtilen adres bir blob zaten mevcut olduğunda davranışı belirtir. Bu parametre için geçerli değerler şunlardır: `rename`, `no-overwrite` ve `overwrite`. Bu değerleri büyük küçük harfe duyarlı olduğunu unutmayın. Herhangi bir değer belirtilirse, varsayılan değer `rename`.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Hedef BLOB'ları için blob türünü belirtir. Geçerli değerler: `BlockBlob` ve `PageBlob`. Bu değerleri büyük küçük harfe duyarlı olduğunu unutmayın. Herhangi bir değer belirtilirse, varsayılan değer `BlockBlob`.<br /><br /> Çoğu durumda, `BlockBlob` önerilir. Belirtirseniz `PageBlob`, her dosya dizininde uzunluğu 512, sayfa bloblarını için bir sayfa boyutunu katları olmalıdır.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Hedef BLOB'ları için özellik dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Hedef BLOB'ları için meta veri dosyasının yolu. Bkz: [içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi](../storage-import-export-file-format-metadata-and-properties.md) daha fazla bilgi için.|

### <a name="resuming-an-interrupted-copy-session"></a>Bir kesintiye uğramış kopyalama oturum sürdürme
 Herhangi bir nedenle bir kopya oturumu kesintiye uğrarsa, yalnızca belirtilen günlük dosyası ile aracını çalıştırarak devam edebilirsiniz:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Yalnızca en son kopyasını oturumu, anormal varsa ettirilebilir.

> [!IMPORTANT]
>  Kopya oturumu çağırdığınızda, kaynak veri dosyaları ve dizinleri ekleyerek veya kaldırarak dosyaları değiştirmeyin.

### <a name="aborting-an-interrupted-copy-session"></a>Bir kesintiye uğramış kopya oturumu durduruluyor
 Kopya oturumu kesintiye ve (örneğin, bir kaynak dizin erişilemez oluyor uygulamasına yol açıyordu varsa) sürdürmek mümkün değilse, böylece geri alınabilir ve yeni kopya oturumları başlatılabilir geçerli oturum iptal gerekir:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Yalnızca son kopyalama oturumu, anormal olursa iptal. Bir sürücü için ilk kopyalama oturum iptal edilemiyor unutmayın. Bunun yerine yeni bir günlük dosyasıyla kopyalama oturumlarını yeniden başlatmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma aracı ayarlama](storage-import-export-tool-setup-v1.md)
* [İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Sık kullanılan komutlar için hızlı başvuru](storage-import-export-tool-quick-reference-v1.md) 
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Bir dışarı aktarma işini onarma](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
