---
title: StorSimple 8000 serisi güncelleştirme 2.2 sürüm notları | Microsoft Docs
description: Yeni özellikler, sorunlar ve geçici çözümler için StorSimple 8000 serisi güncelleştirme 2.2 açıklar.
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
ms.openlocfilehash: 78be340b4a47fed88f5e8c3f5741ae7024124bd5
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057851"
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 serisi güncelleştirme 2.2 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 2.2 tanımlayın. Ayrıca bu sürümde dahil StorSimple yazılım güncelleştirmelerinin bir listesini içerir.

Güncelleştirme 2.2 sürüm (GA) veya güncelleştirme 0.1 güncelleştirme 2. 1 ' çalışan herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 2.2 ile ilişkili aygıt 6.3.9600.17708 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 2.2 yalnızca yazılım güncelleştirmeleri sahiptir. Yaklaşık 1,5-2 Bu güncelleştirmeyi yüklemek için saat sürer. 
> * Güncelleştirme 2.1 çalıştırıyorsanız, güncelleştirme 2.2 mümkün olan en kısa sürede uygulamanızı öneririz.
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Birkaç gün bekleyin ve ardından güncelleştirmeleri taramak üzere yeniden bunlar olarak yakında kullanılabilir hale gelecektir.
> 
> 

## <a name="whats-new-in-update-22"></a>Güncelleştirme 2.2 yenilikler nelerdir?
Güncelleştirme 2.2 aşağıdaki anahtar iyileştirmeler yapılmıştır.

* **Alan geri kazanma iyileştirme otomatik** – ölçülü kaynak kullanan birimlerde geri kazanılacak kullanılmamış depolama blokları gereken veriler silinir. Bu sürüm için alan geri kazanma işlemini önceki sürümlere kıyasla daha hızlı kullanılabilir hale gelmeden kullanılmayan alanı sonuçta bulut geliştirilmiştir.
* **Performans iyileştirmeleri anlık görüntü** – güncelleştirme 2.2 Bulutu belirli senaryolarda büyük olduğunda birimleri kullanıldığından olup olmadığını hiç veri dalgalanmasına en az anlık görüntü işlemek için zaman iyileştirilmiştir. Bu geliştirme için yararlı bir senaryo arşiv birimleri olacaktır.
* **Destek Paketi toplama sağlamlaştırma** – geliştirmeler destek paketi toplanan ve bu sürümde karşıya şekilde olmuştur. 
* **Güvenilirlik iyileştirmeleri güncelleştirme** – içinde geliştirilmiş bir güncelleştirme güvenilirliği neden hata düzeltmeleri bu sürüme sahip.

## <a name="issues-fixed-in-update-22"></a>Güncelleştirme 2.2 giderilen sorunlar
Aşağıdaki tablolarda güncelleştirmeler 2.2 ve 2.1 düzeltilen sorunlardan özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ana performans |Önceki sürümde, ana bilgisayar tarafı performans sorunlarını yerel olarak sabitlenmiş bir birim oluşturulması sırasında yerel olarak sabitlenmiş bir birim katmanlı birim dönüştürülmesi sırasında gözlemlenen. Böylece bir geliştirme birim oluşturma ve dönüştürme yordamları sırasında konak performansta bunun sonucunda bu sürümde bu sorunlar giderildi. |Evet |Hayır |
| 2 |Yerel olarak sabitlenmiş birimleri |Nadir durumlarda, sistem, yerel olarak sabitlenmiş bir birim oluştururken kilitlenme. Bu hata, bu sürümde düzeltilmiştir. |Evet |Hayır |
| 3 |Katmanlama |StorSimple bulut cihazları (8010 hem de 8020) için meta verileri buluta katmanlı zaman aralıklı çökme (Crash) vardı. Bu sürümde bu sorun düzeltilmiştir. |Hayır |Evet |
| 4 |Anlık görüntü oluşturma |İlgili senaryolarda büyük birimleri ile artımlı anlık görüntü oluşturmaya ve hiçbir veri dalgalanmasına için en az sorunları. Bu sürümde bu sorunlar giderildi. |Evet |Evet |
| 5 |Openstack kimlik doğrulaması |Openstack bulut hizmeti sağlayıcısı olarak kullanırken, kullanıcı kimlik doğrulaması ile ilgili sık bir hatayı içine burada JSON ayrıştırıcı bir kilitlenme sonuçlandı çalışır. Bu hata, bu sürümde sabittir. |Evet |Hayır |
| 6 |Konak tarafındaki kopyalama |Yazılım önceki sürümlerde, verileri başka bir birime bir birimden kopyalarken ODX zamanlama ilgili sık bir hatayı görüldü. Bu bir denetleyici yük devretme kümesinde neden olur ve sistemin potansiyel olarak kurtarma Modu'na. Bu hata, bu sürümde sabittir. |Evet |Hayır |
| 7 |Windows Yönetim Araçları (WMI) |Yazılım önceki sürümlerinde, birçok özel web proxy hatası örneklerini vardı "<ManagementException> Sağlayıcı yükleme hatası". Bu hata, WMI bellek sızıntısı için öznitelikli ve şimdi sabit. |Evet |Hayır |
| 8 |Güncelleştirme |Yazılım, önceki sürümlerinde nadir bazı durumlarda, tarama veya güncelleştirmelerini yüklemeye çalışırken bir "CisPowershellHcsscripterror" kullanıcı aldı. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |
| 9 |Destek paketi |Bu sürümde, destek paketi toplanan ve karşıya şekilde geliştirmeleri olmuştur. |Evet |Evet |

