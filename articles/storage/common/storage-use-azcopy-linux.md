---
title: Linux üzerinde Azure depolama AzCopy ile verileri taşıma veya kopyalama | Microsoft Docs
description: AzCopy yardımcı programından Linux taşımak veya için veya blob ve dosya içeriği veri kopyalamak için kullanın. Yerel dosyaları Azure depolama alanına veri kopyalama veya içinde veya depolama hesapları arasında verileri kopyalayabilirsiniz. Kolayca verilerinizi Azure Depolama'ya geçirin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/26/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 0f87645537576f49ee04b823341acf8853798f88
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730064"
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Linux üzerinde AzCopy ile veri aktarma

AzCopy, en iyi performans için tasarlanmış basit komut kullanarak Microsoft Azure Blob ve dosya depolama içine/dışına veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. Bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz.  

İndirebileceğiniz AzCopy iki sürümü vardır. Linux üzerinde AzCopy POSIX stili komut satırı seçeneklerini sunarak Linux platformlarını hedefler. [Windows üzerinde AzCopy](../storage-use-azcopy.md) Windows stili komut satırı seçenekleri sunar. Bu makalede, Linux üzerinde AzCopy yer almaktadır. 

> [!NOTE]  
> AzCopy 7.2 sürüm başlayarak, .NET Core bağımlılıkları AzCopy paket ile paketlenmiştir. 7.2 sürümünü kullanın veya daha sonra artık .NET Core bir önkoşul yüklemeniz gerekir

## <a name="download-and-install-azcopy"></a>AzCopy yükleyip

### <a name="installation-on-linux"></a>Linux'ta yükleme

> [!NOTE]
> Bu konuda vurgulanmış .NET Core 2.1 bağımlılıkları yüklemeniz gerekebilir [.NET Core önkoşulları makale](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x) dağıtımınıza bağlı olarak. 
>
> RHEL 7 dağıtımları için ICU ve libunwind bağımlılıklarını yükleyin: ```yum install -y libunwind icu```

Linux üzerinde AzCopy yükleme (v7.2 veya üzeri) tar paketi ayıklanıyor ve yükleme betiğini çalıştırmak kadar kolaydır. 

