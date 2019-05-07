---
title: Kopyalayın veya Windows üzerinde AzCopy ile Azure depolama veri taşıma | Microsoft Docs
description: AzCopy yardımcı programından Windows taşımak veya için veya blob, tablo ve dosya içeriği veri kopyalamak için kullanın. Yerel dosyaları Azure depolama alanına veri kopyalama veya içinde veya depolama hesapları arasında verileri kopyalayabilirsiniz. Kolayca verilerinizi Azure Depolama'ya geçirin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 01/03/2019
ms.author: normesta
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: ead5729496253b553ea453a9d6ce33b6b673e027
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149051"
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a>Windows üzerinde AzCopy ile veri aktarma

AzCopy, en iyi performans için tasarlanmış basit komut kullanarak Microsoft Azure Blob, dosya ve tablo depolama içine/dışına veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. Bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz.  

> [!IMPORTANT]
> Bu makalede, AzCopy daha eski bir sürümünü açıklar.
>En güncel AzCopy sürümünü yüklemek için bkz [AzCopy v10](storage-use-azcopy-v10.md).

AzCopy (AzCopy v8.1) daha eski sürümünü yüklemeyi seçerseniz, indirebileceğiniz birden çok sürümü vardır. Windows üzerinde AzCopy Windows stili komut satırı seçenekleri sunar. [Linux üzerinde AzCopy](storage-use-azcopy-linux.md) POSIX stili komut satırı seçeneklerini sunarak Linux platformlarını hedefler. Bu makale, Windows üzerinde AzCopy kapsar.

## <a name="download-and-install-azcopy-v81-on-windows"></a>İndirin ve Windows üzerinde AzCopy (v8.1) yükleyin

