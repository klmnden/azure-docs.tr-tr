---
title: StorSimple 8000 serisi güncelleştirme 2.2 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 2.2 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 12d11cddf077d4d07732490255d44e89ddaf3217
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527124"
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 serisi güncelleştirme 2.2 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 2.2 belirleyin. Ayrıca bu sürüme dahil edilen StorSimple yazılım güncelleştirmeleri listesini içerirler.

Güncelleştirme 2.2 sürüm (GA) veya güncelleştirme 0.1, güncelleştirme 2. 1 ' çalıştıran tüm StorSimple cihazı için uygulanabilir. Güncelleştirme 2.2 ile ilişkili cihaz 6.3.9600.17708 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 2.2 yalnızca yazılım güncelleştirmeleri vardır. Yaklaşık 1,5 2 Bu güncelleştirmeyi yüklemek için saat sürer. 
> * Güncelleştirme 2.1 çalıştırıyorsanız, güncelleştirme 2.2 olabildiğince çabuk uygulamanızı öneririz.
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Birkaç gün bekleyin ve ardından güncelleştirmeleri bu olarak yeniden tarama yakında kullanıma sunulacaktır.
> 
> 

## <a name="whats-new-in-update-22"></a>Güncelleştirme 2.2 içinde yenilikler nelerdir?
Güncelleştirme 2.2 içinde aşağıdaki önemli geliştirmeler yapıldı.

* **Alan geri kazanma iyileştirme otomatik** – veri ölçülü kaynak kullanan birimler üzerinde kazanılacak kullanılmamış depolama blokları gereken silindiğinde. Bu sürüm, önceki sürümlere göre daha hızlı kullanılabilir hale gelmeden kullanılmayan alanı výsledek buluttan alan geri kazanma işlemi geliştirilmiştir.
* **Performans iyileştirmeleri anlık görüntü** – güncelleştirme 2.2 belirli senaryolarda büyük olduğunda birimlerin kullanıldığı ve olduğundan hiçbir veri değişim sıklığı için en az bir anlık görüntü Bulutu işlem süresi geliştirildi. Bu geliştirme avantaj elde edecektir bir senaryo, arşiv birimleri olacaktır.
* **Destek Paketi toplama sağlamlaştırma** – destek paketi toplanır ve bu sürümde karşıya şekilde geliştirmeleri yapıldı. 
* **Güvenilirlik geliştirmeleri güncelleştirme** – bu sürüm hata düzeltmeleri, geliştirilmiş bir güncelleştirme güvenilirliği neden sahiptir.

## <a name="issues-fixed-in-update-22"></a>Güncelleştirme 2.2 içinde düzeltilen sorunlar
Aşağıdaki tablolarda, güncelleştirmeleri 2.2 ve 2.1 düzeltilen sorunlara ilişkin bir Özet sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- |
| 1 |Konak performansı |Önceki sürümde, konak tarafı performans sorunlarını, yerel olarak sabitlenmiş birimin oluşturulması sırasında ve yerel olarak sabitlenmiş bir birim katmanlı birim dönüştürme sırasında gözlemlenmedi. Bu sorunları, böylece birim oluşturma ve dönüştürme yordamları sırasında konak performansında bir gelişme sonuçta bu sürümde giderilen. |Evet |Hayır |
| 2 |Yerel olarak sabitlenmiş birimler |Nadir durumlarda, sistem, yerel olarak sabitlenmiş bir birim oluştururken kilitlenme. Bu sürümde bu hata düzeltildi. |Evet |Hayır |
| 3 |Katman ayarlama |StorSimple bulut Gereçleri (8010 ve 8020) için meta verileri buluta katmanlanmış, ara sıra kilitlenmeleri vardı. Bu sürümde bu sorun düzeltilmiştir. |Hayır |Evet |
| 4 |Anlık görüntü oluşturma |Artımlı anlık senaryolarında büyük birimleri ile oluşturulmasını ilgili ve hiçbir veri değişim sıklığı için en az sorunlar oluştu. Bu sürümde bu sorunlar düzeltildikten. |Evet |Evet |
| 5 |Openstack kimlik doğrulaması |Openstack bulut hizmeti sağlayıcısı olarak kullanırken, kullanıcının burada JSON ayrıştırıcısı içindeki bir kilitlenme sonuçlanan kimlik doğrulaması ile ilgili bir seyrek hata halinde çalışır. Bu hata, bu sürümde sabittir. |Evet |Hayır |
| 6 |Konak tarafı kopyalama |Yazılım daha önceki sürümlerinde, ODX zamanlamasıyla ilgili bir seyrek hata verileri başka bir birime bir birimden kopyalarken görüldü. Bu denetleyici yük devretme neden olur ve sistemin potansiyel olarak kurtarma moduna gidebilirsiniz. Bu hata, bu sürümde sabittir. |Evet |Hayır |
| 7 |Windows Yönetim Araçları (WMI) |Yazılım önceki sürümlerinde, özel durum ile web proxy hatası çeşitli örneklerini vardı "\<ManagementException > Sağlayıcı yükleme hatası". Bu hata için bir WMI bellek sızıntısıdır öznitelikli ve artık düzeltildi. |Evet |Hayır |
| 8 |Güncelleştirme |Önceki yazılım sürümlerindeki bazı nadir durumlarda tarama veya güncelleştirmeleri yüklemeye çalışırken "CisPowershellHcsscripterror" kullanıcı aldı. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |
| 9 |Destek paketi |Bu sürümde, destek paketi toplanan ve karşıya olduğu gibi iyileştirmeler yapıldı. |Evet |Evet |

