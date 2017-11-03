---
title: "Kopyalama veya Windows Azure Storage ile AzCopy için veri taşıma | Microsoft Docs"
description: "AzCopy üzerinde bir Windows yardımcı programı taşımak veya için veya blob, tablo ve dosya içeriği veri kopyalamak için kullanın. Verileri Azure depolama birimine yerel dosyalarından kopyalamak veya içinde veya depolama hesapları arasında veri kopyalama. Kolayca verilerinizi Azure depolama alanına geçiş."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 1a4c52babe76e59eacb30e8be91ed934cdbe305b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a>Windows AzCopy ile veri aktarımı
AzCopy en uygun performans ile basit komutları kullanarak Microsoft Azure Blob, dosya ve tablo depolama gelen ve giden veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır. Verileri bir nesneden diğerine depolama hesabınızda veya depolama hesapları arasında kopyalayabilirsiniz.

İndirebilirsiniz AzCopy iki sürümü vardır. AzCopy Windows .NET Framework ile oluşturulur ve Windows stili komut satırı seçenekleri sunar. [Linux üzerinde AzCopy](storage-use-azcopy-linux.md) çekirdek, POSIX stili komut satırı seçenekleri sunan Linux platformlar hedefler ile .NET Framework yerleşik olarak bulunur. Bu makalede Windows AzCopy kapsar.

## <a name="download-and-install-azcopy-on-windows"></a>AzCopy Windows yükleyip

