---
title: Kopyalama veya Linux'ta Azure Storage ile AzCopy için veri taşıma | Microsoft Docs
description: AzCopy Linux yardımcı taşımak veya için veya blob ve dosya içeriği veri kopyalamak için kullanın. Verileri Azure depolama birimine yerel dosyalarından kopyalamak veya içinde veya depolama hesapları arasında veri kopyalama. Kolayca verilerinizi Azure depolama alanına geçiş.
services: storage
documentationcenter: ''
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: seguler
ms.openlocfilehash: 80b112de1fd8417dd64d9d95b7a037ec876d18c7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Linux'ta AzCopy ile veri aktarımı

AzCopy, en iyi performans için tasarlanmış basit komutlarını kullanarak denetleyicisinden Microsoft Azure Blob ve dosya depolama, veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır. Bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz.  

İndirebilirsiniz AzCopy iki sürümü vardır. Linux üzerinde AzCopy komut satırı seçenekleri POSIX stili sunumu Linux platformlar hedefler. [AzCopy Windows](../storage-use-azcopy.md) Windows stili komut satırı seçenekleri sunar. Bu makalede, AzCopy Linux üzerinde yer almaktadır. 

> [!NOTE]  
> AzCopy 7.2 sürümünde başlayarak, .NET Core bağımlılıkları AzCopy paketi ile birlikte paketlenmiştir. 7.2 sürümü kullanmak isterseniz daha sonra artık .NET Core bir önkoşul yüklemeniz gerekir.

## <a name="download-and-install-azcopy"></a>AzCopy yükleyip

### <a name="installation-on-linux"></a>Linux üzerinde yükleme

> [!NOTE]
> Bu konuda vurgulanmış .NET Core 2.1 bağımlılıkları yüklemeniz gerekebilir [.NET Core ön koşullar makale](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x) dağıtımınız bağlı olarak. Ubuntu 16.04 ve RHEL 7 gibi temel dağıtımları için bu genellikle gerekli değildir.

Linux'ta AzCopy yükleme (v7.2 veya sonrası) tar paketi ayıklanıyor ve yükleme komut dosyası çalıştırma olarak kadar kolaydır. 

