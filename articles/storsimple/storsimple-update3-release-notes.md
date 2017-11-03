---
title: "StorSimple 8000 serisi güncelleştirme 3 sürüm notları | Microsoft Docs"
description: "Yeni özellikler, sorunlar ve geçici çözümler için StorSimple 8000 serisi güncelleştirme 3 açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/25/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8b230904e1e079417c3b39bbc281bc3a87668a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>3 sürüm notları, StorSimple 8000 serisi cihazınız için güncelleştirme

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 3 tanımlayın. Ayrıca bu sürümde dahil StorSimple yazılım güncelleştirmelerinin bir listesini içerir. 

Güncelleştirme 3 sürüm (GA) veya güncelleştirme 0.1 güncelleştirme 2.2 çalışan herhangi bir StorSimple aygıtı uygulanabilir. Güncelleştirme 3 ile ilişkili aygıt 6.3.9600.17759 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 3 aygıt yazılımı, LSI sürücü ve bellenim ve Storport sahiptir ve Spaceport güncelleştirir. Yaklaşık 1,5-2 Bu güncelleştirmeyi yüklemek için saat sürer. 
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Birkaç gün bekleyin ve ardından güncelleştirmeleri taramak üzere yeniden bunlar olarak yakında kullanılabilir hale gelecektir.
> 
> 

## <a name="whats-new-in-update-3"></a>Güncelleştirme 3'te yenilikler nelerdir?
Güncelleştirme 3'te, aşağıdaki anahtar geliştirmeler ve hata düzeltmeleri yapıldı.

