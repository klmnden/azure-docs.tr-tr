---
title: "Kopyalama veya Linux'ta Azure Storage ile AzCopy için veri taşıma | Microsoft Docs"
description: "AzCopy Linux yardımcı taşımak veya için veya blob ve dosya içeriği veri kopyalamak için kullanın. Verileri Azure depolama birimine yerel dosyalarından kopyalamak veya içinde veya depolama hesapları arasında veri kopyalama. Kolayca verilerinizi Azure depolama alanına geçiş."
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
ms.date: 28/9/2017
ms.author: seguler
ms.openlocfilehash: e73a2424d3eb633f6bec63189786a67161750d4f
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Linux'ta AzCopy ile veri aktarımı
Linux üzerinde AzCopy en uygun performans ile basit komutları kullanarak Microsoft Azure Blob ve dosya depolama gelen ve giden veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır. Verileri bir nesneden diğerine depolama hesabınızda veya depolama hesapları arasında kopyalayabilirsiniz.

İndirebilirsiniz AzCopy iki sürümü vardır. Linux üzerinde AzCopy .NET Core POSIX stili komut satırı seçenekleri sunan Linux platformlar hedefler Framework ile yerleşik olarak bulunur. [AzCopy Windows](../storage-use-azcopy.md) .NET Framework ile oluşturulan ve Windows stili komut satırı seçenekleri sunar. Bu makalede, AzCopy Linux üzerinde yer almaktadır.

## <a name="download-and-install-azcopy"></a>AzCopy yükleyip
### <a name="installation-on-linux"></a>Linux üzerinde yükleme

Makaleyi Ubuntu çeşitli sürümleri için komutlar içerir.  Kullanım `lsb_release -a` doğrulamak için dağıtım sürümü ve kod adı komutu. 