İndirme [Windows üzerinde AzCopy (v8.1)](https://aka.ms/downloadazcopy).

#### <a name="azcopy-on-windows-81-release-notes"></a>Windows 8.1 üzerinde AzCopy sürüm notları

- Tablo hizmeti, en son sürümü artık desteklenmiyor. Tabloyu dışarı aktarma özelliğini kullanırsanız, AzCopy 7.3 sürümünü indirin.
- .NET Core 2.1 ile oluşturulmuş ve tüm .NET Core bağımlılıklarını yüklemenin artık paketlenir.
- OAuth kimlik doğrulaması desteği eklendi. Kullanım ```azcopy login``` Azure Active Directory kullanarak oturum.

### <a name="azcopy-with-table-support-v73"></a>Azcopy ile tablo desteği (v7.3)

İndirme [tablo desteği ile AzCopy 7.3](https://aka.ms/downloadazcopynet).

### <a name="post-installation-step"></a>Yükleme sonrası adımı

Yükleyicisi'ni kullanarak Windows üzerinde AzCopy yükledikten sonra bir komut penceresi açın ve bilgisayarınızda - AzCopy yükleme dizinine gidin burada `AzCopy.exe` yürütülebilir bulunur. İsterseniz, AzCopy yükleme konumunu sistem yolunuza ekleyebilirsiniz. AzCopy varsayılan olarak, yüklü `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` veya `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>İlk, AzCopy komutuna yazma

AzCopy komutları için temel sözdizimi aşağıdaki gibidir:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

Aşağıdaki örnekler, çeşitli veri ve Microsoft Azure Blobları, dosyalar ve tablolardan kopyalamak için senaryoları göstermektedir. Başvurmak [AzCopy parametreleri](#azcopy-parameters) bölümünde ayrıntılı bir açıklama her örnekte kullanılan parametrelerden biri.

## <a name="download-blobs-from-blob-storage"></a>Blob depolama alanından blobları indirin

AzCopy kullanarak blobları indirmek için çeşitli yollar göz atalım.

### <a name="download-a-single-blob"></a>Tek bir blob indirme

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Klasör unutmayın `C:\myfolder` yok, AzCopy oluşturur ve indirme `abc.txt` yeni klasöre kopyalar.

### <a name="download-a-single-blob-from-the-secondary-region"></a>İkincil bölgeden tek blob indirme

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Okuma erişimli coğrafi olarak yedekli depolama ikincil bölgeye erişmek için etkin olması gerektiğini unutmayın.

### <a name="download-all-blobs-in-a-container"></a>Bir kapsayıcıdaki tüm blobları indirin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Aşağıdaki blobların belirtilen kapsayıcıda bulunan varsayın:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Dizin indirme işleminden sonra `C:\myfolder` aşağıdaki dosyaları içerir:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Seçeneği belirtmezseniz, `/S`, BLOB indirilir.

### <a name="download-blobs-with-a-specific-prefix"></a>Belirli bir önek ile blobları indirin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Aşağıdaki blobların belirtilen kapsayıcıda bulunan varsayılır. Önekiyle başlayan tüm blobları `a` yüklenir:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Klasör indirme işleminden sonra `C:\myfolder` aşağıdaki dosyaları içerir:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Ön ek, blob adının ilk bölümü forms bir sanal dizin geçerlidir. Yukarıda gösterilen örnekte, sanal dizin yüklenmemesi için belirtilen bir önek eşleşmiyor. Ayrıca, varsa seçeneği `/S` belirtilmezse, AzCopy blobları indirmek değil.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Kaynak BLOB'ları olarak aynı olacak şekilde, dışarı aktarılan dosyaların son değiştirilme saatini ayarlayın

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Ayrıca, kendi son değiştirilme zamanına göre indirme işleminden blobları hariç tutabilirsiniz. Son değişiklik zamanı blobları dışlamak istiyorsanız, aynı veya daha yeni hedef dosya eklemektir `/XN` seçeneği:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Son değişiklik zamanı blobları dışlamak istiyorsanız aynı veya hedef dosyanın daha eski eklemektir `/XO` seçeneği:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="upload-blobs-to-blob-storage"></a>BLOB'lar, Blob depolamaya yükleme

AzCopy kullanarak blobları karşıya yüklemek için birkaç yolu göz atalım.

### <a name="upload-a-single-blob"></a>Tek bir blob yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler.

### <a name="upload-a-single-blob-to-a-virtual-directory"></a>Bir sanal dizin için tek bir blob karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Belirtilen sanal dizin yoksa, AzCopy sanal dizin adını içerecek şekilde dosyayı yükler (*örn*, `vd/abc.txt` Yukarıdaki örnekteki).

### <a name="upload-all-blobs-in-a-folder"></a>Bir klasördeki tüm blobları karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Seçeneğini belirterek `/S` tüm alt klasörler ve bunların dosyaları da karşıya yüklenir, yani Blob depolama alanına yinelemeli olarak belirtilen dizinin içeriklerini yükler. Örneğin, aşağıdaki dosyalar klasöründe bulunan varsayar `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Sonra karşıya yükleme işlemi, kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Seçeneği belirtmezseniz, `/S`, AzCopy yinelemeli olarak karşıya değil. Sonra karşıya yükleme işlemi, kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-blobs-matching-a-specific-pattern"></a>Belirli bir düzene eşleşen blobları karşıya yüklemek

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Aşağıdaki dosyalar klasöründe bulunduğu varsayılır `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Sonra karşıya yükleme işlemi, kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Seçeneği belirtmezseniz, `/S`, AzCopy, yalnızca sanal bir dizinde bulunan yoksa BLOB'ları yükler:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Hedef blob MIME içerik türünü belirtin

Varsayılan olarak, içerik türü için bir hedef blobun AzCopy ayarlar `application/octet-stream`. 3.1.0 sürümünden başlayarak, içerik türü seçeneği aracılığıyla açıkça belirtebilirsiniz `/SetContentType:[content-type]`. Bu sözdizimi, bir karşıya yükleme işleminde tüm bloblar için içerik türünü ayarlar.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Belirtirseniz `/SetContentType` her blob veya dosyanın dosya uzantısını göre içerik türü bir değer olmadan AzCopy ayarlar.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="copy-blobs-in-blob-storage"></a>Blob depolamada blobları kopyalama

Blobları bir konumdan diğerine kopyalamak için çeşitli yollar göz atalım AzCopy kullanarak.

### <a name="copy-a-single-blob-from-one-container-to-another-within-the-same-storage-account"></a>Bir blobun bir kapsayıcıdan aynı depolama hesabındaki başka bir kopyalayın. 

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Bir depolama hesabı içinde blob kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-a-single-blob-from-one-storage-account-to-another"></a>Tek bir blobu bir depolama hesabından diğerine kopyalama

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Bir blob depolama hesapları arasında kopyaladığınızda, bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-a-single-blob-from-the-secondary-region-to-the-primary-region"></a>Tek bir blobu ikincil bölgesinden birincil bölgeye kopyalayın.

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Okuma erişimli coğrafi olarak yedekli depolama ikincil depolamaya erişmek için etkin olması gerektiğini unutmayın.

### <a name="copy-a-single-blob-and-its-snapshots-from-one-storage-account-to-another"></a>Tek bir blob ve anlık görüntüleri bir depolama hesabından diğerine kopyalama

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Kopyalama işleminden sonra hedef kapsayıcı, blob ve anlık görüntüleri içerir. Kapsayıcı, blob Yukarıdaki örnekteki iki anlık görüntü sahip olduğunu varsayarak, aşağıdaki blob ve anlık görüntüleri içerir:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="copy-all-blobs-in-a-container-to-another-storage-account"></a>Tüm bloblar bir kapsayıcıda başka bir depolama hesabına kopyalayın. 

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 
/Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /S
```

/S belirtme seçeneği belirtilen kapsayıcı yinelemeli olarak içeriğini yükler. Bkz: [bir klasördeki tüm blobları karşıya yükleme](#upload-all-blobs-in-a-folder) daha fazla bilgi ve örnek için.

### <a name="synchronously-copy-blobs-from-one-storage-account-to-another"></a>Zaman uyumlu olarak blobları bir depolama hesabından diğerine kopyalama

AzCopy varsayılan olarak, zaman uyumsuz olarak iki depolama uç noktaları arasında veri kopyalar. Bu nedenle, kopyalama işlemi hiçbir SLA'nızı nasıl hızlı olan yedek bant genişliği kapasitesi kullanarak arka plan bir bloba kopyalanır ve kopyalama tamamlandı başarısız oldu veya cihaz silinene kadar AzCopy kopya durumunu düzenli olarak denetler. çalışır.

`/SyncCopy` Seçeneği, kopyalama işlemi tutarlı hızı alır sağlar. AzCopy, BLOB'ları için yerel bellek belirtilen kaynaktan kopyalanacak indiriliyor ve bunları Blob Depolama hedefe karşıya yüklemeyi zaman uyumlu kopya gerçekleştirir.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy` zaman uyumsuz kopya kıyasla ek çıkış maliyet oluşturabilir, çıkış maliyet önlemek için kaynak depolama hesabının aynı bölgede olan bir Azure VM'de bu seçeneği kullanmak için önerilen yaklaşımdır.

## <a name="download-files-from-file-storage"></a>Dosyaları dosya depolama'yı indirin

AzCopy kullanarak dosyaları indirmek için çeşitli yollar göz atalım.

### <a name="download-a-single-file"></a>Tek bir dosyayı indirin

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Belirtilen kaynağı olan bir Azure dosya paylaşımı sonra ya da tam dosya adı belirtmeniz gerekir (*örn* `abc.txt`) tek bir dosya indirin veya seçeneği belirtin `/S` paylaşımı yinelemeli olarak tüm dosyaları indirilemedi. Bir dosya deseni ve seçenek belirlemek çalışırken `/S` birlikte hatayla sonuçlanır.

### <a name="download-all-files-in-a-directory"></a>Bir dizindeki tüm dosyaları indirme

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Boş klasörleri indirilmez unutmayın.

## <a name="upload-files-to-an-azure-file-share"></a>Bir Azure dosya paylaşımına dosya yükleme

AzCopy kullanarak karşıya dosya yükleme için çeşitli yollar göz atalım.

### <a name="upload-a-single-file"></a>Tek bir dosyayı karşıya yükleyin

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files-in-a-folder"></a>Bir klasördeki tüm dosyaları karşıya yükle

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Boş klasörler karşıya unutmayın.

### <a name="upload-files-matching-a-specific-pattern"></a>Belirli bir desenle eşleşen dosyaları karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="copy-files-in-file-storage"></a>Dosya Depolama'da dosyaları kopyalama

AzCopy kullanarak Azure dosya paylaşımı dosyaları kopyalamak için çeşitli yollar bakalım.

### <a name="copy-from-one-file-share-to-another"></a>Bir dosya paylaşımından diğerine kopyalama

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Dosya paylaşımları arasında dosya kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-from-an-azure-file-share-to-blob-storage"></a>Bir Azure dosya paylaşımından Blob depolamaya kopyalama

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Bir dosyayı blob için dosya paylaşımından kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-a-blob-from-blob-storage-to-an-azure-file-share"></a>Bir blob Blob depolamadan bir Azure dosya paylaşımına kopyalayın.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Bir blobun bir dosya paylaşımı için bir dosya kopyaladığınızda, bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="synchronously-copy-files"></a>Dosyaları eşzamanlı olarak Kopyala

Belirtebileceğiniz `/SyncCopy` eş zamanlı olarak File Storage için dosya depolama, Blob depolamaya dosya depolama ve dosya depolama için Blob depolamadan veri kopyalamak için seçeneğinde, AzCopy bu yerel bellek için kaynak verilerini indirerek yapar ve yeniden karşıya yükleyin Hedef. Standart çıkış ücreti uygulanır.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Dosya depolama'yı Blob depolama alanına kopyalama işlemi sırasında varsayılan blob türü blok blobudur; Kullanıcı seçeneğini belirtebilir `/BlobType:page` hedef blob türünü değiştirmek için.

Unutmayın `/SyncCopy` ek kullanım maliyetleri için zaman uyumsuz kopya karşılaştırıldığında oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabının aynı bölgede olan bir Azure VM'de bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="export-data-from-table-storage"></a>Tablo depolama'yı verileri dışarı aktarma

AzCopy kullanarak Azure tablo Depolama'dan veri aktarma göz atalım.

### <a name="export-a-table"></a>Bir tabloyu dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy, belirtilen hedef klasöre bir bildirim dosyası yazar. Bildirim dosyası içeri aktarma işleminde gerekli veri dosyaları bulun ve veri doğrulama gerçekleştirmek için kullanılır. Bildirim dosyasının varsayılan olarak aşağıdaki adlandırma kuralını kullanır:

    <account name>_<table name>_<timestamp>.manifest

Kullanıcı seçeneği belirtebilirsiniz `/Manifest:<manifest file name>` bildirim dosyası adı ayarlamak için.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-an-export-from-table-storage-into-multiple-files"></a>Tablo depolama biriminden bir dışarı aktarma birden çok dosyaya bölme

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy kullanan bir *toplu dizin* birden çok dosya ayırt etmek için bölünmüş veri dosya adları. Ses dizini iki bölümden oluşur bir *bölüm anahtar aralığı dizin* ve *bölünmüş dosya dizini*. Her iki dizinler sıfır tabanlıdır.

Kullanıcı seçeneği belirtilmezse bölüm anahtar aralığı dizini 0'dır `/PKRS`.

Örneğin, AzCopy seçeneği kullanıcının belirttiği sonra iki veri dosyalarını oluşturur düşünelim `/SplitSize`. Sonuçta elde edilen veri dosya adları olabilir:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Olası en düşük değer seçeneği Not `/SplitSize` 32 MB'dir. Belirtilen hedef Blob Depolama ise, kendi boyutları blob boyutu sınırlaması (200 GB) ulaştığında AzCopy veri dosyası ayırır, açmamasından olup seçeneği `/SplitSize` kullanıcı tarafından belirtilen.

### <a name="export-a-table-to-json-or-csv-data-file-format"></a>Bir tablo JSON veya CSV verileri dosyası biçiminde dışarı aktarma

Varsayılan olarak, AzCopy tabloları JSON veri dosyalarını dışarı aktarır. Seçeneğini belirtebilirsiniz `/PayloadFormat:JSON|CSV` tabloları JSON veya CSV dışarı aktarmak için.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

CSV yük biçimi belirtirken, AzCopy dosya uzantısına sahip bir şema dosyası da oluşturur. `.schema.csv` her veri dosyası için.

### <a name="export-table-entities-concurrently"></a>Tablo varlıkları eşzamanlı olarak dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy başlatan kullanıcı seçeneğini belirttiğinde varlıkları dışarı aktarmak için eşzamanlı işlem `/PKRS`. Her işlem bir bölüm anahtar aralığı dışarı aktarır.

Eşzamanlı işlemlerin sayısını seçeneği tarafından denetlendiğini unutmayın `/NC`. AzCopy varsayılan değeri olarak çekirdek işlemci sayısını kullanır `/NC` tablo varlıkları kopyalarken bile `/NC` belirtilmedi. Kullanıcının belirttiği seçeneği ne zaman `/PKRS`, AzCopy ve iki değerden daha küçük kullanır - bölüm anahtar aralığı ve örtük veya açık olarak belirtilen eşzamanlı işlem - çok başlatmak için eşzamanlı işlemlerin sayısını belirler. Daha fazla bilgi için türü `AzCopy /?:NC` komut satırına.

### <a name="export-a-table-to-blob-storage"></a>Blob depolama alanına bir tabloyu dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy şu adlandırma kuralını blob kapsayıcısına bir JSON veri dosyası oluşturur:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Yük biçimi en küçük meta verileri için oluşturulan JSON veri dosyasındaki izler. Bu yük biçimi hakkında daha fazla bilgi için bkz: [tablo hizmeti işlemleri için yükü biçimi](https://msdn.microsoft.com/library/azure/dn535600.aspx).

Tablolar için BLOB'ları dışa aktarırken AzCopy tablo varlıkları yerel geçici veri dosyalarını indirir ve ardından bu varlıkların bloba yükler unutmayın. Bu geçici veri dosyalarını varsayılan yol ile günlük dosyası klasörü içine yerleştirilir "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", seçeneği belirtebilirsiniz/değişiklik günlüğü için [günlük dosyası klasörü] Z: dosya klasör konumu ve bu nedenle geçici veri dosyalarının konumu değiştirin. Geçici verileri bloba yüklendikten sonra yerel disk geçici veri dosyasında anında silindi ancak dosyalarının boyutu kullanarak tablo varlıklarını boyutu ve seçeneği /SplitSize ile belirtilen boyutu tarafından belirlenir emin olduğunuz yeterince yerel olun silinmeden önce bu geçici veri dosyalarını depolamak için disk alanı.

## <a name="import-data-into-table-storage"></a>Tablo depolama alanına veri alma

AzCopy kullanarak Azure tablo depolama alanına veri alma göz atalım.

### <a name="import-a-table"></a>Tablo alma

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Seçenek `/EntityOperation` tablosuna varlıkları nasıl ekleneceğini gösterir. Olası değerler şunlardır:

* `InsertOrSkip`: Var olan bir varlığa atlar veya tablodaki mevcut değilse yeni bir varlık ekler.
* `InsertOrMerge`: Var olan bir varlığa birleştirir veya tablodaki mevcut değilse yeni bir varlık ekler.
* `InsertOrReplace`: Var olan bir varlığa değiştirir veya tablodaki mevcut değilse yeni bir varlık ekler.

Not seçeneği belirtilemez `/PKRS` alma senaryosunda. Dışarı aktarma seçeneği belirttiğiniz gerekir senaryosu, farklı `/PKRS` bir tabloyu içe aktarması eşzamanlı işlemlerin başlatmak için AzCopy eşzamanlı işlem varsayılan olarak başlatılır. Varsayılan eş zamanlı işlemleri başlatıldı çekirdekli işlemci sayısına eşit sayısıdır; seçeneği ile aynı anda farklı sayıda belirtebilirsiniz ancak `/NC`. Daha fazla bilgi için türü `AzCopy /?:NC` komut satırına.

JSON için değil CSV içe aktarma AzCopy yalnızca desteklediğini unutmayın. AzCopy, kullanıcı tarafından oluşturulan JSON tablo içeri aktarmalar desteklemek ve bildirim dosyaları desteklemez. Bu dosyaların her ikisini de AzCopy tablo verme gelmelidir. Hataları önlemek için lütfen dışarı aktarılan JSON değiştirmeyin veya bildirim dosyası.

### <a name="import-entities-into-a-table-from-blob-storage"></a>Blob depolamadan bir tabloya varlıkları İçeri Aktar

Bir Blob kapsayıcısı aşağıdaki yer aldığını varsayalım: Azure tablo ve eşlik eden bildirim dosyasını temsil eden bir JSON dosyası.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Bu blob kapsayıcısında bildirim dosyası kullanarak tablo varlıklarını almak için aşağıdaki komutu çalıştırabilirsiniz:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Diğer AzCopy özellikleri

Diğer bazı AzCopy özellikleri bir göz atalım.

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Hedefte mevcut olmayan veri kopyalama

`/XO` Ve `/XN` parametreleri, sırasıyla kopyalanmasını daha eski veya yeni bir kaynak kaynakları hariç tut izin verir. Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, her iki parametre AzCopy komutunda belirtebilirsiniz:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Kaynak veya hedef tablo olduğunda bu desteklenmediğini unutmayın.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Komut satırı parametreleri belirtmek için bir yanıt dosyası kullanmak

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

AzCopy komut satırı parametreleri herhangi bir yanıt dosyasında içerebilir. Komut satırı üzerinde belirtilmiş gibi AzCopy Parametreler dosyasında dosyanın içeriği ile doğrudan bir değiştirme gerçekleştirme işler.

Adlı bir yanıt dosyası varsayar `copyoperation.txt`, aşağıdaki satırları içeren. Tek bir satıra her AzCopy parametresi belirtilebilir.

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

veya satırları ayrı:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy, iki satır arasında parametre bölmeniz için burada gösterildiği gibi başarısız `/sourcekey` parametresi:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Komut satırı parametreleri belirtmek için birden çok yanıt dosyalarını kullan

Adlı bir yanıt dosyası varsayar `source.txt` kaynak kapsayıcı belirtir:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Ve adlandırılmış bir yanıt dosyası `dest.txt` bir hedef klasör dosya sistemindeki belirtir:

    /Dest:C:\myfolder

Ve adlandırılmış bir yanıt dosyası `options.txt` AzCopy seçeneklerini belirtir:

    /S /Y

AzCopy ile bu yanıt dosyaları aramak için her biri bir dizinde bulunan `C:\responsefiles`, bu komutu kullanın:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

Tüm Bireysel parametreleri komut satırında eklediyseniz gibi AzCopy bu komutu işler:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) belirtin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Bir SAS URI kapsayıcıdaki belirtebilirsiniz:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Günlük dosyası klasörü

AzCopy için bir komut dağıttığınız her saat bir günlük dosyası varsayılan klasör var olup veya bu seçeneği belirtilen klasörde mevcut olup olmadığını denetler. Günlük dosyası ya da yerinde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı komut satırında günlük dosyası ile eşleşip eşleşmediğini denetler. AzCopy komut satırlarını eşleşirse, işlem tamamlanmadı devam ettirir. Bunlar eşleşmiyorsa, yeni bir işlem başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyası ya da üzerine istenir.

Varsayılan konumu için günlük dosyası kullanmak istiyorsanız:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Seçeneğini atlarsanız `/Z`, veya seçeneği belirtin `/Z` klasör yolu, yukarıda gösterildiği gibi AzCopy günlük dosyası olan varsayılan konumda oluşturur `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Günlük dosyası zaten varsa, AzCopy günlük dosyasını temel alarak işlemi devam eder.

Günlük dosyası için özel bir konum belirtmek istiyorsanız:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Bu örnek, zaten yoksa, günlük dosyası oluşturur. Mevcut değilse, AzCopy günlük dosyasını temel alarak işlemi devam eder.

AzCopy çalışmaya devam etmesini istiyorsanız:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Bu örnekte, son işlemi tamamlamak için başarısız olan sürdürür.

### <a name="generate-a-log-file"></a>Bir günlük dosyası oluşturur

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Seçeneğini belirtirseniz `/V` ayrıntılı günlüğü dosya yoluna sağlamadan sonra AzCopy günlük dosyası olan varsayılan konumda oluşturur `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Aksi takdirde, özel bir konumda bir günlük dosyası oluşturabilirsiniz:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Aşağıdaki seçeneği göreli bir yol belirtirseniz unutmayın `/V`, gibi `/V:test/azcopy1.log`, ayrıntılı günlük adlı bir alt klasör içinde geçerli çalışma dizininde oluşturulur `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Başlamak için eşzamanlı işlemlerin sayısını belirtin

Seçenek `/NC` eşzamanlı kopyalama işlemleri sayısını belirtir. Varsayılan olarak, belirli bir veri aktarımı verimliliğini artırmak için eşzamanlı işlemlerin sayısını AzCopy başlatılır. Tablo işlemleri için eşzamanlı işlemlerin sayısını için sahip olduğunuz işlemci sayısına eşittir. BLOB ve dosya işlemleri, eşzamanlı işlemlerin sayısını için sahip olduğunuz işlemci sayısı 8 katı eşit. Düşük bant genişliğine sahip ağ üzerinden AzCopy çalıştırıyorsanız, kaynak yarışmaya göre neden hatadan kaçınmak /NC için daha düşük bir sayı belirtebilirsiniz.

### <a name="run-azcopy-against-the-azure-storage-emulator"></a>AzCopy, Azure Storage öykünücüsüne karşı çalıştırma

AzCopy karşı çalıştırabilirsiniz [Azure Storage öykünücüsü](storage-use-emulator.md) bloblar için:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

Ayrıca tablolar için çalıştırabilirsiniz:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

### <a name="automatically-determine-content-type-of-a-blob"></a>Otomatik olarak bir Blob içerik türünü belirleme

AzCopy, içerik türü için dosya uzantısı eşleştirme depolayan bir JSON dosyası dayalı bir blobun içerik türünü belirler. Bu JSON dosyası AzCopyConfig.json adlı ve AzCopy dizininde bulunur. Listede olmayan bir dosya türü varsa JSON dosyasına eşleme ekleyebilirsiniz:

```
{
  "MIMETypeMapping": {
    ".myext": "text/mycustomtype",
    .
    .
  }
}
```     

## <a name="azcopy-parameters"></a>AzCopy parametreleri

AzCopy için Parametreler aşağıda verilmiştir. Yardım için komut satırından aşağıdaki komutlardan birini, AzCopy kullanarak da yazabilirsiniz:

* AzCopy için ayrıntılı komut satırı Yardımı için: `AzCopy /?`
* AzCopy herhangi bir parametre ile ilgili ayrıntılı yardım için: `AzCopy /?:SourceKey`
* Komut satırı örnekleri için: `AzCopy /?:Sample`

### <a name="sourcesource"></a>/ Source: "kaynak"

Kopyalanacak kaynak verileri belirtir. Kaynak dosya sistemi dizin, bir blob kapsayıcı, blob sanal dizin, bir depolama dosya paylaşımı, depolama dosyası dizini veya bir Azure tablosu olabilir.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="destdestination"></a>/ Hedef: "hedef"

Kopyalamak için hedef belirtir. Hedef dosya sistemi dizin, bir blob kapsayıcı, blob sanal dizin, bir depolama dosya paylaşımı, depolama dosyası dizini veya bir Azure tablosu olabilir.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="patternfile-pattern"></a>/ Desen: "dosya deseni"

Kopyalanacak dosyaları belirten bir dosya deseni belirtir. /Pattern parametre davranışını, kaynak verilerin konumu ve yinelemeli modu seçeneği varlığını göre belirlenir. Özyinelemeli modu /S. seçeneği belirtildiğinden

Belirtilen kaynak dosya sistemindeki bir dizin ise, ardından standart joker karakterler geçerli olur ve dizin içindeki dosyalar sağlanan dosya deseni eşleştirme. Seçeneği sonra AzCopy /S belirtilirse, ayrıca dizini altındaki tüm alt klasörlerdeki tüm dosyaları belirtilen desenle eşleşiyorsa.

Belirtilen kaynak, bir blob kapsayıcısı veya sanal dizin ise, joker karakter olarak uygulanmaz. Seçenek sonra AzCopy /S belirtilirse, belirtilen dosya deseni blob ön eki olarak yorumlar durumunda. Seçeneği /S belirtilmezse, sonra AzCopy tam blob adları karşı dosya deseni eşleşiyorsa.

Belirtilen kaynağı olan bir Azure dosya paylaşımı sonra gerekir ya da tek bir dosya kopyalamak için tam dosya adı (örneğin abc.txt) belirtin veya seçeneği belirtin /S paylaşımı yinelemeli olarak tüm dosyaları kopyalamak için. Hem bir dosya deseni ve seçeneği /S birlikte sonuçları hata belirtin çalışılıyor.

/ Source bir blob kapsayıcı veya blob sanal dizin ve diğer tüm durumlarda büyük küçük harf duyarsız eşleştirme kullanan büyük/küçük harfe eşleşen AzCopy kullanır.

Hiçbir dosya deseni belirtildiğinde kullanılan varsayılan dosya Düzen *.* bir dosya sistemi konumundan veya bir Azure depolama konumu için boş bir önek. Birden çok dosya desenlerinin belirtilmesi desteklenmiyor.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="destkeystorage-key"></a>/ DestKey: "depolama-key"

Hedef kaynak için depolama hesabı anahtarını belirtir.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="destsassas-token"></a>/DestSAS:"sas-token"

(Eğer varsa) hedef için okuma ve yazma izinlerine sahip paylaşılan erişim imzası (SAS) belirtir. İçerdiği gibi komut satırı özel karakterler çift tırnak işaretleri ile SAS alın.

Hedef kaynak, bir blob kapsayıcısını, dosya paylaşımı veya tablo ise, bu seçenek, ardından SAS belirtecini ya da belirtebilir veya SAS, hedef blob kapsayıcısını, dosya paylaşımı veya tablonun URI, bu seçenek olmadan bir parçası olarak belirtebilirsiniz.

Kaynak ve hedef hem BLOB ise, hedef blob kaynak blob olarak aynı depolama hesabında bulunmalıdır.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="sourcekeystorage-key"></a>/ SourceKey: "depolama-key"

Kaynak kaynak için depolama hesabı anahtarını belirtir.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas belirteci"

Kaynağı için okuma ve liste izinleri ile bir paylaşılan erişim imzası (varsa) belirtir. İçerdiği gibi komut satırı özel karakterler çift tırnak işaretleri ile SAS alın.

Kaynak bir blob kapsayıcı bir kaynaktır ve ne anahtar ne de bir SAS sağlanır, blob kapsayıcısını üzerinden anonim erişim okuyun.

Kaynak dosya paylaşımı veya tablo ise, bir anahtar veya bir SAS sağlanmalıdır.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="s"></a>/S

Kopyalama işlemleri için özyinelemeli modunu belirtir. Özyinelemeli modunda AzCopy tüm bloblar veya alt klasördekiler de dahil olmak üzere belirtilen dosya deseni ile eşleşen dosyaları kopyalar.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blok" | "page" | "ekleme"

Hedef blobun blok blobu, sayfa blobu veya ekleme blobu olup olmadığını belirtir. Bu seçenek yalnızca bir blob karşıya yüklenirken kullanılır. Aksi takdirde bir hata oluşturulur. AzCopy, hedef blob ise ve bu seçenek, varsayılan olarak, belirtilmemiş bir blok blobu oluşturur.

**İçin uygulanabilir:** Bloblar

### <a name="checkmd5"></a>/ CheckMD5

İndirilen veriler için bir MD5 karma değeri hesaplar ve MD5 karma değeri blobu'nda depolanan veya dosyanın içeriği MD5 özelliği hesaplanan karma eşleşen doğrular. Değerler eşleşmiyorsa, AzCopy, verileri indirmek başarısız olur. MD5 denetimi verilerini yüklerken gerçekleştirmek için bu seçeneği belirtmeniz gerekir böylece MD5 onay varsayılan olarak kapalıdır.

Azure depolama blob veya dosya için depolanan MD5 karma değeri güncel olduğunu garanti etmez unutmayın. Blob veya dosya değiştirildiğinde MD5 güncelleştirme istemcinin sorumluluğundadır. Disk görüntülerini (yönetilen veya yönetilmeyen diskler), söz konusu olduğunda Azure Vm'leri MD5 değeri disk içeriğinin değişiklik olarak güncelleştirilmiyor, bu nedenle /CheckMD5 hata disk görüntüleri indirirken oluşturmaz.

AzCopy v8 hizmete yüklemeden sonra her zaman bir Azure blob veya dosya için içerik MD5 özelliği ayarlar.  

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="snapshot"></a>/ Anlık görüntü

Anlık görüntüleri aktarmak görüntülenip görüntülenmeyeceğini gösterir. Bu seçenek, yalnızca kaynak, bir blob olduğunda geçerlidir.

Aktarılan blob anlık görüntüleri şu biçimde adlandırılır: .extension blob-name (anlık görüntü-time)

Varsayılan olarak, anlık görüntüler kopyalanmaz.

**İçin uygulanabilir:** Bloblar

### <a name="vverbose-log-file"></a>/ V: [ayrıntılı-günlük dosyası]

Bir günlük dosyasına çıkışları ayrıntılı durum iletileri.

Varsayılan olarak, ayrıntılı günlük dosyası içinde AzCopyVerbose.log adlı `%LocalAppData%\Microsoft\Azure\AzCopy`. Var olan bir dosya konumu için bu seçeneği belirtirseniz, o dosya için ayrıntılı günlük eklenir.  

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="zjournal-file-folder"></a>/Z:[journal-file-folder]

Bir işlemi sürdürülüyor bir günlük dosyası klasörü belirtir.

AzCopy, her zaman bir işlem kesildiğinde sürdürme destekler.

Bu seçenek belirtilmezse veya bir klasör yolu belirtildi AzCopy günlük dosyası % LocalAppData%\Microsoft\Azure\AzCopy olan varsayılan konumda oluşturur.

AzCopy için bir komut dağıttığınız her saat bir günlük dosyası varsayılan klasör var olup veya bu seçeneği belirtilen klasörde mevcut olup olmadığını denetler. Günlük dosyası ya da yerinde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı komut satırında günlük dosyası ile eşleşip eşleşmediğini denetler. AzCopy komut satırlarını eşleşirse, işlem tamamlanmadı devam ettirir. Bunlar eşleşmiyorsa, yeni bir işlem başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyası ya da üzerine istenir.

Günlük dosyası, işlemin başarıyla tamamlanmasından sonra silinir.

AzCopy önceki bir sürümünde oluşturulmuş bir günlük dosyasından bir işlemi sürdürülüyor desteklenmediğini unutmayın.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="parameter-file"></a>/@:"Parameter-File"

Parametreler içeren bir dosyayı belirtir. Komut satırında, sadece belirtilmiş gibi AzCopy dosyasındaki parametreleri işler.

Bir yanıt dosyası içinde tek bir satırda birden çok parametreleri belirtin veya kendi satırında her bir parametre belirtin. Tek bir parametre birden fazla satır yayılamaz unutmayın.

Yanıt dosyaları # sembolü ile başlayan açıklamaları satır içerebilir.

Birden çok yanıt dosyaları belirtebilirsiniz. Ancak, AzCopy iç içe geçmiş yanıt dosyaları desteklemediğini unutmayın.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="y"></a>/Y

Tüm AzCopy onay istemlerini bastırır. /XO ve /XN belirlenmediğinde bu seçenek ayrıca salt yazılır SAS belirteçlerini veri karşıya yükleme senaryoları için izin verir.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="l"></a>/ L

Bir listeleme işlemi yalnızca belirtir. hiçbir veri kopyalanır.

AzCopy yorumlar birini kullanarak bir simülasyonu çalıştırmaya yönelik olarak bu seçenek olmadan bu komut satırı seçeneği/l ve sayıları kaç nesneler kopyalanır, hangi nesnelerin denetlemek için aynı anda /V Ayrıntılı günlüğe kopyalanır seçeneği belirtebilirsiniz.

Bu seçeneğin davranışı, ayrıca kaynak verilerin konumu ve yinelemeli modu seçeneği /S ve dosya deseni seçeneği /Pattern varlığını göre belirlenir.

Bu seçeneği tercih edildiğinde, AzCopy bu kaynak konumunun, liste ve okuma izni gerektirir.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="mt"></a>/MT

Kaynak blob veya dosya olarak aynı olacak şekilde indirilen dosyanın son değiştirilme saati ayarlar.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="xn"></a>/XN

Daha yeni bir kaynak kaynak dışlar. Kaynağın son değiştirildiği tarihi aynı ise kaynak kopyalanmaz veya hedef daha yeni.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="xo"></a>/XO
Eski bir kaynak kaynak dışlar. Kaynağın son değiştirildiği tarihi aynı ise kaynak kopyalanır değil ya da hedef daha eski.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="a"></a>/A

Arşiv özniteliği olan dosyaları karşıya yükler.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="iarashcnetoi"></a>/IA:[RASHCNETOI]

Belirtilen öznitelikler kümesi olan dosyaları karşıya yükler.

Kullanılabilir öznitelikler içerir:

* R = Salt okunur dosyaları
* Bir = dosyalarını arşivlemek için hazır
* S sistem dosyaları =
* H = Gizli dosyalar
* C Sıkıştırılmış dosyalar =
* N = Normal dosyaları
* E şifreli dosyaları =
* T = geçici dosyalar
* O çevrimdışı dosyalar =
* Ben dışındaki dizin dosyaları =

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="xarashcnetoi"></a>/XA:[RASHCNETOI]

Belirtilen öznitelik kümeleri hiçbirini dosyaları hariç tutar.

Kullanılabilir öznitelikler içerir:

* R = Salt okunur dosyaları
* Bir = dosyalarını arşivlemek için hazır
* S sistem dosyaları =
* H = Gizli dosyalar
* C Sıkıştırılmış dosyalar =
* N = Normal dosyaları
* E şifreli dosyaları =
* T = geçici dosyalar
* O çevrimdışı dosyalar =
* Ben dışındaki dizin dosyaları =

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="delimiterdelimiter"></a>/Delimiter:"delimiter"

Blob adındaki sanal dizinleri sınırlandırmak için kullanılan sınırlayıcı karakter gösterir.

Sınırlayıcı karakter olarak / varsayılan olarak, AzCopy kullanır. Ancak, ortak hiçbir karakteri kullanarak AzCopy destekler (gibi @, # ya da %) ayırıcı olarak. Komut satırında şu özel karakterlerden birini eklemek gereken dosya adı çift tırnak içine alın.

Bu seçenek, yalnızca BLOB'lar indiriliyor için geçerlidir.

**İçin uygulanabilir:** Bloblar

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "sayı-ın-eşzamanlı-işlemler"

Eşzamanlı işlemlerin sayısını belirtir.

AzCopy varsayılan olarak, belirli bir veri aktarımı verimliliğini artırmak için eşzamanlı işlemlerin sayısını başlatır. Çok fazla sayıda eş zamanlı işlem düşük bant genişlikli ortamında ve ağ bağlantısı doldurmaya tamamen tamamlanmasını işlemlerini önlemek unutmayın. Eşzamanlı işlemlerin gerçek kullanılabilir ağ bant genişliğine göre kısıtlama.

Eşzamanlı işlemlerin üst sınırı 512'dır.

**İçin uygulanabilir:** Bloblar, dosyalar, tablolar

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tablo"

Belirten `source` kaynaktır bir blob depolama öykünücüsünde çalışan yerel geliştirme ortamında kullanılabilir.

**İçin uygulanabilir:** Bloblar, tablolar

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tablo"

Belirten `destination` kaynaktır bir blob depolama öykünücüsünde çalışan yerel geliştirme ortamında kullanılabilir.

**İçin uygulanabilir:** Bloblar, tablolar

### <a name="pkrskey1key2key3"></a>/PKRS:"key1#key2#key3#..."

Tablo verilerini dışarı aktarma işlemi hızını artıran paralel olarak dışarı aktarma özelliğini etkinleştirmesi için bölüm anahtar aralığı böler.

Bu seçenek belirtilmezse, AzCopy tablo varlıklarını dışarı aktarmak için tek bir iş parçacığı kullanır. Örneğin, kullanıcı /PKRS belirtir: "aa #bb" daha sonra AzCopy üç eşzamanlı işlem başlar.

Her işlem, aşağıda gösterildiği gibi üç bölüm anahtar aralığı, biri verir:

  [ilk bölüm anahtarı, aa)

  [aa, bb)

  [bb, geçen bölüm anahtarı]

**İçin uygulanabilir:** Tablolar

### <a name="splitsizefile-size"></a>/ SplitSize: "dosya boyutu"

Dışarı aktarılan dosyanın bölme boyutu MB, izin verilen minimum değer 32'dir belirtir.

Bu seçenek belirtilmezse, AzCopy tablo verilerini tek bir dosyaya dışarı aktarır.

Bu seçenek belirtilmemiş olsa bile bir blob'a tablo verilerini dışarı aktarılır ve blob boyutu 200 GB sınırını dışarı aktarılan dosya boyutu, AzCopy dışarı aktarılan dosyanın böler.

**İçin uygulanabilir:** Tablolar

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "Yerleştir veya Değiştir"

Tablo veri alma davranışını belirtir.

* InsertOrSkip - var olan bir varlığa atlar veya tablodaki mevcut değilse yeni bir varlık ekler.
* InsertOrMerge - var olan bir varlığa birleştirir veya tablodaki mevcut değilse yeni bir varlık ekler.
* Insertorreplace - var olan bir varlığa değiştirir veya tablodaki mevcut değilse yeni bir varlık ekler.

**İçin uygulanabilir:** Tablolar

### <a name="manifestmanifest-file"></a>/ MANIFEST: "bildirim dosyası"

Tabloyu dışarı aktarma ve içeri aktarma işlemi için bildirim dosyasını belirtir.

Bu seçenek dışarı aktarma işlemi sırasında isteğe bağlı olduğundan, bu seçenek belirtilmezse, AzCopy önceden tanımlanmış bir ada sahip bir bildirim dosyası oluşturur.

Bu seçenek, veri dosyalarını bulmak için içeri aktarma işlemi sırasında gereklidir.

**İçin uygulanabilir:** Tablolar

### <a name="synccopy"></a>/ SyncCopy

BLOB'ları veya iki Azure depolama uç noktaları arasında dosyaları zaman uyumlu olarak kopyalanıp kopyalanmayacağını belirtir.

AzCopy varsayılan olarak, sunucu tarafı zaman uyumsuz kopya kullanır. Yerel belleğe BLOB veya dosyalar indirir ve ardından bunları Azure Depolama'ya yükler bir zaman uyumlu bir kopyalama işlemini gerçekleştirmek için bu seçeneği belirtin.

Blob Depolama, dosya depolama içinde ya da dosya depolamaya veya Blob Depolama içinde dosya kopyalarken bu seçeneği kullanabilirsiniz.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="setcontenttypecontent-type"></a>/SetContentType:"content-type"

Hedef BLOB veya dosyalar için MIME içerik türünü belirtir.

AzCopy, uygulama/octet-akış için blob veya dosya için içerik türü varsayılan olarak ayarlar. İçerik türü tüm BLOB veya dosyalar için bu seçenek için bir değer belirterek açıkça ayarlayabilirsiniz.

Bu seçenek olmadan bir değer belirtirseniz, AzCopy her blob veya dosyanın içerik türü dosya uzantısını göre ayarlar.

**İçin uygulanabilir:** Bloblar, dosyalar

### <a name="payloadformatjson--csv"></a>/PayloadFormat:"JSON" | "CSV"

Tablo dışarı aktarılan verileri dosyasının biçimini belirtir.

Bu seçenek belirtilmezse, varsayılan olarak AzCopy tablo veri dosyası JSON biçiminde dışa aktarır.

**İçin uygulanabilir:** Tablolar

## <a name="known-issues-and-best-practices"></a>Bilinen sorunlar ve en iyi uygulamalar

Bazı bilinen sorunlar ve en iyi bir göz atalım.

### <a name="limit-concurrent-writes-while-copying-data"></a>Veri kopyalama sırasında eş zamanlı yazma sınırı

AzCopy ile dosyaları veya BLOB'ları kopyaladığınızda, bunu kopyalamaya çalışırken, başka bir uygulama verileri değiştirme aklınızda bulundurun. Mümkünse, Kopyalamakta olduğunuz veri kopyalama işlemi sırasında değiştirilmeyen emin olun. Örneğin, bir Azure sanal makinesiyle ilişkili bir VHD kopyalama yapılırken, başka bir uygulama için VHD'yi şu anda yazıyorsanız emin olun. Bunu yapmak için iyi bir Kopyalanacak kaynak kiralama tarafından yoludur. Alternatif olarak, ilk VHD anlık görüntüsünü oluşturun ve sonra anlık görüntüyü kopyalayın.

Diğer uygulamaların bloblar ya da dosyalara yazma sırasında bunlar kopyalanan önleyemez, ardından projenin bittiği zamanı tarafından kopyalanan kaynakları artık kaynak kaynaklarla tam eşlik gerekebileceğini aklınızda bulundurun.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>MD5 FIPS uyumlu algoritmaları etkinleştirmek için AzCopy tıklattığınızda, "şifreleme, karma ve imza için kullanım FIPS uyumlu algoritmaları."

AzCopy varsayılan olarak .NET MD5 uygulaması MD5 nesneleri kopyalarken hesaplamak için kullanır. ancak AzCopy FIPS uyumlu MD5 ayarı etkinleştirmek için gereken bazı güvenlik gereksinimleri vardır.

Bir app.config dosyası oluşturabilirsiniz `AzCopy.exe.config` özelliğiyle `AzureStorageUseV1MD5` ve AzCopy.exe ile kenara koyun.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

"AzureStorageUseV1MD5" özelliği için:

* TRUE - varsayılan değer, AzCopy .NET MD5 uygulaması kullanır.
* Yanlış – AzCopy MD5 FIPS uyumlu algoritma kullanır.

FIPS uyumlu algoritmalar Windows üzerinde varsayılan olarak devre dışı bırakılır. Bu ilke ayarı, makinenizde değiştirebilirsiniz. (Windows + R) Çalıştır penceresinde açmak için secpol.msc yazın **yerel güvenlik ilkesi** penceresi. İçinde **güvenlik ayarları** penceresinde gidin **güvenlik ayarları** > **yerel ilkeler** > **güvenlikseçenekleri**. Bulun **Sistem şifrelemesi: Şifreleme, karma ve imza için FIPS uyumlu algoritmaları kullan** ilkesi. Görüntülenen değer görmek için ilkeye çift **güvenlik ayarı** sütun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Depolama ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure depolama belgeleri:
* [Azure Depolama’ya giriş](../storage-introduction.md)
* [Net'ten BLOB storage kullanma](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Dosya depolama'yı .NET kullanma](../storage-dotnet-how-to-use-files.md)
* [Tablo depolama'yı .NET kullanma](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Oluşturma, yönetme veya bir depolama hesabını Sil](../storage-create-storage-account.md)
* [Linux üzerinde AzCopy ile veri aktarma](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Azure depolama blog gönderilerini:
* [Azure depolama veri taşıma kitaplığı Önizleme Tanıtımı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Zaman uyumlu kopya ve özelleştirilmiş içerik türü ile tanışın](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Genel kullanılabilirlik, AzCopy 3.0 Duyurusu yanı sıra tablo ve dosya desteği AzCopy 4.0 sürümü önizlemesi](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Büyük ölçekli kopyalama senaryoları için iyileştirilmiş](https://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Okuma erişimli coğrafi olarak yedekli depolama için destek](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Yeniden başlatılabilir modu ve SAS belirteci ile veri aktarma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Çapraz-hesap kopya blob'u kullanma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Azure blobları karşıya yükleme/indirme dosyaları](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