**RHEL 6 tabanlı dağıtımlar**: [indirme bağlantısı](https://aka.ms/downloadazcopylinuxrhel6)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinuxrhel6
tar -xf azcopy.tar.gz
sudo ./install.sh
```

**Diğer Linux dağıtımlarına**: [indirme bağlantısı](https://aka.ms/downloadazcopylinux64)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinux64
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Linux üzerinde AzCopy yüklendikten sonra ayıklanan dosyaları kaldırabilirsiniz. Alternatif olarak, süper kullanıcı ayrıcalıkları yoksa da çalıştırabilirsiniz `azcopy` ayıklanan klasöründe Kabuk betiği azcopy kullanarak.

### <a name="alternative-installation-on-ubuntu"></a>Ubuntu'da alternatif yükleme

**Ubuntu 14.04**

Microsoft Linux ürün depo için apt kaynak ekleyin ve AzCopy yükleyin:

```bash
sudo echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod/ trusty main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Microsoft Linux ürün depo için apt kaynak ekleyin ve AzCopy yükleyin:

```bash
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod/ xenial main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a>İlk, AzCopy komutuna yazma
AzCopy komutları için temel sözdizimi aşağıdaki gibidir:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

Aşağıdaki örnekler, veri ve Microsoft Azure BLOB'ları ve dosyaları kopyalamak için çeşitli senaryolar gösterir. Başvurmak `azcopy --help` menüsünde her örnekte kullanılan parametrelerden ayrıntılı bir açıklama.

## <a name="blob-download"></a>Blob: İndirme
### <a name="download-single-blob"></a>Tek bir blob indirme

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key> 
```

Varsa klasörü `/mnt/myfiles` yok, AzCopy oluşturur ve indirir `abc.txt` yeni klasöre kopyalar. 

### <a name="download-single-blob-from-secondary-region"></a>İkincil bölgeden tek blob indirme

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Okuma erişimli coğrafi olarak yedekli depolama etkin olması gerektiğini unutmayın.

### <a name="download-all-blobs"></a>Tüm blobları indirin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Aşağıdaki blobların belirtilen kapsayıcıda bulunan varsayın:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Dizin indirme işleminden sonra `/mnt/myfiles` aşağıdaki dosyaları içerir:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Seçeneği belirtmezseniz, `--recursive`, hiçbir blob indirilir.

### <a name="download-blobs-with-specified-prefix"></a>Belirtilen bir önek ile blobları indirin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Aşağıdaki blobların belirtilen kapsayıcıda bulunan varsayılır. Önekiyle başlayan tüm blobları `a` indirilir.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Klasör indirme işleminden sonra `/mnt/myfiles` aşağıdaki dosyaları içerir:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

Ön ek, blob adının ilk bölümü forms bir sanal dizin geçerlidir. Yukarıda gösterilen örnekte, sanal dizin yok blob indirdiğiniz için belirtilen bir önek eşleşmiyor. Ayrıca, varsa seçeneği `--recursive` belirtilmezse, AzCopy blobları indirmek değil.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Kaynak BLOB'ları olarak aynı olacak şekilde, dışarı aktarılan dosyaların son değiştirilme saatini ayarlayın

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

Ayrıca, kendi son değiştirilme zamanına göre indirme işleminden blobları hariç tutabilirsiniz. Son değişiklik zamanı blobları dışlamak istiyorsanız, aynı veya daha yeni hedef dosya eklemektir `--exclude-newer` seçeneği:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Ya da son değişiklik zamanı blobları dışlamak istiyorsanız aynı veya hedef dosyanın daha eski ekleyin `--exclude-older` seçeneği:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>Blob: Karşıya Yükle
### <a name="upload-single-file"></a>Tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --dest-key <key>
```

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler.

### <a name="upload-single-file-to-virtual-directory"></a>Tek Dosyalı sanal dizine yükleyin

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/vd/abc.txt \
    --dest-key <key>
```

Belirtilen sanal dizin yoksa, AzCopy blob adı, sanal dizin eklemek için dosyayı yükler (*örn*, `vd/abc.txt` Yukarıdaki örnekteki).

### <a name="redirect-from-stdin"></a>Stdin yeniden yönlendirme

```azcopy
gzip myarchive.tar -c | azcopy \
    --destination https://myaccount.blob.core.windows.net/mycontainer/mydir/myarchive.tar.gz \
    --dest-key <key>
```

### <a name="upload-all-files"></a>Tüm dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Seçeneğini belirterek `--recursive` tüm alt klasörler ve bunların dosyaları da karşıya yüklenir, yani Blob depolama alanına yinelemeli olarak belirtilen dizinin içeriklerini yükler. Örneğin, aşağıdaki dosyalar klasöründe bulunan varsayar `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Sonra karşıya yükleme işlemi, kapsayıcı aşağıdaki dosyaları içerir:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Zaman seçeneği `--recursive` belirtilmezse, yalnızca aşağıdaki üç dosyalar yüklenir:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a>Belirtilen desenle eşleşen dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

Aşağıdaki dosyalar klasöründe bulunduğu varsayılır `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Sonra karşıya yükleme işlemi, kapsayıcı aşağıdaki dosyaları içerir:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Zaman seçeneği `--recursive` belirtilmezse, AzCopy atlar alt dizinlerde dosyaları:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Hedef blob MIME içerik türünü belirtin
Varsayılan olarak, içerik türü için bir hedef blobun AzCopy ayarlar `application/octet-stream`. Ancak, içerik türü seçeneği aracılığıyla açıkça belirtebileceğiniz `--set-content-type [content-type]`. Bu sözdizimi, bir karşıya yükleme işleminde tüm bloblar için içerik türünü ayarlar.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Varsa seçeneği `--set-content-type` AzCopy her blob veya dosyanın içerik türü dosya uzantısını göre ayarlar daha sonra bir değer olmadan belirtildi.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

### <a name="customizing-the-mime-content-type-mapping"></a>MIME içerik türü eşlemeyi özelleştirme
AzCopy, içerik türü için dosya uzantısı eşlemesi içeren bir yapılandırma dosyası kullanır. Bu eşlemeyi özelleştirme ve yeni çiftleri gerektiği gibi ekleyin. Eşleme şu konumdadır:  ```/usr/lib/azcopy/AzCopyConfig.json```

## <a name="blob-copy"></a>Blob: Kopyala
### <a name="copy-single-blob-within-storage-account"></a>Tek bir blob depolama hesabında kopyalama

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key> \
    --dest-key <key>
```

--Eşitleme kopyalama seçeneği olmadan bir blob kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-single-blob-across-storage-accounts"></a>Tek bir blob depolama hesabı arasında kopyalayın

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

--Eşitleme kopyalama seçeneği olmadan bir blob kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Tek bir blob ikincil bölgesinden birincil bölgeye kopyalayın.

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1/abc.txt \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Okuma erişimli coğrafi olarak yedekli depolama etkin olması gerektiğini unutmayın.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Tek bir blob ve anlık görüntüleri, depolama hesabı arasında kopyalayın

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/ \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Kopyalama işleminden sonra hedef kapsayıcı, blob ve anlık görüntüleri içerir. Kapsayıcı aşağıdaki blob ve anlık görüntüleri içerir:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Zaman uyumlu olarak BLOB Depolama hesabı arasında kopyalayın
AzCopy varsayılan olarak, zaman uyumsuz olarak iki depolama uç noktaları arasında veri kopyalar. Bu nedenle, arka planda ne kadar hızlı bir blob açısından SLA yoktur yedek bant genişliği kapasitesi kullanarak kopyalama işlemi çalışır kopyalanır. 

`--sync-copy` Seçeneği, kopyalama işlemi tutarlı hızı alır sağlar. AzCopy, BLOB'ları için yerel bellek belirtilen kaynaktan kopyalanacak indiriliyor ve bunları Blob Depolama hedefe karşıya yüklemeyi zaman uyumlu kopya gerçekleştirir.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy` zaman uyumsuz kopya kıyasla ek çıkış maliyet oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabının aynı bölgede olan bir Azure VM'de, bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="file-download"></a>Dosya: İndirme
### <a name="download-single-file"></a>Tek dosya indirme

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
```

Belirtilen kaynağı olan bir Azure dosya paylaşımı sonra ya da tam dosya adı belirtmeniz gerekir (*örn* `abc.txt`) tek bir dosya indirin veya seçeneği belirtin `--recursive` paylaşımı yinelemeli olarak tüm dosyaları indirilemedi. Bir dosya deseni ve seçenek belirlemek çalışırken `--recursive` birlikte hatayla sonuçlanır.

### <a name="download-all-files"></a>Tüm dosyaları indirme

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Boş klasörleri indirilmez unutmayın.

## <a name="file-upload"></a>Dosya: Karşıya Yükle
### <a name="upload-single-file"></a>Tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.file.core.windows.net/myfileshare/abc.txt \
    --dest-key <key>
```

### <a name="upload-all-files"></a>Tüm dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Boş bir klasör değil karşıya unutmayın.

### <a name="upload-files-matching-specified-pattern"></a>Belirtilen desenle eşleşen dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Dosya: Kopyala
### <a name="copy-across-file-shares"></a>Dosya paylaşımlarında kopyalayın

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Dosya paylaşımları arasında dosya kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-from-file-share-to-blob"></a>BLOB, dosya paylaşımından kopyalayın

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Bir dosyayı blob için dosya paylaşımından kopyalarken bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="copy-from-blob-to-file-share"></a>BLOB'dan dosya paylaşımına kopyalayın.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
BLOB'dan dosya paylaşımı için bir dosya kopyaladığınızda, bir [sunucu tarafı kopyalama](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirildi.

### <a name="synchronously-copy-files"></a>Dosyaları eşzamanlı olarak Kopyala
Belirtebileceğiniz `--sync-copy` veri dosya depolama'yı dosya depolama, Blob depolamaya dosya depolama ve Blob depolama biriminden dosya depolama için zaman uyumlu olarak kopyalamak için seçenek. AzCopy, bu işlem yerel bellek için kaynak verileri indirip hedefe karşıya yükleme çalıştırır. Bu durumda, standart çıkış ücreti uygulanır.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Dosya depolama'yı Blob depolama alanına kopyalama işlemi sırasında varsayılan blob türü blok blobudur, kullanıcı seçeneği belirtebilirsiniz `--blob-type page` hedef blob türünü değiştirmek için. Kullanılabilir türler `page | block | append`.

Unutmayın `--sync-copy` ek çıkış için zaman uyumsuz kopya karşılaştırma maliyet oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabının aynı bölgede olan bir Azure VM'de, bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="other-azcopy-features"></a>Diğer AzCopy özellikleri
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Hedefte mevcut olmayan veri kopyalama
`--exclude-older` Ve `--exclude-newer` parametreleri, sırasıyla kopyalanmasını daha eski veya yeni bir kaynak kaynakları hariç tut izin verir. Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, her iki parametre AzCopy komutunda belirtebilirsiniz:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a>Komut satırı parametreleri belirtmek için bir yapılandırma dosyası kullanın

```azcopy
azcopy --config-file "azcopy-config.ini"
```

AzCopy komut satırı parametreleri herhangi bir yapılandırma dosyasında içerebilir. Komut satırı üzerinde belirtilmiş gibi AzCopy Parametreler dosyasında dosyanın içeriği ile doğrudan bir değiştirme gerçekleştirme işler.

Adlı bir yapılandırma dosyası varsayar `copyoperation`, aşağıdaki satırları içeren. Tek bir satıra her AzCopy parametresi belirtilebilir.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

veya satırları ayrı:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy, iki satır arasında parametre bölmeniz için burada gösterildiği gibi başarısız `--source-key` parametresi:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) belirtin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-sas <SAS1> \
    --dest-sas <SAS2>