Linux üzerinde AzCopy .NET Core framework gerektirir (sürüm 1.1.x) platformunda. Yükleme yönergelerine bakın [.NET Core](https://www.microsoft.com/net/download/linux) sayfası.

Örnek olarak, .NET Core üzerinde Ubuntu 16.04 şimdi yükleyin. En son kurulum kılavuzunu için ziyaret [.NET Core Linux'ta](https://www.microsoft.com/net/download/linux) yükleme sayfası.


```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
sudo apt-get install dotnet-dev-1.1.4
```

.NET Core yüklendiğinde, indirin ve AzCopy yükleyin.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

AzCopy Linux'ta yüklendikten sonra ayıklanan dosyaları kaldırabilirsiniz. Alternatif olarak süper kullanıcı ayrıcalıkları yoksa Kabuk betiği 'azcopy' ayıklanan klasöründe kullanarak AzCopy de çalıştırabilirsiniz. 


## <a name="writing-your-first-azcopy-command"></a>İlk AzCopy komut yazma
AzCopy komutları temel sözdizimi aşağıdaki gibidir:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

Aşağıdaki örnekler verileri için ve Microsoft Azure BLOB'ları ve dosyalarından kopyalamak için çeşitli senaryolar gösterilmektedir. Başvurmak `azcopy --help` her örnekte kullanılan parametreleri ayrıntılı bir açıklaması için menüsü.

## <a name="blob-download"></a>BLOB: karşıdan yükle
### <a name="download-single-blob"></a>Tek blob indirin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Varsa klasörü `/mnt/myfiles` yok, AzCopy oluşturur ve indirir `abc.txt ` yeni klasöre.

### <a name="download-single-blob-from-secondary-region"></a>Tek blob ikincil bölge ' indirin

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Okuma erişimli coğrafi olarak yedekli depolama etkin olması gerektiğini unutmayın.

### <a name="download-all-blobs"></a>Tüm BLOB'ları indirme

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Aşağıdaki BLOB'ları belirtilen kapsayıcıda bulunan varsayın:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Dizin yükleme işleminden sonra `/mnt/myfiles` aşağıdaki dosyaları içerir:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Seçeneği belirtmezseniz, `--recursive`, hiçbir blob indirilir.

### <a name="download-blobs-with-specified-prefix"></a>BLOB'lar ile belirtilen bir önek indirin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Aşağıdaki BLOB'ları belirtilen kapsayıcıda bulunan varsayalım. Önek ile başlayan tüm BLOB'lar `a` indirilir.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Klasör yükleme işleminden sonra `/mnt/myfiles` aşağıdaki dosyaları içerir:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

Önek blob adı ilk bölümü forms bir sanal dizin geçerlidir. Hiçbir blob indirilen şekilde yukarıda gösterilen örnekte, sanal dizin belirtilen bir önek eşleşmiyor. Ayrıca, varsa seçeneği `--recursive` belirtilmezse, AzCopy BLOB indirmek değil.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Kaynak BLOB olarak aynı olmalıdır dışarı aktarılan dosyaların son değiştirme zamanı ayarlama

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

Son değiştiren bunların zamana dayalı indirme işlemi de BLOB'lar hariç tutabilirsiniz. Son değişiklik zamanı BLOB'lar dışlamak istiyorsanız, örneğin, aynı veya daha yeni hedef dosya eklemektir `--exclude-newer` seçeneği:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Ya da son değişiklik zamanı BLOB'lar hariç tutmak istediğiniz ise, aynı veya daha eski hedef dosya ekleme `--exclude-older` seçeneği:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>BLOB: karşıya yükle
### <a name="upload-single-file"></a>Tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy oluşturur ve dosyayı içine yükler.

### <a name="upload-single-file-to-virtual-directory"></a>Sanal dizin için tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Belirtilen sanal dizin yoksa, AzCopy sanal dizin içinde blob adını içerecek şekilde dosyayı yükler (*örneğin*, `vd/abc.txt` Yukarıdaki örnekteki).

### <a name="upload-all-files"></a>Tüm dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Seçeneğini belirterek `--recursive` belirtilen dizin içeriğini tüm alt klasörleri ve bunların dosyaları da karşıya anlamı Blob Depolama yinelemeli olarak yükler. Örneğin, aşağıdaki dosyaları klasöründe bulunan varsayın `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Karşıya yükleme işleminden sonra kapsayıcı aşağıdaki dosyaları içerir:

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

Aşağıdaki dosyaları klasöründe bulunan varsayın `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Karşıya yükleme işleminden sonra kapsayıcı aşağıdaki dosyaları içerir:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

Zaman seçeneği `--recursive` belirtilmezse, AzCopy atlar alt dizinlerde olan dosyaları:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Hedef blob MIME içerik türünü belirtin
Varsayılan olarak, içerik türü için bir hedef blob AzCopy ayarlar `application/octet-stream`. Ancak, açıkça seçeneği aracılığıyla içerik türünü belirtebilirsiniz `--set-content-type [content-type]`. Bu sözdiziminin bir karşıya yükleme işleminde tüm BLOB'lar için içerik türünü ayarlar.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Varsa seçeneği `--set-content-type` AzCopy her bir blob veya dosyanın içerik türü dosya uzantısını göre ayarlar daha sonra bir değer belirtilirse.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a>BLOB: kopyalama
### <a name="copy-single-blob-within-storage-account"></a>Depolama hesabı içinde tek blob kopyalama

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

Bir blob--eşitleme kopyalama seçeneği olmadan kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-single-blob-across-storage-accounts"></a>Tek blob depolama hesapları arasında kopyalama

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Bir blob--eşitleme kopyalama seçeneği olmadan kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Birincil bölge ikincil bölge'den blob'a tek kopyalama

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Okuma erişimli coğrafi olarak yedekli depolama etkin olması gerektiğini unutmayın.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Depolama hesapları arasında tek blob ve onun anlık görüntüleri kopyalama

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Kopyalama işleminden sonra hedef kapsayıcı blob ve onun anlık görüntülerini içerir. Kapsayıcı aşağıdaki blob ve onun anlık görüntülerini içerir:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Zaman uyumlu olarak depolama hesapları arasında BLOB kopyalama
AzCopy varsayılan olarak, iki depolama uç noktaları arasında verileri zaman uyumsuz olarak kopyalar. Bu nedenle, hiçbir SLA ne kadar hızlı blob bakımından sahip yedek bant genişliğini kapasite kullanarak arka plan kopyalama işlemi çalıştırmalarında kopyalanır. 

`--sync-copy` Seçeneği sağlar kopyalama işlemi tutarlı hızı alır. AzCopy zaman uyumlu kopyası yerel bellek için belirtilen kaynak kopyalamak için BLOB'ları karşıdan yükleyip ardından Blob Depolama hedefe karşıya yükleme gerçekleştirir.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy`zaman uyumsuz kopyaya karşılaştırıldığında ek çıkışı maliyeti oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'deki, bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="file-download"></a>Dosya: karşıdan yükle
### <a name="download-single-file"></a>Tek dosya indirme

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Belirtilen kaynak bir Azure dosya paylaşımıdır sonra ya da tam dosya adını belirtmeniz gerekir (*örneğin* `abc.txt`) tek bir dosya indirme veya seçenek belirtmek için `--recursive` paylaşımı yinelemeli olarak tüm dosyaları indirmek için. Bir dosya düzeni ve seçenek belirtmek çalışırken `--recursive` birlikte hatayla sonuçlanır.

### <a name="download-all-files"></a>Tüm dosyaları indirme

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Boş klasörleri indirilmez unutmayın.

## <a name="file-upload"></a>Dosya: karşıya yükle
### <a name="upload-single-file"></a>Tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a>Tüm dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Boş klasörleri değil karşıya unutmayın.

### <a name="upload-files-matching-specified-pattern"></a>Belirtilen desenle eşleşen dosyaları karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Dosya: kopyalama
### <a name="copy-across-file-shares"></a>Dosya paylaşımlarında kopyalayın

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Dosya paylaşımlarında bir dosya kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-from-file-share-to-blob"></a>BLOB dosya paylaşımından kopyalayın

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Dosya blob öğesine dosya paylaşımından kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-from-blob-to-file-share"></a>BLOB üzerinden dosya paylaşımına kopyalayın

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
Dosya Paylaşımı için blobundan bir dosya kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="synchronously-copy-files"></a>Zaman uyumlu olarak dosyaları kopyalayın
Belirleyebileceğiniz `--sync-copy` veri dosya depolama biriminden dosya depolama için dosya depolama Blob depolama alanına ve Blob depolama biriminden dosya depolama alanına eşzamanlı olarak kopyalamak için seçeneği. AzCopy bu işlem için yerel bellek veri kaynağını indirme ve hedef için karşıya yükleme olarak çalışır. Bu durumda, standart çıkış maliyet geçerlidir.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

Dosya depolama biriminden Blob depolama alanına kopyalama işlemi sırasında varsayılan blob türü blok blobu, kullanıcı seçeneği belirtebilirsiniz `--blob-type page` hedef blob türünü değiştirmek için. Kullanılabilir türler `page | block | append`.

Unutmayın `--sync-copy` zaman uyumsuz kopyaya karşılaştırma maliyet ek çıkış oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'deki, bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="other-azcopy-features"></a>Diğer AzCopy özellikleri
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Hedefte mevcut olmayan veri Kopyala
`--exclude-older` Ve `--exclude-newer` parametreleri, sırasıyla kopyalanmasını daha eski veya yeni kaynak kaynakları hariç tut izin verir. Yalnızca hedef yoksa kaynak kaynakları kopyalamak isterseniz, AzCopy komut parametrelerinin her ikisini de belirtebilirsiniz:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a>Komut satırı parametrelerini belirtmek için bir yapılandırma dosyası kullanın

```azcopy
azcopy --config-file "azcopy-config.ini"
```

AzCopy komut satırı parametreleri herhangi bir yapılandırma dosyası içerebilir. Komut satırında belirtilmiş gibi AzCopy dosyasının içeriğiyle doğrudan bir değiştirme gerçekleştirme dosyasındaki parametreleri işler.

Adlı bir yapılandırma dosyası varsayın `copyoperation`, aşağıdaki satırları içeren. Tek bir satıra her AzCopy parametresi belirtilebilir.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

veya satırları ayrı:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy bozulursa parametresi iki satır arasında bölmek için aşağıda gösterildiği gibi `--source-key` parametre:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) belirtin

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

Ayrıca, bir SAS kapsayıcısında URI belirtebilirsiniz:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

AzCopy şu anda yalnızca destekler Not [hesap SAS](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

### <a name="journal-file-folder"></a>Günlük dosyası klasörü
AzCopy için bir komut sorun her zaman varsayılan klasöründe bir günlük dosyası var olup var veya bu seçeneği belirtilen bir klasörde varolup denetler. Günlük dosyası her iki yerde mevcut değilse, AzCopy işlemi yeni olarak değerlendirir ve yeni bir günlük dosyası oluşturur.

Günlük dosyası mevcut değilse, AzCopy girdiğiniz komut satırı günlük dosyası komut satırında eşleşip eşleşmediğini denetler. İki komut satırları eşleşirse, AzCopy tamamlanmamış işlemi sürdürür. Bunlar eşleşmiyorsa AzCopy kullanıcı ya da yeni bir işlemi başlatmak için ya da geçerli işlemi iptal etmek için günlük dosyasının üzerine yazmak ister.

Varsayılan konumu için günlük dosyası kullanmak istiyorsanız:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Seçeneği unutursanız `--resume`, veya seçeneğini belirtin `--resume` klasör yolu, yukarıda gösterildiği gibi AzCopy günlük dosyası olan varsayılan konumda oluşturur `~\Microsoft\Azure\AzCopy`. Günlük dosyası zaten varsa, AzCopy günlük dosyasına dayalı işlemi sürdürür.

Günlük dosyası için özel bir konum belirtmek istiyorsanız:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

Zaten yoksa, bu örnek günlük dosyası oluşturur. Mevcut değilse, AzCopy günlük dosyasına dayalı işlemi sürdürür.

AzCopy çalışmaya devam etmesini istiyorsanız, aynı komutu yineleyin. AzCopy sonra Linux'ta onay ister:

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Çıktı ayrıntılı günlükleri

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Başlatmak için eşzamanlı işlem sayısını belirtin
Seçenek `--parallel-level` eşzamanlı kopyalama işlemlerinin sayısını belirtir. Varsayılan olarak, belirli bir sayıda veri aktarımı verimliliğini artırmak için eşzamanlı işlem AzCopy başlatır. Eşzamanlı işlem sayısını eşittir sekiz katı elinizde işlemci sayısı. Düşük bant genişlikli ağ üzerinden AzCopy çalıştırıyorsanız, kaynak rekabet tarafından nedeniyle başarısız oldu önlemek için daha düşük bir sayı--paralel düzeyi için belirtebilirsiniz.

[!TIP]
>AzCopy parametreler tam listesini görüntülemek için 'azcopy--Yardım' denetleyin menüsü.

## <a name="known-issues-and-best-practices"></a>Bilinen sorunlar ve en iyi uygulamalar
### <a name="error-net-core-is-not-found-in-the-system"></a>Hata: .NET Core sistemde bulunamadı.
.NET Core .NET Core ikili yolu sisteminde yüklü olmadığını belirten bir hatayla karşılaşırsanız `dotnet` eksik olabilir.

Bu sorunu gidermek için .NET Core sisteminde ikili bulun:
```bash
sudo find / -name dotnet
```

Bu yol için dotnet ikili döndürür. 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

Şimdi bu yolu PATH değişkenine ekleyin. Sudo için secure_path ikili dotnet yolunu içerecek şekilde düzenleyin:
```bash 
sudo visudo
### Append the path found in the preceding example to 'secure_path' variable
```

Bu örnekte, secure_path değişken olarak okur:

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

Geçerli kullanıcı için PATH değişkeni ikili dotnet yolunu içerecek şekilde.bash_profile/.profile Düzenle 
```bash
vi ~/.bash_profile
### Append the path found in the preceding example to 'PATH' variable
```

.NET Core şimdi yolunda olduğundan emin olun:
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a>AzCopy yükleme hatası
AzCopy yükleme ile ilgili sorunlarla karşılaşırsanız, AzCopy bash betik ayıklanan kullanarak çalıştırmak çalışabilir `azcopy` klasör.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Veri kopyalama sırasında sınırı eşzamanlı yazma
BLOB veya AzCopy dosyalarıyla kopyaladığınızda, bunu kopyalamaya çalışırken, başka bir uygulama verileri değiştirme göz önünde bulundurun. Mümkünse, Kopyalamakta olduğunuz veri kopyalama işlemi sırasında değiştirilmeyen emin olun. Örneğin, bir Azure sanal makine ile ilişkili bir VHD'nin kopyalarken, başka bir uygulama şu anda VHD'ye yazıyorsanız emin olun. Bunu yapmak için iyi bir Kopyalanacak kaynak kiralama tarafından yoldur. Alternatif olarak, bir anlık görüntü VHD'yi ilk oluşturun ve sonra anlık görüntü kopyalayın.

Bunlar kopyalanan sırada BLOB veya dosyalar için yazma diğer uygulamaların önleyemez, ardından işi tamamlanana zamanına göre kopyalanan kaynakların artık kaynak kaynaklarla tam eşlik gerekebileceğini aklınızda bulundurun.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Bir Azcopy'i örnek bir makinede çalıştırın.
AzCopy veri aktarımını hızlandırmak için makine kaynak kullanımını en üst düzeye çıkarmak için tasarlanmış, yalnızca bir Azcopy'i örnek bir makinede çalıştırın ve seçeneğini belirtin öneririz `--parallel-level` fazla eşzamanlı işlem gerekiyorsa. Daha fazla ayrıntı için yazın `AzCopy --help parallel-level` komut satırında.

## <a name="next-steps"></a>Sonraki adımlar
Azure Storage ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure Storage belgeleri:
* [Azure Storage'a giriş](../storage-introduction.md)
* [Depolama hesabı oluşturma](../storage-create-storage-account.md)
* [BLOB Depolama Gezgini ile yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)
* [Azure Storage ile Azure CLI 2.0 kullanma](../storage-azure-cli.md)
* [C++ içinden BLOB storage kullanma](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Java'da Blob Depolama'yı kullanma](../blobs/storage-java-how-to-use-blob-storage.md)
* [Node.js'de Blob Depolama'yı kullanma](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Python'da Blob Depolama'yı kullanma](../blobs/storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Azure depolama blog gönderileri:
* [AzCopy Linux preview'daki tanışın](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Azure Storage veri hareketi kitaplığı Önizleme Tanıtımı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: zaman uyumlu kopyası ve özelleştirilmiş içerik türü tanışın](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Genel kullanılabilirlik, AzCopy 3.0 artı AzCopy 4.0 Önizleme sürümü tablo ve dosya desteği ile tanışın](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Büyük ölçekli kopyalama senaryoları için en iyi duruma getirilmiş](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Coğrafi olarak yedekli depolamaya okuma erişimi desteği](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: yeniden başlatılabilir modu ve SAS belirteci ile veri aktarımı](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: arası hesap kopyalama Blob kullanma](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Azure BLOB'ları karşıya yükleme ve indirme dosyaları](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

