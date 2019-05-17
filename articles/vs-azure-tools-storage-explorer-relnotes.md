---
title: Microsoft Azure Depolama Gezgini sürüm notları
description: Microsoft Azure Depolama Gezgini sürüm notları
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2018
ms.author: cawa
ms.openlocfilehash: b753b565b7dae6cdc244d05d051df964eda3c6f2
ms.sourcegitcommit: 3675daec6c6efa3f2d2bf65279e36ca06ecefb41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65620484"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Microsoft Azure Depolama Gezgini sürüm notları

Bu makale, Azure Depolama Gezgini 1.8.1 yayın yönelik sürüm notlarına yanı sıra, önceki sürümleri için sürüm notları içerir.

[Microsoft Azure Depolama Gezgini](./vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, macOS ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına uygulamadır.

## <a name="version-181"></a>Sürümü 1.8.1
5/13/2019

### <a name="download-azure-storage-explorer-181"></a>Azure Depolama Gezgini 1.8.1 indirin
- [Windows için Azure Depolama Gezgini 1.8.1](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.8.1](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.8.1](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="hotfixes"></a>Düzeltmeler
* Bazı durumlarda, "Yük daha fazla" kaynak düzeyinde tıklayarak sonraki sayfaya kaynakların döndürecekti değil. Bu düzeltilmiştir. [#1359](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1359)
* Dosya veya klasör adı için bir Windows yolu geçersiz bir karakter vardı ve tek dosya veya klasör yüklenmiş Windows üzerinde AzCopy yükleme başarısız olur. Bu düzeltilmiştir. [#1350](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1350)
* Ender durumlarda, bir yeniden adlandırma, bir dosya paylaşımı veya bir yeniden adlandırma bir dosya paylaşımında kopyalarını yeniden adlandırma başarısız olursa veya depolama keşfedin kopyalar, Azure'la başarısını onaylayamadı gerçekleştirilirken oluştu olası o silmek Depolama Gezgini zgün dosyaları kopyalama bitmeden. Bu düzeltilmiştir.

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümünü 10.1.0 sürümüne güncelleştirildi.
* CTRL / Cmd + R artık o anda odaklanılan Düzenleyicisi'ni yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Stack depolama API Sürüm 2017-04-17 değiştirildi.
* ADLS Gen2'ye ait yönetme erişimi iletişim artık maske bir şekilde eşitlenmiş POSIX izinleri araçlara benzer tutar. Kullanıcı arabirimini de bir değişiklik yapılması durumunda maske sınırları aşan bir kullanıcı veya grup izinlerini neden olan uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy yüklemeler için MD5 karma değeri hesaplamak ve bayrağı şimdi etkinleştirildi. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akış oturum açma Önizleme kullanıma sunulmuştur. Bunu etkinleştirmek için "Kullanımı cihaz kod akış oturum açma" "Preview" → gidin. Oturum açma daha güvenilir bir biçimi olması kanıtlayın gibi windows ile birlikte bu özelliği denemek için boş oturum açma sorunları materyaline kullanıcılardan öneririz.
* AzCopy ile tümleşik Depolama Gezgini önizlemesi şu anda kullanılabilir. Bunu etkinleştirmek için "Preview" → "Kullanın AzCopy için geliştirilmiş Blob karşıya yükleme ve indirme" gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha fazla performansa sahip.

### <a name="fixes"></a>Düzeltmeleri

* Erişim ilkeleri iletişim bundan böyle bir sona erme tarihi depolama erişim ilkelerine bağlı bir süre sonu olmayan ayarlar. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Saklı erişim ilkeleri düzgün bir SAS oluştururken kullanılan emin olmak için SAS oluşturma iletişim kutusu için bazı değişiklikler yapıldı. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* Bir sayfa blobu bir dosyaya hizalanmış bir olmayan 512 baytlık karşıya yüklemeye, Depolama Gezgini artık daha fazla ilgili bir hata açığa çıkarır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görünen ad olarak kullanılan bir Blob kapsayıcısı kopyalama başarısız olur. Şimdi, Blob kapsayıcısının gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Unicode karakterler adına sahip bir ADLS 2. nesil klasöründeki belirli eylemleri gerçekleştirme girişimi başarısız. Tüm eylemleri çalışması gerekir. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy Blob yükleme gerçekleştirirken, büyük dosyalar için MD5 Doğrulanmakta olan değil. Depolama SDK'sı bir hata nedeniyle budur. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanırken, Depolama Gezgini depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Bkz: [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) daha fazla bilgi için.
* ADLS Gen2 BLOB'ları, bir proxy'nin arkasında erişmeye başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2. nesil
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini Linux'ta çalışan ilk yüklenecek belirli bağımlılıkları gerektirir. Depolama Gezgini denetleyin [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) daha fazla bilgi için.

## <a name="previous-releases"></a>Önceki sürümler

* [Sürüm 1.8.0](#version-180)
* [Sürüm 1.7.0](#version-170)
* [Sürüm 1.6.2](#version-162)
* [Sürümü 1.6.1](#version-161)
* [Sürüm 1.6.0](#version-160)
* [Sürüm 1.5.0](#version-150)
* [Sürüm 1.4.4](#version-144)
* [Sürüm 1.4.3](#version-143)
* [Sürüm 1.4.2](#version-142)
* [Sürüm 1.4.1](#version-141)
* [Sürüm 1.3.0](#version-130)
* [Sürümü 1.2.0](#version-120)
* [Sürüm 1.1.0](#version-110)
* [Sürüm 1.0.0](#version-100)
* [Sürüm 0.9.6](#version-096)
* [Sürüm 0.9.5](#version-095)
* [0.9.4 ve 0.9.3 sürümü](#version-094-and-093)
* [Sürüm 0.9.2](#version-092)
* [0.9.1 ve 0.9.0 sürümü](#version-091-and-090)
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

## <a name="version-180"></a>Sürüm 1.8.0
5/1/2019

### <a name="download-azure-storage-explorer-180"></a>Azure Depolama Gezgini 1.8.0 indirin
- [Windows için Azure Depolama Gezgini 1.8.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.8.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.8.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümünü 10.1.0 sürümüne güncelleştirildi.
* CTRL / Cmd + R artık o anda odaklanılan Düzenleyicisi'ni yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Stack depolama API Sürüm 2017-04-17 değiştirildi.
* ADLS Gen2'ye ait yönetme erişimi iletişim artık maske bir şekilde eşitlenmiş POSIX izinleri araçlara benzer tutar. Kullanıcı arabirimini de bir değişiklik yapılması durumunda maske sınırları aşan bir kullanıcı veya grup izinlerini neden olan uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy yüklemeler için MD5 karma değeri hesaplamak ve bayrağı şimdi etkinleştirildi. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akış oturum açma Önizleme kullanıma sunulmuştur. Bunu etkinleştirmek için "Kullanımı cihaz kod akış oturum açma" "Preview" → gidin. Oturum açma daha güvenilir bir biçimi olması kanıtlayın gibi windows ile birlikte bu özelliği denemek için boş oturum açma sorunları materyaline kullanıcılardan öneririz.
* AzCopy ile tümleşik Depolama Gezgini önizlemesi şu anda kullanılabilir. Bunu etkinleştirmek için "Preview" → "Kullanın AzCopy için geliştirilmiş Blob karşıya yükleme ve indirme" gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha fazla performansa sahip.

### <a name="fixes"></a>Düzeltmeleri

* Erişim ilkeleri iletişim bundan böyle bir sona erme tarihi depolama erişim ilkelerine bağlı bir süre sonu olmayan ayarlar. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Saklı erişim ilkeleri düzgün bir SAS oluştururken kullanılan emin olmak için SAS oluşturma iletişim kutusu için bazı değişiklikler yapıldı. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* Bir sayfa blobu bir dosyaya hizalanmış bir olmayan 512 baytlık karşıya yüklemeye, Depolama Gezgini artık daha fazla ilgili bir hata açığa çıkarır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görünen ad olarak kullanılan bir Blob kapsayıcısı kopyalama başarısız olur. Şimdi, Blob kapsayıcısının gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Unicode karakterler adına sahip bir ADLS 2. nesil klasöründeki belirli eylemleri gerçekleştirme girişimi başarısız. Tüm eylemleri çalışması gerekir. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy Blob yükleme gerçekleştirirken, büyük dosyalar için MD5 Doğrulanmakta olan değil. Depolama SDK'sı bir hata nedeniyle budur. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanırken, Depolama Gezgini depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Bkz: [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) daha fazla bilgi için.
* ADLS Gen2 BLOB'ları, bir proxy'nin arkasında erişmeye başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2. nesil
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini Linux'ta çalışan ilk yüklenecek belirli bağımlılıkları gerektirir. Depolama Gezgini denetleyin [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) daha fazla bilgi için.

## <a name="version-170"></a>Sürüm 1.7.0
3/5/2019

### <a name="download-azure-storage-explorer-170"></a>Azure Depolama Gezgini 1.7.0 indirin
- [Windows için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Şimdi, sahibi ve sahip olan Grup bir ADLS Gen2 kapsayıcı, dosya veya klasör için erişim yönetirken değiştirebilirsiniz.
* Windows üzerinde depolama Gezgini'nden ürün içinde güncelleştirmek artımlı yükleme şimdi kullanılır. Bu, daha hızlı bir güncelleştirme deneyimi neden olur. Temiz yükleme tercih sonra indirebileceğiniz [yükleyici](https://azure.microsoft.com/features/storage-explorer/) kendiniz ve el ile yükleyin. #1089

### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akış oturum açma Önizleme kullanıma sunulmuştur. Bunu etkinleştirmek için "Kullanımı cihaz kod akış oturum açma" "Preview" → gidin. Oturum açma daha güvenilir bir biçimi olması kanıtlayın gibi windows ile birlikte bu özelliği denemek için boş oturum açma sorunları materyaline kullanıcılardan öneririz. #938
* AzCopy ile tümleşik Depolama Gezgini önizlemesi şu anda kullanılabilir. Bunu etkinleştirmek için "Preview" → "Kullanın AzCopy için geliştirilmiş Blob karşıya yükleme ve indirme" gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha fazla performansa sahip.

### <a name="fixes"></a>Düzeltmeleri

* Artık olarak AzCopy etkinleştirildiğinde karşıya yüklemek istediğiniz blob türü seçebilirsiniz. #1111
* Statik Web sitelerine ADLS Gen2'ye depolama hesabınız için etkin ve adı ve anahtarı ile bağlı, daha önce Depolama Gezgini, hiyerarşik ad alanı etkinleştirildi algılanmadı. Bu düzeltilmiştir. #1081
* Blob Düzenleyicisi'nde, bekletme gün kaldı ya da durum Sıralama kesildi. Bu düzeltilmiştir. #1106
* 1.5.0 sonra Depolama Gezgini'ni yeniden adlandırma ya da kopyalama sırasında başarı raporlamadan önce son & Yapıştır sunucu tarafı kopyaları için artık beklendi. Bu düzeltilmiştir. #976
* "Komutunu Panoya Kopyala" a tıkladıktan sonra kopyalanan komut Deneysel AzCopy özelliğini kullanırken, her zaman kendi başına çalıştırılabilir değil. Şimdi, aktarımı el ile çalıştırmak için gereken tüm komutları kopyalanır. #1079
* Daha önce ADLS Gen2 bloblar bir proxy'nin arkasındayken olsaydı erişilebilir değildi. Bu depolama SDK'sı tarafından kullanılan yeni ağ kitaplığındaki bir hata nedeniyle oluştu. 1.7.0, bu sorunu gidermek için bir girişimde bulunuldu, ancak bazı kişiler sorunları görmek devam edebilir. Tam bir düzeltme gelecek bir güncelleştirmede yayınlanacaktır. #1090
* 1.7.0, kaydetme, dosya iletişim kutusu artık, kaydettiğiniz dosyaya son konum doğru hatırlar. #16
* Özellikler panelinde, bir depolama hesabı SKU katmanı hesabın türü olarak gösterilen. Bu düzeltilmiştir. #654
* Bazı durumlarda bile blob adını doğru girdiğinizi bir blobun kira kesilmeye mümkün değildi. Bu düzeltilmiştir. #1070

### <a name="known-issues"></a>Bilinen Sorunlar

* RBAC kullanırken, Depolama Gezgini depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Bkz: [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) daha fazla bilgi için.
* ADLS Gen2 BLOB'ları, bir proxy'nin arkasında erişmeye başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-162"></a>Sürüm 1.6.2
1/9/2019

### <a name="hotfixes"></a>Düzeltmeler
* 1.6.1 içinde kullanıcıların değildi ADLS Gen2 ACL'ler için objectID tarafından eklenen varlıklar her zaman gruplar olarak eklendi. Şimdi, yalnızca gruplar, grupları ve ilkeleri kullanıcılar olarak eklenen kurumsal uygulamalar andService gibi varlıkları olarak eklenir. [#1049](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1049)
* ADLS Gen2'ye depolama hesabınız kapsayıcı sahipti ve adı ve anahtarı ile ilişkili olduğu, sonra Depolama Gezgini'ni depolama hesabındaki ADLS Gen2 olduğunu algılamamak. Bu düzeltilmiştir. [#1048](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1048)
* 1.6.0 kopyalama ve yapıştırma sırasında çakışma çözümlemesi için istemleri. Bunun yerine, çakışan kopyalama basitçe başarısız olur. Şimdi ilk çakışma, çözülmesi için nasıl istediğinizi istenecektir. [#1014](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1014)
* API sınırlamaları nedeniyle, tüm doğrulama ObjectIds erişimi Yönet iletişim kutusunda, devre dışı bırakıldı. Şimdi, doğrulama yalnızca kullanıcının UPN meydana gelir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS Gen2 erişimi Yönet iletişim kutusunda, bir grup için izinleri değiştirilemedi. Bu düzeltilmiştir. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* Eklenen sürükle ve bırak desteği ADLS Gen2 düzenleyiciye karşıya yükleyin. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* Bazen ADLS Gen2'ye dosya ve klasörler için Özellikler iletişim kutusunda URL'si özelliği eksik bir '/'. Bu düzeltilmiştir. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Geçerli izinler ADLS Gen2 kapsayıcısı için alma, dosya veya klasör başarısız olur ve artık etkinlik günlüğü'nde görüntülenen propertly hatadır. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol üzerinde Windows MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltıldı. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Bağlan iletişim kutusu artık doğru şekilde oturum açmış olan kullanıcı yok ve kaynak bağlı görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1.6.0 içinde HNS BLOB ve dosyalar için özellikleri kaydetme, her özelliğin değerini kodlayın. Bu, yalnızca ASCII karakterler bulunan değerleri gereksiz kodlama sonuçlandı. Şimdi, ASCII olmayan karakterler içeriyorsa değerleri yalnızca şifrelenecek. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Bir HNS Blob kapsayıcısına bir klasöre yüklemek, SAS kullanıldı ve SAS okuma izni olmaması durumunda başarısız olur. Bu düzeltilmiştir. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımı iptal etme başarısız oldu. Bu düzeltilmiştir. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy, klasör adında boşluklar varsa, bir klasöre bir ADLS Gen2 Blob kapsayıcısından karşıdan yüklemeye çalışırken başarısız olur. Bu düzeltilmiştir. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB Düzenleyicisi içinde 1.6.0 bozuk. Artık sabittir. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık Depolama Gezgini aracılığıyla Blob verilerinize erişmek için kullanabileceğiniz [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Oturum açtıktan ve Depolama Gezgini'ni depolama hesabınız için anahtarları alamıyor, OAuth belirteci verilerinizle etkileşim kurulurken kimlik doğrulaması için kullanılır.
* Depolama Gezgini, artık ADLS Gen2 depolama hesaplarını destekler. Depolama Gezgini, hiyerarşik ad alanı için bir depolama hesabı etkin algıladığında, şunları yapacaksınız depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" bakın. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanı etkin olup olmadığını ya da depolama hesabınızın adı ve anahtarıyla eklediyseniz algılayabilir. ADLS Gen2 depolama hesapları için Depolama Gezgini'ni kullanabilirsiniz:
  * Oluşturun ve kapsayıcıları Sil
  * Kapsayıcı özellikleri ve izinleri (sol taraf) yönetme
  * Kapsayıcı içinde veri gitmek ve görüntülemek
  * Yeni klasör oluşturma
  * Yükleme, indirme, yeniden adlandırın ve dosya ve klasörleri Sil
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Geçici silme ve anlık görüntüleri gibi diğer tipik Blob özellikler şu anda kullanılamıyor. İzinleri yönetme de yalnızca oturum açarken kullanılabilir. Ayrıca, bir ADLS Gen2 depolama hesabında çalışırken, Depolama Gezgini AzCopy tüm karşıya ve karşıdan yüklemeler ve adını ve anahtar kimlik bilgileri tüm işlemler için kullanılabilir değilse varsayılan olarak kullanır.
* Güçlü kullanıcı geri bildirim sonra kesme kira yine aynı anda birden çok bloblarda kiraları bölümlere ayırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Zaten aktarılan dosyaların varsa, bir ADLS Gen2 depolama hesabından indirirken AzCopy bazen kilitleniyor. Bu işlem yaklaşan bir düzeltmeyi düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-161"></a>Sürümü 1.6.1
12/18/2018

### <a name="hotfixes"></a>Düzeltmeler
* API sınırlamaları nedeniyle, tüm doğrulama ObjectIds erişimi Yönet iletişim kutusunda, devre dışı bırakıldı. Şimdi, doğrulama yalnızca kullanıcının UPN meydana gelir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS Gen2 erişimi Yönet iletişim kutusunda, bir grup için izinleri değiştirilemedi. Bu düzeltilmiştir. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* Eklenen sürükle ve bırak desteği ADLS Gen2 düzenleyiciye karşıya yükleyin. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* Bazen ADLS Gen2'ye dosya ve klasörler için Özellikler iletişim kutusunda URL'si özelliği eksik bir '/'. Bu düzeltilmiştir. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Geçerli izinler ADLS Gen2 kapsayıcısı için alma, dosya veya klasör başarısız olur ve artık etkinlik günlüğü'nde görüntülenen propertly hatadır. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol üzerinde Windows MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltıldı. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Bağlan iletişim kutusu artık doğru şekilde oturum açmış olan kullanıcı yok ve kaynak bağlı görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1.6.0 içinde HNS BLOB ve dosyalar için özellikleri kaydetme, her özelliğin değerini kodlayın. Bu, yalnızca ASCII karakterler bulunan değerleri gereksiz kodlama sonuçlandı. Şimdi, ASCII olmayan karakterler içeriyorsa değerleri yalnızca şifrelenecek. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Bir HNS Blob kapsayıcısına bir klasöre yüklemek, SAS kullanıldı ve SAS okuma izni olmaması durumunda başarısız olur. Bu düzeltilmiştir. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımı iptal etme başarısız oldu. Bu düzeltilmiştir. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy, klasör adında boşluklar varsa, bir klasöre bir ADLS Gen2 Blob kapsayıcısından karşıdan yüklemeye çalışırken başarısız olur. Bu düzeltilmiştir. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB Düzenleyicisi içinde 1.6.0 bozuk. Artık sabittir. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık Depolama Gezgini aracılığıyla Blob verilerinize erişmek için kullanabileceğiniz [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Oturum açtıktan ve Depolama Gezgini'ni depolama hesabınız için anahtarları alamıyor, OAuth belirteci verilerinizle etkileşim kurulurken kimlik doğrulaması için kullanılır.
* Depolama Gezgini, artık ADLS Gen2 depolama hesaplarını destekler. Depolama Gezgini, hiyerarşik ad alanı için bir depolama hesabı etkin algıladığında, şunları yapacaksınız depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" bakın. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanı etkin olup olmadığını ya da depolama hesabınızın adı ve anahtarıyla eklediyseniz algılayabilir. ADLS Gen2 depolama hesapları için Depolama Gezgini'ni kullanabilirsiniz:
  * Oluşturun ve kapsayıcıları Sil
  * Kapsayıcı özellikleri ve izinleri (sol taraf) yönetme
  * Kapsayıcı içinde veri gitmek ve görüntülemek
  * Yeni klasör oluşturma
  * Yükleme, indirme, yeniden adlandırın ve dosya ve klasörleri Sil
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Geçici silme ve anlık görüntüleri gibi diğer tipik Blob özellikler şu anda kullanılamıyor. İzinleri yönetme de yalnızca oturum açarken kullanılabilir. Ayrıca, bir ADLS Gen2 depolama hesabında çalışırken, Depolama Gezgini AzCopy tüm karşıya ve karşıdan yüklemeler ve adını ve anahtar kimlik bilgileri tüm işlemler için kullanılabilir değilse varsayılan olarak kullanır.
* Güçlü kullanıcı geri bildirim sonra kesme kira yine aynı anda birden çok bloblarda kiraları bölümlere ayırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Zaten aktarılan dosyaların varsa, bir ADLS Gen2 depolama hesabından indirirken AzCopy bazen kilitleniyor. Bu işlem yaklaşan bir düzeltmeyi düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-160"></a>Sürüm 1.6.0
12/5/2018

### <a name="new"></a>Yeni

* Artık Depolama Gezgini aracılığıyla Blob verilerinize erişmek için kullanabileceğiniz [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Oturum açtıktan ve Depolama Gezgini'ni depolama hesabınız için anahtarları alamıyor, OAuth belirteci verilerinizle etkileşim kurulurken kimlik doğrulaması için kullanılır.
* Depolama Gezgini, artık ADLS Gen2 depolama hesaplarını destekler. Depolama Gezgini, hiyerarşik ad alanı için bir depolama hesabı etkin algıladığında, şunları yapacaksınız depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" bakın. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanı etkin olup olmadığını ya da depolama hesabınızın adı ve anahtarıyla eklediyseniz algılayabilir. ADLS Gen2 depolama hesapları için Depolama Gezgini'ni kullanabilirsiniz:
  * Oluşturun ve kapsayıcıları Sil
  * Kapsayıcı özellikleri ve izinleri (sol taraf) yönetme
  * Kapsayıcı içinde veri gitmek ve görüntülemek
  * Yeni klasör oluşturma
  * Yükleme, indirme, yeniden adlandırın ve dosya ve klasörleri Sil
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Geçici silme ve anlık görüntüleri gibi diğer tipik Blob özellikler şu anda kullanılamıyor. İzinleri yönetme de yalnızca oturum açarken kullanılabilir. Ayrıca, bir ADLS Gen2 depolama hesabında çalışırken, Depolama Gezgini AzCopy tüm karşıya ve karşıdan yüklemeler ve adını ve anahtar kimlik bilgileri tüm işlemler için kullanılabilir değilse varsayılan olarak kullanır.
* Güçlü kullanıcı geri bildirim sonra kesme kira yine aynı anda birden çok bloblarda kiraları bölümlere ayırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Zaten aktarılan dosyaların varsa, bir ADLS Gen2 depolama hesabından indirirken AzCopy bazen kilitleniyor. Bu işlem yaklaşan bir düzeltmeyi düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-150"></a>Sürüm 1.5.0
29/10/2018

### <a name="new"></a>Yeni

* Artık [AzCopy v10 (Önizleme)](https://github.com/Azure/azure-storage-azcopy) karşıya yükleme ve indirme Blobları için. Bu özelliği etkinleştirmek için "Deneysel" menüsüne gidin ve "Kullanın AzCopy için geliştirilmiş Blob karşıya yükleme ve indirme"'ye tıklayın. AzCopy, etkin olduğunda, aşağıdaki senaryolarda kullanılır:
   * Blob kapsayıcıları, araç yoluyla için dosyaların ve klasörlerin karşıya yükleyin veya sürükleyin ve bırakın.
   * Araç çubuğunda veya bağlam menüsü aracılığıyla ya da dosyaların ve klasörlerin indiriliyor.

* Ayrıca, AzCopy kullanırken:
   * Panonuza aktarımı yürütmek için kullanılan AzCopy komutunu kopyalayabilirsiniz. Etkinlik günlüğü'nde "AzCopy komutunu Panoya Kopyala" tıklamanız yeterlidir.
   * Düzenleyici blob karşıya yükledikten sonra el ile yenilemeniz gerekecektir.
   * Ekleme blobları için dosyaları karşıya yükleme desteklenmez, vhd dosyaları sayfa blobu olarak yüklenecek ve blok blobları olarak diğer tüm dosyalar yüklenir.
   * Hataları ve karşıya yükleme veya indirme sırasında oluşan çakışmaları kadar bir karşıya yükleme sonrasında ortaya değil veya indirme işlemi tamamlandı.

Son olarak, dosya paylaşımları ile AzCopy kullanma desteği gelecekte kullanıma sunulacaktır.
* Depolama Gezgini, artık Elektron 2.0.11 sürümü kullanıyor.
* Kiraları bozmak artık yalnızca bir blob üzerinde aynı anda gerçekleştirilemez. Ayrıca, kira bozucu blob adını girmek zorunda. Yanlışlıkla bir, özellikle sanal makineler için kiranın olasılığını azaltmak için bu değişiklik yapılmıştır. #394
* Oturum açma sorunları karşılaşırsanız, kimlik doğrulaması sıfırlama artık deneyebilirsiniz. "Yardım" menüsüne gidin ve bu özellik erişmek için "Sıfırla" seçeneğine tıklayın. #419

### <a name="fix"></a>Düzelt

* Güçlü kullanıcı geri bildirim sonra varsayılan öykünücü düğümünü yeniden etkin olmuştur. Bağlan iletişim kutusu aracılığıyla ek öykünücü bağlantıları eklemeye devam edebilirsiniz, ancak uygulamanızı öykünücü varsayılan bağlantı noktalarını kullanacak şekilde yapılandırılmışsa "Öykünücüsü * varsayılan bağlantı noktalarını" düğümünün altında "Yerel ve ekli/depolama hesapları" de kullanabilirsiniz. #669
* Depolama Gezgini, başında veya sonunda boşluk olan blob meta verileri değerlerini ayarlamak artık olanak tanır. #760
* "Oturum Aç" düğmesini her zaman aynı Bağlan iletişim sayfalarında etkinleştirildi. Şimdi, uygun olduğunda dışıdır. #761
* Hızlı erişim öğe eklendiğinde hızlı erişim artık konsolda bir hata oluşturur.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. #537 daha fazla bilgi için bkz.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması durumunda, bu sorunla ilgili yorum engeli değil.
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik. Bu sorun için karşıya yüklenirken veya indirilirken bir blob kapsayıcısından geçici olarak çözmek için Deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack ile çalışırken bu özellikleri kullanmak deneyen kaynakları beklenmeyen hatalarına neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-144"></a>Sürüm 1.4.4
15/10/2018

### <a name="hotfixes"></a>Düzeltmeler
* Azure kaynak yönetimi API'si sürümü, Azure ABD kamu kullanıcıları engelini kaldırmak için geri alındı. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Yüklenirken değer değiştiriciler, Depolama Gezgini tarafından kullanılan GPU miktarını azaltmak için CSS animasyonları artık kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* Dış kaynak ekler, örneğin SAS bağlantılarının ve Öykünücüler, önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * Görünen ad iliştirmekte olduğunuz kaynak özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel öykünücüleri ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Bağlı kaynaklar, hızlı erişim ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini, artık geçici silme destekler. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob kapsayıcıları düğümünü sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Görünüm geçici silinen blobları Blob seçerek Düzenleyicisi'nde "etkin ve Silinen blobları" gezinti çubuğunun yanındaki açılır.
   * Geçici silinen blobları silmeyi geri alma.

### <a name="fixes"></a>Düzeltmeleri
* Premium depolama hesabı CORS desteklemediği "CORS ayarlarını yapılandır" eylemi artık Premium depolama hesaplarında kullanılabilir. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık bir SAS bağlı Hizmetleri için paylaşılan erişim imzası özelliği yoktur. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarlama" eylemi için hızlı erişim sabitlenmiş kullanılabilir için Blob ve GPV2 depolama hesapları sunulmuştur. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazı durumlarda, Depolama Gezgini, Klasik depolama hesapları gösterecek şekilde başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Öykünücüler, Azure Storage öykünücüsü veya Azurite, gibi kullanırken, bunları kendi varsayılan bağlantı noktalarından bağlantıları için dinlemek sahip olması gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanmak mümkün olmayacaktır.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-143"></a>Sürüm 1.4.3
11/10/2018

### <a name="hotfixes"></a>Düzeltmeler
* Azure kaynak yönetimi API'si sürümü, Azure ABD kamu kullanıcıları engelini kaldırmak için geri alındı. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Yüklenirken değer değiştiriciler, Depolama Gezgini tarafından kullanılan GPU miktarını azaltmak için CSS animasyonları artık kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* Dış kaynak ekler, örneğin SAS bağlantılarının ve Öykünücüler, önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * Görünen ad iliştirmekte olduğunuz kaynak özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel öykünücüleri ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Bağlı kaynaklar, hızlı erişim ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini, artık geçici silme destekler. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob kapsayıcıları düğümünü sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Görünüm geçici silinen blobları Blob seçerek Düzenleyicisi'nde "etkin ve Silinen blobları" gezinti çubuğunun yanındaki açılır.
   * Geçici silinen blobları silmeyi geri alma.

### <a name="fixes"></a>Düzeltmeleri
* Premium depolama hesabı CORS desteklemediği "CORS ayarlarını yapılandır" eylemi artık Premium depolama hesaplarında kullanılabilir. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık bir SAS bağlı Hizmetleri için paylaşılan erişim imzası özelliği yoktur. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarlama" eylemi için hızlı erişim sabitlenmiş kullanılabilir için Blob ve GPV2 depolama hesapları sunulmuştur. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazı durumlarda, Depolama Gezgini, Klasik depolama hesapları gösterecek şekilde başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Öykünücüler, Azure Storage öykünücüsü veya Azurite, gibi kullanırken, bunları kendi varsayılan bağlantı noktalarından bağlantıları için dinlemek sahip olması gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanmak mümkün olmayacaktır.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-142"></a>Sürüm 1.4.2
24/09/2018

### <a name="hotfixes"></a>Düzeltmeler
* Azure kaynak yönetimi API sürümü yeni Azure depolama hesabı türleri için destek eklemek için 2018-07-01 güncelleştirin. [#652](https://github.com/Microsoft/AzureStorageExplorer/issues/652)

### <a name="new"></a>Yeni
* Dış kaynak ekler, örneğin SAS bağlantılarının ve Öykünücüler, önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * Görünen ad iliştirmekte olduğunuz kaynak özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel öykünücüleri ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Bağlı kaynaklar, hızlı erişim ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini, artık geçici silme destekler. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob kapsayıcıları düğümünü sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Görünüm geçici silinen blobları Blob seçerek Düzenleyicisi'nde "etkin ve Silinen blobları" gezinti çubuğunun yanındaki açılır.
   * Geçici silinen blobları silmeyi geri alma.

### <a name="fixes"></a>Düzeltmeleri
* Premium depolama hesabı CORS desteklemediği "CORS ayarlarını yapılandır" eylemi artık Premium depolama hesaplarında kullanılabilir. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık bir SAS bağlı Hizmetleri için paylaşılan erişim imzası özelliği yoktur. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarlama" eylemi için hızlı erişim sabitlenmiş kullanılabilir için Blob ve GPV2 depolama hesapları sunulmuştur. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazı durumlarda, Depolama Gezgini, Klasik depolama hesapları gösterecek şekilde başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Öykünücüler, Azure Storage öykünücüsü veya Azurite, gibi kullanırken, bunları kendi varsayılan bağlantı noktalarından bağlantıları için dinlemek sahip olması gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanmak mümkün olmayacaktır.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-141"></a>Sürüm 1.4.1
08/28/2018

### <a name="hotfixes"></a>Düzeltmeler
* İlk başlatma sırasında Depolama Gezgini hassas verileri şifrelemek için kullanılan anahtar oluşturamıyor. Bu hızlı erişim kullanılırken sorunlara neden ve kaynaklar ekleme. [#535](https://github.com/Microsoft/AzureStorageExplorer/issues/535)
* Hesabınız, kendi giriş Kiracı için MFA gerektirmeyen ancak diğer kiracılar için yaptığınız, Depolama Gezgini listesi aboneliklerini veremeyebilir. Şimdi, bu tür bir hesabıyla oturum sonra Depolama Gezgini, kimlik bilgilerinizi yeniden girmeniz ve MFA gerçekleştirmek için sorar. [#74](https://github.com/Microsoft/AzureStorageExplorer/issues/74)
* Depolama Gezgini, kaynakları Azure Almanya ve Azure ABD kamu ekleyemedi. [#572](https://github.com/Microsoft/AzureStorageExplorer/issues/572)
* Aynı e-posta adresine sahip iki hesap için azure'da oturum açarsanız, Depolama Gezgini ağaç görünümünde kaynaklarınızı göstermek bazen başarısız olur. [#580](https://github.com/Microsoft/AzureStorageExplorer/issues/580)
* Daha yavaş Windows makinelerde Karşılama ekranında görünmesini zaman önemli ölçüde bazen sürecektir. [#586](https://github.com/Microsoft/AzureStorageExplorer/issues/586)
* Vardı ekli hesapları veya hizmetleri bile bağlan iletişim kutusu görünür. [#588](https://github.com/Microsoft/AzureStorageExplorer/issues/588)

### <a name="new"></a>Yeni
* Dış kaynak ekler, örneğin SAS bağlantılarının ve Öykünücüler, önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * Görünen ad iliştirmekte olduğunuz kaynak özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel öykünücüleri ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Bağlı kaynaklar, hızlı erişim ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini, artık geçici silme destekler. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob kapsayıcıları düğümünü sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Görünüm geçici silinen blobları Blob seçerek Düzenleyicisi'nde "etkin ve Silinen blobları" gezinti çubuğunun yanındaki açılır.
   * Geçici silinen blobları silmeyi geri alma.

### <a name="fixes"></a>Düzeltmeleri
* Premium depolama hesabı CORS desteklemediği "CORS ayarlarını yapılandır" eylemi artık Premium depolama hesaplarında kullanılabilir. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık bir SAS bağlı Hizmetleri için paylaşılan erişim imzası özelliği yoktur. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarlama" eylemi için hızlı erişim sabitlenmiş kullanılabilir için Blob ve GPV2 depolama hesapları sunulmuştur. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazı durumlarda, Depolama Gezgini, Klasik depolama hesapları gösterecek şekilde başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Öykünücüler, Azure Storage öykünücüsü veya Azurite, gibi kullanırken, bunları kendi varsayılan bağlantı noktalarından bağlantıları için dinlemek sahip olması gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanmak mümkün olmayacaktır.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-130"></a>Sürüm 1.3.0
07/09/2018

### <a name="new"></a>Yeni
* Statik Web siteleri tarafından kullanılan $web kapsayıcılarını erişmesini artık desteklenmektedir. Bu, bir kolayca karşıya yüklemek ve dosya ve klasörleri, Web sitesinin kullandığı yönetmek sağlar. [#223](https://github.com/Microsoft/AzureStorageExplorer/issues/223)
* MacOS uygulama çubuğunda düzenlenirse. Değişiklikler, bir dosya menüsü, bazı kısayol önemli değişiklikler ve uygulama menüsü altında birkaç yeni komutlar içerir. [#99](https://github.com/Microsoft/AzureStorageExplorer/issues/99)
* Azure ABD devlet kurumları için oturum açmak için yetkili uç noktası değiştirildi. https://login.microsoftonline.us/
* Erişilebilirlik: Klavye ile gezinme artık ekran okuyucu etkin olduğunda, sağ tarafta öğeleri görüntülemek için kullanılan tablolar çalışır. Satırları ve sütunları, bir öğe için bağlam menüsünü açın ve Shift veya çoklu seçim için denetlemek için bağlam menüsünü anahtarı varsayılan eylemleri çağırmak için Enter gezinmek için ok tuşlarını kullanabilirsiniz. [#103](https://github.com/Microsoft/AzureStorageExplorer/issues/103)

### <a name="fixes"></a>Düzeltmeleri
*  Bazı makinelerde alt işlemleri başlatmak için uzun gerçekleştirdiğimizden. Bu, bir "zamanında başlatmak alt işlemi başarısız oldu" hatası görüntülenir. Başlatmak bir alt işlem için ayrılan süre daha artık 90 saniye için 20'den çıkarılmıştır. Yine de bu sorundan etkilenen, bağlantılı GitHub sorunu değerlendirin. [#281](https://github.com/Microsoft/AzureStorageExplorer/issues/281)
* Okuma izinlerine sahip değil bir SAS kullanırken, büyük blob yüklemek mümkün değildi. Bu senaryoda çalışmak için karşıya yükleme için mantıksal değiştirildi. [#305](https://github.com/Microsoft/AzureStorageExplorer/issues/305)
* Genel erişim düzeyi ayarı tüm erişim ilkeleri, bir kapsayıcı kaldırır ve bunun tersi de geçerlidir. Artık, genel erişim düzeyi ve erişim ilkeleri iki ya da ayarlarken korunur. [#197](https://github.com/Microsoft/AzureStorageExplorer/issues/197)
* Özellikler iletişim kutusunda "AccessTierChangeTime" kesildi. Bu düzeltilmiştir. [#145](https://github.com/Microsoft/AzureStorageExplorer/issues/145)
* "Microsoft Azure Depolama Gezgini-" ön eki yeni dizin oluştur iletişim kutusundan eksik. Bu düzeltilmiştir. [#299](https://github.com/Microsoft/AzureStorageExplorer/issues/299)
* Erişilebilirlik: Varlık Ekle iletişim kutusu VoiceOver kullanırken gidin zordu. İyileştirmeler yapılmıştır. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Erişilebilirlik: Akce a Vlastnosti bölmesi için Daralt/Genişlet düğmenin arka plan rengi, yüksek karşıtlık siyah tema benzer kullanıcı Arabirimi denetimleri ile tutarsız. Renk değiştirildi. [#123](https://github.com/Microsoft/AzureStorageExplorer/issues/123)
* Erişilebilirlik: Yüksek karşıtlık siyah temada 'X' düğmesini Özellikleri iletişim kutusu için stil oluşturma odağı görünür değil. Bu düzeltilmiştir. [#243](https://github.com/Microsoft/AzureStorageExplorer/issues/243)
* Erişilebilirlik: Akce a Vlastnosti sekmeleri subpar ekran okuyucu deneyimde sonuçlandı birkaç aria değeri eksikti. Eksik aria değerleri şimdi ekledik. [#316](https://github.com/Microsoft/AzureStorageExplorer/issues/316)
* Erişilebilirlik: İşlecin sol tarafındaki daraltılmış ağaç düğümleri aria-genişletilmiş bir false değerini verilen değil. Bu düzeltilmiştir. [#352](https://github.com/Microsoft/AzureStorageExplorer/issues/352)

### <a name="known-issues"></a>Bilinen Sorunlar
* Bir blob kapsayıcısı gibi SAS URI'sini eklenen bir kaynağından ayırma doğru gösteren gelen diğer ekleri engelleyen hataya neden olabilir. Bu sorunu çözmek için yalnızca Grup düğümünü yenileyin. Bkz: [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/537) daha fazla bilgi için.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez ve Azure Stack ile çalışırken kullanılmaya çalışılırsa, beklenmeyen hatalarına neden olabilir:
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-120"></a>Sürümü 1.2.0
06/12/2018

### <a name="new"></a>Yeni
* Depolama Gezgini yalnızca kiracılarınızın bir alt kümesinden aboneliklerinizi başarısız olursa, ardından başarıyla yüklenen tüm abonelikleri özellikle başarısız olan kiracılar için bir hata iletisi ile birlikte gösterilir. [#159](https://github.com/Microsoft/AzureStorageExplorer/issues/159)
* Bir güncelleştirme kullanılabilir olduğunda Windows üzerinde artık "Güncelleştirme üzerinde Kapat" seçebilirsiniz. Bu seçenek seçilir, Depolama Gezgini kapattıktan sonra güncelleştirme yükleyicisi çalışır. [#21](https://github.com/Microsoft/AzureStorageExplorer/issues/21)
* Geri yükleme anlık görüntü bir dosya paylaşımı anlık görüntüsü görüntülerken için dosya paylaşımı Düzenleyicisi bağlam menüsü eklendi. [#131](https://github.com/Microsoft/AzureStorageExplorer/issues/131)
* Sırayı Temizle düğmesi artık her zaman etkindir. [#135](https://github.com/Microsoft/AzureStorageExplorer/issues/135)
* Azure Stack'e ADFS oturum açma desteği yeniden etkinleştirildi olmuştur. Azure Stack 1804 veya üzeri bir sürüm gereklidir. [#150](https://github.com/Microsoft/AzureStorageExplorer/issues/150)

### <a name="fixes"></a>Düzeltmeleri
* Bir önek aynı depolama hesabındaki başka bir dosya paylaşımı adı olan bir dosya paylaşımı için anlık görüntüleri görüntülerse, bir dosya paylaşımı için anlık görüntüleri de listelenir. Bu sorun düzeltilmiştir. [#255](https://github.com/Microsoft/AzureStorageExplorer/issues/255)
* Bir dosyayı dosya paylaşım anlık görüntüsünden geri yükleme, SAS bağlı olduğunda, bir hataya neden olur. Bu sorun düzeltilmiştir. [#211](https://github.com/Microsoft/AzureStorageExplorer/issues/211)
* Temel blob ve tek bir anlık görüntü seçilmedi, bir blobun anlık görüntülerini görüntülerken, anlık görüntü yükseltme eylemi etkinleştirildi. Tek bir anlık görüntü seçtiyseniz eylemi artık yalnızca etkindir. [#230](https://github.com/Microsoft/AzureStorageExplorer/issues/230)
* (Örneğin, bir blob yükleme) tek bir iş başlatıldı ve daha sonra başarısız oldu, aynı türde başka bir iş başlatıldı kadar otomatik olarak yeniden. Tüm işleri yeniden deneme artık otomatik, bağımsız olarak kaç tane işin kuyruğa alınmış.
* Düzenleyiciler için yeni GPV2 içinde oluşturulan blob kapsayıcıları açılır ve Blob Depolama hesaplarına erişim katmanı sütun yok. Bu sorun düzeltilmiştir. [#109](https://github.com/Microsoft/AzureStorageExplorer/issues/109)
* Bir depolama hesabı, bir erişim katmanı sütunu bazen gösterilmeyebiliyor veya blob kapsayıcısı SAS eklendi. Sütun şimdi her zaman gösterilir, ancak erişim katmanını ayarlanmadı ise boş bir değere sahip. [#160](https://github.com/Microsoft/AzureStorageExplorer/issues/160)
* Karşıya yüklenen yeni blok blobun erişim katmanını ayarlama devre dışı bırakıldı. Bu sorun düzeltilmiştir. [#171](https://github.com/Microsoft/AzureStorageExplorer/issues/171)
* Klavyeyi kullanarak "Sekmesini açık tutun" düğmesini çağrıldı, klavye odağına kaybolur. Şimdi, odağı açık tutulduğunda sekmenin taşınır. [#163](https://github.com/Microsoft/AzureStorageExplorer/issues/163)
* Sorgu Oluşturucusu sorgu için VoiceOver kullanılabilir geçerli işleç açıklamasını vererek değil. Artık daha açıklayıcı olduğundan. [#207](https://github.com/Microsoft/AzureStorageExplorer/issues/207)
* Çeşitli düzenleyiciler için sayfalandırma bağlantıları açıklayıcı değil. Daha açıklayıcı olması için değiştirildi. [#205](https://github.com/Microsoft/AzureStorageExplorer/issues/205)
* Varlık Ekle iletişim kutusunda VoiceOver hangi sütunun bir input öğesi parçası olan Duyurusu değil. Geçerli sütunun adı, Açıklama öğesinin artık dahildir. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Radyo düğmeleri ve onay kutularını odaklama sırasında görünür bir kenarlığa sahip değil. Bu sorun düzeltilmiştir. [#237](https://github.com/Microsoft/AzureStorageExplorer/issues/237)

### <a name="known-issues"></a>Bilinen Sorunlar
* Öykünücüler, Azure Storage öykünücüsü veya Azurite, gibi kullanırken, bunları kendi varsayılan bağlantı noktalarından bağlantıları için dinlemek sahip olması gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanmak mümkün olmayacaktır.
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-110"></a>Sürüm 1.1.0
09/05/2018

### <a name="new"></a>Yeni
* Depolama Gezgini, artık Azurite kullanımını destekler. Not: varsayılan geliştirme uç noktalarına ' belirtebilseydik Azurite için bağlantıdır.
* Depolama Gezgini, artık yalnızca Blob ve GPV2 depolama hesapları için erişim katmanları destekler. Erişim katmanları hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).
* Başlangıç zamanı, artık bir SAS oluşturulurken gereklidir.

### <a name="fixes"></a>Düzeltmeleri
* ABD kamu hesaplar için abonelikleri almaya kesildi. Bu sorun düzeltilmiştir. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Süre sonu için erişim ilkeleri doğru şekilde kaydedilmedi. Bu sorun düzeltilmiştir. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Bir öğe için bir SAS URL'si bir kapsayıcıda oluştururken, öğenin adını URL'ye eklenen değil. Bu sorun düzeltilmiştir. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Bir SAS oluştururken, daha önce olan bitiş kez bazen varsayılan değer olacaktır. Bu, son başlangıç ve bitiş saati varsayılan değerleri kullanılan Depolama Gezgini'ni kullanarak nedeniyle oluştu. Şimdi SAS iletişim kutusu her açışlarında, varsayılan değerleri yeni bir dizi oluşturulur. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Depolama hesapları arasında kopyalama yapılırken, 24 saatlik bir SAS oluşturulur. 24 saatten fazla kopyayı görüşmelerin Süren, kopyalama başarısız olur. Biz, süresi dolmuş bir SAS nedeniyle başarısız olan bir kopyasını olasılığını azaltmak için SAS ait son 1 hafta artırdık. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* Bazı etkinlikler için 'İptal "'i tıklatarak her zaman çalışmaz. Bu sorun düzeltilmiştir. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Bazı etkinlikler için aktarım hızı yanlıştı. Bu sorun düzeltilmiştir. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* Görünüm menüsünde "Geri" yazımını yanlıştı. Bunu şimdi doğru yazıldığından. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* Windows Installer'ın son sayfa, "İleri" düğmesi vardı. "Son" düğmesini değiştirildi. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* Sekme odağı, HC siyah tema kullanırken kutularındaki düğmeler için görünür değil. Artık görünür durumda. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* "Otomatik Çözümle" büyük/küçük harf eylemleri etkinlik günlüğünde için yanlış. Artık doğru mu? [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Varlığın tablodan silinirken bir hata simgesi onaylanmasını isteyen bir iletişim kutusu görüntülenir. İletişim kutusu, bir uyarı simgesi artık kullanır. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bilinen Sorunlar
* Mac için VS kullanın ve özel bir AAD yapılandırmasına hiç olmadığı kadar oluşturdunuz, oturum açma alınamıyor olabilir. Sorunu çözmek için içeriğini silin. ~ /. IdentityService/AadConfigurations. Bunun yapılması engeli değil, lütfen yorum [bu sorunu](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite henüz tam olarak tüm depolama API'leri uygulamadı. Bu nedenle, beklenmeyen hatalar veya olabilir davranışı Azurite geliştirme depolaması için kullanılırken.
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Nodejs'de bir hata nedeniyle, OneDrive klasöründen karşıya çalışmaz. Hata düzeltildi, ancak henüz Elektron tümleşik.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-100"></a>Sürüm 1.0.0
16/04/2018

### <a name="new"></a>Yeni
* Depolama Gezgini, Visual Studio 2017 aynı hesap deposu kullanmak izin veren gelişmiş kimlik doğrulama. Bu özelliği kullanmak için yeniden oturum açma, hesaplarınıza ve filtrelenmiş aboneliklerinizi yeniden ayarlamak gerekir.
* AAD tarafından desteklenen Azure Stack hesapları için 'Hedef Azure Stack' etkinleştirildiğinde Depolama Gezgini'ni Azure Stack aboneliklerini artık alır. Artık bir özel oturum açma ortamı oluşturmak gerekir.
* Bazı kısayollar, daha hızlı bir gezinti olanağı eklendi. Bunlar, çeşitli paneller geçiş ve düzenleyicileri arasında taşımayı içerir. Görünüm menüsü daha fazla ayrıntı için bkz.
* Depolama Gezgini geri bildirim, artık Github'da bulunur. Sola veya giderek için alt geri bildirim düğmesini tıklatarak sorunları sayfamızı size ulaşabildiğimizden [ https://github.com/Microsoft/AzureStorageExplorer/issues ](https://github.com/Microsoft/AzureStorageExplorer/issues). Önerilerde bulunmak, ilgili sorunları bildirmek, soru veya diğer türde bir geri bildirim bırakmak çekinmeyin.
* SSL sertifikası bir sorunla karşılaşırsanız çalıştırıyorsanız ve soruna neden olan bir sertifika bulamadı, artık Depolama Gezgini ile komut satırından başlatabilirsiniz `--ignore-certificate-errors` bayrağı. Depolama Gezgini ile bu bayrağı başlatıldığında SSL sertifika hataları göz ardı eder.
* Artık bir blob ve dosya öğeleri için bağlam menüsünden 'İndir' seçeneği yoktur.
* Geliştirilmiş erişilebilirlik ve ekran okuyucu desteği. Erişilebilirlik özelliklerini kullanan olup bizim [erişilebilirlik belgeleri](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-accessibility) daha fazla bilgi için.
* Depolama Gezgini'ni Elektron 1.8.3 artık kullanır.

### <a name="breaking-changes"></a>Hataya Neden Olan Değişiklikler
* Depolama Gezgini için yeni bir kimlik doğrulama kitaplığı geçti. Kitaplığı geçiş işleminin bir parçası olarak yeniden oturum açma, hesaplarınıza ve filtrelenmiş aboneliklerinizi yeniden ayarlayın gerekir
* Hassas verileri şifrelemek için kullanılan yöntem değişti. Bu yeniden eklenmesi gerek kalmadan, hızlı erişim öğelerden bazıları neden olabilir ve/veya bazı iliştirilmesi gerek kalmadan kaynaklara bağlı.

### <a name="fixes"></a>Düzeltmeleri
* Proxy arkasında bazı kullanıcılar grubu blob karşıya yüklemeleri veya indirmeleri 'bir çözümlenemiyor tarafından' kesintiye gerekir. hata iletisi. Bu sorun düzeltilmiştir.
* Oturum açma 'Oturum açma' satırına tıklayarak doğrudan bir bağlantı kullanarak boş bir iletişim kutusu sorar sırada gerekiyorsa. Bu sorun düzeltilmiştir.
* Depolama Gezgini, bir GPU işlem kilitlenmesi nedeniyle başlatılamıyor ise, Linux'ta, artık kullanmak için bir uyarıyla kilitlenme gönderilerek '--gpu devre dışı bırak ' geçiş ve Depolama Gezgini'ni etkin anahtar ile otomatik olarak yeniden başlatın.
* Geçersiz erişim ilkeleri, erişim ilkeleri iletişim kimliği sabit. Geçersiz erişim ilkesi kimlikleri artık özetlenen kırmızı daha fazla görünürlük.
* Etkinlik günlüğü, bazen bir etkinlik farklı bölümleri arasındaki boşluk geniş alanlar olması gerekir. Bu sorun düzeltilmiştir.
* Tablo Sorgu Düzenleyicisi'nde, bir zaman damgası yan tümcesi geçersiz bir durumda kaldı ve ardından başka bir yan tümcesi değiştirmeye çalıştınız Düzenleyicisi dondurma. Başka bir yan tümcesinde bir değişiklik algıladığında Düzenleyicisi artık zaman damgası yan tümcesi son geçerli durumuna geri yükler.
* Ağaç görünümünde arama sorgunuzda yazarken duraklatılmışsa, aramayı başlatmak ve odak metin kutusundan çalınırsa. Şimdi, 'Enter' tuşuna basarak veya başlangıç arama düğmesine tıklayarak arama açıkça başlatmanız gerekir.
* 'Paylaşılan erişim imzası Al' komutunu bazen bir dosya paylaşımı bir dosyaya sağ tıklayıp devre dışı. Bu sorun düzeltilmiştir.
* Arama sırasında kaynak ağaç düğümü odaklanılan filtrenin gerekirse değil kaynak ağacına sekme ve kaynak ağaç gezinmek için ok tuşlarını kullanın. Odaklanmış kaynak ağaç düğümü gizli ise, artık kaynak ağacında ilk düğümü otomatik olarak odaklanmış.
* Ek bir ayırıcı bazen Düzenleyici araç çubuğunda görünür olur. Bu sorun düzeltilmiştir.
* İçerik haritası metin kutusu bazen taşma oluşturursunuz. Bu sorun düzeltilmiştir.
* Blob ve dosya paylaşımı düzenleyicileri sürekli olarak bazen aynı anda çok sayıda dosya karşıya yüklenirken yeniler. Bu sorun düzeltilmiştir.
* 'Klasör İstatistikleri' özelliği, dosya paylaşımı anlık görüntüleri yönetim görünümünde herhangi bir amacı vardı. Şimdi devre dışı.
* Linux üzerinde Dosya menüsünü görülmedi. Bu sorun düzeltilmiştir.
* Bir klasörü varsayılan olarak bir dosya paylaşımına karşıya yüklenirken klasörünün içeriği karşıya yüklendi. Şimdi, eşleşen bir dosya paylaşımı klasörüne klasörünün içeriği karşıya yüklemek için varsayılan davranışı geldi.
* Birkaç iletişim kutusu düğmeleri sıralama ters. Bu sorun düzeltilmiştir.
* Çeşitli güvenlik düzeltmeleri ilgili.

### <a name="known-issues"></a>Bilinen Sorunlar
* Nadiren de olsa, ağaç odağı Hızlı erişimi takılabilir. Odağı Ayır için Tümünü Yenile yapabilirsiniz.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için yüklemeniz gerekecek [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Sürüm 0.9.6
02/28/2018

### <a name="fixes"></a>Düzeltmeleri
* Bir sorun, beklenen blobları/dosya Düzenleyicisi'nde listelenen engelledi. Bu sorun düzeltilmiştir.
* Hatalı öğeleri görüntülemek için anlık görüntü görünümleri arasında geçişi kaynaklanan bir sorun. Bu sorun düzeltilmiştir.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini, ADFS hesaplarını desteklemez.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu iptal filtre geçici kullanıyoruz çünkü açıklanan [burada](https://github.com/Azure/azure-storage-node/issues/317).
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Sürüm 0.9.5
02/06/2018

### <a name="new"></a>Yeni

* Dosya Paylaşımı anlık görüntüleri için destek:
    * Oluşturup, dosya paylaşımları için anlık görüntüleri yönetin.
    * Keşif yaparken görünümleri, dosya paylaşımlarının anlık görüntüler arasında kolayca geçiş yapabilirsiniz.
    * Dosyalarınızı önceki sürümlerini geri yükleyin.
* Azure Data Lake Store için Önizleme desteği:
    * Birden çok hesabındaki ADLS kaynaklarınızı bağlanın.
    * Bağlanmak ve ADLS kaynaklarında ADL URI'lerini kullanarak paylaşın.
    * Temel dosya/klasör işlemleri yinelemeli olarak gerçekleştirin.
    * Hızlı erişim için PIN tek tek klasörler.
    * Klasör istatistikleri görüntüler.

### <a name="fixes"></a>Düzeltmeleri
* Başlangıç performans geliştirmeleri.
* Çeşitli hata düzeltmeleri.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini, ADFS hesaplarını desteklemez.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>0.9.4 ve 0.9.3 sürümü
21/01/2018

### <a name="new"></a>Yeni
* Mevcut Depolama Gezgini pencereniz ne zaman yeniden kullanılan olacaktır:
    * Depolama Gezgini'nde oluşturulan doğrudan bağlantılar açılıyor.
    * Depolama Gezgini, portaldan açılıyor.
    * Depolama Gezgini (çok yakında) Azure depolama VS Code uzantı açılıyor.
* Depolama Gezgini içinde yeni Depolama Gezgini pencereden açtığınız özelliği eklendi.
    * Windows için bir dosya menüsü altında ve bağlam menüsünde görev çubuğunun 'Yeni pencere' seçeneği yoktur.
    * Mac için bir uygulama menüsü altındaki 'Yeni pencere' seçeneği yoktur.

### <a name="fixes"></a>Düzeltmeleri
* Bir güvenlik sorunu düzeltildi. En yakın zamanda 0.9.4 yükseltin.
* Eski etkinlikler uygun şekilde temizlendi değil. Bu, uzun çalışan işleri performansı etkilenir. Bunlar artık doğru şekilde temizlenir.
* Çok sayıda dosya ve dizinleri içeren Eylemler, bazen dondurmak Depolama Gezgini'ni neden olur. Azure dosya paylaşımları için istekleri artık sistem kaynak kullanımını sınırlamak için kısıtlanmış.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini, ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Sürüm 0.9.2
11/01/2017

### <a name="hotfixes"></a>Düzeltmeler
* Beklenmeyen veri değişikliklerini Edm.DateTime değerlerini yerel saat dilimi bağlı olarak tablo varlıkları düzenlerken mümkün. Düzenleyicisi bir düz metin kutusu Edm.DateTime değerleri üzerinde kesin, tutarlı denetim vererek olarak kullanır.
* Bir grup adı ve anahtarı ile eklendiğinde blobları karşıya yükleme/indirme başlatılmayacaktır. Bu sorun düzeltilmiştir.
* Daha önce Depolama Gezgini yalnızca eski hesap varsa sağlamalarını ister veya daha fazla hesabın aboneliklerini seçilmiştir. Hesabı tamamen filtrelendi bile Depolama Gezgini, artık ister.
* Azure ABD kamu uç noktaları etki alanı yanlış. Düzeltilmiştir.
* Bazen hesaplarını yönetme panosunda Uygula düğmesini tıklatın bırakmak zordu. Bu artık gerçekleşmelidir.

### <a name="new"></a>Yeni
* Azure Cosmos DB için Önizleme desteği:
    * [Çevrimiçi belgeleri](./cosmos-db/storage-explorer.md)
    * Veritabanları ve Koleksiyonlar oluşturun
    * Verileri denetleme
    * Sorgu, oluşturma veya belgeler silme
    * Saklı yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler güncelleştir
    * Bağlanmak ve veritabanlarınızı yönetmek için bağlantı dizelerini kullanma
* Birçok küçük blobu karşıya yükleme/indirme performansı geliştirildi.
* Blobu karşıya yükleme grubu veya blob indirme grubundaki başarısızlık varsa "Tümünü yeniden deneyin" eylemi eklendi.
* Ağ bağlantısı kaybedildi algılarsa, Depolama Gezgini artık yineleme sırasında blob karşıya yükleme/indirme duraklatılır. Ağ bağlantısı yeniden kurulmuş silindikten sonra yineleme sonra sürdürebilirsiniz.
* "Tümünü Kapat", "Kapat diğerleri" ve bağlam menüsünden "Kapat" sekmeler özelliği eklendi.
* Depolama Gezgini, artık yerel iletişim kutuları ve yerel bağlam menüleri kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunlardır:
    * Geliştirilmiş ekran okuyucu desteği, NVDA üzerinde Windows ve Mac üzerinde VoiceOver
    * Geliştirilmiş Yüksek karşıtlık teması oluşturma
    * Sekme ve klavye klavye odağı giderir

### <a name="fixes"></a>Düzeltmeleri
* Açın veya geçersiz bir Windows dosya adına sahip bir blobu indirmek denediyseniz, işlem başarısız olur. Depolama Gezgini, artık bir blob adı geçersiz tümüyse ve, kodlayamadığı veya blob atlamak isteyip istemediğinizi sorar. Depolama Gezgini, bir dosya adı kodlanması ve size sorun görünüp görünmeyeceğini de algılar, karşıya yüklemeden önce çözmek istiyor.
* BLOB karşıya yükleme sırasında hedef blob kapsayıcısı için düzenleyici bazen düzgün yenilenmemesi. Bu sorun düzeltilmiştir.
* Bağlantı dizelerini ve SAS URI'ın çeşitli forms desteği gerileyen. Size, tüm bilinen sorunları giderdikten ancak başka bir sorunla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0 bazı kullanıcılar için kesildi. Bu sorun düzeltilmiştir ve bunlar için hatadan etkilenen, Depolama Gezgini en son sürümünü el ile karşıdan yükleyebileceğiniz [burada](https://azure.microsoft.com/features/storage-explorer/).

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini, ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>0.9.1 ve 0.9.0 sürümü
10/20/2017
### <a name="new"></a>Yeni
* Azure Cosmos DB için Önizleme desteği:
    * [Çevrimiçi belgeleri](./cosmos-db/storage-explorer.md)
    * Veritabanları ve Koleksiyonlar oluşturun
    * Verileri denetleme
    * Sorgu, oluşturma veya belgeler silme
    * Saklı yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler güncelleştir
    * Bağlanmak ve veritabanlarınızı yönetmek için bağlantı dizelerini kullanma
* Birçok küçük blobu karşıya yükleme/indirme performansı geliştirildi.
* Blobu karşıya yükleme grubu veya blob indirme grubundaki başarısızlık varsa "Tümünü yeniden deneyin" eylemi eklendi.
* Ağ bağlantısı kaybedildi algılarsa, Depolama Gezgini artık yineleme sırasında blob karşıya yükleme/indirme duraklatılır. Ağ bağlantısı yeniden kurulmuş silindikten sonra yineleme sonra sürdürebilirsiniz.
* "Tümünü Kapat", "Kapat diğerleri" ve bağlam menüsünden "Kapat" sekmeler özelliği eklendi.
* Depolama Gezgini, artık yerel iletişim kutuları ve yerel bağlam menüleri kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunlardır:
    * Geliştirilmiş ekran okuyucu desteği, NVDA üzerinde Windows ve Mac üzerinde VoiceOver
    * Geliştirilmiş Yüksek karşıtlık teması oluşturma
    * Sekme ve klavye klavye odağı giderir

### <a name="fixes"></a>Düzeltmeleri
* Açın veya geçersiz bir Windows dosya adına sahip bir blobu indirmek denediyseniz, işlem başarısız olur. Depolama Gezgini, artık bir blob adı geçersiz tümüyse ve, kodlayamadığı veya blob atlamak isteyip istemediğinizi sorar. Depolama Gezgini, bir dosya adı kodlanması ve size sorun görünüp görünmeyeceğini de algılar, karşıya yüklemeden önce çözmek istiyor.
* BLOB karşıya yükleme sırasında hedef blob kapsayıcısı için düzenleyici bazen düzgün yenilenmemesi. Bu sorun düzeltilmiştir.
* Bağlantı dizelerini ve SAS URI'ın çeşitli forms desteği gerileyen. Size, tüm bilinen sorunları giderdikten ancak başka bir sorunla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0 bazı kullanıcılar için kesildi. Bu sorun düzeltilmiştir ve bunlar için hatadan etkilenen, Depolama Gezgini en son sürümünü el ile karşıdan yükleyebileceğiniz [burada](https://azure.microsoft.com/features/storage-explorer/)

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini, ADFS hesaplarını desteklemez.
* Kısayol tuşları "Görünümü Gezgini" ve "Görünümü hesap yönetimi" Ctrl olmalıdır / Cmd + SHIFT + E ve Ctrl / Cmd + Shift + A sırasıyla.
* Azure Stack hedeflerken, ekleme blobları gibi belirli dosyaları karşıya yükleme başarısız olabilir.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. İptal filtre geçici kullanıyoruz çünkü burada açıklanmıştır.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan Elektron Kabuk bazı GPU (grafik işlem birimi) Donanım hızlandırmalı sorun vardır. Depolama Gezgini (boş) bir boş ana penceresi görüntüleniyorsa, deneyebileceğiniz Depolama Gezgini komut satırından başlatmak ve ekleyerek GPU hızlandırmasını devre dışı bırakma `--disable-gpu` geçin:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Sürüm 0.8.16
8/21/2017

### <a name="new"></a>Yeni
* Bir blob açtığınızda, Depolama Gezgini, bir değişiklik algılanırsa indirilen dosyayı karşıya yüklemeyi ister
* Gelişmiş Azure Stack oturum açma deneyimi
* Karşıya yükleme/birçok küçük dosyaları aynı anda indirme performansı geliştirildi


### <a name="fixes"></a>Düzeltmeleri
* Bazı blob türleri için "bir karşıya yükleme çakışması sırasında değiştirmek" seçerek yeniden başlatılmadan karşıya yükleme bazen sonuçlanır.
* 0.8.15 sürümünde yüklemeleri bazen 99 oranında kabin.
* Belirtmiyor bir dizine seçerseniz, bir dosya paylaşımına dosya yükleme henüz mevcut olduğunda, karşıya yükleme işleminiz başarısız olur.
* Depolama Gezgini hatalı zaman damgaları için paylaşılan erişim imzalarını ve tablo sorguları oluşturma.


### <a name="known-issues"></a>Bilinen Sorunlar
* Bir ad ve anahtar bağlantı dizesini kullanarak şu anda çalışmıyor. Sonraki sürümde düzeltilecektir. O zamana kadar kullandığınız adı ve anahtarı ile ekleyin.
* İndirme, geçersiz bir Windows dosya adı ile bir dosyayı açmaya çalışırsanız, bir dosya bulunamadı hatası neden olur.
* "İptal" görevde tıklandıktan sonra onu iptal etmek bu görev için biraz zaman alabilir. Bu, Azure Depolama düğümü kitaplığının bir sınırlamasıdır.
* Bir blob karşıya yükleme işlemini tamamladıktan sonra karşıya yükleme başlatılan sekmesini yenilenir. Bu, önceki davranışı farklıdır ve ayrıca bulunduğunuz kapsayıcı köküne geri alınması için neden olur.
* Yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz, depolama Gezgini'nin kararı unutursanız studio'durdurup yeniden başlatmanız gerekir.
* Hesap Ayarları panelinde abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Ubuntu 14.04 kullanıcıları için GCC güncel - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir emin olmak gerekir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları, GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırıp makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Sürüm 0.8.14
06/22/2017

### <a name="new"></a>Yeni

* 1.7.2 birden fazla kritik güvenlik güncelleştirmelerini yararlanabilmek için güncelleştirilmiş Elektron sürümü
* Artık hızlı bir şekilde çevrimiçi sorun giderme kılavuzu için Yardım menüsünden erişebilirsiniz
* Depolama Gezgini sorunlarını giderme [Kılavuzu][2]
* [Yönergeler] [ 3] bir Azure Stack aboneliğine bağlanma

### <a name="known-issues"></a>Bilinen Sorunlar

* Düğmeleri silme klasör onay iletişim kutusunda, Linux üzerinde fare tıklama ile kayıt ettirmezseniz. İş Enter tuşunu kullanmak için yaklaşık değildir.
* Ardından yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz kararı unutursanız Depolama Gezgini sahip olmak için yeniden başlatmanız gerekecektir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir
* Abonelikleri Filtrele için kimlik bilgilerini yeniden girmeye gerek Hesap Ayarları panelini Göster
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme gcc sürümü güncelleştirildi gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Sürüm 0.8.13
12.05.2017

#### <a name="new"></a>Yeni

* Depolama Gezgini sorunlarını giderme [Kılavuzu][2]
* [Yönergeler] [ 3] bir Azure Stack aboneliğine bağlanma

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: Karşıya dosya yükleme, yetersiz bellek hatası neden yüksek şansınız
* Düzeltildi: Artık PIN/akıllı kart ile oturum
* Düzeltildi: Portalda artık Azure Çin 21Vianet, Azure Almanya, Azure ABD kamu ve Azure Stack ile works açın
* Düzeltildi: Bir blob kapsayıcısına bir klasörü karşıya yüklenirken bir "İşlem geçersiz" hatası bazen oluşacak
* Düzeltildi: Tümünü Seç anlık görüntüleri yönetirken devre dışı bırakıldı
* Düzeltildi: Meta verileri temel BLOB anlık görüntüleri özelliklerini görüntüledikten sonra üzerine

#### <a name="known-issues"></a>Bilinen Sorunlar

* Ardından yanlış PIN/akıllı kart sertifikası seçeneğini belirlerseniz kararı unutursanız Depolama Gezgini sahip olmak için yeniden başlatmanız gerekecektir
* Yakınlaştırma düzeyi veya uzaklaştırılacağını sırasında kısa bir süre içinde varsayılan düzeyini sıfırlayabilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir
* Abonelikleri Filtrele için kimlik bilgilerini yeniden girmeye gerek Hesap Ayarları panelini Göster
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme gcc sürümü güncelleştirildi gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Sürüm 0.8.12 ve 0.8.11 ve 0.8.10
04/07/2017

#### <a name="new"></a>Yeni

* Güncelleştirme bildiriminden bir güncelleştirme yüklediğinizde, Depolama Gezgini otomatik olarak kapatılacak
* Yerinde hızlı erişim sık erişilen kaynaklarınızla çalışmak için iyileştirilmiş bir deneyim sağlar.
* Blob kapsayıcı Düzenleyicisi'nde kiralanmış blob ait hangi sanal makine artık görebilirsiniz
* Artık sol tarafta Panel'i Daralt
* Bulma indirme ile aynı zamanda artık çalışır.
* İstatistikleri, kaynak ya da seçimi boyutunu görmek için Blob kapsayıcısını, dosya paylaşımı ve tablo düzenleyicilerde kullanın.
* Azure Stack hesabı oturum açma için Azure Active Directory (AAD) tabanlı artık.
* Arşiv dosyalarını üzerinde 32 MB Premium depolama hesapları için şimdi karşıya yükleyebilirsiniz
* Geliştirilmiş erişilebilirlik desteği
* Artık güvenilir Base-64 ekleyebilirsiniz giderek düzenleme - kodlanmış X.509 SSL sertifikaları&gt; SSL sertifikaları -&gt; sertifikaları İçeri Aktar

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: bir hesabın kimlik bilgilerini yeniledikten sonra ağaç görünümünde bazen otomatik olarak yeniler değil
* Düzeltildi: öykünücüsü kuyruklar ve tablolar için SAS oluşturma geçersiz bir URL neden olur
* Düzeltildi: proxy etkinken premium depolama hesapları artık Genişletilebilir
* Düzeltildi: Seçili 1 veya 0 hesaplarını olsaydı hesapları Yönetim sayfasında Uygula düğmesi çalışmıyordu
* Düzeltildi: çakışma çözümlemelerini gerektiren blobları karşıya yükleme - 0.8.11 içinde sabit başarısız olabilir
* Düzeltildi: geri bildirim gönderme 0.8.12 içinde sabit 0.8.11 içinde-hatalı olan

#### <a name="known-issues"></a>Bilinen Sorunlar

* İçin 0.8.10 yükselttikten sonra tüm kimlik bilgilerinizi yenilemeniz gerekecektir.
* Veya uzaklaştırılacağını olsa da Yakınlaştırma düzeyini varsayılan düzeyini kısa bir süre içinde sıfırlayabilir.
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir.
* Abonelikleri Filtrele için kimlik bilgilerini yeniden girmeye gerek Hesap Ayarları panelini gösterebilir.
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur.
* Azure Stack, şu anda dosya paylaşımlarını desteklemiyor olsa da, dosya paylaşımları düğümündeki bağlı bir Azure Stack depolama hesaplarının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme gcc sürümü güncelleştirildi gerekir veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:

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

* Depolama Gezgini 0.8.9 güncelleştirmeleri için en son sürümü otomatik olarak indirir.
* Düzeltme: bir portal kullanarak bir depolama hesabı eklemek için oluşturulan SAS URI'SİNİN bir hataya neden olur.
* Şimdi, oluşturmak, yönetmek ve blob anlık görüntüleri tanıtın.
* Artık Azure Çin 21Vianet, Azure Almanya ve Azure ABD kamu hesapları için oturum.
* Şimdi, yakınlaştırma düzeyini değiştirebilirsiniz. Yakınlaştırma, uzaklaştırma ve yakınlaştırma sıfırlama Görünüm menüsündeki seçenekleri kullanın.
* Unicode karakterler artık kullanıcı meta verileri BLOB ve dosyalar için desteklenir.
* Erişilebilirlik geliştirmeleri.
* Sonraki sürüme ait sürüm notları güncelleştirme bildiriminden görüntülenebilir. Geçerli sürüm notları Yardım menüsünden de görüntüleyebilirsiniz.

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: sürüm numarası artık doğru şekilde Windows üzerinde Denetim Masası'nda görüntülenir
* Düzeltildi: arama artık 50.000 düğümlerine sınırlıdır
* Düzeltildi: hedef dizin zaten mevcut değilse sonsuza kadar hazırladık bir dosya paylaşımına yükleyin
* Düzeltildi: Geliştirilmiş kararlılık ve uzun indirmelere için

#### <a name="known-issues"></a>Bilinen Sorunlar

* Veya uzaklaştırılacağını olsa da Yakınlaştırma düzeyini varsayılan düzeyini kısa bir süre içinde sıfırlayabilir.
* Hızlı erişim, yalnızca abonelik bağlı öğeleri ile çalışır. Yerel veya anahtarı veya SAS belirteci bağlı kaynaklar bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir.
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir.

12/16/2016
### <a name="version-087"></a>Sürüm 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Yeni

* Bir update, indirme veya kopyalama oturumu başına etkinlikleri penceresinde çakışmalarını nasıl seçebilirsiniz
* Depolama kaynağı tam yolunu görmek için bir sekmenin üzerine getirin
* Bir sekmede tıkladığınızda, sol tarafındaki gezinme bölmesinde, konumu ile eşitler

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: Depolama Gezgini artık Mac üzerinde güvenilen bir uygulama
* Düzeltildi: Ubuntu 14.04 yeniden desteklenir
* Düzeltildi: Abonelikler yüklenirken kullanıcı Arabirimi Hesap Ekle bazen yanıp
* Düzeltildi: Bazen tüm depolama kaynaklarını sol tarafındaki gezinme bölmesinde listelenen
* Düzeltildi: Eylem bölmesinde görüntülenen bazen boş eylemleri
* Düzeltildi: Pencere boyutu son kapalı oturumundan artık korunur
* Düzeltildi: Bağlam menüsünü kullanarak aynı kaynak için birden fazla sekme açabilirsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Hızlı erişim, yalnızca abonelik bağlı öğeleri ile çalışır. Yerel veya anahtarı veya SAS belirteci bağlı kaynaklar bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir
* Bundan sonra yaklaşık 50.000 düğümlere - arama arama tanıtıcıları performans etkileniyor olabilir veya işlenmeyen özel durumuna neden olabilir
* MacOS üzerinde Depolama Gezgini'ni kullanarak ilk kez, anahtar zinciri erişim için kullanıcının izni isteyen birden çok istemler görebilirsiniz. Bu istemi yeniden görünmez şekilde her zaman izin ver seçeneğini öneririz

11/18/2016
### <a name="version-086"></a>Sürüm 0.8.6

#### <a name="new"></a>Yeni

* PIN, kolay gezinme için Hizmetleri hızlı erişim için en sık kullanılan artık
* Bu gibi durumlarda, birden çok düzenleyicileri şimdi farklı sekmelerde açabilirsiniz. Geçici bir sekmede açmak için tek tıklamayla; kalıcı bir sekmede açmak için çift tıklayın. Kalıcı bir sekme sağlamak için geçici sekmesinde de tıklayabilirsiniz
* Yaptık gözle görülür bir performans ve kararlılık geliştirmeleri yükler ve hızlı makinelerinde büyük dosyalar için özellikle indirir
* "Sanal" boş klasörler şimdi blob kapsayıcıları oluşturulabilir
* Artık arama yapmak için iki seçeneğiniz için sunduğumuz yeni Gelişmiş alt dize arama ile kapsamlı arama yeniden ekledik:
    * Genel arama - yalnızca arama metin kutusuna bir arama terimi girin
    * Kapsamlı arama - Büyüteç simgesinin yanında bir düğümü tıklatın ardından bir arama terimi yolun sonuna ekleyin veya sağ tıklayın ve "Arama gelen burada" seçin
* Çeşitli temaları ekledik: Açık (varsayılan), koyu, yüksek karşıtlık siyah ve yüksek karşıtlık beyaz. Düzenleme - Git&gt; Tema oluşturma tercihlerinizi değiştirmek için temalar
* Blob ve dosya özelliklerini değiştirebilirsiniz.
* Kodlanmış (base64) ve kodlanmamış kuyruk iletilerine artık desteklenmektedir
* Linux üzerinde bir 64-bit işletim sistemi sunulmuştur gerekli. Bu sürüm için yalnızca 64 bit Ubuntu 16.04.1 destekliyoruz LTS
* Bizim logosu güncelleştirdik!

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: Ekran sorunları dondurma
* Düzeltildi: Gelişmiş güvenlik
* Düzeltildi: Bazen yinelenen ekli hesapları görünebilir
* Düzeltildi: Bir blob tanımlanmamış bir içerik türüne sahip bir özel durum oluşturabilir
* Düzeltildi: Boş bir tablo üzerinde sorgu paneli açma mümkün değildi
* Düzeltildi: Arama hataları değişir
* Düzeltildi: 50'den 100'e tıklandığında "Daha fazla Yükle" yüklü kaynak sayısı artar
* Düzeltildi: Hesabınız, imzalanmış olup olmadığını ilk çalıştırılmasında artık tüm abonelikler için bu hesap varsayılan olarak seçiyoruz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Depolama Gezgini bu sürümde Ubuntu 14.04 üzerinde çalışmaz.
* Aynı kaynak için birden fazla sekme açmak için sürekli olarak aynı kaynak tıklamayın. Başka bir kaynaktaki ve geri dönün ve ardından özgün kaynakta, yeniden başka bir sekmede açmak için tıklayın
* Hızlı erişim, yalnızca abonelik bağlı öğeleri ile çalışır. Yerel veya anahtarı veya SAS belirteci bağlı kaynaklar bu sürümde desteklenmez.
* Hızlı erişim, bir hedef kaynak, sahip olduğunuz kaç kaynak bağlı olarak gitmek için birkaç saniye sürebilir
* BLOB'ları veya aynı anda yüklenen dosyalar 3'ten fazla gruba sahip hatalarına neden olabilir
* Bundan sonra yaklaşık 50.000 düğümlere - arama arama tanıtıcıları performans etkileniyor olabilir veya işlenmeyen özel durumuna neden olabilir

10/03/2016
### <a name="version-085"></a>Sürüm 0.8.5

#### <a name="new"></a>Yeni

* Artık depolama hesaplarına ve kaynaklarına iliştirmek için Portal tarafından oluşturulan SAS anahtarları kullanabilirsiniz

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: yarış durumu bazen arama sırasında düğümlerin Genişletilebilir olmayan bir duruma neden
* Düzeltildi: Depolama hesapları için hesap adı ve anahtarı ile bağlanırken "HTTP kullan" çalışmıyor
* Düzeltildi: SAS anahtarları (Portal oluşturulan özel olanları) "sonunda eğik çizgili" bir hata döndürür
* Düzeltildi: tablo alma sorunları
    * Bölüm anahtarını ve satır anahtarı bazen ters
    * "Null" Bölüm anahtarları okunamadı

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bundan sonra yaklaşık 50.000 düğümlere - arama arama tanıtıcıları performans etkileniyor olabilir
* Dosyaları genişletin çalışılırken bir hata gösterir şekilde azure Stack dosyaları, şu anda desteklemiyor

09/12/2016
### <a name="version-084"></a>Sürüm 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesapları, kapsayıcılar, kuyruklar, tablolar veya paylaşım için dosya paylaşımları için doğrudan bağlantılar oluşturmak ve kaynaklarınıza - Windows ve Mac OS kolay erişim desteği
* Blob kapsayıcıları, tablo, kuyruk, dosya paylaşımları veya arama kutusuna depolama hesaplarından arayın
* Tablo Sorgu Oluşturucu yan tümcelerinde artık gruplandırabilirsiniz
* Yeniden adlandırmak ve blob kapsayıcıları, dosya paylaşımları, tablolar, BLOB'lar, blob klasörleri, dosyaları ve dizinlerden SAS-eklenmiş hesapları ve kapsayıcılar içindeki kopyala/yapıştır
* Kapsayıcıları yeniden adlandırma ve kopyalama blob ve dosya paylaşımları artık özellikleri ve meta verileri koruyun

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: Boole veya ikili özellikleri içerdikleri tablo varlıkları düzenleyemezsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bundan sonra yaklaşık 50.000 düğümlere - arama arama tanıtıcıları performans etkileniyor olabilir

08/03/2016
### <a name="version-083"></a>Sürüm 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Yeni

* Kapsayıcılar, tablolar, dosya paylaşımları yeniden adlandır
* Sorgu Oluşturucu deneyimi İyileştirildi
* Kaydetme ve yükleme olanağı sorgular
* Doğrudan bağlantılar için depolama hesapları veya kapsayıcılar, kuyruklar, tablolar veya dosya paylaşımı ve kaynaklarınızı kolayca erişmek için paylaşımları (salt Windows - macOS desteği çok yakında!)
* CORS kurallarını yapılandırma ve yönetme olanağı

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: Microsoft Accounts 8-12 saatte bir yeniden kimlik doğrulaması gerektir

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bazen UI dondurulmuş görünebilir - penceresinin ekranı kaplamasını bu sorunu gidermeye yardımcı olur
* macOS yükleme yükseltilmiş izinler gerektirebilir.
* Abonelikleri Filtrele için kimlik bilgilerini yeniden girmeye gerek Hesap Ayarları panelini Göster
* Dosya paylaşımları, blob kapsayıcıları ve tabloları yeniden adlandırma meta verilerini veya dosya paylaşım kotası, genel erişim düzeyi veya erişim ilkeleri gibi kapsayıcı üzerindeki diğer özellikleri korumaz
* BLOB'ları (ayrı ayrı veya yeniden adlandırılmış blob kapsayıcının içinde) yeniden adlandırma, anlık görüntüler korumaz. Diğer tüm özellikleri ve meta verileri bloblar, dosyalar ve varlıklar için bir yeniden adlandırma sırasında korunur
* Kopyalama veya kaynakları yeniden adlandırma SAS-eklenmiş hesapları içinde çalışmıyor

07/07/2016
### <a name="version-082"></a>Sürüm 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesapları, abonelikleri tarafından gruplandırılır; Geliştirme depolama ve anahtarı veya SAS bağlı kaynaklara (yerel ve eklenmiş) düğümünde gösterilir
* "Azure hesap ayarları" panelinde hesaplarından Oturumu Kapat
* Etkinleştirin ve oturum açma yönetmek için proxy ayarlarını yapılandırma
* Oluşturma ve blob kiralama Kes
* Açık blob kapsayıcıları, kuyruklar, tablolar ve dosyalar tek bir tıklatmayla

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: .NET veya Java kitaplıkları ile eklenen kuyruk iletileri düzgün base64 kodlaması çözülür değil
* Düzeltildi: $metrics tablolarını Blob Depolama hesapları için gösterilmez
* Düzeltildi: tablolar düğümü için yerel (Geliştirme) depolama çalışmıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* macOS yükleme yükseltilmiş izinler gerektirebilir.

06/15/2016
### <a name="version-080"></a>Sürüm 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Yeni

* Dosya paylaşım desteği: görüntüleme, karşıya yükleme, indirme, dosyaları ve dizinleri, SAS URI'leriyle kopyalama (oluşturma ve bağlanma)
* Depolama için SAS URI'leri veya hesap anahtarları ile bağlanmak için kullanıcı deneyimi İyileştirildi
* Tablo sorgusu sonuçlarını dışarı aktarma
* Tablo sütunları yeniden sıralamayı ve özelleştirme
* $Logs blob kapsayıcıları ve $metrics tablolarını depolama hesapları için etkinleştirilmiş ölçümleri ile görüntüleme
* Geliştirilmiş dışarı aktarma ve alma davranışı, özellik değeri türü artık içerir

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: yükleme veya indirme büyük BLOB'ları eksik karşıya yükleme/yüklemeler neden olabilir
* Düzeltildi: düzenleme, ekleme veya bir sayısal dize değeri ("1") sahip bir varlık alma, çift dönüştürür
* Düzeltildi: Yerel geliştirme ortamında tablo düğüm genişletilemiyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* $metrics tablolarını, Blob Depolama hesapları için görünür değildir.
* İletileri Base64 kodlama kullanılarak kodlanır, program aracılığıyla eklenen kuyruk iletileri düzgün görüntülenmeyebilir

05/17/2016
### <a name="version-07201605090"></a>Sürüm 0.7.20160509.0

#### <a name="new"></a>Yeni

* Daha iyi hata işleme için uygulama kilitlenmeleri

#### <a name="fixes"></a>Düzeltmeleri

* Oturum açma kimlik bilgileri gerekli olduğunda burada bilgi çubuğu iletileri bazen gösterilmiyorsa hata düzeltildi

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tablolar: Ekleme, düzenleme veya "1" veya "1.0" gibi muğlak sayısal bir değer ve kullanıcı bir özelliği olan bir varlık olarak gönder dener alma bir `Edm.String`, değeri olarak bir Edm.Double API istemcisi üzerinden geri gelir

03/31/2016

### <a name="version-07201603250"></a>Sürüm 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Yeni

* Tablo desteği: görüntüleme, sorgulama, dışarı aktarma, alma ve varlıklar için CRUD işlemleri
* Kuyruk desteği: görüntüleme, ekleme, dequeueing iletileri
* Depolama hesapları için SAS URI'leri oluşturuluyor
* SAS URI'leriyle birlikte depolama hesaplarınızı bağlama
* Depolama Gezgini'ne güncelleştirme gelecekteki güncelleştirmeleri için bildirimleri
* Güncelleştirilmiş Görünüm

#### <a name="fixes"></a>Düzeltmeleri

* Performans ve güvenilirlik geliştirmeleri

### <a name="known-issues-amp-mitigations"></a>Bilinen sorunlar &amp; risk azaltma işlemleri

* Biz bu soruna sırada AzCopy kullanılmasını öneririz - büyük blob dosyalarını indirilmesini düzgün çalışmıyor
* Hesap kimlik bilgileri değil alınan ya da giriş klasörü bulunamıyor ya da yazılamaz önbelleğe alınmış
* Biz ekleme, düzenleme veya "1" veya "1.0" gibi bir muğlak sayısal değerine sahip bir özelliği olan bir varlık alma ve kullanıcı olarak göndermeye çalışırsa bir `Edm.String`, değeri olarak bir Edm.Double API istemcisi üzerinden geri gelir
* Çok satırlı kayıtları içeren CSV dosyalarını içeri aktarırken, verileri kesilmiş Tasarımlı karıştırılmış veya

02/03/2016

### <a name="version-07201601291"></a>Sürüm 0.7.20160129.1

#### <a name="fixes"></a>Düzeltmeleri

* Karşıya yükleme, indirme ve blobları kopyalama genel performansı geliştirildi

01/14/2016

### <a name="version-07201601050"></a>Sürüm 0.7.20160105.0

#### <a name="new"></a>Yeni

* Linux desteği (OSX eşlik özellikleri)
* BLOB kapsayıcıları ile paylaşılan erişim imzaları (SAS) anahtarı ekleme
* Azure Çin 21Vianet için depolama hesapları ekleme
* Özel uç noktaları ile depolama hesapları ekleme
* Açın ve içeriğini metin ve resim blobları görüntüleyin
* Blob özelliklerini ve meta verileri görüntüleyin ve düzenleyin

#### <a name="fixes"></a>Düzeltmeleri

* Düzeltildi: karşıya yükleme veya indirme çok sayıda BLOB (500 +) bazen beyaz bir ekran uygulamanın neden
* Düzeltildi: kapsayıcı odaklanmak yeniden ayarlanıncaya kadar blob kapsayıcısı genel erişim düzeyi ayarlarken, yeni değer güncelleştirilmez. Ayrıca, iletişim her zaman "Genel erişim" ve değil gerçek geçerli değeri varsayılandır.
* Daha iyi genel klavye/erişilebilirlik ve kullanıcı Arabirimi desteği
* Boşluk ile uzun olduğunda, içerik haritaları geçmişi metni kaydırır
* Giriş doğrulaması SAS iletişim kutusu destekler
* Yerel depolama kullanıcı kimlik bilgilerinin süresi dolmuş olsa bile kullanılabilir olmaya devam eder
* Blob Gezgini sağ tarafta açılan blob kapsayıcı silindiğinde, kapalı

#### <a name="known-issues"></a>Bilinen Sorunlar

* Linux yüklemesi gereken gcc sürümü güncelleştirildi veya yükseltilmiş – yükseltme adımları aşağıda verilmiştir:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Sürüm 0.7.20151116.0

#### <a name="new"></a>Yeni

* macOS ve Windows sürümleri
* Oturum açma, depolama hesaplarınıza görünümü: Kurumsal hesap, Microsoft, 2fa'yı, Account kullanın.
* Yerel geliştirme deposu (depolama öykünücüsünü kullanma yalnızca Windows)
* Azure Resource Manager ve klasik kaynak desteği
* Oluşturma ve bloblar, kuyruklar veya tablolar silme
* Belirli bloblar, kuyruklar veya tablolar arayın
* Blob kapsayıcıları içerikleri keşfedin
* Dizinlerinden gitmek ve görüntülemek
* Yükleme, indirme ve BLOB'ları ve klasörleri Sil
* Blob özelliklerini ve meta verileri görüntüleyin ve düzenleyin
* SAS anahtarları oluşturun
* Saklı erişim ilkeleri (SAP) oluşturma ve yönetme
* Ön eke göre BLOB Ara
* Dosyaları karşıya yükleme veya indirme için sürükleyip bırakabilir 'n sürükleyin

#### <a name="known-issues"></a>Bilinen Sorunlar

* Odağı kapsayıcı üzerindeki yeniden ayarlanıncaya kadar blob kapsayıcısı genel erişim düzeyi ayarlarken, yeni değer güncelleştirilmez
* Genel erişim düzeyi ayarlamak için iletişim kutusunu açtığınızda, bu her zaman "Genel erişim" varsayılan ve değil gerçek geçerli değer gösterilir
* İndirilen BLOB'ları yeniden adlandırılamıyor
* Etkinlik günlüğü girdilerini bazen "devam eden takılıyor" ne zaman bir hata oluşur ve bir hata görüntülenmez belirtin.
* Bazen kilitleniyor ya da karşıya yükleme veya çok sayıda BLOB indirme çalışırken tamamen beyaz kapatır
* Bazen bir kopyalama işlemi iptal ediliyor çalışmıyor
* Geçersiz bir ad girin ve başka farklı kapsayıcı türü altında oluşturmaya devam (kuyruk/blob/tablo) bir kapsayıcı oluşturma sırasında odak yeni türüne ayarlanamaz
* Yeni klasör oluşturamaz veya klasörü yeniden adlandır




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