```

Bir SAS URI kapsayıcıdaki belirtebilirsiniz:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

### <a name="journal-file-folder"></a>Günlük dosyası klasörü
AzCopy için bir komut dağıttığınız her saat bir günlük dosyası varsayılan klasör var olup veya bu seçeneği belirtilen klasörde mevcut olup olmadığını denetler. Günlük dosyası ya da yerinde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı komut satırında günlük dosyası ile eşleşip eşleşmediğini denetler. AzCopy komut satırlarını eşleşirse, işlem tamamlanmadı devam ettirir. Bunlar eşleşmiyorsa, AzCopy kullanıcı ya da yeni bir işlem başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyası üzerine yazmak ister.

Varsayılan konumu için günlük dosyası kullanmak istiyorsanız:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Seçeneğini atlarsanız `--resume`, veya seçeneği belirtin `--resume` klasör yolu, yukarıda gösterildiği gibi AzCopy günlük dosyası olan varsayılan konumda oluşturur `~\Microsoft\Azure\AzCopy`. Günlük dosyası zaten varsa, AzCopy günlük dosyasını temel alarak işlemi devam eder.

Günlük dosyası için özel bir konum belirtmek istiyorsanız:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

Bu örnek, zaten yoksa, günlük dosyası oluşturur. Mevcut değilse, AzCopy günlük dosyasını temel alarak işlemi devam eder.

AzCopy çalışmaya devam etmesini istiyorsanız, aynı komutu yineleyin. Ardından Linux üzerinde AzCopy, onay isteminde bulunur:

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Çıkış ayrıntılı günlükleri

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Başlamak için eşzamanlı işlemlerin sayısını belirtin
Seçenek `--parallel-level` eşzamanlı kopyalama işlemleri sayısını belirtir. Varsayılan olarak, belirli bir veri aktarımı verimliliğini artırmak için eşzamanlı işlemlerin sayısını AzCopy başlatılır. Eşzamanlı işlemlerin sayısını eşittir elinizde işlemci sayısını sekiz katı. Düşük bant genişliğine sahip ağ üzerinden AzCopy çalıştırıyorsanız, kaynak yarışmaya göre neden hatadan kaçınmak için daha düşük bir sayı için--paralel düzeyi belirtebilirsiniz.

>[!TIP]
>AzCopy parametrelerin tam listesini görüntülemek için 'azcopy--help' denetleyin menüsü.

## <a name="installation-steps-for-azcopy-71-and-earlier-versions"></a>Yükleme adımları için AzCopy 7.1 ve önceki sürümleri

Linux üzerinde AzCopy (v7. 1've daha önce yalnızca) .NET Core framework gerektirir. Yükleme yönergeleri bulunur [.NET Core yüklemesi](https://www.microsoft.com/net/core#linuxubuntu) sayfası.

Örneğin, Ubuntu 16.10 üzerinde .NET Core yükleyerek başlayın. Son kurulum kılavuzunu ziyaret [Linux üzerinde .NET Core](https://www.microsoft.com/net/core#linuxubuntu) yükleme sayfası.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-sdk-2.0.0
```

