---
title: "StorSimple yerel olarak sabitlenmiş birimleri ile ilgili SSS | Microsoft Docs"
description: "StorSimple yerel olarak sabitlenmiş birimleri hakkında sık sorulan soruların yanıtlarını sağlar."
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 7a2f4b1a-4ac9-4ce1-9359-bd40a9cbbb5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/03/2017
ms.author: manuaery
ms.openlocfilehash: 6c892cbc4acd5531100f45c26a1b81cd5468d255
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple yerel olarak sabitlenmiş birimleri: sık sorulan sorular (SSS)
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [StorSimple yerel olarak sabitlenmiş birimleri: sık sorulan sorular (SSS)](storsimple-8000-local-volume-faq.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Aşağıdaki sorular ve StorSimple yerel olarak sabitlenmiş birim oluşturma, yerel olarak sabitlenmiş bir birim için (ve tersi), katmanlı birim Dönüştür veya yedeklemeniz ve yerel olarak sabitlenmiş bir birim geri yükleme sırasında karşılaşabileceğiniz yanıtlarını geçerlidir.

Sorular ve yanıtlar Aşağıdaki kategorilerde düzenlenir

* Yerel olarak sabitlenmiş bir birim oluşturma
* Yerel olarak sabitlenmiş yedekleme
* Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme
* Yerel olarak sabitlenmiş bir birim geri yükleme
* Yerel olarak sabitlenmiş bir birim yapabilmesini

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim oluşturma hakkında sorular
**S.** 8000 serisi cihazlar üzerinde oluşturabilmeniz için yerel olarak sabitlenmiş bir birim en büyük boyutu nedir?

**A** StorSimple 8000 serisi güncelleştirme 3.0 çalıştıran cihazlarda, yerel olarak sabitlenmiş birimlerin sağlayabilirsiniz 8,5 TB'ye kadar kadar katmanlı birimleri 8100 cihazda 200 TB'ye kadar. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz.    
StorSimple 8000 serisi güncelleştirme çalıştıran cihazlarda 2.x, yerel olarak sabitlenmiş birimlerin kadar hazırlayabilirsiniz 8 TB'ye kadar katmanlı birimleri 8100 cihazda 200 TB'ye kadar. Daha büyük olan 8600 cihazında yerel olarak sabitlenmiş birimleri 20 TB'ye kadar, katmanlı birimleri de 500 TB'ye kadar hazırlayabilirsiniz.   

**S.** Yakın zamanda 8100 cihazımı Update 2.0 sürümüne yükselttiğimde ve yerel olarak sabitlenmiş bir birim oluşturmak çalıştığınızda kullanılabilir en büyük boyutu yalnızca 6 TB ve 8 TB değil. Neden bir 8 TB birim oluşturamazsınız?

**A** Cihazınızı güncelleştirme 2.0 çalıştırıyorsa, yerel olarak sabitlenmiş birimleri 8 adede kadar sağlayabilir veya TB katmanlı birimlerin 200 TB'ye kadar 8100 cihazda. Aygıtınız zaten birimleri yeniden oluşturmak için kullanılabilir alanı katmanlı yerel olarak sabitlenmiş bir birim bu Maksimum boyuttan orantılı olarak daha düşük olacaktır. 100 TB katmanlı birimlerin (olan katmanlı kapasite yarısı) 8100 Cihazınızda sağlanmamışsa, örneğin, ardından 8100 cihazda oluşturabilirsiniz yerel bir birimi en büyük boyutunu gelenlere (kabaca yarısı yerel olarak en fazla birim kapasitesi sabitlenmiş) 4 TB azalır.

Cihazda yerel alan katmanlı birimlerin çalışma kümesini barındırmak için kullanıldığından, cihaz birimleri katmanlı, yerel olarak sabitlenmiş bir birim oluşturmak için kullanılabilir alan azalır. Buna karşılık, yerel olarak sabitlenmiş bir birim orantılı olarak oluşturma katmanlı birimler kullanılabilir alanı azaltır. Aşağıdaki tablolarda, yerel olarak sabitlenmiş birimleri oluşturduğunuzda kullanılabilir katmanlı kapasite 8100 ve 8600 cihazlarda özetler.

####<a name="update-30"></a>Güncelleştirme 3.0 
| Yerel olarak sabitlenmiş birimlerin sağlanan kapasite | 8100 katmanlı birimler için - sağlanacak kullanılabilir kapasite | Katmanlı birimler için - 8600 sağlanması için kullanılabilir kapasite |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176.5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |NA |277.8 TB |
| 15 TB |NA |166.7 TB |
| 22.5 TB |NA |0 TB |

####<a name="update-2x"></a>2.x güncelleştir  
 | Yerel olarak sabitlenmiş birimlerin sağlanan kapasite | 8100 katmanlı birimler için - sağlanacak kullanılabilir kapasite | Katmanlı birimler için - 8600 sağlanması için kullanılabilir kapasite |  
 | --- | --- | --- |  
 | 0 |200 TB |500 TB |  
 | 1 TB |25 TB |475 TB |  
 | 4 TB |100 TB |400 TB |  
 | 8 TB |0 TB |300 TB |  
 | 10 TB |NA |250 TB |  
 | 15 TB |NA |125 TB |  
 | 20 TB |NA |0 TB |   

**S.** Yerel olarak sabitlenmiş birimin oluşturulmasına neden uzun süren bir işlem mi? 

**C.** Yerel olarak sabitlenmiş birimlerin sıkı sağlanır. Cihaz yerel katmanlarda üzerinde alan oluşturmak için var olan katmanlı birimler bazı verileri buluta sağlama işlemi sırasında gönderilen. Ve sağlanacak, cihazınız ve bulut için kullanılabilir bant genişliğini var olan verilere birimin boyutuna bağlıdır bu yana yerel birim oluşturmak için geçen süre birkaç saat olabilir.

**S.** Ne kadar yerel olarak sabitlenmiş bir birim oluşturmak için sürer?

**C.** Yerel olarak sabitlenmiş birimlerin sıkı sağlanan olduğundan, katmanlı birimleri varolan bazı verileri buluta sağlama işlemi sırasında edilmesini. Bu nedenle, yerel olarak sabitlenmiş bir birim oluşturmak için geçen süre birimi, Cihazınızı ve kullanılabilir bant genişliğini veri boyutu gibi birden çok etkene bağlıdır. Birim olan yeni yüklenmiş cihazda yerel olarak sabitlenmiş bir birim oluşturmak için yaklaşık 10 dakika veri terabayt başına zamandır. Ancak, yerel birimlerin oluşturması kullanımda olan bir aygıtta yukarıda açıklanan etkenlere bağlı olarak birkaç saat sürebilir.

**S.** Yerel olarak sabitlenmiş bir birim oluşturmak istiyorum. Farkında olmanız gereken herhangi bir en iyi uygulamalar var mı?

**C.** Yerel olarak sabitlenmiş birimlerin, veri yerel GARANTİLERİN her zaman gerektiren ve gecikme bulut hassas iş yükleri için uygundur. Herhangi bir iş yükleriniz için yerel birimleri kullanımını düşünüldüğünde oluştu, lütfen aşağıdakilere dikkat edin:

* Yerel olarak sabitlenmiş birimlerin sıkı sağlanır ve yerel birimleri oluşturmak için katmanlı birimler kullanılabilir alanı etkiler. Bu nedenle, küçük boyutlu birimler ile başlamalı ve depolama gereksinimi arttıkça ölçeği öneririz.
* Yerel birimlerin sağlanması, var olan verileri buluta katmanlı birimlerin Ftp'den gerektirebilir uzun süren bir işlemdir. Sonuç olarak, bu birimlerdeki azaltılmış performansla karşılaşabilirsiniz.
* Yerel birimlerin sağlanması uzun süren bir işlemdir. Söz konusu gerçek zaman birden çok etkenlere bağlıdır: sağlanacak birim, cihaz ve kullanılabilir bant genişliğini veri boyutu. Buluta mevcut birimlerinizi yedeklemediyseniz birim oluşturma yavaştır. Yerel bir birim sağlamadan önce var olan birimlerinizi bulut anlık görüntüsünü öneririz.
* Var olan katmanlı birimler için yerel olarak sabitlenmiş birimlerin dönüştürebilir ve bu dönüştürme (ek olarak katmanlı veri buluttan [varsa] hale getirme) sonuçta elde edilen yerel olarak sabitlenmiş birim için cihazda alanı sağlama içerir. Yeniden, biz yukarıda açıklanan faktöre bağlıdır uzun süre çalışan bir işlemin budur. İşlem var olan birimler yedeklenmez, hatta daha yavaş olarak mevcut birimlerinizi dönüştürme önce yedeklemeniz öneririz. Cihazınızı da bu işlem sırasında azaltılmış performansla karşılaşabilirsiniz.

Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birim oluşturun](storsimple-manage-volumes-u2.md#add-a-volume)

**S.** Aynı anda birden çok yerel olarak sabitlenmiş birimler oluşturabilir miyim?

**C.** Evet, ancak herhangi bir yerel olarak sabitlenmiş birim oluşturma ve genişletme işi sıralı olarak işlenir.

Yerel olarak sabitlenmiş birimlerin sıkı sağlanır ve bu (hangi buluta sağlama işlemi sırasında edilmesini katmanlı birimlerin var olan verilerden sonuçlanabilir) cihazda yerel alan oluşturulmasını gerektirir. Sağlama işi devam ediyorsa, bu iş tamamlanana kadar bu nedenle, diğer yerel birim oluşturma işleri sıraya alınır.

Önceki iş tamamlanana kadar benzer şekilde, var olan bir yerel toplu Genişletilmekte olan veya yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürülecek, ardından yeni bir yerel olarak sabitlenmiş birim oluşturma sıraya alındı. Yerel olarak sabitlenmiş bir birim boyutunu genişletme, genişletme o birim için var olan yerel alanı içerir. Dönüştürme yerel olarak sabitlenmiş için katmanlı birim elde edilen yerel olarak sabitlenmiş birim için yerel alan oluşturulmasını da içerir. Hem bu işlemler, oluşturma veya yerel alan genişlemesi uzun işi çalışıyor.

Bu işleri görüntüleyebilirsiniz **işleri** Azure StorSimple Yöneticisi hizmet sayfası. Etkin olarak işlenen iş sürekli olarak alanı sağlama ilerleme durumunu yansıtacak şekilde güncelleştirilir. Kalan yerel olarak sabitlenmiş birim işleri çalıştırma olarak işaretlenmiş, ancak kendi ilerleme durduruldu ve sıraya alınan sırayla çekilir.

**S.** I yerel olarak sabitlenmiş bir birim silindi. Yeni birim oluşturmak çalıştığınızda kullanılabilir alanı yansıtılan reclaimed alanı neden göremiyorum? 

**C.** Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple Yöneticisi hizmeti yerel alanınız yaklaşık olarak saatte güncelleştirir. Yeni birim oluşturmak denemeden önce bir saat bekleyin öneririz.

**S.** Yerel olarak sabitlenmiş birimlerin bulut aygıtındaki destekleniyor mu?

**C.** Yerel olarak sabitlenmiş birimlerin bulut uygulaması (eskiden StorSimple sanal cihazı olarak başvurulan 8010 ve 8020 aygıtlar) desteklenmez.

**S.** Oluşturma ve yerel olarak sabitlenmiş birimleri yönetmek için Azure PowerShell cmdlet'lerini kullanabilir miyim? 

**C.** Hayır, Azure PowerShell cmdlet'lerini (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlı) aracılığıyla yerel olarak sabitlenmiş birimler oluşturamazsınız. Ayrıca birim türünü değiştirme istenmeyen etkisi olacağından, Azure PowerShell cmdlet'lerini yerel olarak sabitlenmiş bir birim tüm özelliklerini değiştirmek için kullanmamanızı öneririz çok katmanlı.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim yedekleme hakkında sorular
**S.** Desteklenen yerel olarak sabitlenmiş birimlerin yerel anlık görüntüleri misiniz?

**C.** Evet, yerel olarak sabitlenmiş birimlerin yerel anlık görüntüleri alabilir. Ancak, düzenli olarak, yerel olarak sabitlenmiş birimleri verilerinizi bir olağanüstü durum planlıyorsanız korunduğundan emin olmak için bulut anlık görüntüleri ile yedeklemenizi öneririz.

**S.** Yerel olarak sabitlenmiş birimleri için yerel anlık görüntülerini yönetmek için herhangi bir yönerge var mı?

**C.** Yerel olarak sabitlenmiş birimin veri karmaşıklık oranı yüksek yanında sık yerel anlık görüntüler, hızlı bir şekilde kullanılması ve buluta gönderilen katmanlı birimler verilerden neden için cihazda yerel alan neden olabilir. Bu nedenle, yerel anlık görüntü sayısını en aza öneririz.

**S.** Yerel olarak sabitlenmiş birimlerin my yerel anlık görüntüleri geçersiz bildiren bir uyarı aldım. Ne zaman bu durum oluşabilir?

**C.** Yerel olarak sabitlenmiş birimin veri karmaşıklık oranı yüksek yanında sık yerel anlık görüntüleri hızlı bir şekilde tüketilmesi cihazda yerel alan neden olabilir. Cihaz yerel katmanlarda yoğun olarak kullanılıyorsa, genişletilmiş bulut kesinti dolmasını aygıtı neden olabilir ve (üzerine yazıldı veri eski bloklarına başvurmak için anlık görüntülerini güncelleştirmek için boşluk var gibi) birime gelen yazma anlık görüntüyü geçersiz kılma sağlayabilir. Böyle bir durumda sunulacak birimdeki yazma işlemleri devam edecek, ancak yerel anlık görüntüleri geçersiz olabilir. Mevcut bulut anlık görüntüleri hiçbir etkisi yoktur.

Böyle bir durum ortaya çıkar ve aynı zamanında daha az sıklıkta yerel anlık görüntülerini almak için yerel anlık görüntü zamanlama gözden geçirme veya artık gerekli olmayan eski yerel anlık görüntülerin silinmesi adresini olun bildirmek için uyarı uyarı değil.

Yerel anlık görüntüleri doğrulanamazsa, zaman damgaları geçersiz kılındı yerel anlık görüntü listesi yanı sıra yerel anlık görüntüleri belirli yedekleme ilkesi için geçersiz kılındı bildiren bir bilgi uyarısı alırsınız. Bu anlık görüntüler otomatik silindi ve artık görüntülemeye çalıştıramaz **yedekleme kataloglar** Klasik Azure portalındaki sayfası.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında sorular
**S.** Bazı yavaşlığı cihazda yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürülürken Gözlemleme. Bunun nedeni nedir? 

**C.** Dönüştürme işlemi iki adımdan oluşur: 

1. Alanı en kısa sürede-için--dönüştürülmesi yerel olarak için cihazda sağlama birim sabitlenir.
2. Yerel emin olmak için buluttan herhangi bir katmanlı veri indirme güvence altına alır.

Bu adımları hem de uzun süreli dönüştürülür, aygıt ve kullanılabilir bant genişliğini verileri birimin boyutunu bağımlı işlemler. Var olan katmanlı birimler bazı verileri bulut için sağlama işleminin bir parçası sığdırmaya olarak Cihazınızı bu süre boyunca düşük performansla karşılaşabilirsiniz. Ayrıca, dönüştürme işlemi yavaş olabilir varsa:

* Var olan birimler buluta yedeklenmiş değil; bir dönüştürme başlatmadan önce birimlerinizden yedekleme önerdiğimiz şekilde.
* Hangi bulut için kullanılabilir bant genişliğini kısıtlayabilir bant genişliği azaltma ilkeleri uygulanmış; Bu nedenle, adanmış bir 40 MB/sn veya daha fazla bağlantı buluta sahip öneririz.
* Dönüştürme işlemi, yukarıda açıklanan birden çok Etkenler nedeniyle birkaç saat sürebilir; Bu nedenle, yükselmeleri olmayan saatlerde veya bir hafta sonu tüketiciler üzerindeki etkiyi önlemek için bu işlemi gerçekleştirmek öneririz.

Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birim için katmanlı birim Dönüştür](storsimple-manage-volumes-u2.md#change-the-volume-type)

**S.** Birim dönüştürme işlemi iptal edebilir miyim?

**C.** Hayır, bir kez başlatılan dönüştürme işlemi iptal edilemez. Önceki sorunun açıklandığı gibi işlemi sırasında karşılaştığınız ve, dönüştürme planlarken, yukarıda listelenen en iyi uygulamaları izleyerek olası performans sorunları lütfen unutmayın.

**S.** My birime, dönüştürme işlemi başarısız olursa ne olur?

**C.** Birim dönüştürme bulut bağlantı sorunları nedeniyle başarısız olabilir. Aygıt, bir dizi buluttan katmanlı verileri getirmek için başarısız deneme sonrasında dönüştürme işlemi sonunda vermeyebilir. Böyle bir senaryoda birim türü dönüştürme önce kaynak birim türü olmaya devam edecek ve:

* Birimi dönüştürme hatası bildiren kritik bir uyarı gerçekleştirilecektir. Daha fazla bilgi [ilgili yerel olarak sabitlenmiş birimlerin uyarıları](storsimple-manage-alerts.md#locally-pinned-volume-alerts)
* Bir katmanlı yerel olarak sabitlenmiş bir birim dönüştürüyorsanız toplu veri hala bulutta bulunabilecek şekilde katmanlı birim özelliklerini sergiler devam eder. Bağlantı sorunları çözün ve dönüştürme işlemi yeniden deneyin öneririz.
* Birimin yerel olarak sabitlenmiş bir birim işaretlenir ancak yerel olarak sabitlenmiş dönüştürme katmanlı birim için başarısız olduğunda, (verileri buluta geçmiş çünkü) benzer şekilde, bu bir katmanlı birim olarak çalışacaktır. Bununla birlikte, cihaz yerel katmanlarda alanı kaplar devam eder. Bu alan diğer yerel olarak sabitlenmiş birimler için kullanılamaz. Birim dönüştürme tamamlandıktan ve cihazda yerel alan geri alınabilmesini sağlamak için bu işlemi yeniden deneyin öneririz.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim geri yükleme hakkında sorular
**S.** Yerel olarak sabitlenmiş birimlerin anında geri olan?

**C.** Evet, yerel olarak sabitlenmiş birimlerin anında geri yüklenir. Birim meta veri bilgilerini buluttan geri yükleme işleminin bir parçası olarak çekilir hemen birimi çevrimiçi olana ve ana bilgisayar tarafından erişilebilir. Ancak, yerel GARANTİLERİN birimi için veri tüm verileri, buluttan indirilen ve karşılaşabileceğiniz kadar mevcut olmaz geri yükleme süresince bu birimlerdeki performans azalır.

**S.** Ne kadar yerel olarak sabitlenmiş bir birim geri yüklemek için sürer?

**C.** Yerel olarak sabitlenmiş birimlerin anında geri ve toplu veri arka planda yüklenmesine devam ederken birim meta veri bilgileri buluttan alınır hemen çevrimiçi. Yerel GARANTİLERİN birim verilerini--geri alma geri yükleme işlemi--bu ikinci bölümü, uzun süreli bir işlemdir ve tüm verilerin yeniden yerel yapılması birkaç saat sürebilir. Aynı tamamlamak için geçen süre, geri yüklenen birimin boyutunu ve kullanılabilir bant genişliğini gibi birden çok faktöre bağlıdır. Geri yüklenen özgün birimin sildiyseniz geri yükleme işleminin bir parçası olarak cihazda yerel alan oluşturmak için ek süre ulaşabilirsiniz.

**S.** My var olan yerel olarak sabitlenmiş birim (birim katmanlı gerçekleştirilecek) daha eski bir anlık görüntüye geri yüklemeniz gerekecektir. Birim, bu durumda katmanlı olarak geri yüklenir?

**C.** Hayır, birimin yerel olarak sabitlenmiş bir birim geri yüklenir. Şu anda mevcut olduğundan zaman birimi katmanlı, saat için anlık görüntü tarihleri var olan birimler geri yüklenirken StorSimple her zaman birim türünü diskte kullansa da.

**S.** My yerel olarak sabitlenmiş bir birim son genişletilmiş ancak şimdi ne zaman birimi boyutu daha küçük bir süre için verileri geri yüklemek ihtiyacım. Geri yükleme geçerli birim yeniden boyutlandırma ve geri yükleme tamamlandıktan sonra birimin boyutunu genişletmek gerekecek?

**C.** Evet, geri yükleme birim yeniden boyutlandırma ve geri yükleme tamamlandıktan sonra birimin boyutunu genişletmek gerekir.

**S.** Geri yükleme sırasında bir birim türünü değiştirebilir miyim?

**A.**Hayır, geri yükleme sırasında birim türünü değiştiremezsiniz.

* Silinmiş birimleri anlık depolanan türü olarak geri yüklenir.
* Var olan birimler geri yüklenir anlık saklanan türü ne olursa olsun geçerli tipine göre (önceki iki soruya bakın).

**S.** My yerel olarak sabitlenmiş bir birim geri gerekir, ancak ı yanlış noktanız zaman anlık Çekildi. Geçerli geri yükleme işlemi iptal edebilir miyim?

**C.** Evet, devam eden geri yükleme işlemini iptal edebilirsiniz. Birimin durumunu geri yükleme başlangıcında durumuna geri alınacak. Ancak, geri yükleme devam ederken birime yapılan yazma kaybolur.

**S.** My yerel olarak sabitlenmiş birimlerin birinde bir geri yükleme işlemi başlatıldı ve anlık görüntü oluşturma recollect yok my biriktirme listesi kataloğunda şimdi görüyorum. Ne için kullanılır?

**C.** Bu geri yükleme işlemi önce oluşturulan ve geri yükleme iptal edildi veya başarısız durumda geri almak için kullanılan geçici bir anlık görüntüdür. Bu anlık görüntü silmeyin; geri yükleme tamamlandıktan sonra otomatik olarak silinir. Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez.

**S.** Yerel olarak sabitlenmiş bir birim kopyalamak?

**C.** Evet, kullanabilirsiniz. Ancak, yerel olarak sabitlenmiş bir birim katmanlı birim varsayılan olarak kopyalanma. Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birim kopyalama](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim yapabilmesini hakkında sorular
**S.** Cihazımı başka bir fiziksel aygıt için yük devri gerekir. My yerel olarak sabitlenmiş birimleri üzerinde yerel olarak sabitlenmiş veya katmanlı başarısız?

**C.** Hedef aygıta yazılım sürümüne bağlı olarak, yerel olarak sabitlenmiş birimleri olarak yük devretmenin:

* Hedef cihaz StorSimple 8000 çalışıyorsa, yerel olarak sabitlenmiş serisi güncelleştirme 2
* Hedef cihaz StorSimple çalışıyorsa katmanlı 1.x 8000 serisi güncelleştirme
* Hedef cihaz bulut uygulaması, katmanlı (yazılım sürümü güncelleştirme 2 veya 1.x güncelleştirme)

Daha fazla bilgi [yük devretme ve kurtarma, yerel olarak sabitlenmiş birimleri sürümleri arasında](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**S.** Yerel olarak sabitlenmiş birimlerin anında olağanüstü durum kurtarma (DR) sırasında geri yüklenir?

**C.** Evet, yerel olarak sabitlenmiş birimlerin anında yük devretme sırasında geri yüklenir. Meta veri bilgileri birimi için yük devretme işleminin bir parçası olarak buluttan çekilir hemen birim hedef cihazda çevrimiçi olana ve ana bilgisayar tarafından erişilebilir. Bu sırada, toplu veri arka planda yüklemeye devam eder ve yük devretme süresince bu birimlerdeki azaltılmış performansla karşılaşabilirsiniz.

**S.** Yük devretme iş tamamlandı, nasıl ı hedef cihazda geri yüklenen yerel olarak sabitlenmiş bir birim ilerlemesini izleyebilirsiniz görüyorum?

**C.** Bir yük devretme işlemi sırasında yük devretme işlemi tamamlandıktan sonra tüm birimler yük devretme kümesindeki alınan anında geri ve hedef cihazda çevrimiçi olarak işaretlenir. Üzerinden başarısız tüm yerel olarak sabitlenmiş birimler de buna dahildir; Ancak, birim için tüm veriler yüklenen verilerin yerel GARANTİLERİN yalnızca kullanılabilir olur. Yük devretme bir parçası oluşturulan karşılık gelen geri yükleme işleri izleme tarafından devralınırsa başarısız oldu her yerel olarak sabitlenmiş bir birim için bu ilerleme durumunu izleyebilirsiniz. Bu tek tek geri yükleme işleri, yalnızca yerel olarak sabitlenmiş birimler için oluşturulur.

**S.** Yük devretme sırasında bir birim türünü değiştirebilir miyim?

**C.** Hayır, bir yük devretme sırasında birim türünü değiştiremezsiniz. Başarısız olan üzerinden StorSimple 8000 çalıştıran başka bir fiziksel aygıt serisi güncelleştirme 2, birimler üzerinde anlık depolanan birim türüne göre devredilir. Üzerinden başka bir aygıt sürüme başarısız olduğunda, soru birim türü üzerinde bir yük devretme sonrasında bakın.

**S.** Bir birim kapsayıcısı ile bulut uygulaması için yerel olarak sabitlenmiş birimlerin üzerinden başarısız olabilir?

**C.** Evet, kullanabilirsiniz. Yerel olarak sabitlenmiş birimleri üzerinde katmanlı birimler olarak devredilir. Daha fazla bilgi [yük devretme ve kurtarma, yerel olarak sabitlenmiş birimleri sürümleri arasında](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)

