---
title: StorSimple 8000 serisi güncelleştirme 3 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 3 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d18feba4ded3dfccb8f774112a7dc8d42b12f1d5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60530949"
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>Güncelleştirme 3 sürüm notları için StorSimple 8000 serisi Cihazınızı

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 3 için kritik açık sorunları tanımlayın. Ayrıca bu sürüme dahil edilen StorSimple yazılım güncelleştirmeleri listesini içerirler. 

Güncelleştirme 3 sürüm (GA) ya da güncelleştirme 0.1 güncelleştirme 2.2 çalıştıran tüm StorSimple cihazı için uygulanabilir. Güncelleştirme 3 ile ilişkili cihaz 6.3.9600.17759 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 3 cihaz yazılımı, LSI sürücü ve bellenim Storport vardır ve Spaceport güncelleştirir. Yaklaşık 1,5 2 Bu güncelleştirmeyi yüklemek için saat sürer. 
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Birkaç gün bekleyin ve ardından güncelleştirmeleri bu olarak yeniden tarama yakında kullanıma sunulacaktır.
> 
> 

## <a name="whats-new-in-update-3"></a>Güncelleştirme 3'teki yenilikler
Güncelleştirme 3'te aşağıdaki anahtar geliştirmeleri ve hata düzeltmeleri yapıldı.