.NET Core yükledikten sonra indirin ve AzCopy yükleyin.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Linux üzerinde AzCopy yüklendikten sonra ayıklanan dosyaları kaldırabilirsiniz. Alternatif olarak süper kullanıcı ayrıcalıkları yoksa da çalıştırabilirsiniz `azcopy` ayıklanan klasöründe Kabuk betiği azcopy kullanarak.

## <a name="known-issues-and-best-practices"></a>Bilinen sorunlar ve en iyi uygulamalar
### <a name="error-installing-azcopy"></a>AzCopy yüklenirken hata oluştu
AzCopy yükleme sorunlarla karşılaşırsanız, AzCopy ayıklanan bash betiğini kullanarak çalıştırmayı deneyebilirsiniz `azcopy` klasör.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Veri kopyalama sırasında eş zamanlı yazma sınırı
AzCopy ile dosyaları veya BLOB'ları kopyaladığınızda, bunu kopyalamaya çalışırken, başka bir uygulama verileri değiştirme aklınızda bulundurun. Mümkünse, Kopyalamakta olduğunuz veri kopyalama işlemi sırasında değiştirilmeyen emin olun. Örneğin, bir Azure sanal makinesiyle ilişkili bir VHD kopyalama yapılırken, başka bir uygulama için VHD'yi şu anda yazıyorsanız emin olun. Bunu yapmak için iyi bir Kopyalanacak kaynak kiralama tarafından yoludur. Alternatif olarak, ilk VHD anlık görüntüsünü oluşturun ve sonra anlık görüntüyü kopyalayın.