## <a name="known-issues-in-update-22"></a>Güncelleştirme 2.2 bilinen sorunlar
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
| 9 |Azure PowerShell |StorSimple cmdlet'ini kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmesi için ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet parantez içine aşağıdaki gibi kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - First 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk birim kapsayıcısı ilk 4 yedeklere geçirildikten sonra paralel geçiş başlar. |Bir kerede bir birim kapsayıcısı geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklendikten sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme İlkesi bu birimleri eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |Geçiş tamamlandıktan sonra 5000/7000 Serisi aygıt geçirilen verileri kapsayıcıları erişmelisiniz değil. |Tam ve kaydedilmiş geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları silme öneririz. |Evet |Hayır |
| 13 |Kopya ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazı kopyalama veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef aygıt güncelleştirme işlemlerini izin vermek için 1 olarak güncelleştirmeniz gerekir |Evet |Evet |
| 14 |Geçiş |Birim grupları ile ilişkili birim olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi aygıtta başarısız olabilir. |Tüm boş birim gruplarıyla ilişkili birim silin ve yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |
| 15 |Azure PowerShell cmdlet'leri ve yerel olarak sabitlenmiş birimleri |Azure PowerShell cmdlet'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlı.) |Her zaman StorSimple Yöneticisi hizmeti yerel olarak sabitlenmiş birimlerin yapılandırmak için kullanın. |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimleri için kullanılabilir alanı |Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple Yöneticisi hizmeti yerel alanınız yaklaşık olarak saatte güncelleştirir. |Yeni birim oluşturmak denemeden önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi geçici anlık görüntü yedekleme yedekleme kataloğunda, ancak yalnızca geri yükleme işi süresini gösterir. Ayrıca, bir sanal disk grubu önekiyle gösterir **tmpCollection** üzerinde **yedekleme ilkeleri** geri yükleme işi süresince yalnızca sayfa. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimleri |Bir geri yükleme işi iptal edin ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **başarısız** yerine **iptal edildi**. Bir geri yükleme işi başarısız olur ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **iptal edildi** yerine **başarısız**. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi iptal etmek veya geri yükleme başarısız olursa ve denetleyici yük devretmesi oluşur ek geri yükleme işi görünür **işleri** sayfası. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimleri |Yerel olarak sabitlenmiş bir birim için katmanlı birim (oluşturulan ve güncelleştirme 1.2 ile kopyalanan veya öncesi) dönüştürmek deneyin ve Cihazınızı alana sahip değil veya bir bulut kesinti varsa, clone(s) bozulmuş. |Bu sorun, oluşturulan ve kopyalanan ile güncelleştirme öncesi 2.1 yazılım olan birimlerle oluşur. Bu, seyrek bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Bir birim dönüştürme işlemi devam ederken bir birime bağlı ACRs güncelleştirilmiyor (yerel olarak sabitlenmiş katmanlı veya tersi). ACRs güncelleştirme veri bozulmasına neden olabilir. |Gerekirse, birimi dönüştürmeden önce ACRs güncelleştirin ve dönüştürme işlemi devam ederken ACR güncelleştirmeleri başka yapmayın. | | |

## <a name="controller-and-firmware-updates-in-update-22"></a>Güncelleştirme 2.2 denetleyici ve bellenim güncelleştirmeleri
Bu sürüm yalnızca yazılım güncelleştirme içerir. Ancak, güncelleştirme 2 önce bir sürümünden güncelleştiriyorsanız sürücü, Storport, Spaceport, yüklemek gerekecektir ve (bazı durumlarda) disk aygıtınızda Bellenim güncelleştirmeleri.

Sürücü, Storport, Spaceport ve disk Bellenim güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz: [güncelleştirme 2.2 yüklemek](storsimple-install-update-21.md) , StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-22"></a>Güncelleştirme 2.2 sanal aygıt güncelleştirmeleri
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi nasıl [güncelleştirme 2.2 yüklemek](storsimple-install-update-21.md) , StorSimple Cihazınızda.