**RHEL 6 tabanlı dağıtımlar**: [bağlantı indirin](https://aka.ms/downloadazcopylinuxrhel6)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinuxrhel6
tar -xf azcopy.tar.gz
sudo ./install.sh
```

**Diğer tüm Linux dağıtımları**: [bağlantı indirin](https://aka.ms/downloadazcopylinux64)
```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinux64
tar -xf azcopy.tar.gz
sudo ./install.sh
```

AzCopy Linux'ta yüklendikten sonra ayıklanan dosyaları kaldırabilirsiniz. Alternatif olarak, süper kullanıcı ayrıcalıkları yoksa de çalıştırabilirsiniz `azcopy` ayıklanan klasöründe Kabuk komut dosyası azcopy kullanma.

### <a name="alternative-installation-on-ubuntu"></a>Ubuntu alternatif yükleme

**Ubuntu 14.04**

Microsoft Linux ürün depo apt kaynağı ekleyin ve AzCopy yükleyin:

```bash
sudo echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod/ trusty main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
apt-key adv --keyserver packages.microsoft.com --recv-keys B02C46DF417A0893
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Microsoft Linux ürün depo apt kaynağı ekleyin ve AzCopy yükleyin:

```bash
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod/ xenial main" > azure.list
sudo cp ./azure.list /etc/apt/sources.list.d/
apt-key adv --keyserver packages.microsoft.com --recv-keys B02C46DF417A0893
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

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
    --source https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key> 
```

Varsa klasörü `/mnt/myfiles` yok, AzCopy oluşturur ve indirir `abc.txt ` yeni klasöre. 

### <a name="download-single-blob-from-secondary-region"></a>Tek blob ikincil bölge ' indirin

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
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
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/abc.txt \
    --dest-key <key>
```

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler.

### <a name="upload-single-file-to-virtual-directory"></a>Sanal dizin için tek dosya karşıya yükleme

```azcopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer/vd/abc.txt \
    --dest-key <key>
```

Belirtilen sanal dizin yoksa, AzCopy sanal dizin içinde blob adını içerecek şekilde dosyayı yükler (*örneğin*, `vd/abc.txt` Yukarıdaki örnekteki).

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
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key> \
    --dest-key <key>
```

Bir blob--eşitleme kopyalama seçeneği olmadan kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-single-blob-across-storage-accounts"></a>Tek blob depolama hesapları arasında kopyalama

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Bir blob--eşitleme kopyalama seçeneği olmadan kopyaladığınızda bir [sunucu tarafı kopyası](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) işlemi gerçekleştirilir.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Birincil bölge ikincil bölge'den blob'a tek kopyalama

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1/abc.txt \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2/abc.txt \
    --source-key <key1> \
    --dest-key <key2>
```

Okuma erişimli coğrafi olarak yedekli depolama etkin olması gerektiğini unutmayın.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Depolama hesapları arasında tek blob ve onun anlık görüntüleri kopyalama

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1/ \
    --destination https://destaccount.blob.core.windows.net/mycontainer2/ \
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

`--sync-copy` zaman uyumsuz kopyaya karşılaştırıldığında ek çıkışı maliyeti oluşturabilir. Çıkış maliyet önlemek için kaynak depolama hesabınız ile aynı bölgede olan Azure VM'deki, bu seçeneği kullanmak için önerilen yaklaşımdır bakın.

## <a name="file-download"></a>Dosya: karşıdan yükle
### <a name="download-single-file"></a>Tek dosya indirme

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/abc.txt \
    --destination /mnt/myfiles/abc.txt \
    --source-key <key>
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
    --source https://myaccount.blob.core.windows.net/mycontainer1/abc.txt \
    --destination https://myaccount.blob.core.windows.net/mycontainer2/abc.txt \
    --source-sas <SAS1> \
    --dest-sas <SAS2>
```

Ayrıca, bir SAS kapsayıcısında URI belirtebilirsiniz:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

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

>[!TIP]
>AzCopy parametreler tam listesini görüntülemek için 'azcopy--Yardım' denetleyin menüsü.

## <a name="installation-steps-for-azcopy-71-and-earlier-versions"></a>AzCopy 7.1 ve daha önceki sürümleri için yükleme adımları

Linux üzerinde AzCopy (v7.1 ve daha önce yalnızca) .NET Core framework gerektirir. Yükleme yönergeleri bulunur [.NET Core yüklemesi](https://www.microsoft.com/net/core#linuxubuntu) sayfası.

Örneğin, .NET Core üzerinde Ubuntu 16.10 yükleyerek başlayın. En son kurulum kılavuzunu için ziyaret [.NET Core Linux'ta](https://www.microsoft.com/net/core#linuxubuntu) yükleme sayfası.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-sdk-2.0.0
```

.NET Core yüklendiğinde, indirin ve AzCopy yükleyin.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

AzCopy Linux'ta yüklendikten sonra ayıklanan dosyaları kaldırabilirsiniz. Alternatif olarak süper kullanıcı ayrıcalıkları yoksa de çalıştırabilirsiniz `azcopy` ayıklanan klasöründe Kabuk komut dosyası azcopy kullanma.

## <a name="known-issues-and-best-practices"></a>Bilinen sorunlar ve en iyi uygulamalar
### <a name="error-installing-azcopy"></a>AzCopy yükleme hatası
AzCopy yükleme ile ilgili sorunlarla karşılaşırsanız, AzCopy bash betik ayıklanan kullanarak çalıştırmak çalışabilir `azcopy` klasör.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Veri kopyalama sırasında sınırı eşzamanlı yazma
BLOB veya AzCopy dosyalarıyla kopyaladığınızda, bunu kopyalamaya çalışırken, başka bir uygulama verileri değiştirme göz önünde bulundurun. Mümkünse, Kopyalamakta olduğunuz veri kopyalama işlemi sırasında değiştirilmeyen emin olun. Örneğin, bir Azure sanal makine ile ilişkili bir VHD'nin kopyalarken, başka bir uygulama şu anda VHD'ye yazıyorsanız emin olun. Bunu yapmak için iyi bir Kopyalanacak kaynak kiralama tarafından yoldur. Alternatif olarak, bir anlık görüntü VHD'yi ilk oluşturun ve sonra anlık görüntü kopyalayın.

Bunlar kopyalanan sırada BLOB veya dosyalar için yazma diğer uygulamaların önleyemez, ardından işi tamamlanana zamanına göre kopyalanan kaynakların artık kaynak kaynaklarla tam eşlik gerekebileceğini aklınızda bulundurun.

### <a name="running-multiple-azcopy-processes"></a>Çalışan birden çok AzCopy işlemleri
Birden çok AzCopy işlem, farklı günlük klasörleri kullanmanızı sağlayan tek bir istemcide çalıştırabilirsiniz. Birden çok AzCopy işlemleri için tek günlük klasörü kullanılması desteklenmez.

1 işlem:
```azcopy
azcopy \
    --source /mnt/myfiles1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles1 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal1"
```

2 işlemi:
```azcopy
azcopy \
    --source /mnt/myfiles2 \
    --destination https://myaccount.blob.core.windows.net/mycontainer/myfiles2 \
    --dest-key <key> \
    --resume "/mnt/myazcopyjournal2"
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Depolama ve AzCopy hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="azure-storage-documentation"></a>Azure Storage belgeleri:
* [Azure Depolama’ya giriş](../storage-introduction.md)
* [Depolama hesabı oluşturma](../storage-create-storage-account.md)
* [Depolama Gezgini ile blobları yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)
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