Karşıdan [AzCopy Windows en son sürümünü](http://aka.ms/downloadazcopy).

Windows Installer kullanarak AzCopy yükledikten sonra bir komut penceresi açın ve bilgisayarınızda - AzCopy yükleme dizinine gidin nerede `AzCopy.exe` yürütülebilir bulunur. İsterseniz, AzCopy yükleme konumu sistem yoluna ekleyin. AzCopy varsayılan olarak, yüklü `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` veya `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>İlk AzCopy komut yazma

AzCopy komutları temel sözdizimi aşağıdaki gibidir:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

Aşağıdaki örnekler, çeşitli veri ve Microsoft Azure BLOB'ları, dosyaları ve tablolardan kopyalama için senaryolarda göstermektedir. Başvurmak [AzCopy parametreleri](#azcopy-parameters) her örnekte kullanılan parametreleri ayrıntılı bir açıklaması için bölüm.

## <a name="download-blobs-from-blob-storage"></a>Blob depolama alanından BLOB'ları indirme

AzCopy kullanarak blob'lara indirmek için çeşitli yollardan bakalım.

### <a name="download-a-single-blob"></a>Tek bir blob indirin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Klasör unutmayın `C:\myfolder` yok, AzCopy oluşturur ve indirme `abc.txt ` yeni klasöre.

### <a name="download-a-single-blob-from-the-secondary-region"></a>Tek bir blob ikincil bölge ' indirin

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Okuma erişimli coğrafi olarak yedekli depolama ikincil bölge erişmek için etkin olması gerektiğini unutmayın.

### <a name="download-all-blobs-in-a-container"></a>Bir kapsayıcıdaki tüm blob'lara indirin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Aşağıdaki BLOB'ları belirtilen kapsayıcıda bulunan varsayın:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Dizin yükleme işleminden sonra `C:\myfolder` aşağıdaki dosyaları içerir:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Seçeneği belirtmezseniz, `/S`, blob yok indirilir.

### <a name="download-blobs-with-a-specific-prefix"></a>BLOB'lar belirli bir önek ile indirme

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Aşağıdaki BLOB'ları belirtilen kapsayıcıda bulunan varsayalım. Önek ile başlayan tüm BLOB'lar `a` yüklenir:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Klasör yükleme işleminden sonra `C:\myfolder` aşağıdaki dosyaları içerir:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Önek blob adı ilk bölümü forms bir sanal dizin geçerlidir. Yüklenmez şekilde yukarıda gösterilen örnekte, sanal dizin belirtilen bir önek eşleşmiyor. Ayrıca, varsa seçeneği `\S` belirtilmezse, AzCopy BLOB indirmek değil.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Kaynak BLOB olarak aynı olmalıdır dışarı aktarılan dosyaların son değiştirme zamanı ayarlama

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Son değiştiren bunların zamana dayalı indirme işlemi de BLOB'lar hariç tutabilirsiniz. Son değişiklik zamanı BLOB'lar dışlamak istiyorsanız, örneğin, aynı veya daha yeni hedef dosya eklemektir `/XN` seçeneği:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Son değişiklik zamanı BLOB'lar hariç tutmak istediğiniz ise, aynı veya daha eski hedef dosya ekleme `/XO` seçeneği:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="upload-blobs-to-blob-storage"></a>Blob depolama alanına BLOB yükleme

AzCopy kullanarak blob'lara karşıya yüklemek için çeşitli yollardan bakalım.

### <a name="upload-a-single-blob"></a>Tek bir blob karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy oluşturur ve dosyayı içine yükler.

### <a name="upload-a-single-blob-to-a-virtual-directory"></a>Bir sanal dizin için tek bir blob karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Belirtilen sanal dizin yoksa, AzCopy sanal dizin adını eklemek için dosyayı yükler (*örneğin*, `vd/abc.txt` Yukarıdaki örnekteki).

### <a name="upload-all-blobs-in-a-folder"></a>Bir klasördeki tüm BLOB'lar karşıya yükle

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Seçeneğini belirterek `/S` belirtilen dizin içeriğini tüm alt klasörleri ve bunların dosyaları da karşıya anlamı Blob Depolama yinelemeli olarak yükler. Örneğin, aşağıdaki dosyaları klasöründe bulunan varsayın `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Karşıya yükleme işleminden sonra kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Seçeneği belirtmezseniz, `/S`, AzCopy yinelemeli olarak karşıya değil. Karşıya yükleme işleminden sonra kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-blobs-matching-a-specific-pattern"></a>Belirli bir desen eşleştirme BLOB karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Aşağıdaki dosyaları klasöründe bulunan varsayın `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Karşıya yükleme işleminden sonra kapsayıcı aşağıdaki dosyaları içerir:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Seçeneği belirtmezseniz, `/S`, AzCopy yalnızca sanal bir dizinde bulunan değilsiniz BLOB'ları yükler:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Hedef blob MIME içerik türünü belirtin

Varsayılan olarak, içerik türü için bir hedef blob AzCopy ayarlar `application/octet-stream`. 3.1.0 sürümünden başlayarak, içerik türü seçeneği açıkça belirtebilirsiniz `/SetContentType:[content-type]`. Bu sözdiziminin bir karşıya yükleme işleminde tüm BLOB'lar için içerik türünü ayarlar.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Belirtirseniz `/SetContentType` olmayan bir değer, AzCopy her bir blob veya dosyanın içerik türü dosya uzantısını göre ayarlar.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="copy-blobs-in-blob-storage"></a>Blob depolama alanına BLOB kopyalama

BLOB'ları bir konumdan diğerine kopyalamak için çeşitli yollar bakalım AzCopy kullanma.

### <a name="copy-a-single-blob-from-one-container-to-another-within-the-same-storage-account"></a>Tek bir blob bir kapsayıcı başka aynı depolama hesabındaki kopyalama 

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Bir depolama hesabındaki blob kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-a-single-blob-from-one-storage-account-to-another"></a>Tek bir blob depolama hesabından kopyalayın

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Bir blob depolama hesapları arasında kopyaladığınızda, bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-a-single-blob-from-the-secondary-region-to-the-primary-region"></a>Tek bir blob ikincil bölgesinden birincil bölge kopyalayın.

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Okuma erişimli coğrafi olarak yedekli depolama ikincil depolama erişmek için etkin olması gerektiğini unutmayın.

### <a name="copy-a-single-blob-and-its-snapshots-from-one-storage-account-to-another"></a>Tek bir blob ve onun anlık görüntüler bir depolama hesabından kopyalayın

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Kopyalama işleminden sonra hedef kapsayıcı blob ve onun anlık görüntülerini içerir. Kapsayıcı, blob yukarıdaki örnekte iki anlık görüntülere sahip olduğu varsayılarak, aşağıdaki blob ve anlık görüntüleri içerir:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="copy-all-blobs-in-a-container-to-another-storage-account"></a>Tüm BLOB'ları bir kapsayıcıda başka bir depolama hesabına kopyalayın. 

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 
/Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /S
```

Seçeneğini belirterek /S Belirtilen kapsayıcı yinelemeli olarak içeriğini yükler. Bkz: [bir klasördeki tüm BLOB'lar karşıya](#upload-all-blobs-in-a-folder) daha fazla bilgi ve örnek.

### <a name="synchronously-copy-blobs-from-one-storage-account-to-another"></a>Zaman uyumlu olarak BLOB'ları bir depolama hesabından diğerine Kopyala

AzCopy varsayılan olarak, iki depolama uç noktaları arasında verileri zaman uyumsuz olarak kopyalar. Bu nedenle, kopyalama işlemi nasıl bakımından hiçbir SLA hızlı sahip yedek bant genişliğini kapasite kullanarak arka blob kopyalanır ve kopyalama tamamlandı başarısız oldu veya kadar AzCopy kopya durumunu düzenli olarak denetler. çalışır.

`/SyncCopy` Seçeneği sağlar kopyalama işlemi tutarlı hızı alır. AzCopy zaman uyumlu kopyası yerel bellek için belirtilen kaynak kopyalamak için BLOB'ları karşıdan yükleyip ardından Blob Depolama hedefe karşıya yükleme gerçekleştirir.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`zaman uyumsuz kopyaya karşılaştırıldığında ek çıkışı maliyeti oluşturabilir, çıkışı maliyeti önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'deki bu seçeneği kullanmak için önerilen yaklaşımdır.

## <a name="download-files-from-file-storage"></a>Dosya depolama biriminden dosyaları indirme

AzCopy kullanarak dosyaları indirmek için çeşitli yollardan bakalım.

### <a name="download-a-single-file"></a>Tek bir dosya indirme

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Belirtilen kaynak bir Azure dosya paylaşımıdır sonra ya da tam dosya adını belirtmeniz gerekir (*örneğin* `abc.txt`) tek bir dosya indirme veya seçenek belirtmek için `/S` paylaşımı yinelemeli olarak tüm dosyaları indirmek için. Bir dosya düzeni ve seçenek belirtmek çalışırken `/S` birlikte hatayla sonuçlanır.

### <a name="download-all-files-in-a-directory"></a>Bir dizindeki tüm dosyaları indirme

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Boş klasörleri indirilmez unutmayın.

## <a name="upload-files-to-an-azure-file-share"></a>Bir Azure dosya paylaşımına dosyaları karşıya yükleme

AzCopy kullanarak dosyaları karşıya yüklemek için çeşitli yollardan bakalım.

### <a name="upload-a-single-file"></a>Tek bir dosyayı karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files-in-a-folder"></a>Bir klasördeki tüm dosyaları karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Boş klasörler değil karşıya unutmayın.

### <a name="upload-files-matching-a-specific-pattern"></a>Belirli bir desen eşleştirme dosyaları karşıya yükleme

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="copy-files-in-file-storage"></a>Dosya depolama dosyaları Kopyala

AzCopy kullanarak, Azure dosya paylaşımı dosyaları kopyalamak için çeşitli yollar bakalım.

### <a name="copy-from-one-file-share-to-another"></a>Bir dosya paylaşımından başka kopyalama

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Dosya paylaşımlarında bir dosya kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-from-an-azure-file-share-to-blob-storage"></a>Bir Azure dosya paylaşımından Blob depolama alanına kopyalama

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Dosya blob öğesine dosya paylaşımından kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-a-blob-from-blob-storage-to-an-azure-file-share"></a>Bir blob Blob depolama alanından bir Azure dosya paylaşımına kopyalayın.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Bir dosya paylaşımı için bir blob üzerinden bir dosya kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="synchronously-copy-files"></a>Zaman uyumlu olarak dosyaları kopyalayın

Belirleyebileceğiniz `/SyncCopy` dosya depolama dosya depolama alanına, Blob Depolama birimine dosya depolama ve File Storage için Blob depolama biriminden verileri zaman uyumlu olarak kopyalamak için seçenek, AzCopy bu veri kaynağını yerel belleğe yükleyerek yapar ve hedefe yeniden yükleyin. Standart çıkış maliyet geçerlidir.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Dosya depolama biriminden Blob depolama alanına kopyalama işlemi sırasında varsayılan blob türü blok blobu olduğu; Kullanıcı seçeneğini belirtebilir `/BlobType:page` hedef blob türünü değiştirmek için.

Unutmayın `/SyncCopy` zaman uyumsuz kopyaya karşılaştırıldığında ek çıkışı maliyeti oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'de bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="export-data-from-table-storage"></a>Veri tablosu depodan dışarı aktarma

Verileri dışarı aktarma AzCopy kullanarak Azure Table depolama biriminden bir bakalım.

### <a name="export-a-table"></a>Bir tabloyu dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy bildirim dosyası belirtilen hedef klasöre yazar. Bildirim dosyası alma işleminde gerekli veri dosyalarını bulmak ve veri doğrulama gerçekleştirmek için kullanılır. Bildirim dosyası, varsayılan olarak aşağıdaki adlandırma kuralını kullanır:

    <account name>_<table name>_<timestamp>.manifest

Kullanıcı ayrıca seçeneğini belirtin `/Manifest:<manifest file name>` yayılma dosyası adı ayarlamak için.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-an-export-from-table-storage-into-multiple-files"></a>Tablo depolama biriminden bir verme birden çok dosyaya bölün

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy kullanan bir *birim dizin* birden çok dosya ayırt etmek için bölünmüş veri dosya adları. Birim dizini iki bölümden oluşur bir *bölüm anahtarı aralığının dizin* ve *bölünmüş dosya dizini*. Her iki dizinleri sıfır tabanlı.

Kullanıcı seçeneği belirlemezse bölüm anahtarı aralığının dizini 0'dır `/PKRS`.

Örneğin, AzCopy seçeneği kullanıcının belirttiği sonra iki veri dosyalarını oluşturur düşünelim `/SplitSize`. Sonuçta elde edilen veri dosya adları olabilir:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Mümkün olan en düşük değer seçenek için Not `/SplitSize` 32 MB'dir. Belirtilen hedef Blob Depolama alanı ise, kendi boyutları blob boyutu sınırlaması (200 GB) ulaştığında AzCopy veri dosyasını ayırır, bakılmaksızın olup seçeneği `/SplitSize` kullanıcı tarafından belirtilen.

### <a name="export-a-table-to-json-or-csv-data-file-format"></a>JSON veya CSV veri dosyası biçiminde bir tabloyu dışarı aktarma

Varsayılan olarak, AzCopy tablolar JSON veri dosyalarını dışarı aktarır. Seçeneğini belirtebilirsiniz `/PayloadFormat:JSON|CSV` tablolar JSON veya CSV dışarı aktarmak için.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

AzCopy CSV yük biçimi belirtirken, aynı zamanda dosya uzantısına sahip bir şema dosyası oluşturur. `.schema.csv` her veri dosyası için.

### <a name="export-table-entities-concurrently"></a>Tablo varlıkları eşzamanlı olarak dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy başlatır kullanıcı seçeneği belirttiğinde varlıklar dışarı aktarmak için eşzamanlı işlem `/PKRS`. Her bir işlemin bir bölüm anahtarı aralığının dışa aktarır.

Eşzamanlı işlem sayısını da seçeneği tarafından denetlendiğini unutmayın `/NC`. AzCopy kullandığı Çekirdek İşlemci sayısı varsayılan değer olarak `/NC` tablo varlıkları kopyalarken olsa bile `/NC` belirtilmedi. Kullanıcı seçeneği belirttiğinde `/PKRS`, AzCopy kullanan iki değerden daha küçük - bölüm anahtar aralıklarına karşı örtük veya açık olarak belirtilen eşzamanlı operasyonlar - çok başlatmak için eşzamanlı işlem sayısını belirler. Daha fazla ayrıntı için yazın `AzCopy /?:NC` komut satırında.

### <a name="export-a-table-to-blob-storage"></a>Blob depolama alanına bir tabloyu dışarı aktarma

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy bir JSON veri dosyası adlandırma kuralı aşağıdaki ile blob kapsayıcısı içinde oluşturur:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Oluşturulan JSON veri dosyası en küçük meta verileri için yük biçimi izler. Bu yük biçimi hakkında daha fazla bilgi için bkz: [tablo hizmeti işlemleri için yük biçimi](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Tablolar için BLOB'ları dışarı aktarılırken AzCopy tablo varlıkları yerel geçici veri dosyalarını indirir ve ardından bu varlıkların blob yükler olduğunu unutmayın. Bu geçici veri dosyalarını varsayılan yolu ile günlük dosyası klasöre yerleştirilir "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>" olarak seçeneği belirtebilirsiniz/değişiklik günlüğü için [günlük dosyası klasörü] Z: dosya klasör konumu ve böylece geçici veri dosyaları konumunu değiştirin. Blob için karşıya sonra yerel disk geçici veri dosyasında hemen silinir rağmen dosyalarının boyutu, tablo varlıklarınızı boyutu ve seçeneği /SplitSize ile belirtilen boyutu tarafından belirlenir geçici verileri Lütfen elinizde yeterince yerel sağlayın silinmeden önce bu geçici veri dosyalarını depolamak için disk alanı.

## <a name="import-data-into-table-storage"></a>Tablo depolama alanına veri al

AzCopy kullanarak Azure Table depolama alanına veri alma bir bakalım.

### <a name="import-a-table"></a>Tablo alma

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Seçenek `/EntityOperation` tabloya varlıklar ekleme gösterir. Olası değerler şunlardır:

* `InsertOrSkip`: Var olan bir varlığı atlar veya tabloda mevcut değilse yeni bir varlık ekler.
* `InsertOrMerge`: Birleştirir var olan bir varlığı veya tabloda mevcut değilse yeni bir varlık ekler.
* `InsertOrReplace`: Var olan bir varlığı değiştirir veya tabloda mevcut değilse yeni bir varlık ekler.

Seçeneği belirtemezsiniz Not `/PKRS` alma senaryoda. İçinde belirtmelisiniz seçeneği verme senaryo aksine `/PKRS` tablo içeri aktardığınızda eşzamanlı işlem başlatmak için AzCopy eşzamanlı operasyonlar varsayılan olarak başlatılır. Varsayılan eşzamanlı işlemleri başlatıldı çekirdekli işlemciler sayıya eşit sayısıdır; Bununla birlikte, eşzamanlı seçeneğiyle farklı sayıda belirtebilirsiniz `/NC`. Daha fazla ayrıntı için yazın `AzCopy /?:NC` komut satırında.

AzCopy yalnızca JSON değil CSV içe aktarma desteklediğini unutmayın. AzCopy değil kullanıcı tarafından oluşturulan JSON öğesinden tablo içeri aktarmalar destekler ve dosyaları bildirimi. Bu dosyaların her ikisinin bir AzCopy tablo verme etki alanından gelmesi gerekir. Hatalarını önlemek için lütfen dışarı aktarılan JSON değiştirmeyin veya bildirim dosyası.

### <a name="import-entities-into-a-table-from-blob-storage"></a>Blob depolama biriminden bir tabloya varlıkları İçeri Aktar

Bir Blob kapsayıcısını içeren aşağıdaki varsayalım: Azure tablo ve eşlik eden bildirim dosyasını temsil eden bir JSON dosyası.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Bu blob kapsayıcısında bildirim dosyası kullanarak bir tabloya varlıkları içeri aktarmak için aşağıdaki komutu çalıştırabilirsiniz:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Diğer AzCopy özellikleri

Diğer bir AzCopy özellikleri bir göz atalım.

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Hedefte mevcut olmayan veri Kopyala

`/XO` Ve `/XN` parametreleri, sırasıyla kopyalanmasını daha eski veya yeni kaynak kaynakları hariç tut izin verir. Yalnızca hedef yoksa kaynak kaynakları kopyalamak isterseniz, AzCopy komut parametrelerinin her ikisini de belirtebilirsiniz:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Kaynak veya hedef tablo olduğunda bu desteklenmediğini unutmayın.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Komut satırı parametrelerini belirtmek için bir yanıt dosyası kullanın

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

AzCopy komut satırı parametreleri bir yanıt dosyası içerebilir. Komut satırında belirtilmiş gibi AzCopy dosyasının içeriğiyle doğrudan bir değiştirme gerçekleştirme dosyasındaki parametreleri işler.

Adlı bir yanıt dosyası varsayın `copyoperation.txt`, aşağıdaki satırları içeren. Tek bir satıra her AzCopy parametresi belirtilebilir.

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

veya satırları ayrı:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy bozulursa parametresi iki satır arasında bölmek için aşağıda gösterildiği gibi `/sourcekey` parametre:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Komut satırı parametrelerini belirtmek için birden çok yanıt dosyası kullanın

Adlı bir yanıt dosyası varsayın `source.txt` kaynak kapsayıcı belirtir:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Ve adlı bir yanıt dosyası `dest.txt` bir hedef klasör dosya sistemindeki belirtir:

    /Dest:C:\myfolder

Ve adlı bir yanıt dosyası `options.txt` AzCopy seçeneklerini belirtir:

    /S /Y

Bu yanıt dosyaları ile AzCopy çağırmak için her biri bir dizinde bulunan `C:\responsefiles`, bu komutu kullanın:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

Tüm Bireysel parametreleri komut satırında eklenirse gibi AzCopy bu komut işler:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) belirtin

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Ayrıca, bir SAS kapsayıcısında URI belirtebilirsiniz:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Günlük dosyası klasörü

AzCopy için bir komut sorun her zaman varsayılan klasöründe bir günlük dosyası var olup var veya bu seçeneği belirtilen bir klasörde varolup denetler. Günlük dosyası her iki yerde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı günlük dosyası komut satırında eşleşip eşleşmediğini denetler. İki komut satırları eşleşirse, AzCopy tamamlanmamış işlemi sürdürür. Bunlar eşleşmiyorsa ya da yeni bir işlemi başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyasının üzerine istenir.

Varsayılan konumu için günlük dosyası kullanmak istiyorsanız:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Seçeneği unutursanız `/Z`, veya seçeneğini belirtin `/Z` klasör yolu, yukarıda gösterildiği gibi AzCopy günlük dosyası olan varsayılan konumda oluşturur `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Günlük dosyası zaten varsa, AzCopy günlük dosyasına dayalı işlemi sürdürür.

Günlük dosyası için özel bir konum belirtmek istiyorsanız:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Zaten yoksa, bu örnek günlük dosyası oluşturur. Mevcut değilse, AzCopy günlük dosyasına dayalı işlemi sürdürür.

AzCopy çalışmaya devam etmesini istiyorsanız:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Bu örnek tamamlamak için başarısız olan son işlem sürdürür.

### <a name="generate-a-log-file"></a>Bir günlük dosyası oluşturur

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Seçeneğini belirtirseniz `/V` ayrıntılı günlük dosya yoluna sağlamadan sonra AzCopy günlük dosyası olan varsayılan konumda oluşturur `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Aksi takdirde, özel bir konumda bir günlük dosyası oluşturabilirsiniz:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Seçenek aşağıdaki göreli bir yol belirtirseniz, unutmayın `/V`, gibi `/V:test/azcopy1.log`, ayrıntılı günlük adlı bir alt klasör içinde geçerli çalışma dizininde oluşturulduktan sonra `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Başlatmak için eşzamanlı işlem sayısını belirtin

Seçenek `/NC` eşzamanlı kopyalama işlemlerinin sayısını belirtir. Varsayılan olarak, belirli bir sayıda veri aktarımı verimliliğini artırmak için eşzamanlı işlem AzCopy başlatır. Tablo işlemleri için eşzamanlı işlem sayısını elinizde işlemci sayısına eşittir. BLOB ve dosya işlemleri, eşzamanlı işlem sayısını için sahip olduğunuz işlemcilerin sayısı 8 kat eşit. Düşük bant genişlikli ağ üzerinden AzCopy çalıştırıyorsanız, daha düşük bir sayı /NC tarafından kaynak rekabet nedeniyle başarısız olmaması için belirtebilirsiniz.

### <a name="run-azcopy-against-the-azure-storage-emulator"></a>AzCopy Azure Storage öykünücüsüne karşı çalıştırma

AzCopy karşı çalıştırabilirsiniz [Azure Storage öykünücüsü](storage-use-emulator.md) BLOB'lar için:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

Ayrıca tablolar için çalıştırabilirsiniz:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>AzCopy parametreleri

AzCopy için Parametreler aşağıda açıklanmıştır. Yardım için komut satırından aşağıdaki komutlardan birini, AzCopy kullanarak da yazabilirsiniz:

* AzCopy için ayrıntılı komut satırı Yardım için:`AzCopy /?`
* Herhangi bir AzCopy parametre ile ilgili ayrıntılı yardım için:`AzCopy /?:SourceKey`
* Komut satırı örnekleri için:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Kaynak: "kaynak"

Kopyalanacak kaynak verileri belirtir. Kaynak dosya sistemi dizini, bir blob kapsayıcı, blob sanal dizin, bir depolama dosya paylaşımı, bir depolama dosyası dizini veya bir Azure tablosu olabilir.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="destdestination"></a>/ Taşınmaya: "hedef"

Kopyalamak için hedef belirtir. Hedef dosya sistemi dizini, bir blob kapsayıcı, blob sanal dizin, bir depolama dosya paylaşımı, bir depolama dosyası dizini veya bir Azure tablosu olabilir.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="patternfile-pattern"></a>/ Desen: "dosya deseni"

Kopyalamak için hangi dosyaların gösteren bir dosya düzeni belirtir. /Pattern parametresi davranışını kaynak verilerin konumu ve yinelemeli modu seçeneği varlığını tarafından belirlenir. /S. seçeneği belirtilen Özyinelemeli modu

Belirtilen kaynak dosya sisteminde bir dizin ise, standart joker karakterler yürürlükte olan ve dizin içindeki dosyaları sağlanan dosya desen eşleştirme. /S Belirtilen seçeneği sonra AzCopy de karşı dizini altındaki tüm alt klasörlerdeki tüm dosyaları belirtilen desenle eşleşiyorsa.

Belirtilen kaynak blob kapsayıcısı veya sanal dizin ise, joker karakter uygulanmaz. Seçenek sonra AzCopy /S belirtilirse, belirtilen dosya düzeni blob önek olarak yorumlar varsa. /S belirtilmezse, seçeneği sonra AzCopy tam blob adlarıyla dosya desen eşleşirse.

Belirtilen kaynak bir Azure dosya paylaşımıdır sonra gerekir ya da tek bir dosya kopyalamak için tam dosya adını (örneğin abc.txt) belirtin veya seçeneğini belirtin /S paylaşımı yinelemeli olarak tüm dosyaları kopyalayın. Her iki bir dosya düzeni ve seçeneği /S birlikte sonuçlar hata belirleyin çalışılıyor.

AzCopy / Source bir blob kapsayıcısı veya blob sanal dizin ve büyük küçük harf duyarsız tüm diğer durumlarda eşleşen kullanması durumunda büyük küçük harfe duyarlı eşleşen kullanır.

Hiçbir dosya deseni belirtildiğinde kullanılan varsayılan dosya Düzen *.* bir dosya sistemi konumu veya bir Azure depolama konumu için boş bir önek. Birden çok dosya desenlerinin belirtilmesi desteklenmiyor.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="destkeystorage-key"></a>/ DestKey: "depolama anahtar"

Hedef kaynak için depolama hesabı anahtar belirtir.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="destsassas-token"></a>/ DestSAS: "sas belirteci"

(Eğer varsa) hedef için okuma ve yazma izinlerine sahip paylaşılan erişim imzası (SAS) belirtir. İçerdiğinden özel komut satırı karakter çift tırnak işareti ile SAS koyun.

Hedef kaynak, bir blob kapsayıcısını, dosya paylaşımı veya tablo ise, bu seçenek SAS belirteci tarafından izlenen ya da belirtebilirsiniz veya hedef blob kapsayıcısını, dosya paylaşımı veya tablonun URI, bu seçeneği olmadan bir parçası olarak SAS belirtebilirsiniz.

Kaynak ve hedef hem de blob'ları varsa, hedef blob aynı kaynak blob depolama hesabı içinde bulunmalıdır.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="sourcekeystorage-key"></a>/ SourceKey: "depolama anahtar"

Kaynak kaynak için depolama hesabı anahtar belirtir.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas belirteci"

Kaynak için okuma ve liste izinlerine sahip paylaşılan erişim imzası (varsa) belirtir. İçerdiğinden özel komut satırı karakter çift tırnak işareti ile SAS koyun.

Bir blob kapsayıcısını kaynak kaynak ise ve bir anahtar ya da SAS sağlanan blob kapsayıcısında anonim erişim okuyun.

Kaynak dosya paylaşımı veya tablo ise, bir anahtar veya bir SAS sağlanmalıdır.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="s"></a>/ S

Kopyalama işlemleri için özyinelemeli modunu belirtir. Özyinelemeli modunda AzCopy tüm BLOB'ları veya alt klasörler de dahil olmak üzere belirtilen dosya desenle eşleşen dosyaları kopyalar.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blok" | "Sayfa" | "Ekle"

Hedef blob blok blobu, bir sayfa blob'u ya da bir ek blobu olup olmadığını belirtir. Bu seçenek yalnızca bir blob karşıya yüklenirken kullanılır. Aksi takdirde bir hata oluşturulur. Hedef blob ise ve bu seçenek, varsayılan olarak, belirtilmemiş bir blok blobu AzCopy oluşturur.

**Uygulanabilir:** BLOB'ları

### <a name="checkmd5"></a>/ CheckMD5

Karşıdan yüklenen veriler için MD5 karma değeri hesaplar ve MD5 karma değeri blob içinde depolanan veya hesaplanan karma dosyanın içeriği MD5 özelliği eşleşen doğrular. MD5 denetimi verilerini yüklerken gerçekleştirmek için bu seçeneği belirtmeniz gerekir böylece MD5 onay varsayılan olarak kapalıdır.

Azure Storage blob veya dosya için depolanan MD5 karma değeri güncel olduğunu garanti etmez unutmayın. Blob veya dosya değiştirildiğinde MD5 güncelleştirmesi istemcinin sorumluluğundadır.

AzCopy hizmete karşıya sonra bir Azure blob veya dosya için Content-MD5 özelliği her zaman ayarlar.  

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="snapshot"></a>/ Anlık görüntü

Anlık görüntüler aktarmayı gösterir. Bu seçenek, yalnızca kaynak blob olduğunda geçerlidir.

Aktarılan blob anlık görüntüler şu biçimde adlandırılır: blob adı (anlık görüntü-time) .extension

Varsayılan olarak, anlık görüntüleri kopyalanmaz.

**Uygulanabilir:** BLOB'ları

### <a name="vverbose-log-file"></a>/ V: [verbose-günlük dosyası]

Bir günlük dosyasına çıkarır ayrıntılı durum iletileri.

Varsayılan olarak, içinde AzCopyVerbose.log adlı ayrıntılı günlük dosyası `%LocalAppData%\Microsoft\Azure\AzCopy`. Varolan bir dosya konumu için bu seçeneği belirtirseniz, ayrıntılı günlük bu dosyaya eklenir.  

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="zjournal-file-folder"></a>/ Z: [günlük dosyası klasörü]

Bir işlemi sürdürme bir günlük dosyası klasörü belirtir.

AzCopy her zaman bir işlem yarıda kesildi durumunda sürdürme destekler.

Bu seçenek belirtilmezse ya da bir klasör yolu belirtilen AzCopy günlük dosyası % LocalAppData%\Microsoft\Azure\AzCopy olan varsayılan konumda oluşturur.

AzCopy için bir komut sorun her zaman varsayılan klasöründe bir günlük dosyası var olup var veya bu seçeneği belirtilen bir klasörde varolup denetler. Günlük dosyası her iki yerde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı günlük dosyası komut satırında eşleşip eşleşmediğini denetler. İki komut satırları eşleşirse, AzCopy tamamlanmamış işlemi sürdürür. Bunlar eşleşmiyorsa ya da yeni bir işlemi başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyasının üzerine istenir.

Günlük dosyası işlemi başarıyla tamamlandıktan sonra silinir.

AzCopy önceki bir sürümü tarafından oluşturulmuş bir günlük dosyasından bir işlemi sürdürme desteklenmediğini unutmayın.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="parameter-file"></a>/@:"Parameter-File"

Parametreleri içeren dosyayı belirtir. Komut satırında bunlar yalnızca belirtilmiş gibi AzCopy dosyasındaki parametreleri işler.

Bir yanıt dosyası, tek bir satıra birden çok parametreleri belirtin veya kendi satırında her parametre belirtin. Tek bir parametre birden çok satıra yayılamaz unutmayın.

Yanıt dosyaları # sembolü ile başlayan açıklamaları satırları içerebilir.

Birden çok yanıt dosyaları belirtebilirsiniz. Ancak, AzCopy iç içe yanıt dosyaları desteklemiyor unutmayın.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="y"></a>/Y

Tüm AzCopy onay komut istemlerini bastırır.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="l"></a>/L

Yalnızca bir listeleme işlemi belirtir; hiçbir veri kopyalanır.

AzCopy yorumlar birini kullanarak bu seçeneği olarak çalıştırmak için bir benzetimi bu olmadan komut satırı seçeneği /L ve kaç tane nesneleri kopyalanır sayılar, hangi nesnelerin denetlemek için aynı anda /V Ayrıntılı günlüğe kopyalanır seçeneği belirtebilirsiniz.

Bu seçenek davranışını Ayrıca kaynak verilerin konumu ve yinelemeli modu seçeneği/s ve dosya düzeni seçeneği /Pattern varlığını tarafından belirlenir.

AzCopy, bu seçenek kullanıldığında bu kaynak konumunun listesi ve okuma izni gerektirir.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="mt"></a>/ MT

Aynı kaynak blob veya dosya indirilen dosyanın son değişiklik süreyi ayarlar.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="xn"></a>/XN

Daha yeni bir kaynak kaynak dışlar. Son değişiklik zamanını kaynağının aynı ise kaynak kopyalanmaz veya hedef daha yeni.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="xo"></a>/XO
Eski bir kaynak kaynak dışlar. Son değişiklik zamanını kaynağının aynı ise kaynak kopyalanmaz veya hedef daha eski.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="a"></a>/A

Yalnızca arşiv özniteliği kümesine sahip dosyaları karşıya yükleme.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Belirtilen öznitelikler kümesi olan dosyaları karşıya yükler.

Mevcut öznitelikleri şunlardır:

* R Salt okunur dosyalar =
* Arşivlenmeye hazır dosyalar bir =
* S sistem dosyaları =
* H Gizli dosyalar =
* C Sıkıştırılmış dosyaları =
* N Normal dosyalar =
* E Şifrelenmiş dosyaları =
* T = geçici dosyalar
* O çevrimdışı dosyalar =
* I dosyaları olmayan dizini =

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Belirtilen öznitelikler kümesi olan dosyaları dışlar.

Mevcut öznitelikleri şunlardır:

* R Salt okunur dosyalar =
* Arşivlenmeye hazır dosyalar bir =
* S sistem dosyaları =
* H Gizli dosyalar =
* C Sıkıştırılmış dosyaları =
* N Normal dosyalar =
* E Şifrelenmiş dosyaları =
* T = geçici dosyalar
* O çevrimdışı dosyalar =
* I dosyaları olmayan dizini =

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="delimiterdelimiter"></a>/ Sınırlayıcı: "sınırlayıcı"

Bir blob adındaki sanal dizinleri sınırlandırmak için kullanılan sınırlayıcı karakter gösterir.

Varsayılan olarak, AzCopy kullanır / sınırlayıcı karakter olarak. Ancak, ortak bir karakterle kullanarak AzCopy destekler (gibi @, # ya da %) ayırıcı olarak. Komut satırında şu özel karakterleri birini içeren ihtiyacınız varsa, bir dosya adı ile çift tırnak içine alın.

Bu seçenek, yalnızca BLOB indirmek için geçerlidir.

**Uygulanabilir:** BLOB'ları

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "numarası-in-eşzamanlı-işlemler"

Eşzamanlı işlem sayısını belirtir.

AzCopy varsayılan olarak, belirli bir sayıda veri aktarımı verimliliğini artırmak için eşzamanlı işlem başlatır. Çok sayıda eş zamanlı görevleri bir düşük bant genişlikli ortamında ve ağ bağlantısını doldurmaya tam olarak tamamlanmasını işlemlerini önlemek unutmayın. Eşzamanlı operasyonlar gerçek kullanılabilir ağ bant genişliğine bağlı kısıtlama.

Eşzamanlı operasyonlar için üst sınırı 512'dir.

**Uygulanabilir:** BLOB'lar, dosyalar, tablolar

### <a name="sourcetypeblob--table"></a>/ Kaynak türü: "Blob" | "Tablo"

Belirleyen `source` kaynaktır bir blob depolama öykünücüsünde çalıştıran yerel geliştirme ortamında kullanılabilir.

**Uygulanabilir:** BLOB'ların, tabloları

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tablo"

Belirleyen `destination` kaynaktır bir blob depolama öykünücüsünde çalıştıran yerel geliştirme ortamında kullanılabilir.

**Uygulanabilir:** BLOB'ların, tabloları

### <a name="pkrskey1key2key3"></a>/ PKRS: "anahtar&#1;key2 anahtar&#3;..."

Tablo verileri dışarı aktarma işlemi hızını artırır paralel dışarı aktarma etkinleştirmek için bölüm anahtarı aralığının böler.

Bu seçenek belirtilmezse, AzCopy tablo varlıkları vermek için tek bir iş parçacığı kullanır. Örneğin, kullanıcı /PKRS belirtir: "aa #bb" sonra AzCopy üç eşzamanlı işlem başlatır.

Her işlem, aşağıda gösterildiği gibi üç bölüm anahtarı aralıkları, birini verir:

  [ilk bölüm anahtarı, aa)

  [aa, bb)

  [bb, son bölüm anahtarı]

**Uygulanabilir:** tabloları

### <a name="splitsizefile-size"></a>/ SplitSize: "dosya boyutu"

32 MB, izin verilen minimum değer boyutu bölme dışarı aktarılan dosyayı olduğunu belirtir.

Bu seçenek belirtilmezse, AzCopy tablo verileri tek bir dosyaya dışa aktarır.

Bu seçenek belirtilmezse bile tablo verileri için blob aktarılır ve blob boyutu 200 GB sınırını dışarı aktarılan dosya boyutu, AzCopy dışarı aktarılan dosyayı ayırır.

**Uygulanabilir:** tabloları

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "Yerleştir veya Değiştir"

Tablo verileri alma davranışını belirtir.

* InsertOrSkip - var olan bir varlığı atlar veya tabloda mevcut değilse yeni bir varlık ekler.
* InsertOrMerge - var olan bir varlığı birleştirir veya tabloda mevcut değilse yeni bir varlık ekler.
* Yerleştir veya Değiştir - var olan bir varlığı değiştirir veya tabloda mevcut değilse yeni bir varlık ekler.

**Uygulanabilir:** tabloları

### <a name="manifestmanifest-file"></a>/ MANIFEST: "bildirim dosyası"

Tabloyu dışarı ve içeri aktarma işlemi için bildirim dosyasını belirtir.

Dışa aktarma işlemi sırasında bu seçenek isteğe bağlıdır, bu seçenek belirtilmezse, AzCopy önceden tanımlanmış ada sahip bir bildirim dosyası oluşturur.

Bu seçenek, veri dosyalarını bulmak için içeri aktarma işlemi sırasında gereklidir.

**Uygulanabilir:** tabloları

### <a name="synccopy"></a>/ SyncCopy

BLOB veya iki Azure Storage uç noktalar arasında dosyaları zaman uyumlu olarak kopyalamak gösterir.

AzCopy varsayılan olarak, sunucu tarafı zaman uyumsuz kopyayı kullanır. BLOB veya dosyaları için yerel bellek indirir ve bunları Azure depolama alanına yükler bir zaman uyumlu bir kopyasını gerçekleştirmek için bu seçeneği belirtin.

Blob storage, dosya depolama içinde veya Blob depolamadan dosya depolama (veya tersi) içinde dosya kopyalarken, bu seçeneği kullanabilirsiniz.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "content-type"

Hedef BLOB'ları veya dosyaları için MIME içerik türünü belirtir.

AzCopy içerik türü bir blob veya dosya için varsayılan olarak application/octet-stream, için ayarlar. Bu seçenek için bir değer açıkça belirterek tüm BLOB'ları veya dosyaları için içerik türü ayarlayabilirsiniz.

Bu seçenek olmadan bir değer belirtirseniz, AzCopy her bir blob veya dosyanın içerik türü dosya uzantısını göre ayarlar.

**Uygulanabilir:** BLOB'ların, dosyaları

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Tablo dışarı aktarılan veri dosyasının biçimi belirtir.

Bu seçenek belirtilmezse, varsayılan olarak AzCopy tablo veri dosyası JSON biçiminde dışa aktarır.

**Uygulanabilir:** tabloları

## <a name="known-issues-and-best-practices"></a>Bilinen sorunlar ve en iyi uygulamalar

Bazı bilinen sorunlar ve en iyi yöntemler bir göz atalım.

### <a name="limit-concurrent-writes-while-copying-data"></a>Veri kopyalama sırasında sınırı eşzamanlı yazma

BLOB veya AzCopy dosyalarıyla kopyaladığınızda, bunu kopyalamaya çalışırken, başka bir uygulama verileri değiştirme göz önünde bulundurun. Mümkünse, Kopyalamakta olduğunuz veri kopyalama işlemi sırasında değiştirilmeyen emin olun. Örneğin, bir Azure sanal makine ile ilişkili bir VHD'nin kopyalarken, başka bir uygulama şu anda VHD'ye yazıyorsanız emin olun. Bunu yapmak için iyi bir Kopyalanacak kaynak kiralama tarafından yoldur. Alternatif olarak, bir anlık görüntü VHD'yi ilk oluşturun ve sonra anlık görüntü kopyalayın.

Bunlar kopyalanan sırada BLOB veya dosyalar için yazma diğer uygulamaların önleyemez, ardından işi tamamlanana zamanına göre kopyalanan kaynakların artık kaynak kaynaklarla tam eşlik gerekebileceğini aklınızda bulundurun.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Bir Azcopy'i örnek bir makinede çalıştırın.

AzCopy veri aktarımını hızlandırmak için makine kaynak kullanımını en üst düzeye çıkarmak için tasarlanmış, yalnızca bir Azcopy'i örnek bir makinede çalıştırın ve seçeneğini belirtin öneririz `/NC` fazla eşzamanlı işlem gerekiyorsa. Daha fazla ayrıntı için yazın `AzCopy /?:NC` komut satırında.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS uyumlu MD5 algoritmaları etkinleştirmek için AzCopy olduğunda, "şifreleme, karma ve imzalama için kullan FIPS uyumlu algoritmaları."

AzCopy varsayılan olarak, MD5 nesneleri kopyalarken hesaplamak için .NET MD5 uygulama kullanır ancak AzCopy FIPS uyumlu MD5 ayarı etkinleştirmek için gereken bazı güvenlik gereksinimleri vardır.

Bir app.config dosyası oluşturabilirsiniz `AzCopy.exe.config` özelliğiyle `AzureStorageUseV1MD5` ve AzCopy.exe ile kenara yerleştirin.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

"AzureStorageUseV1MD5" özelliği için:

* TRUE - varsayılan değer, AzCopy .NET MD5 uygulama kullanır.
* Yanlış – AzCopy MD5 FIPS uyumlu algoritma kullanır.

FIPS uyumlu algoritmaları Windows üzerinde varsayılan olarak devre dışı bırakılır. Bu ilke ayarı, makinenizde değiştirebilirsiniz. (Windows + R) Çalıştır penceresinde açmak için secpol.msc yazın **yerel güvenlik ilkesi** penceresi. İçinde **güvenlik ayarları** penceresinde gidin **güvenlik ayarları** > **yerel ilkeler** > **güvenlikseçenekleri**. Bulun **Sistem şifrelemesi: şifreleme, karma ve imzalama için kullan FIPS uyumlu algoritmaları** ilkesi. Görüntülenen değeri görmek için ilkeye çift **güvenlik ayarı** sütun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Storage ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure Storage belgeleri:
* [Azure Storage'a giriş](../storage-introduction.md)
* [BLOB depolama alanından .NET kullanma](../blobs/storage-dotnet-how-to-use-blobs.md)
* [.NET dan File storage kullanma](../storage-dotnet-how-to-use-files.md)
* [Table storage .NET'kullanma](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Oluşturma, yönetme veya bir depolama hesabını silme](../storage-create-storage-account.md)
* [Linux'ta AzCopy ile veri aktarımı](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Azure depolama blog gönderileri:
* [Azure Storage veri hareketi kitaplığı Önizleme Tanıtımı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: zaman uyumlu kopyası ve özelleştirilmiş içerik türü tanışın](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Genel kullanılabilirlik, AzCopy 3.0 artı AzCopy 4.0 Önizleme sürümü tablo ve dosya desteği ile tanışın](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Büyük ölçekli kopyalama senaryoları için en iyi duruma getirilmiş](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Coğrafi olarak yedekli depolamaya okuma erişimi desteği](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: yeniden başlatılabilir modu ve SAS belirteci ile veri aktarımı](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: arası hesap kopyalama Blob kullanma](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Azure BLOB'ları karşıya yükleme ve indirme dosyaları](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)