* **Alan geri kazanma değişiklikleri otomatik** – başlangıç güncelleştirme 3, alan geri kazanma algoritmaları daha hızlı yürütülmesine neden sistemin hazır bekleyen denetleyicinin çalıştıracağınız. Alan geri kazanma ile çalışmak için gereken bağlantı noktaları hakkında daha fazla bilgi için başvurmak [gereksinimlerinde StorSimple](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Performans iyileştirmeleri** – güncelleştirme 3, buluta okuma / yazma performans geliştirildi.
* **Geçiş ile ilgili geliştirmeler** – bu sürümde, çeşitli hata düzeltmeleri ve geliştirmeleri bitti geçiş özellik için 5000/7000 Serisi cihazlar için 8000 serisi cihazlar. Geçiş özelliğini kullanma hakkında daha fazla bilgi için Git [8000 serisi cihaz 5000/7000 Serisi CİHAZDAN geçiş](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **İlgili düzeltmeleri izleme** - hataları bu sürümde, ilgili izleme grafikleri, hizmet Panosu, için ve cihaz Panosu sorunlar giderildi.

## <a name="issues-fixed-in-update-3"></a>Güncelleştirme 3'te giderilen sorunlar
Aşağıdaki tablolarda, güncelleştirme 3'te düzeltilen sorunlara ilişkin bir Özet sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- |
| 1 |Konak tarafı veri geçişi |Önceki sürümde, StorSimple Cloud Appliance konak tarafı veri geçişi sırasında çevrimdışı yüklemesiyle. Bu sürümde bu sorun düzeltilmiştir. |Hayır |Evet |
| 2 |Yerel olarak sabitlenmiş birimler |Önceki sürümde, g/ç hataları, birim dönüştürme hataları ve datapath hataları yerel olarak sabitlenmiş birimler için ilgili sorunlar oluştu. Bu sorunların kök-neden ve bu sürümde giderilen. |Evet |Hayır |
| 3 |İzleme |Birimleri raporlama ve cihaz Pano grafikleri yanlış bilgileri yerel olarak sabitlenmiş birimler için burada görüntülenen yanı sıra izleme ilgili birden çok sorunlar oluştu. Bu sürümde bu sorunlar düzeltildikten. |Evet |Hayır |
| 4 |Ağır yazma g/ç |StorSimple ağır yazma işlemleri içeren iş yükleri için kullanılırken, burada çalışma kümesi buluta katmanlanmış seyrek bir hata ile çalışır. Bu hata, bu sürümde sabittir. |Evet |Evet |
| 5 |Backup |Yazılım, önceki sürümlerinde nadir bazı durumlarda, kullanıcı uzak bir kopya yedeği sürdü, bunlar bulut hatalarla karşılaşırsanız çalıştırılır ve işlemi hataya neden olur. Bu sürümde, sorun düzeltildi ve işlem başarıyla tamamlar. |Evet |Evet |
| 6 |Yedekleme ilkesi |Bazı ender durumlarda yazılımının daha önceki sürümlerde vardır yedekleme ilkesini silme işlemi için ilgili bir hata oluştu. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |

## <a name="known-issues-in-update-3"></a>Güncelleştirme 3'te bilinen sorunlar
Aşağıdaki tabloda, bu sürümdeki bilinen sorunlara ilişkin bir Özet sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 bir cihaz EBOD muhafazası diskler çoğu hiçbir disk çekirdeği kaynaklanan kesilirse nadir durumlarda, ardından depolama havuzunun çevrimdışı geçer. Diskleri yeniden bağlantı kuruldu bile çevrimdışı kalır. |Cihaz yeniden başlatma gerekir. Lütfen sorun devam ederse, Microsoft Support için sonraki adımlara başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değişiklik yapıldığında, denetleyici 0 denetleyici 1 görünebilir. Eş düğümünden, görüntüyü yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilirsiniz Nadir durumlarda, bu davranışı sistem yeniden başlatma sonrası görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendi çözülecektir. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için depolama hizmeti kullanarak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerini geri alınamaz bir durum için yol açacaktır. | |Evet |Evet |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme işlemleri desteklenmiyor. Birden fazla cihaza tek bir ölü CİHAZDAN yük devretme veri sahipliği kaybetmek birim kapsayıcıları cihaz başarısız ilk neden olur. Böyle bir yük devrinden sonra bu birim kapsayıcıları görünür veya Azure Klasik Portalı'nda görüntülediğinizde farklı davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple bağdaştırıcısını sırasında yükleme başarıyla tamamlanması için sırayla bir cihazın IP sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web ara sunucusu |Belirtilen Protokolü HTTPS, web proxy yapılandırması varsa, cihazı hizmeti iletişiminizin etkilenecek ve cihaz çevrimdışı olarak geçer. Destek paketleri, Cihazınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-8000-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web ara sunucusu |Yapılandırırsanız ve bir kayıtlı cihazda web Ara sunucusunu etkinleştirme, Cihazınızda etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Bulut yüksek gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye cinsinden sırası) ve yüksek g/ç iş yükü karşılaştığında, cihaz birimlerine düzeyi düşürülmüş bir duruma geçmesine ve g/ç bir "cihaz hazır değil" hatası ile başarısız olabilir. |El ile cihaz denetleyicileri yeniden veya bu durumdan kurtulmak için cihaz yük devretme gerçekleştirmeniz gerekecektir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmeniz ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet, parantez içine şu şekilde kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - ilk 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA en son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk 4 yedeklemeler ilk birim kapsayıcısı, geçirildikten sonra paralel geçiş başlar. |Bir birim kapsayıcısı teker teker geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklemeden sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme ilkesi için bu birimler eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |5000/7000 Serisi cihaz, geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları erişmemelidir. |Geçiş tamamlandı ve kaydedilmiş duruma geldikten sonra geçirilen verileri kapsayıcıları silmenizi öneririz. |Evet |Hayır |
| 13 |Kopyalama ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazına kopyalayın veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef cihaz güncelleştirme 1 için güncelleştirme işlemlerini izin vermek için gerekecektir |Evet |Evet |
| 14 |Geçiş |Hiçbir ilişkili birimleri ile birim gruplarını olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi cihazda başarısız olabilir. |Hiçbir ilişkili birimleri ile tüm boş birim gruplarını silin ve ardından yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |
| 15 |Azure PowerShell cmdlet'leri ve yerel olarak sabitlenmiş birimler |Azure PowerShell cmdlet'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlanmış olmaz.) |Her zaman yerel olarak sabitlenmiş birimler yapılandırmak için StorSimple Yöneticisi hizmetini kullanın. |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimler için kullanılabilir alan |Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimler için mevcut olan alanı hemen güncelleştirilmeyebilir. StorSimple Yöneticisi hizmeti, kullanılabilir yerel alanın yaklaşık saatte güncelleştirir. |Yeni birim oluşturmak denemeden önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimler |Yedekleme kataloğunu, ancak yalnızca geri yükleme işi süresi için geçici bir anlık görüntü yedekleme, geri yükleme işi gösterir. Ayrıca, ön ekine sahip bir sanal disk grubu kullanıma sunduğu **tmpCollection** üzerinde **yedekleme ilkeleri** sayfasında, ancak yalnızca geri yükleme işi süresi için. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimler |Geri yükleme işi iptal edin ve daha sonra geri yükleme işi gösterecektir hemen bir denetleyici yük devretmesi oluşursa **başarısız** yerine **iptal**. Geri yükleme işi başarısız olursa ve daha sonra geri yükleme işi gösterecektir hemen bir denetleyici yük devretme gerçekleştikten **iptal** yerine **başarısız**. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimler |Geri yükleme işi iptal etmek veya geri yükleme başarısız olursa ve ardından bir denetleyici yük devretme gerçekleştikten bir ek geri yükleme işi görünür **işleri** sayfası. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimler |Yerel olarak sabitlenmiş bir birim için (oluşturulan ve güncelleştirme 1.2 ile kopyalanan veya öncesi) katmanlı Birim dönüştürmeyi denemeden ve alana sahip Cihazınızı değil veya bir bulut kesinti varsa, clone(s) bozulabilir. |Oluşturulan ve klonlanan ile ön güncelleştirme 2.1 yazılım olan birimleri ile bu sorun oluşur. Bu, nadir bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Bir birim dönüştürme işlemi devam ederken bir birime bağlı Acr'leri güncelleştirmez (yerel olarak sabitlenmiş katmanlı ya da tam tersi). Acr'leri güncelleştiriliyor, veri bozulmasına neden olabilir. |Gerekirse, önce birim dönüştürme Acr'leri güncelleştirin ve dönüştürme işlemi devam ederken ACR güncelleştirmeleri başka yapmayın. | | |
| 22 |Güncelleştirmeler |Güncelleştirme 3'ü uygularken **Bakım** sayfasında Klasik Azure portalında, güncelleştirme 2'ye ilgili aşağıdaki ileti görüntülenir: "StorSimple 8000 serisi güncelleştirme 2 özelliğini içerir Microsoft için proaktif olarak toplama günlüğüne bilgi biz olası sorunları algıladığınızda cihazınızdan". Cihaz güncelleştirme 2 için güncelleştirilmiş olduğunu belirttiği yanıltıcıdır. Bu ileti, cihaz güncelleştirme 3'e başarıyla güncelleştirildikten sonra kaybolur. |Bu davranış, gelecekteki bir sürümde düzeltilecektir. |Evet |Hayır |

## <a name="controller-and-firmware-updates-in-update-3"></a>Güncelleştirme 3'te denetleyici ve üretici yazılımı güncelleştirmeleri
Bu yayın LSI sahip olan sürücü ve bellenim güncelleştirmeleri. LSI sürücü ve bellenim güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz. [güncelleştirme 3'ü yükleme](storsimple-install-update-3.md) StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-3"></a>Güncelleştirme 3'te sanal cihaz güncelleştirmeleri
Bu güncelleştirme, StorSimple Cloud Appliance (sanal cihaz olarak da bilinir) uygulanamaz. Yeni sanal cihazları oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi nasıl [güncelleştirme 3'ü yükleme](storsimple-install-update-3.md) StorSimple Cihazınızda.