## <a name="known-issues-in-update-22"></a>Güncelleştirme 2.2'de bilinen sorunlar
Aşağıdaki tabloda, bu sürümdeki bilinen sorunlara ilişkin bir Özet sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 bir cihaz EBOD muhafazası diskler çoğu hiçbir disk çekirdeği kaynaklanan kesilirse nadir durumlarda, ardından depolama havuzunun çevrimdışı geçer. Diskleri yeniden bağlantı kuruldu bile çevrimdışı kalır. |Cihaz yeniden başlatma gerekir. Lütfen sorun devam ederse, Microsoft Support için sonraki adımlara başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değişiklik yapıldığında, denetleyici 0 denetleyici 1 görünebilir. Eş düğümünden, görüntüyü yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilirsiniz Nadir durumlarda, bu davranışı sistem yeniden başlatma sonrası görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendi çözülecektir. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için depolama hizmeti kullanarak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerini geri alınamaz bir durum için yol açacaktır. | |Evet |Evet |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme işlemleri desteklenmiyor. Birden fazla cihaza tek bir ölü CİHAZDAN yük devretme veri sahipliği kaybetmek birim kapsayıcıları cihaz başarısız ilk neden olur. Böyle bir yük devrinden sonra bu birim kapsayıcıları görünür veya Azure Klasik Portalı'nda görüntülediğinizde farklı davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple bağdaştırıcısını sırasında yükleme başarıyla tamamlanması için sırayla bir cihazın IP sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy'si |Belirtilen Protokolü HTTPS, web proxy yapılandırması varsa, cihazı hizmeti iletişiminizin etkilenecek ve cihaz çevrimdışı olarak geçer. Destek paketleri, Cihazınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy'si |Yapılandırırsanız ve bir kayıtlı cihazda web Ara sunucusunu etkinleştirme, Cihazınızda etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
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

## <a name="controller-and-firmware-updates-in-update-22"></a>Güncelleştirme 2.2 denetleyici ve üretici yazılımı güncelleştirmeleri
Bu sürüm yalnızca yazılım güncelleştirmeleri sahiptir. Ancak, güncelleştirme 2'den önceki bir sürümü güncelleştiriyorsanız, Storport, Spaceport, sürücü yüklemek gerekecektir (bazı durumlarda) ve disk üretici yazılımı güncelleştirmelerini Cihazınızda.

Sürücü, Storport, Spaceport ve disk üretici yazılımı güncelleştirmelerini yükleme hakkında daha fazla bilgi için bkz. [güncelleştirme 2.2 yükleme](storsimple-install-update-21.md) StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-22"></a>Güncelleştirme 2.2 sanal cihaz güncelleştirmeleri
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazları oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi edinmek için nasıl [güncelleştirme 2.2 yükleyin](storsimple-install-update-21.md) StorSimple Cihazınızda.