Diğer uygulamaların bloblar ya da dosyalara yazma sırasında bunlar kopyalanan önleyemez, ardından projenin bittiği zamanı tarafından kopyalanan kaynakları artık kaynak kaynaklarla tam eşlik gerekebileceğini aklınızda bulundurun.

### <a name="running-multiple-azcopy-processes"></a>Çalışan birden çok AzCopy işlemleri
Birden çok AzCopy işlem farklı günlük klasörleri kullanmanızı sağlayan tek bir istemcide çalışmasına. Tek günlük klasörü kullanarak birden çok AzCopy işlemleri için desteklenmiyor.

1. işlem:
```azcopy
azcopy \
    --source /mnt/myfiles1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles1 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal1"
```

2. işlem:
```azcopy
azcopy \
    --source /mnt/myfiles2 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles2 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal2"
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Depolama ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure depolama belgeleri:
* [Azure Depolama’ya giriş](../storage-introduction.md)
* [Depolama hesabı oluşturma](../storage-create-storage-account.md)
* [Depolama Gezgini ile blobları yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)
* [Azure Depolama ile Azure CLI kullanma](../storage-azure-cli.md)
* [BLOB depolama alanından C++ kullanma](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Java'da Blob Depolama'yı kullanma](../blobs/storage-java-how-to-use-blob-storage.md)
* [Node.js'de Blob Depolama'yı kullanma](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Python'da Blob Depolama'yı kullanma](../blobs/storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Azure depolama blog gönderilerini:
* [AzCopy üzerinde Linux Önizleme Duyurusu](https://azure.microsoft.com/blog/announcing-azcopy-on-linux-preview/)
* [Azure depolama veri taşıma kitaplığı Önizleme Tanıtımı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Zaman uyumlu kopya ve özelleştirilmiş içerik türü ile tanışın](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Genel kullanılabilirlik, AzCopy 3.0 Duyurusu yanı sıra tablo ve dosya desteği AzCopy 4.0 sürümü önizlemesi](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Büyük ölçekli kopyalama senaryoları için iyileştirilmiş](https://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Okuma erişimli coğrafi olarak yedekli depolama için destek](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Yeniden başlatılabilir modu ve SAS belirteci ile veri aktarma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Çapraz-hesap kopya blob'u kullanma](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Azure blobları karşıya yükleme/indirme dosyaları](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