* **Alan geri kazanma değişiklikleri otomatik** – daha hızlı yürütme sonuçlanan Sistem bekleme denetleyicisini güncelleştirme 3, başlangıç alan geri kazanma algoritmaları çalıştırmak. Alan geri kazanma ile çalışmak için gerekli bağlantı noktaları hakkında daha fazla bilgi için bkz [ağ gereksinimleri StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Performans iyileştirmeleri** – güncelleştirme 3'ü okuma-yazma performansı bulut için iyileştirilmiştir.
* **Geçiş ile ilgili geliştirmeler** – bu sürümde, çeşitli hata düzeltmeleri ve geliştirmeleri bitti için geçiş özelliğini 5000/7000 Serisi cihazlar 8000 serisi cihazlar için. Geçiş özelliğini kullanma hakkında daha fazla bilgi için Git [8000 serisi aygıt 5000/7000 Serisi cihaz geçiş](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **İlgili düzeltmeleri izleme** - bu sürümde, hataları ile ilgili izleme grafikleri, hizmet panosunu ve cihaz Pano giderildi.

## <a name="issues-fixed-in-update-3"></a>Güncelleştirme 3'te giderilen sorunlar
Aşağıdaki tablolarda, güncelleştirme 3'te düzeltilen sorunlardan özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Konak tarafındaki veri geçişi |Önceki sürümde, StorSimple bulut uygulaması bir ana bilgisayar tarafı veri geçişi sırasında çevrimdışı duruma geçiyor. Bu sürümde bu sorun düzeltilmiştir. |Hayır |Evet |
| 2 |Yerel olarak sabitlenmiş birimleri |Önceki sürümde, g/ç hataları, toplu dönüştürme hataları ve yerel olarak sabitlenmiş birimlerin datapath hataları ile ilgili sorunlar vardı. Bu sorunları kök neden oldu ve bu sürümde sabit. |Evet |Hayır |
| 3 |İzleme |Birimleri raporlama ve cihaz Pano grafikleri yanlış bilgileri yerel olarak sabitlenmiş birimler için burada görüntülenen yanı sıra izleme ilgili birden çok sorun oluştu. Bu sürümde bu sorunlar giderildi. |Evet |Hayır |
| 4 |Ağır yazma g/ç |StorSimple ağır yazma işlemleri içeren iş yükleri için kullanırken, kullanıcının sık bir hata burada çalışma kümesi bulutunu katmanlı çalışır. Bu hata, bu sürümde sabittir. |Evet |Evet |
| 5 |Backup |Yazılım, önceki sürümlerinde nadir bazı durumlarda, kullanıcı uzak bir kopya yedeği sürdü, bulut hatalarla karşılaşırsanız çalışır ve kullanıma alma hatası işlemi gerekir. Bu sürümde, sorun giderilmiştir ve işlemi başarıyla tamamlandığında. |Evet |Evet |
| 6 |Yedekleme ilkesi |Belirli nadir durumlarda, yazılım, önceki sürümlerde oluştu yedekleme ilkesi silme işlemini ilgili bir hata. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |

## <a name="known-issues-in-update-3"></a>Güncelleştirme 3'te bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı geçer. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında aygıt IP başarıyla tamamlamak bir yükleme için sırayla sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy |Web proxy yapılandırması belirtilen protokol olarak HTTPS varsa, cihazı hizmeti iletişimi etkilenecek ve cihaz çevrimdışı. Destek paketleri aygıtınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP sahip olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy |Yapılandırma ve web proxy bir kayıtlı cihazda etkinleştirirseniz, Cihazınızı etkin denetleyicisinde yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye sırasını) ve yüksek g/ç iş yükü bileşimini karşılaştığında, düzeyi düşürülmüş bir duruma aygıt birimleri gidin ve g/ç "cihaz hazır değil" hatası ile başarısız. |El ile aygıt denetleyicileri yeniden başlatın veya bu durumdan kurtarmak için bir aygıt yük devretme gerçekleştirmek gerekir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet'ini kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmesi için ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet parantez içine aşağıdaki gibi kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - ilk 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk birim kapsayıcısı ilk 4 yedeklere geçirildikten sonra paralel geçiş başlar. |Bir kerede bir birim kapsayıcısı geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklendikten sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme İlkesi bu birimleri eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |Geçiş tamamlandıktan sonra 5000/7000 Serisi aygıt geçirilen verileri kapsayıcıları erişmelisiniz değil. |Tam ve kaydedilmiş geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları silme öneririz. |Evet |Hayır |
| 13 |Kopya ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazı kopyalama veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef aygıt güncelleştirme işlemlerini izin vermek için 1 olarak güncelleştirmeniz gerekir |Evet |Evet |
| 14 |Geçiş |Birim grupları ile ilişkili birim olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi aygıtta başarısız olabilir. |Tüm boş birim gruplarıyla ilişkili birim silin ve yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |
| 15 |Azure PowerShell cmdlet'leri ve yerel olarak sabitlenmiş birimleri |Azure PowerShell cmdlet'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlı.) Birim türünü değiştirme istenmeyen etkisi olacağından ayrıca Azure PowerShell cmdlet'lerini yerel olarak sabitlenmiş bir birim tüm özelliklerini değiştirmek için kullanmayın çok katmanlı. |Her zaman yapılandırın ve yerel olarak sabitlenmiş birimleri değiştirmek için StorSimple Yöneticisi hizmetini kullanma.  |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimleri için kullanılabilir alanı |Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple Yöneticisi hizmeti yerel alanınız yaklaşık olarak saatte güncelleştirir. |Yeni birim oluşturmak denemeden önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi geçici anlık görüntü yedekleme yedekleme kataloğunda, ancak yalnızca geri yükleme işi süresini gösterir. Ayrıca, bir sanal disk grubu önekiyle gösterir **tmpCollection** üzerinde **yedekleme ilkeleri** geri yükleme işi süresince yalnızca sayfa. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimleri |Bir geri yükleme işi iptal edin ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **başarısız** yerine **iptal edildi**. Bir geri yükleme işi başarısız olur ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **iptal edildi** yerine **başarısız**. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi iptal etmek veya geri yükleme başarısız olursa ve denetleyici yük devretmesi oluşur ek geri yükleme işi görünür **işleri** sayfası. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimleri |Yerel olarak sabitlenmiş bir birim için katmanlı birim (oluşturulan ve güncelleştirme 1.2 ile kopyalanan veya öncesi) dönüştürmek deneyin ve Cihazınızı alana sahip değil veya bir bulut kesinti varsa, clone(s) bozulmuş. |Bu sorun, oluşturulan ve kopyalanan ile güncelleştirme öncesi 2.1 yazılım olan birimlerle oluşur. Bu, seyrek bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Bir birim dönüştürme işlemi devam ederken bir birime bağlı ACRs güncelleştirilmiyor (yerel olarak sabitlenmiş katmanlı veya tersi). ACRs güncelleştirme veri bozulmasına neden olabilir. |Gerekirse, birimi dönüştürmeden önce ACRs güncelleştirin ve dönüştürme işlemi devam ederken ACR güncelleştirmeleri başka yapmayın. | | |
| 22 |Güncelleştirmeler |Güncelleştirme 3'ü uygularken **Bakım** "StorSimple 8000 serisi güncelleştirme 2, Microsoft'un size olası sorunları algılar, günlük bilgilerini aygıtınızdan proaktif olarak toplamak özelliğini içerir" - Klasik Azure portalı sayfasında, güncelleştirme 2'ye ilgili aşağıdaki iletisi görüntülenir. Bu cihaz güncelleştirme 2'ye güncelleştiriliyor belirttiği yanıltıcıdır. Güncelleştirme 3'e güncelleştirilmiş succeesfully cihaz olduktan sonra bu ileti kaybolur. |Bu davranış, bir sonraki sürümde düzeltilecektir. |Evet |Hayır |

## <a name="controller-and-firmware-updates-in-update-3"></a>Güncelleştirme 3'te denetleyicisi ve bellenim güncelleştirmeleri
Bu sürüm LSI sahip sürücü ve bellenim güncelleştirmeleri. LSI sürücü ve bellenim güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz: [güncelleştirme 3'ü yükleme](storsimple-install-update-3.md) , StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-3"></a>Güncelleştirme 3'te sanal aygıt güncelleştirmeleri
Bu güncelleştirme, StorSimple bulut uygulaması (sanal aygıt olarak da bilinir) uygulanamaz. Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi nasıl [güncelleştirme 3'ü yükleme](storsimple-install-update-3.md) , StorSimple Cihazınızda.

