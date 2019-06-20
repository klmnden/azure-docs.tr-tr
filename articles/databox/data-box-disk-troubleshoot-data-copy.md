---
title: Azure Data Box Disk sorun giderme veri kopyalama sorunlarını | Microsoft Docs
description: Veri günlüklerini kullanarak Azure Data Box Disk kopyalama sırasında görülen sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148354"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Azure Data Box Disk olarak veri kopyalama sorunlarını giderme

Bu makale, Microsoft Azure Data Box Disk için geçerlidir ve veri diskleri kopyalarken gördüğünüz herhangi bir sorunu gidermeye açıklar. Makalede bölünmüş kopyalama aracı kullanırken sorunları da ele alınmaktadır.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Bir Linux sistemine kullanırken veri kopyalama sorunları

Bu bölümde, diske veri kopyalamak için bir Linux istemcisi kullanılırken karşılaşılan en önemli sorunlardan bazıları açıklanmaktadır.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Sorun: Sürücü salt okunur olarak bağlanmış
 
**Bunun nedeni** 

Bu, bir şekilde çoğaltamaması dosya sistemi nedeniyle olabilir.

Bir sürücü okuma-yazma olarak kaldırmadan veri kutusu disk ile çalışmaz. Bu senaryo dislocker tarafından şifresi sürücülerle desteklenir. Aşağıdaki komutu kullanarak cihaz başarıyla yeniden:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Başarılı kaldırmadan rağmen verilerin kalıcı olmaz.

**Çözümleme**

Linux sisteminizde aşağıdaki adımları uygulayın:

1. Yükleme `ntfsprogs` ntfsfix yardımcı program için paket.
2. Sürücünün kilidini araç tarafından sağlanan bağlama noktaları çıkarın. Bağlama noktaları sayısı sürücüleri için farklılık gösterir.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. Çalıştırma `ntfsfix` karşılık gelen yolda. Vurgulanan sayıda adım 2 ' aynı olmalıdır.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Bağlama soruna neden olabilecek hazırda meta verilerini kaldırmak için aşağıdaki komutu çalıştırın.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Temiz bir çıkarma işlemi yapın.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Temiz bir kilit açma yapın ve bağlama.
7. Bağlama noktası, bir dosya yazarak test edin.
8. Çıkarın ve dosya kalıcılığına doğrulamak için yeniden bağlayın.
9. Veri kopyalama işlemiyle devam edin.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Sorun: Kopyalamadan sonra kalıcı değil verilerle hata
 
**Bunun nedeni** 

Sonra drive'ınızdaki verileri yok görürseniz (veri için kopyalandığı rağmen) sürücü salt okunur olarak oluşturulmasından sonra bir sürücü okuma-yazma olarak geri takılmazsa mümkündür, çıkarılamadı.

**Çözümleme**
 
Bu durumda, bakmalarını için [salt okunur olarak bağlanmış sürücüleri](#issue-drive-getting-mounted-as-read-only).

Bir durum değilse, günlükleri veri kutusu Disk kilidini aracın klasörüne kopyalayın ve [Microsoft Support başvurun](data-box-disk-contact-microsoft-support.md).


## <a name="data-box-disk-split-copy-tool-errors"></a>Data Box Disk Split Copy aracı hataları

Birden çok disk üzerinde verileri bölme bir bölünmüş kopyalama aracı kullanırken görünen sorunları aşağıdaki tabloda özetlenmiştir.

|Hata iletisi/Uyarılar |Öneriler |
|---------|---------|
|[Bilgisi] Birimi için BitLocker parolasını alınıyor: m <br>[Hata] BitLocker anahtarı için birim m: alınırken özel durum yakalandı<br> Sıra hiçbir öğe içermiyor.|Bu hata hedef Data Box Disk çevrimdışı olduğunda gösterilir. <br> Diskleri çevrimiçi duruma getirmek için `diskmgmt.msc` aracını kullanın.|
|[Hata] Özel durum: WMI işlemi başarısız oldu:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=Girilen kurtarma parolası biçimi geçersiz. <br>BitLocker kurtarma parolaları 48 hanelidir. <br>Kurtarma parolasının doğru biçimde olduğunu doğrulayıp yeniden deneyin.|Data Box Disk kilit açma aracını kullanarak disklerin kilidini açın ve komutu yeniden deneyin. Daha fazla bilgi için bkz. <li> [Windows istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Linux istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Hata] Özel durum: Hedef sürücüde DriveManifest.xml dosya var. <br> Bu durum hedef sürücünün farklı bir günlük dosyasıyla hazırlanmış olabileceğini gösterir. <br>Aynı sürücüye daha fazla veri eklemek için önceki günlük dosyasını kullanın. Mevcut verileri silmek ve hedef sürücü için yeni bir içeri aktarma işi yeniden için silin *DriveManifest.xml* sürücüsünde. Bu komutu yeni bir günlük dosyasıyla yeniden çalıştırın.| Bu hata aynı sürücü kümesini birden fazla içeri aktarma oturumunda kullanmaya çalıştığınızda ortaya çıkar. <br> Bir sürücü kümesini yalnızca bir bölme ve kopyalama oturumu için kullanın.|
|[Hata] Özel durum: CopySessionId ImportData-Eylül-test-1, bir önceki kopyalama oturumu ifade eder ve yeni bir kopya oturumu için kullanılamayacak.|Bu hata yeni bir işe önceden başarıyla tamamlanan bir işin adının verilmeye çalışılması durumunda bildirilir.<br> Yeni işiniz için benzersiz bir ad atayın.|
|[Bilgi] Hedef dosya veya dizin adı NTFS uzunluk sınırını aşıyor. |Bu ileti hedef dosya uzun dosya yolu nedeniyle yeniden adlandırıldığında bildirilir.<br> Bu davranışı denetlemek için `config.json` dosyasındaki değerlendirme seçeneğini değiştirin.|
|[Hata] Özel durum: Hatalı JSON kaçış dizisi. |Bu ileti config.json dosyasında geçersiz biçim olduğunda bildirilir. <br> Dosyayı kaydetmeden önce [JSONlint](https://jsonlint.com/) kullanarak `config.json` için doğrulama gerçekleştirin.|


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [doğrulama aracı sorunlarını giderme](data-box-disk-troubleshoot.md).
