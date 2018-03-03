---
title: "Microsoft Azure Storage Gezgini (Önizleme) sürüm notları"
description: "Microsoft Azure Storage Gezgini (Önizleme) için sürüm notları"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 0e5523e297979a89ffd4b4ed51c8476fb1354419
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a>Microsoft Azure Storage Gezgini (Önizleme) sürüm notları

Bu makalede Azure Storage Gezgini 0.9.6 için (Önizleme), önceki sürümler için sürüm notları yanı sıra sürüm notları sürüm içerir.

[Microsoft Azure Storage Gezgini (Önizleme)](./vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, macOS ve Linux Azure Storage ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır.

## <a name="version-096"></a>Sürüm 0.9.6
02/28/2018

### <a name="download-azure-storage-explorer-096-preview"></a>Azure Depolama Gezgini (Önizleme) 0.9.6 indirin
- [Windows için Azure Storage Gezgini 0.9.6 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Storage Gezgini 0.9.6 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Storage Gezgini 0.9.6 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="fixes"></a>Düzeltmeler
* Bir sorun Düzenleyicisi'nde listelenen beklenen BLOB'lar/dosyaları engelledi. Bu düzeltilmiştir.
* Bir sorun yanlış öğeleri görüntülemek için anlık görüntü görünüm arasında geçiş neden oldu. Bu düzeltilmiştir.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure yığın hedeflerken ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Açıklanan iptal filtre geçici çözüm kullanıyoruz olmasıdır [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) donanım hızlandırmasını sorun vardır. Depolama Gezgini boş bir (boş) ana penceresi görüntüleme, deneyebilirsiniz ekleyerek GPU hızlandırmasını devre dışı bırakma ve komut satırından Depolama Gezgini başlatılıyor `--disable-gpu` geçin:

```
./StorageExplorer.exe --disable-gpu
```

* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="previous-releases"></a>Önceki sürümler

* [Sürüm 0.9.5](#version-095)
* [Sürüm 0.9.4 ve 0.9.3](#version-094-and-093)
* [Sürüm 0.9.2](#version-092)
* [Sürüm 0.9.1 ve 0.9.0'dan](#version-091-and-090)
* [Sürüm 0.8.16](#version-0816)
* [Sürüm 0.8.14](#version-0814)
* [Sürüm 0.8.13](#version-0813)
* [Sürüm 0.8.12 ve 0.8.11 ve 0.8.10](#version-0812-and-0811-and-0810)
* [Sürüm 0.8.9 ve 0.8.8](#version-089-and-088)
* [Sürüm 0.8.7](#version-087)
* [Sürüm 0.8.6](#version-086)
* [Sürüm 0.8.5](#version-085)
* [Sürüm 0.8.4](#version-084)
* [Sürüm 0.8.3](#version-083)
* [Sürüm 0.8.2](#version-082)
* [Sürüm 0.8.0](#version-080)
* [Sürüm 0.7.20160509.0](#version-07201605090)
* [Sürüm 0.7.20160325.0](#version-07201603250)
* [Sürüm 0.7.20160129.1](#version-07201601291)
* [Sürüm 0.7.20160105.0](#version-07201601050)
* [Sürüm 0.7.20151116.0](#version-07201511160)

## <a name="version-095"></a>Sürüm 0.9.5
02/06/2018

### <a name="download-azure-storage-explorer-095-preview"></a>Azure Depolama Gezgini (Önizleme) 0.9.5 indirin
- [Windows için Azure Storage Gezgini 0.9.5 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Storage Gezgini 0.9.5 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Storage Gezgini 0.9.5 (Önizleme)](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Dosya paylaşımları anlık görüntüler için destek:
    * Oluşturun ve dosya paylaşımları için anlık görüntüleri yönetin.
    * Keşfetmenizde kolayca görünümleri, dosya paylaşımlarında anlık görüntüleri arasında geçiş yapma.
    * Dosyaların önceki sürümlerini geri yükleyin.
* Azure Data Lake Store desteği önizleme:
    * ADLS kaynaklarınız arasında birden fazla hesap bağlayın.
    * Bağlanmak ve ADL URI'ler kullanarak ADLS kaynakları paylaşabilirsiniz.
    * Temel dosya/klasör işlemleri yinelemeli olarak gerçekleştirin.
    * Hızlı erişim için PIN tek tek klasörler.
    * Klasör istatistikler görüntülenir.

### <a name="fixes"></a>Düzeltmeler
* Başlangıç performans geliştirmeleri.
* Çeşitli hata düzeltmeleri.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure yığın hedeflerken ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Açıklanan iptal filtre geçici çözüm burada kullanıyoruz olmasıdır.
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) donanım hızlandırmasını sorun vardır. Depolama Gezgini boş bir (boş) ana penceresi görüntüleme, deneyebilirsiniz ekleyerek GPU hızlandırmasını devre dışı bırakma ve komut satırından Depolama Gezgini başlatılıyor `--disable-gpu` geçin:

```
./StorageExplorer.exe --disable-gpu
```

* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Sürüm 0.9.4 ve 0.9.3
01/21/2018

### <a name="download-azure-storage-explorer-094-preview"></a>Azure Depolama Gezgini (Önizleme) 0.9.4 indirin
* [Windows Azure Depolama Gezgini (Önizleme) 0.9.4 indirin](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Mac için Azure Storage Gezgini (Önizleme) 0.9.4 indirin](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Linux için Azure Storage Gezgini (Önizleme) 0.9.4 indirin](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>Yeni
* Var olan Depolama Gezgini penceresi ne zaman yeniden kullanılan olacaktır:
    * Depolama Gezgini'nde oluşturulan doğrudan bağlantılar açılıyor.
    * Depolama Gezgini portalından açılıyor.
    * Depolama Gezgini (yakında) Azure depolama VS Code uzantısından açılıyor.
* Yeni bir Depolama Gezgini penceresi öğesinden Depolama Gezgini içinde açmak için özelliği eklenmiştir.
    * Windows için Dosya menüsü altında ve bağlam menüsünden görev çubuğunun 'Yeni pencere' seçeneği yoktur.
    * Mac için uygulama menüsü altındaki 'Yeni pencere' seçeneği yoktur.

### <a name="fixes"></a>Düzeltmeler
* Bir güvenlik sorunu sabit. Lütfen, erken kolaylık adresindeki 0.9.4 yükseltin.
* Eski etkinlikleri uygun şekilde temizlendi değil. Bu, uzun çalışan işleri performansını etkilenen. Bunlar artık doğru temizlenmesini.
* Çok sayıda dosya ve dizinleri dahil eylemleri bazen dondurmak Depolama Gezgini neden olur. Azure dosya paylaşımları için isteklere sistem kaynak tüketimini sınırlamak için trottled sunulmuştur.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure yığın hedeflerken ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Açıklanan iptal filtre geçici çözüm burada kullanıyoruz olmasıdır.
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) donanım hızlandırmasını sorun vardır. Depolama Gezgini boş bir (boş) ana penceresi görüntüleme, deneyebilirsiniz ekleyerek GPU hızlandırmasını devre dışı bırakma ve komut satırından Depolama Gezgini başlatılıyor `--disable-gpu` geçin:
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Sürüm 0.9.2
11/01/2017

### <a name="hotfixes"></a>Hotfixes
* Beklenmeyen veri değişikliklerini Edm.DateTime değerlerini yerel saat dilimine bağlı olarak tablo varlıklar için düzenlerken mümkün. Düzenleyici bir düz metin kutusu Edm.DateTime değerleri üzerinde kesin, tutarlı denetim vermiş olarak kullanır.
* Bir grup adı ve anahtarı eklendiğinde BLOB karşıya yükleme/indirme başlatılmayacaktır. Bu düzeltilmiştir.
* Daha önce Depolama Gezgini yalnızca eski hesabı varsa kimlik doğrulamaya ister veya daha fazla hesabın abonelikleri seçilmedi. Hesabı tamamen filtrelenmelidir olsa bile artık Depolama Gezgini ister.
* Uç noktaları etki alanı Azure ABD devlet kurumları için yanlıştı. Düzeltilmiştir.
* Bazen hesaplarını yönetme panelindeki Uygula düğmesini tıklatın sabit. Bu artık gerçekleşmelidir.

### <a name="new"></a>Yeni
* Azure Cosmos DB desteği önizleme:
    * [Çevrimiçi belgeleri](./cosmos-db/storage-explorer.md)
    * Veritabanları ve Koleksiyonlar oluşturun
    * Verileri denetleme
    * Sorgu, oluşturmak, belgeleri veya silme
    * Saklı yordamlar, kullanıcı tanımlı işlevler veya tetikleyicileri güncelleştirme
    * Bağlantı dizeleri bağlanmak ve veritabanlarınızı yönetmek için kullanın
* Çok sayıda küçük BLOB karşıya yükleme/indirme performansı geliştirildi.
* Bir blob karşıya yükleme grubu veya blob yükleme grubundaki başarısızlık varsa bir "Yeniden deneme tüm" eylemi eklendi.
* Ağ bağlantısı kaybedildi algılarsa Depolama Gezgini şimdi yineleme blob yükleme/indirme sırasında duraklatılır. Ağ bağlantısı yeniden kurulduktan sonra yineleme sonra devam edebilirsiniz.
* "Tümünü Kapat", "Kapat diğerleri" ve "Kapat" sekmeleri bağlam menüsü aracılığıyla özelliği eklenmiştir.
* Depolama Gezgini şimdi yerel iletişim kutuları ve yerel bağlam menülerini kullanır.
* Depolama Gezgini şimdi daha erişilebilir. Geliştirmeleri içerir:
    * Gelişmiş ekran okuyucusu desteği, NVDA Windows ve Mac üzerinde VoiceOver
    * Geliştirilmiş Yüksek karşıtlıklı Tema oluşturma
    * Klavye sekme ve klavye odağını giderir

### <a name="fixes"></a>Düzeltmeler
* Açın veya geçersiz bir Windows dosya adına sahip bir blob indirmek çalıştıysanız işlemi başarısız olur. Depolama Gezgini şimdi blob adı geçersizse algılamak ve kodlamak veya blob atlamak isteyip isteyin. Depolama Gezgini, bir dosya adı kodlanması ve, sorun görünüp görünmeyeceğini de algılar, karşıya yüklemeden önce çözmek istiyor.
* BLOB karşıya yükleme sırasında hedef blob kapsayıcısı için Düzenleyicisi bazen düzgün yeniler değil. Bu düzeltilmiştir.
* Bağlantı dizeleri ve SAS URI'ler çeşitli biçimlerde desteği gerileyen. Biz tüm bilinen sorunlar ele, ancak daha fazla sorunlarla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0'dan bazı kullanıcılar için kesildi. Bu sorun düzeltilmiştir ve bunlar için hatadan etkilenen, el ile Depolama Gezgini en son sürümünü indirebilirsiniz [burada](https://azure.microsoft.com/en-us/features/storage-explorer/).

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure yığın hedeflerken ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Açıklanan iptal filtre geçici çözüm burada kullanıyoruz olmasıdır.
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) donanım hızlandırmasını sorun vardır. Depolama Gezgini boş bir (boş) ana penceresi görüntüleme, deneyebilirsiniz ekleyerek GPU hızlandırmasını devre dışı bırakma ve komut satırından Depolama Gezgini başlatılıyor `--disable-gpu` geçin:
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Sürüm 0.9.1 ve 0.9.0'dan
10/20/2017
### <a name="new"></a>Yeni
* Azure Cosmos DB desteği önizleme:
    * [Çevrimiçi belgeleri](./cosmos-db/storage-explorer.md)
    * Veritabanları ve Koleksiyonlar oluşturun
    * Verileri denetleme
    * Sorgu, oluşturmak, belgeleri veya silme
    * Saklı yordamlar, kullanıcı tanımlı işlevler veya tetikleyicileri güncelleştirme
    * Bağlantı dizeleri bağlanmak ve veritabanlarınızı yönetmek için kullanın
* Çok sayıda küçük BLOB karşıya yükleme/indirme performansı geliştirildi.
* Bir blob karşıya yükleme grubu veya blob yükleme grubundaki başarısızlık varsa bir "Yeniden deneme tüm" eylemi eklendi.
* Ağ bağlantısı kaybedildi algılarsa Depolama Gezgini şimdi yineleme blob yükleme/indirme sırasında duraklatılır. Ağ bağlantısı yeniden kurulduktan sonra yineleme sonra devam edebilirsiniz.
* "Tümünü Kapat", "Kapat diğerleri" ve "Kapat" sekmeleri bağlam menüsü aracılığıyla özelliği eklenmiştir.
* Depolama Gezgini şimdi yerel iletişim kutuları ve yerel bağlam menülerini kullanır.
* Depolama Gezgini şimdi daha erişilebilir. Geliştirmeleri içerir:
    * Gelişmiş ekran okuyucusu desteği, NVDA Windows ve Mac üzerinde VoiceOver
    * Geliştirilmiş Yüksek karşıtlıklı Tema oluşturma
    * Klavye sekme ve klavye odağını giderir

### <a name="fixes"></a>Düzeltmeler
* Açın veya geçersiz bir Windows dosya adına sahip bir blob indirmek çalıştıysanız işlemi başarısız olur. Depolama Gezgini şimdi blob adı geçersizse algılamak ve kodlamak veya blob atlamak isteyip isteyin. Depolama Gezgini, bir dosya adı kodlanması ve, sorun görünüp görünmeyeceğini de algılar, karşıya yüklemeden önce çözmek istiyor.
* BLOB karşıya yükleme sırasında hedef blob kapsayıcısı için Düzenleyicisi bazen düzgün yeniler değil. Bu düzeltilmiştir.
* Bağlantı dizeleri ve SAS URI'ler çeşitli biçimlerde desteği gerileyen. Biz tüm bilinen sorunlar ele, ancak daha fazla sorunlarla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0'dan bazı kullanıcılar için kesildi. Bu sorun düzeltilmiştir ve bunlar için hatadan etkilenen, el ile Depolama Gezgini en son sürümünü indirebilirsiniz [burada](https://azure.microsoft.com/en-us/features/storage-explorer/)

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure yığın hedeflerken ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Açıklanan iptal filtre geçici çözüm burada kullanıyoruz olmasıdır.
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) donanım hızlandırmasını sorun vardır. Depolama Gezgini boş bir (boş) ana penceresi görüntüleme, deneyebilirsiniz ekleyerek GPU hızlandırmasını devre dışı bırakma ve komut satırından Depolama Gezgini başlatılıyor `--disable-gpu` geçin:
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Sürüm 0.8.16
8/21/2017

### <a name="new"></a>Yeni
* Bir blob açtığınızda, Depolama Gezgini bir değişiklik algılandığında, indirilen dosyayı karşıya yüklemeyi isteyip istemediğinizi sorar
* Gelişmiş Azure yığın oturum açma deneyimi
* Karşıya yükleme/pek çok küçük dosya aynı anda indirme performansı geliştirildi


### <a name="fixes"></a>Düzeltmeler
* Bazı blob türleri için "bir karşıya yükleme çakışması sırasında değiştirmek" seçme bazen yeniden başlatılıyor karşıya yükleme neden olur.
* 0.8.15 sürümünde yüklemeleri bazen %99 kabin.
* Hangi belirtmiyor bir dizine seçerseniz bir dosya paylaşımına dosyaları karşıya henüz mevcut olduğunda, karşıya yükleme başarısız olur.
* Depolama Gezgini hatalı zaman damgaları paylaşılan erişim imzaları ve tablo sorguları oluşturma.


### <a name="known-issues"></a>Bilinen Sorunlar
* Bir ad ve anahtar bağlantı dizesi kullanarak şu an çalışmıyor. Sonraki sürümde düzeltilecektir. O zamana kadar kullandığınız adı ve anahtar ekleyebilirsiniz.
* Geçersiz bir Windows dosya adıyla bir dosyayı açmaya çalışırsanız, yükleme bir dosya bulunamadı hatası neden olur.
* "İptal" görevde tıkladıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu, Azure Depolama düğümü kitaplığı kısıtlamasıdır.
* Bir blob karşıya yükleme işlemini tamamladıktan sonra karşıya yükleme başlatılan sekmesini yenilenir. Bu, önceki davranış farklıdır ve ayrıca olduğunuz kapsayıcı kökünde geri alınması için neden olur.
* Yanlış PIN/akıllı kart sertifika seçerseniz, Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir.
* Hesap Ayarları panelini abonelikleri filtrelemek için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Ubuntu 14.04 üzerinde kullanıcıların için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekecek - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Sürüm 0.8.14
06/22/2017

### <a name="new"></a>Yeni

* 1.7.2 birden fazla kritik güvenlik güncelleştirmeleri anlamıyla yararlanabilmek için güncelleştirilmiş Elektron sürüme
* Artık hızlı bir şekilde çevrimiçi sorun giderme kılavuzu Yardım menüsünden erişebilirsiniz
* Depolama Gezgini sorun giderme [Kılavuzu][2]
* [Yönergeler] [ 3] bir Azure yığın aboneliğine bağlanma

### <a name="known-issues"></a>Bilinen Sorunlar

* Düğmeleri silme klasörü onay iletişim kutusunda Linux'ta fare tıklamaları kaydetme yok. Enter tuşunu kullanmak için geçici bir çözüm değildir
* Yanlış PIN/akıllı kart sertifika seçerseniz, ardından Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir
* Hesap Ayarları panelini aboneliklerini filtreleme için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Ubuntu 14.04 yükleme güncelleştirilmiş gcc sürüm gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Sürüm 0.8.13
12.05.2017

#### <a name="new"></a>Yeni

* Depolama Gezgini sorun giderme [Kılavuzu][2]
* [Yönergeler] [ 3] bir Azure yığın aboneliğine bağlanma

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Karşıya dosya yükleme yetersiz bellek hatası neden, yüksek şansınız
* Sabit: Artık PIN/akıllı kart ile oturum
* Sabit: Portalda şimdi works Azure Çin, Azure Almanya, Azure ABD devlet kurumları ve Azure yığın açın
* Sabit: bir klasör bir blob kapsayıcıya karşıya yüklenirken "Geçersiz işlemi" hata bazen oluşacak
* Sabit: Tümünü Seç anlık görüntüleri yönetirken devre dışı bırakıldı
* Sabit: Temel blob meta verilerini, anlık görüntü özelliklerini görüntüledikten sonra üzerine

#### <a name="known-issues"></a>Bilinen Sorunlar

* Yanlış PIN/akıllı kart sertifika seçerseniz, ardından Depolama Gezgini kararı unuttunuz olması için yeniden başlatmanız gerekir
* Yakınlaştırma düzeyini veya uzaklaştırılacağını olsa da, kısa bir süre içinde varsayılan düzeyini sıfırlayabilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir
* Hesap Ayarları panelini aboneliklerini filtreleme için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Ubuntu 14.04 yükleme güncelleştirilmiş gcc sürüm gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Sürüm 0.8.12 ve 0.8.11 ve 0.8.10
04/07/2017

#### <a name="new"></a>Yeni

* Güncelleştirme bildirimden bir güncelleştirmeyi yüklediğinizde Depolama Gezgini otomatik olarak kapatılacak
* Sık erişilen kaynaklarla çalışmak için Gelişmiş bir deneyim yerinde hızlı erişim sağlar
* Blob kapsayıcısına düzenleyicisinde kiralık blob ait sanal makine artık görebilirsiniz
* Artık sol tarafındaki paneli daraltabilirsiniz
* Bulma indirme ile aynı zamanda şimdi çalışıyor.
* İstatistikleri Blob kapsayıcısı, dosya paylaşımı ve tablo düzenleyicilerde kaynak ya da seçimi boyutunu görmek için kullanın.
* Oturum açma için Azure Active Directory (AAD) Azure yığın hesapları dayalı artık kullanabilirsiniz.
* Arşiv dosyalarını üzerinde 32 MB Premium depolama hesapları için şimdi karşıya yükleyebilirsiniz
* Geliştirilmiş erişilebilirlik desteği
* Artık güvenilir Base-64 ekleyebilirsiniz Düzenle - gitmeyi tarafından kodlanmış X.509 SSL sertifikalarını&gt; SSL sertifikalarını -&gt; alma sertifikaları

#### <a name="fixes"></a>Düzeltmeler

* Sabit: bir hesabın kimlik yenilendikten sonra ağaç görünümünde bazen otomatik olarak yeniler değil
* Sabit: öykünücüsü kuyruklar ve tablolar için bir SAS oluşturma geçersiz bir URL neden olur
* Sabit: proxy etkinken premium depolama hesapları artık Genişletilebilir
* Sabit: Seçili 1 veya 0 hesapları olsaydı hesapları Yönetim sayfasında Uygula düğmesi çalışmayan
* Sabit: çakışma çözümü gerektiren BLOB karşıya yükleme - 0.8.11 içinde sabit başarısız olabilir
* Sabit: geri bildirim gönderme 0.8.12 içinde sabit 0.8.11 içinde-hatalı olan

#### <a name="known-issues"></a>Bilinen Sorunlar

* 0.8.10 için yükseltirken, tüm kimlik bilgilerinizi yenilemek gerekir.
* Giriş veya çıkış uzaklaştırılacağını olsa da, yakınlaştırma düzeyini kısa bir süre içinde varsayılan düzeyini sıfırlayabilir.
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir.
* Hesap Ayarları panelini aboneliklerini filtreleme için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir.
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure yığın şu anda dosya paylaşımlarını desteklemez ancak dosya paylaşımlarına düğümü ekli bir Azure yığın depolama hesabı altında görünmeye devam eder.
* Ubuntu 14.04 yükleme güncelleştirilmiş gcc sürüm gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089-and-088"></a>Sürüm 0.8.9 ve 0.8.8
02/23/2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>Yeni

* Depolama Gezgini 0.8.9 güncelleştirmeleri için en son sürümünü otomatik olarak indirir.
* Düzeltme: bir portal kullanarak bir depolama hesabı eklemek için oluşturulan SAS URI'sini hataya neden olur.
* Şimdi oluşturmak, yönetmek ve blob anlık görüntüleri yükseltin.
* Artık Azure Çin, Azure Almanya ve Azure ABD devlet kurumları hesaplarına oturum açabilir.
* Şimdi yakınlaştırma düzeyini değiştirebilirsiniz. Görünüm menüsünde yakınlaştırma, uzaklaştırma ve yakınlaştırma sıfırlama seçenekleri kullanın.
* Unicode karakterler, BLOB'lar ve dosyalar için kullanıcı meta verilerde artık desteklenmektedir.
* Erişilebilirlik geliştirmeleri.
* Sonraki sürüme ait sürüm notları güncelleştirme bildirimden görüntülenebilir. Geçerli sürüm notları Yardım menüsünden de görüntüleyebilirsiniz.

#### <a name="fixes"></a>Düzeltmeler

* Sabit: sürüm numarası artık doğru şekilde Windows Denetim Masası'nda görüntülenir
* Sabit: arama artık 50.000 düğümleriyle sınırlıdır
* Sabit: hedef dizin zaten mevcut değilse sonsuza kadar başlar bir dosya paylaşımına karşıya yükle
* Sabit: kararlılığı uzun ve indirmelere için

#### <a name="known-issues"></a>Bilinen Sorunlar

* Giriş veya çıkış uzaklaştırılacağını olsa da, yakınlaştırma düzeyini kısa bir süre içinde varsayılan düzeyini sıfırlayabilir.
* Hızlı erişim yalnızca abonelik temel öğeleri ile çalışır. Yerel kaynaklar veya anahtar veya SAS belirteci üzerinden bağlı kaynakları bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir.
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir.

12/16/2016
### <a name="version-087"></a>Sürüm 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Yeni

* Bir güncelleştirme, yükleme veya kopyalama oturumu başına etkinlikleri penceresinde çakışmalarını çözmek nasıl seçebilirsiniz
* Depolama kaynağı tam yolunu görmek için bir sekme getirin
* Bir sekmede tıkladığınızda, sol taraftaki gezinti bölmesinde konumuyla eşitler

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Depolama Gezgini güvenilen uygulama Mac üzerinde sunulmuştur
* Sabit: Ubuntu 14.04 yeniden desteklenir
* Sabit: Bazen UI Hesap Ekle abonelikler yüklenirken yanıp sönen
* Sabit: Bazen tüm depolama kaynaklarını sol taraftaki Gezinti Bölmesi'nde listelenen
* Sabit: Eylem bölmesinde bazen boş eylemler görüntülenir
* Sabit: Son kapalı oturumundan pencere boyutu şimdi korunur
* Sabit: Bağlam menüsünü kullanarak aynı kaynak için birden çok sekme açabilirsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Hızlı erişim yalnızca abonelik temel öğeleri ile çalışır. Yerel kaynaklar veya anahtar veya SAS belirteci üzerinden bağlı kaynakları bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir
* Bundan sonra kabaca 50.000 düğümlere - arama arama tanıtıcıları performansı etkilenebilir veya işlenmeyen bir özel duruma neden olabilir
* MacOS üzerinde Depolama Gezgini'ni kullanarak ilk kez, birden çok komut istemlerini Anahtarlık erişim için kullanıcıdan izin isteyen görebilirsiniz. İstemi yeniden göstermeyecektir için her zaman izin ver seçeneğini öneririz

11/18/2016
### <a name="version-086"></a>Sürüm 0.8.6

#### <a name="new"></a>Yeni

* Artık PIN hızlı erişim için Hizmetleri Kolay gezinme için en sık kullanılan yapabilecekleriniz
* Artık farklı sekmelerde birden çok düzenleyicileri açabilirsiniz. Geçici bir sekmede açmak için tek tıklatın; kalıcı sekmesini açmak için çift tıklayın. Kalıcı bir sekme yapmak için geçici sekmesinde tıklatabilirsiniz
* Fark edilebilir performans yapmış olduğunuz ve kararlılık geliştirmeleri için hızlı makinelerde büyük dosyalar için özellikle indirir ve yükler
* Boş "sanal" klasörler şimdi blob kapsayıcı oluşturulabilir
* Şimdi arama yapmak için iki seçeneğiniz vardır böylece biz bizim yeni Gelişmiş substring arama ile kapsamlı arama yeniden sunulmuştur:
    * Genel arama - yalnızca arama metin kutusuna bir arama terimi girin
    * Kapsamlı arama - bir düğüm yanındaki Büyüteç simgesini sonra bir arama terimi yolun sonuna ekleyin veya sağ tıklatın ve "Arama gelen burada" seçin
* Çeşitli temalar ekledik: açık (varsayılan), koyu, yüksek karşıtlık siyah ve yüksek karşıtlık beyaz. Düzen - gidin&gt; tema tercihinizi değiştirmek için temalar
* Blob ve dosya özelliklerini değiştirebilirsiniz
* Kodlanmış (base64) ve kodlanmamış iletileri artık desteklenmektedir
* Linux üzerinde bir 64-bit işletim sistemi sunulmuştur gerekli. Bu sürüm için yalnızca 64-bit Ubuntu 16.04.1 destekliyoruz LTS
* Biz bizim logosu güncelleştirilmiş!

#### <a name="fixes"></a>Düzeltmeler

* Sabit: ekran dondurma sorunları
* Sabit: Gelişmiş Güvenlik
* Sabit: Bazen yinelenen ekli hesapları görünmez
* Sabit: Blob tanımlanmamış bir içerik türüne sahip bir özel durum da oluşturabilir
* Sabit: boş bir tablo sorgu panelde açma mümkün değildi
* Sabit: arama hatalarda değişir
* Sabit: Artan kaynak sayısı 50'den 100'e "Daha yük" tıklatıldığında yüklendi
* Sabit: bir hesap olarak imzalı değilse ilk çalıştırılmasında biz şimdi bu hesap için tüm abonelikleri varsayılan olarak seçin

#### <a name="known-issues"></a>Bilinen Sorunlar

* Depolama Gezgini bu sürümü Ubuntu 14.04 üzerinde çalışmaz
* Aynı kaynak için birden fazla sekme açmak için sürekli olarak aynı kaynak tıklatın değil. Üzerinde başka bir kaynak'ı tıklatın ve sonra geri dönün ve yeniden başka bir sekmede açmak için orijinal kaynak tıklayın
* Hızlı erişim yalnızca abonelik temel öğeleri ile çalışır. Yerel kaynaklar veya anahtar veya SAS belirteci üzerinden bağlı kaynakları bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalara neden olabilir
* Bundan sonra kabaca 50.000 düğümlere - arama arama tanıtıcıları performansı etkilenebilir veya işlenmeyen bir özel duruma neden olabilir

10/03/2016
### <a name="version-085"></a>Sürüm 0.8.5

#### <a name="new"></a>Yeni

* Portal tarafından oluşturulan SAS anahtarları depolama hesapları ve kaynakları eklemek için artık kullanabilirsiniz

#### <a name="fixes"></a>Düzeltmeler

* Sabit: bazen arama sırasında yarış durumu düğümleri Genişletilebilir olmayan hale gelmesine neden
* Sabit: "HTTP kullan" depolama hesapları için hesap adı ve anahtar ile bağlanırken çalışmıyor
* Sabit: SAS anahtarları (olanlar için özel Portal tarafından oluşturulan) "sondaki eğik çizgi" bir hata döndürür
* Sabit: tablo alma sorunları
    * Bölüm anahtarı ve satır anahtarını bazen tersine
    * "Null" bölüm anahtarlarını okunamıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bundan sonra kabaca 50.000 düğümlere - arama arama tanıtıcıları performans etkilenmiş olabilir.
* Dosyaları genişletin çalışılırken bir hata gösterir şekilde azure yığın dosyaları, şu anda desteklemiyor

09/12/2016
### <a name="version-084"></a>Sürüm 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesapları, kapsayıcıları, kuyruklar, tablolar veya paylaşımı için dosya paylaşımlarına doğrudan bağlantı oluşturmak ve kaynaklarınıza - Windows ve Mac OS kolay erişim desteği
* Blob kapsayıcılar, tablolar, kuyruklar, dosya paylaşımları veya arama kutusuna depolama hesaplarından arayın
* Tablo Sorgu Oluşturucusu sorgu yan tümcelerinde şimdi gruplandırabilirsiniz
* Yeniden adlandırın ve blob kapsayıcıları, dosya paylaşımları, tablolar, BLOB'lar, blob klasörleri, dosyaları ve dizinlerden SAS bağlı hesapları ve kapsayıcılar içinde kopyala/yapıştır
* Yeniden adlandırma ve kopyalama kapsayıcıları blob ve dosya paylaşımları artık özellikleri ve meta verileri koru

#### <a name="fixes"></a>Düzeltmeler

* Sabit: boolean veya ikili özellikleri içermiyorsa tablo varlıkları düzenlenemez.

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bundan sonra kabaca 50.000 düğümlere - arama arama tanıtıcıları performans etkilenmiş olabilir.

08/03/2016
### <a name="version-083"></a>Sürüm 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Yeni

* Kapsayıcılar, tablolar, dosya paylaşımları yeniden adlandırma
* Gelişmiş Sorgu Oluşturucu deneyim
* Kaydetme ve yükleme olanağı sorgular
* Doğrudan bağlantılar depolama hesapları veya kapsayıcıları, kuyruklar, tablolar veya dosya paylaşımı ve kolayca kaynaklarınıza erişmek için paylaşımları (yalnızca Windows - macOS desteği yakında!)
* Özelliği yönetmek ve CORS kuralları yapılandırmak için

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Microsoft Accounts yeniden kimlik doğrulaması 8-12 saatte gerektirir

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bazen UI dondurulmuş görünebilir - penceresinin ekranı kaplamasını bu sorunu çözmenize yardımcı olur
* macOS yükleme yükseltilmiş izinler gerektirebilir
* Hesap Ayarları panelini aboneliklerini filtreleme için kimlik bilgilerinizi yeniden girmeniz gerektiğini gösterebilir
* Dosya paylaşımları, blob kapsayıcıları ve tablolar yeniden adlandırma meta verileri veya dosya paylaşım kotası, ortak erişim düzeyi veya erişim ilkeleri gibi kapsayıcı bulunan diğer özelliklerden korumaz
* BLOB'lar (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcısı içinde) yeniden adlandırma anlık görüntüleri korumaz. Diğer tüm özellikleri ve meta veri BLOB'lar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur
* Kopyalama veya kaynakları yeniden adlandırma SAS bağlı hesapları içinde çalışmıyor

07/07/2016
### <a name="version-082"></a>Sürüm 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesapları abonelikler tarafından gruplandırılır; Geliştirme depolama ve anahtar veya SAS aracılığıyla bağlı kaynakları (yerel ve iliştirildiği) düğümünde gösterilir
* "Azure hesap ayarları" panelinde hesaplarından Oturumu Kapat
* Etkinleştirin ve oturum açma yönetmek için proxy ayarlarını yapılandır
* Oluşturma ve blob kira bölün
* Açık blob kapsayıcılar, kuyruklar, tablolar ve tek bir tıklatmayla dosyaları

#### <a name="fixes"></a>Düzeltmeler

* Sabit: .NET veya Java kitaplıklarıyla eklenen iletileri kuyruğa düzgün base64 kodlaması çözülür değil
* Sabit: $metrics tablolar için Blob Storage hesapları gösterilmez
* Sabit: tablolar düğümünü yerel (Geliştirme) depolama için çalışmıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* macOS yükleme yükseltilmiş izinler gerektirebilir

06/15/2016
### <a name="version-080"></a>Sürüm 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Yeni

* Dosya Paylaşımı desteği: görüntüleme, karşıya yükleme, indirme, dosyaları ve dizinleri, SAS URI'ler kopyalama (oluşturma ve bağlanma)
* SAS URI'ler veya hesabı anahtarları ile depolama birimine bağlanmak için kullanıcı deneyimi geliştirildi
* Tablo sorgusu sonuçlarını dışarı aktarma
* Tablo sütun yeniden sıralamayı ve özelleştirme
* $Logs blob kapsayıcıları ve $metrics tabloları depolama hesapları için etkin Ölçümleriyle görüntüleme
* Geliştirilmiş dışarı aktarma ve davranış alma, artık özellik değeri türü içerir

#### <a name="fixes"></a>Düzeltmeler

* Sabit: karşıya yükleme veya büyük BLOB'ları indirme tamamlanmamış yüklemeleri/yüklemeler neden olabilir
* Sabit: düzenleme, ekleme ya da bir sayısal dize değeri ("1") sahip bir varlık alma, çift dönüştürür
* Sabit: Yerel geliştirme ortamında Tablo düğümü genişletilemiyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* $metrics tabloları Blob Depolama hesapları için görünür değildir
* Base64 kodlaması kullanarak iletileri kodlanmış olmadığını program aracılığıyla eklenen iletileri kuyruğa düzgün görüntülenmeyebilir

05/17/2016
### <a name="version-07201605090"></a>Sürüm 0.7.20160509.0

#### <a name="new"></a>Yeni

* Daha iyi hata uygulaması için işleme kilitleniyor

#### <a name="fixes"></a>Düzeltmeler

* Oturum açma kimlik bilgileri gerekli olduğunda burada bilgi çubuğu iletileri bazen görünmüyor sabit hata

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tablolar: Ekleme, düzenleme veya "1" veya "1.0" gibi bir muğlak sayısal değer ve kullanıcı özelliğiyle sahip bir varlık çalışır olarak göndermek için aktarma bir `Edm.String`, değer istemcisi bir Edm.Double olarak API aracılığıyla geri gelir

03/31/2016

### <a name="version-07201603250"></a>Sürüm 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Yeni

* Tablo desteği: görüntüleme, sorgulanırken, dışa aktarma, içeri aktarma ve varlıklar için CRUD işlemleri
* Sıra desteği: görüntüleme, ekleme, dequeueing iletileri
* Depolama hesapları için SAS URI oluşturma
* Depolama hesapları SAS URI ile bağlanma
* Güncelleştirme bildirimlerini için Depolama Gezgini gelecekteki güncelleştirmeleri
* Güncelleştirilmiş Görünüm

#### <a name="fixes"></a>Düzeltmeler

* Performans ve güvenilirlik geliştirmeleri

### <a name="known-issues-amp-mitigations"></a>Bilinen sorunlar &amp; Azaltıcı Etkenler

* İndirme büyük blob dosyaların düzgün çalışmıyor - AzCopy kullanırken, biz bu soruna öneririz
* Hesap kimlik bilgilerini değil alınan ya da giriş klasörü bulunamıyor veya yazılamaz önbelleğe
* Biz ekleme, düzenleme veya "1" veya "1.0" gibi muğlak sayısal bir değere sahip bir özelliği olan bir varlık alma ve kullanıcı olarak göndermek çalışırsa bir `Edm.String`, değer istemcisi bir Edm.Double olarak API aracılığıyla geri gelir
* Çok satırlı kayıtları içeren CSV dosyalarını içeri aktarırken, verileri kesilmiş Tasarımlı karıştırılmış veya olabilir

02/03/2016

### <a name="version-07201601291"></a>Sürüm 0.7.20160129.1

#### <a name="fixes"></a>Düzeltmeler

* Karşıya yükleme, indirme ve BLOB kopyalama genel performansı

01/14/2016

### <a name="version-07201601050"></a>Sürüm 0.7.20160105.0

#### <a name="new"></a>Yeni

* Linux desteği (OSX eşlik özellikleri)
* BLOB kapsayıcıları ile paylaşılan erişim imzaları (SAS) anahtar Ekle
* Azure Çin için depolama hesapları ekleme
* Özel uç noktaları ile depolama hesapları ekleme
* Açın ve içeriğini metin ve resim BLOB'ları görüntüleyin
* Blob özellikleri ve meta verileri görüntüleyin ve düzenleyin

#### <a name="fixes"></a>Düzeltmeler

* Sabit: karşıya yükleme veya indirme çok sayıda BLOB (500 +) bazen beyaz bir ekrana sahip uygulamanın neden olabilir
* Sabit: kapsayıcı odaklanmak yeniden ayarlayana kadar blob kapsayıcısı genel erişim düzeyi ayarlarken, yeni değer güncelleştirilmez. Ayrıca, iletişim her zaman "Public erişim yok" ve değil gerçek geçerli değer varsayılanlarını.
* Daha iyi genel klavye/erişilebilirlik ve kullanıcı Arabirimi desteği
* Boşluk ile uzun olduğunda içerik haritalarında geçmiş metin sarmalar
* Giriş doğrulaması SAS iletişim destekler
* Yerel depolama kullanıcı kimlik bilgilerinin süresi dolmuş olsa bile kullanılabilir olmaya devam eder
* Blob explorer sağ tarafında açılan blob kapsayıcısı silindiğinde, kapalı

#### <a name="known-issues"></a>Bilinen Sorunlar

* Linux yükleme güncelleştirilmiş gcc sürüm gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Sürüm 0.7.20151116.0

#### <a name="new"></a>Yeni

* macOS ve Windows sürümleri
* Hesabınızda oturum açın, depolama hesaplarınıza görüntülemek – Kurumsal hesap, Microsoft, 2FA, Account kullanın.
* Yerel geliştirme depolama (depolama öykünücüsünü kullanma yalnızca Windows)
* Azure Resource Manager ve klasik kaynak desteği
* Oluşturma ve BLOB, kuyruk veya tablo silme
* Belirli BLOB, kuyruk veya tablo Ara
* Blob kapsayıcıları içeriğini keşfedin
* Görüntülemek ve dizinleri gidin
* Karşıya yükleyin, indirin ve BLOB'ları ve klasörleri silme
* Blob özellikleri ve meta verileri görüntüleyin ve düzenleyin
* SAS anahtarları oluştur
* Yönetmek ve depolanan erişim ilkeleri (SAP) oluşturun
* Ön eke göre BLOB Ara
* Sürükleme 'n dosyaları karşıya yükleme veya indirme bırak

#### <a name="known-issues"></a>Bilinen Sorunlar

* Kapsayıcı odaklanmak yeniden ayarlayana kadar blob kapsayıcısı genel erişim düzeyi ayarlarken, yeni değer güncelleştirilmez
* Genel erişim düzeyini ayarlamak için iletişim kutusunu açtığınızda, bu her zaman "Public erişim yok" varsayılan ve değil gerçek geçerli değer gösterir
* İndirilen BLOB'ları yeniden adlandırılamıyor
* Etkinlik günlüğü girişleri bazen "bir sürüyor takılıyor" ne zaman bir hata oluşur ve hata görüntülenmiyor belirtin.
* Bazen kilitleniyor veya karşıya yükleme veya çok sayıda BLOB indirme çalışırken tamamen beyaz kapatır
* Bazen bir kopyalama işlemi iptal ediliyor çalışmıyor
* Geçersiz bir ad girin ve başka farklı kapsayıcı türü altında oluşturmaya devam (sırası/blob/tablosu) kapsayıcı oluşturma sırasında odak yeni tür ayarlanamaz
* Yeni bir klasör oluşturulamaz veya klasörünü yeniden adlandırın




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
