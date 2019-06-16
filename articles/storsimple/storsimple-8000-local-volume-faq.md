---
title: StorSimple'ı yerel olarak sabitlenmiş birimler hakkında SSS | Microsoft Docs
description: StorSimple yerel olarak sabitlenmiş birimler hakkında sık sorulan soruların yanıtlarını sağlar.
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: aa69d8b07d31b5cf0386e34c113475cbf4191891
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319556"
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple'ı yerel olarak sabitlenmiş birimler: sık sorulan sorular (SSS)
## <a name="overview"></a>Genel Bakış
Sorular ve cevaplar, StorSimple yerel olarak sabitlenmiş birim oluşturma, bir katmanlı birimin yerel olarak sabitlenmiş bir birim için (ve tersi) dönüştürme veya yedekleme ve yerel olarak sabitlenmiş bir birim geri yükleme sırasında karşılaşabileceğiniz aşağıda verilmiştir.

Sorular ve cevaplar Aşağıdaki kategorilerde düzenlenir

* Yerel olarak sabitlenmiş birim oluşturma
* Yerel olarak sabitlenmiş yedekleme
* Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme
* Yerel olarak sabitlenmiş bir birim geri yükleme
* Yerel olarak sabitlenmiş bir birim yük devrediliyor

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş birim oluşturma hakkında sorular
**S.** 8000 serisi cihazlarda oluşturabilmeniz için bir yerel olarak sabitlenmiş birimin boyutu üst sınır nedir?

**A** yerel olarak sabitlenmiş birimler, StorSimple 8000 serisi güncelleştirme 3.0 çalıştıran cihazlarda sağlayabilirsiniz 8,5 TB'a kadar veya katmanlı birimlerin 200 TB'a kadar 8100 cihazında. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz.

**S.** Ben en son 8100 model Cihazınızı güncelleştirme 3. 0'için yükseltme ve yerel olarak sabitlenmiş bir birim oluşturmayı denediğinizde kullanılabilir en büyük boyutu yalnızca 6 TB ve 8,5 TB boyutunda değil. 8,5 TB birim neden oluşturamıyorum?

**Bir** Cihazınızı güncelleştirme 3.0 çalışıyorsa, 8.5 kadar yerel olarak sabitlenmiş birimler sağlayabilir veya TB katmanlı birimlerin 200 TB'a kadar 8100 cihazında. Cihazınızı birimleri ve oluşturmak için kullanılabilir alan zaten katmanlı yerel olarak sabitlenmiş bir birim orantılı olarak bu en yüksek boyuttan daha düşük olacaktır. Yaklaşık 106 TB katmanlı birimlerin (olan katmanlı kapasitesi yarısını) 8100 Cihazınızda sağlanmamışsa, örneğin, daha sonra oluşturabileceğiniz 8100 cihazda yerel bir birimi en büyük boyutunu gelenlere 4 TB (yaklaşık azalır en fazla yerel olarak yarısını birim kapasitesi sabitlenmiş).

Bazı cihazda yerel alan, katmanlı birimlerin çalışma kümesini barındırmak için kullanıldığından, cihaz katmanlı birimleri, bir yerel olarak sabitlenmiş birim oluşturmak için kullanılabilir alan azalır. Buna karşılık, orantılı olarak yerel olarak sabitlenmiş birim oluşturma, katmanlı birimler kullanılabilir alanı azaltır. Aşağıdaki tablolarda, yerel olarak sabitlenmiş birimlerin oluşturulması sırasında 8100 ve 8600 cihazlarında kullanılabilir katmanlı kapasitesi özetler.

#### <a name="update-30"></a>Güncelleştirme 3.0 

| Sağlanan kapasiteyi yerel olarak sabitlenmiş birimler | 8100 için katmanlı birim - sağlanacak kullanılabilir kapasite | 8600 için katmanlı birim - sağlanacak kullanılabilir kapasite |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176.5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8,5 TB |0 TB |311.1 TB |
| 10 TB |NA |277.8 TB |
| 15 TB |NA |166.7 TB |
| 22.5 TB |NA |0 TB |

**S.** Neden yerel olarak sabitlenmiş birim oluşturma, uzun süren bir işlem mi?

**C.** Yerel olarak sabitlenmiş birimler bol sağlanır. Cihaz yerel katmanlarda alanı oluşturmak için bazı verileri var olan katmanlı birimler buluta sağlama işlemi sırasında klasörüdür. Ve bunu hala uygulanmakta cihazınız ve bulut için kullanılabilir bant genişliğini var olan verilere birimin boyutuna bağlıdır olduğundan yerel birim oluşturmak için harcanan süre birkaç saat olabilir.

**S.** Ne kadar yerel olarak sabitlenmiş birim oluşturmak için sürer?

**C.** Yerel olarak sabitlenmiş birimler bol sağlanan olduğundan, katmanlı birimlerin varolan bazı veriler buluta sağlama işlemi sırasında itilecek. Bu nedenle, yerel olarak sabitlenmiş birim oluşturmak için harcanan süre birimi, cihazınızın ve kullanılabilir bant genişliğini veri boyutu gibi birden çok etkene bağlıdır. Birim yok olan yeni yüklenmiş bir cihaz üzerinde yerel olarak sabitlenmiş birim oluşturmak için yaklaşık 10 dakika veri terabayt başına zamandır. Ancak, yerel birimlerin oluşturulması, kullanımda olan bir cihazda, yukarıda açıklanan faktörleri temel birkaç saat sürebilir.

**S.** Yerel olarak sabitlenmiş bir birim oluşturmak istiyorsunuz. Tüm en iyi uygulamalarını dikkat edilmesi gereken ihtiyacım var mı?

**C.** Yerel olarak sabitlenmiş birimlerin her zaman verilerin yerel GARANTİLERİN gerektiren ve gecikme buluta hassas iş yükleri için uygundur. Herhangi bir iş yükünüz için yerel birimler kullanımını göz, lütfen aşağıdakilere dikkat edin:

* Yerel olarak sabitlenmiş birimler bol sağlanır ve yerel birimler oluşturmak için katmanlı birimler kullanılabilir alanı etkiler. Bu nedenle, daha küçük boyutlu birimler ile başlayın ve, depolama gereksinimi arttıkça ölçeği öneririz.
* Yerel birimlerin sağlanması, mevcut veri katmanlı birimlerden buluta gönderme gerektirebilir uzun süren bir işlemdir. Sonuç olarak, bu birimlerde düşük performansla karşılaşabilirsiniz.
* Yerel hacimlerdeki sağlama uzun süren bir işlemdir. Gerçek zaman dahil birden çok unsura bağlıdır: birim sağlanırken, cihaz ve kullanılabilir bant genişliğini veri boyutu. Var olan birimlerinizi buluta yedeklemediyseniz, birim oluşturma yavaştır. Yerel birim sağlamadan önce var olan birimlerinizi bulut anlık görüntüsünü öneririz.
* Yerel olarak sabitlenmiş birimler için var olan katmanlı birimler dönüştürebilir ve alanı (ek olarak, katmanlı verileri varsa, buluttan getirme) elde edilen yerel olarak sabitlenmiş birim için cihaz sağlama bu dönüştürmeyi içerir. Yukarıda kısıtlayabildiğinden faktöre bağlıdır uzun süre çalışan bir işlemin yeniden budur. İşlem var olan birimler yedeklenmez, hatta daha yavaş olması gibi dönüştürme önce var olan birimlerinizi yedeklemenizi öneririz. Cihazınız, ayrıca bu işlem sırasında performansın düşmesine karşılaşabilirsiniz.

Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birim oluşturun](storsimple-8000-manage-volumes-u2.md#add-a-volume)

**S.** Aynı anda birden çok yerel olarak sabitlenmiş birimler oluşturabilir miyim?

**C.** Evet, ancak tüm yerel olarak sabitlenmiş birim oluşturma ve genişletme işlemlerini sıralı olarak işlenir.

Yerel olarak sabitlenmiş birimler bol sağlanır ve bu (Bu katmanlı birimler buluta sağlama işlemi sırasında itilecek bulunan mevcut verilerden neden olabilir) cihazda yerel alan oluşturulmasını gerektirir. Sağlama işi devam ediyorsa, bu iş tamamlanana kadar bu nedenle, diğer yerel birim oluşturma işlerini kuyruğa alınır.

Önceki iş tamamlanana kadar benzer şekilde, var olan bir yerel birim Genişletilmekte olan veya katmanlı birim için yerel olarak sabitlenmiş bir birim dönüştürülür, sonra yeni bir yerel olarak sabitlenmiş birim oluşturma kuyruğa alınır. Yerel olarak sabitlenmiş bir birim boyutunu genişletme, genişletme, birim için var olan yerel alanı içerir. Dönüştürme için yerel olarak sabitlenmiş bir katmanlı birim ortaya çıkan yerel olarak sabitlenmiş birim için yerel alan oluşturulmasını da içerir. Bu işlem, oluşturma ya da yerel alanı genişletme hem uzun işi çalışıyor.

Bu işleri görüntüleyebilirsiniz **işleri** StorSimple cihaz Yöneticisi hizmet dikey penceresi. Etkin olarak işlenmekte olan iş sürekli olarak alanı hazırlama işleminizin ilerleme durumunu yansıtacak şekilde güncelleştirilir. Kalan yerel olarak sabitlenmiş birim işleri çalıştırma olarak işaretlenmiş, ancak ilerleme durumunu durduruldu ve sıraya alınan sırayla çekilen.

**S.** I yerel olarak sabitlenmiş bir birim silindi. Yeni birim oluşturmak çalıştığınızda kullanılabilir alana yansıtılan geri kazanılan alanı neden göremiyorum?

**C.** Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimler için mevcut olan alanı hemen güncelleştirilmeyebilir. StorSimple cihaz Yöneticisi hizmeti, kullanılabilir yerel alanın yaklaşık saatte güncelleştirir. Yeni birim oluşturmak denemeden önce bir saat bekleyin öneririz.

**S.** Bulut gerecinde yerel olarak sabitlenmiş birimler destekleniyor mu?

**C.** Bulut Gereci (eski adıyla StorSimple sanal cihazı olarak başvurulan 8010 ve 8020 cihazlar) yerel olarak sabitlenmiş birimler desteklenmez.

**S.** Oluşturma ve yerel olarak sabitlenmiş birimler yönetmek için Azure PowerShell cmdlet'lerini kullanabilir miyim?

**C.** Hayır, Azure PowerShell cmdlet'leri (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlı) aracılığıyla yerel olarak sabitlenmiş birimler oluşturamazsınız. Ayrıca birim türünü değiştirme istenmeyen etkisi olacağından, Azure PowerShell cmdlet'lerinin herhangi bir yerel olarak sabitlenmiş birimin özelliklerini değiştirmek için kullanmamanızı öneririz çok katmanlı.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Bir yerel olarak sabitlenmiş birimin yedeklenmesiyle ilgili sorular
**S.** Desteklenen yerel olarak sabitlenmiş birimlerin yerel anlık görüntüleri misiniz?

**C.** Evet, yerel olarak sabitlenmiş birimlerinizin yerel anlık görüntüler alabilir. Ancak, düzenli olarak bir olağanüstü planlıyorsanız verilerinizin korunduğundan emin olmak için bulut anlık görüntüleri ile yerel olarak sabitlenmiş birimlerinizin yedeklemenizi öneririz.

Yerel olarak sabitlenmiş birimlerin yerel anlık görüntüler de kullanıma buluta katmanı ve cihazın yerel katmanında kalmak için garanti edilmez unutmayın.

**S.** Yerel olarak sabitlenmiş birimler için yerel anlık görüntüleri yönetme için yönergeler var mı?

**C.** Yüksek oranda yerel olarak sabitlenmiş birim veri değişim sıklığının yanı sıra sık yerel anlık görüntüler, katmanlı birimler buluta gönderilen verileri sonuçlanır ve hızlı bir şekilde kullanılması için cihazda yerel alan neden olabilir. Bu nedenle, yerel anlık görüntü sayısını en aza öneririz.

**S.** Yerel olarak sabitlenmiş birimlerin yerel Bilgisayarım anlık görüntüleri geçersiz kılındı bildiren bir uyarı aldım. Bu ne?

**C.** Yüksek oranda yerel olarak sabitlenmiş birim veri değişim sıklığının yanı sıra sık yerel anlık görüntüler, hızlı bir şekilde kullanılması için cihazda yerel alan neden olabilir. Cihaz yerel katmanlarda yoğun olarak kullanılır, genişletilmiş bulut kesinti dolmasını cihazı neden olabilir ve birime gelen yazma işlemleri (eski bloklarını başvurmak için anlık görüntüleri güncelleştirmek için boşluk mevcut olduğundan anlık görüntüleri geçersiz kılma neden olabilir veriler) üzerine yazıldı. Böyle bir durumda birime yazma bölgesinden devam eder, ancak yerel anlık görüntüleri geçersiz olabilir. Var olan bulut anlık görüntüleri için herhangi bir etkisi yoktur.

Uyarı bildirimi, böyle bir durum ortaya çıkar ve aynı zamanında daha az sıklıkta yerel anlık görüntüler, yerel anlık görüntü zamanlamalarını gözden geçirme veya artık gerekli olmayan eski yerel anlık görüntülerin silinmesi adresini olun bildirim sağlamaktır.

Yerel anlık görüntüleri doğrulanamazsa, zaman damgaları geçersiz kılınan yerel anlık görüntüleri listesinde yanı sıra belirli yedekleme ilkesinin yerel anlık görüntüleri geçersiz kılındı bildiren bir bilgi uyarısı alırsınız. Bu anlık görüntüler otomatik silindi ve artık görüntülemeye erişemeyeceksiniz **yedekleme katalogları** Azure portalındaki dikey penceresinde.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında sorular
**S.** Bazı yavaşlık cihazda yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürülürken gözleme. Bunun nedeni nedir?

**C.** Dönüştürme işlemi iki adımdan oluşur:

1. Alan için olan en kısa sürede-için--dönüştürülmesi yerel olarak cihazda sağlama birim sabitlendi.
2. Yerel emin olmak için buluttan herhangi bir katmanlı veri indirme garanti eder.

Bu adımlar hem de uzun süreli dönüştürülecek, cihaz ve kullanılabilir bant genişliğini veri birimin boyutuna bağlı işlemler. Hazırlama işleminin bir parçası olarak bazı verileri var olan katmanlı birimler buluta sığdırmaya gibi Cihazınızı bu süre boyunca düşük performansla karşılaşabilirsiniz. Ayrıca, dönüştürme işlemi daha yavaş olabilir varsa:

* Var olan birimler buluta yedeklenmiş değil; önerdiğimiz biçimde bir dönüştürme başlatmadan önce birimleri yedekleme.
* Bulut için kullanılabilir bant genişliğini kısıtlayabilir, bant genişliği azaltma ilkeleri uygulanmış; Bu nedenle, ayrılmış bir 40 MB/sn veya daha fazla bağlantı buluta sahip öneririz.
* Dönüştürme işlemi, yukarıda açıklanan çoklu faktörlerle nedeniyle birkaç saat sürebilir; Bu nedenle, en yüksek sayılar olmayan saatlerde veya bir hafta sonu tüketiciler üzerindeki etkiyi önlemek için bu işlemi gerçekleştirmenizi öneririz.

Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)

**S.** Birim dönüştürme işlemi iptal edebilir miyim?

**C.** Hayır, başlatıldıktan sonra dönüştürme işlemini iptal edilemez. Önceki soruda açıklanan olabileceğiniz işlemi sırasında karşılaştığınız ve, dönüştürme planlarken, yukarıda listelenen en iyi uygulamaları izleyin, olası performans sorunlarını lütfen unutmayın.

**S.** My birime, dönüştürme işlemi başarısız olursa ne olur?

**C.** Birim dönüştürme, bulut bağlantı sorunları nedeniyle başarısız olabilir. Cihaz, sonunda bir dizi buluttan katmanlı verileri getirmek için başarısız deneme sonrasında dönüştürme işlemi durdurabilir. Böyle bir senaryoda, birim türü dönüştürme önce kaynak birim türünü olmaya devam edecek ve:

* Birim dönüştürme hatası bildiren kritik bir uyarı gerçekleştirilecektir. Daha fazla bilgi [uyarılarla ilgili yerel olarak sabitlenmiş birimler](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts)
* Bir katmanlı yerel olarak sabitlenmiş bir birim için dönüştürüyorsanız, birim veri yine de bulutta bulunabilecek şekilde katmanlı birimin özelliklerini sergilemesini devam eder. Bağlantı sorunları çözün ve sonra dönüştürme işlemi yeniden deneyin öneririz.
* Birimin yerel olarak sabitlenmiş bir birim işaretlenir ancak dönüştürme bir yerel olarak sabitlenmiş bir katmanlı birim başarısız olduğunda, (verileri buluta geçmiş çünkü) benzer şekilde, bu bir katmanlı birim çalışır. Ancak, cihaz yerel katmanlarda alanı kaplayan devam eder. Bu alan için diğer yerel olarak sabitlenmiş birimler kullanılabilir olmayacaktır. Birim dönüştürme tamamlandıktan ve cihazda yerel alan geri alınabilmesini sağlamak için bu işlemi yeniden deneyin öneririz.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim geri yükleme hakkında sorular
**S.** Yerel olarak sabitlenmiş birimler anında geri misiniz?

**C.** Evet, yerel olarak sabitlenmiş birimler anında geri yüklenir. Meta veri bilgilerini birimi için buluttan geri yükleme işleminin bir parçası olarak çekilen hemen sonra birimi çevrimiçi duruma getirildikten ve konak tarafından erişilebilir. Ancak, birim için yerel GARANTİLERİN veri tüm verileri, buluttan indirilen ve karşılaşabileceğiniz kadar mevcut olmaz geri yükleme işlemi boyunca bu birimlerde performans azaltıldı.

**S.** Ne kadar yerel olarak sabitlenmiş bir birim sağlamaya sürer?

**C.** Yerel olarak sabitlenmiş birimler anında geri ve arka planda yüklenecek birimdeki verileri devam ederken birim meta veri bilgilerini buluttan alınan hemen sonra çevrimiçi. Yerel GARANTİLERİN toplu verileri için geri alma geri yükleme işlemi--bu ikinci bölümü, uzun süreli bir işlemdir ve tekrar yerel olarak yapılması tüm veriler için birkaç saat sürebilir. Aynı tamamlamak için geçen süre kurtarılan birimin boyutuna ve kullanılabilir bant genişliği gibi birden çok etkene bağlıdır. Geri yüklenen özgün birimin silinmiş olması durumunda, geri yükleme işleminin bir parçası olarak cihazda yerel alan oluşturmak için ek süre alınır.

**S.** Uygulamam var olan yerel olarak sabitlenmiş birim (birimin katmanlı zaman taken) daha eski bir anlık görüntüye geri yüklemek gerekir. Birim, bu durumda katmanlı olarak geri yüklenir?

**C.** Hayır, birimin yerel olarak sabitlenmiş bir birim geri yüklenir. Şu anda mevcut olduğundan anlık görüntü tarih zaman birimin katmanlı, saat için var olan birimler geri yüklerken, StorSimple her zaman birim türünü diskte kullansa da.

**S.** My yerel olarak sabitlenmiş birim kısa bir süre önce genişletilmiş ancak miyim artık zaman birimin boyutu daha küçük bir saat verileri geri yüklemeniz gerekir. Geri yükleme geçerli birim boyutlandırma ve geri yükleme tamamlandıktan sonra birim boyutunu genişletmek ihtiyacım olacak?

**C.** Evet, geri yükleme birim boyutlandırma ve geri yükleme tamamlandıktan sonra birim boyutunu genişletmek ihtiyacınız olacak.

**S.** Geri yükleme sırasında bir birim türünü değiştirebilir miyim?

**C.** Hayır, geri yükleme sırasında birim türünü değiştiremezsiniz.

* Silinmiş olan birimler anlık görüntüde depolanan tür olarak geri yüklenir.
* Var olan birimler geri yüklenir anlık görüntüde depolanan tür bağımsız olarak geçerli tipine göre (önceki iki sorulara bakın).

**S.** My yerel olarak sabitlenmiş birim geri gerekir, ancak miyim yanlış noktanız zaman anlık görüntüde Çekildi. Geçerli geri yükleme işlemini iptal edebilir miyim?

**C.** Evet, devam eden bir geri yükleme işlemi iptal edebilirsiniz. Biriminin durumunu geri yükleme başındaki durumuna geri alınacak. Bununla birlikte, geri yükleme devam ederken, birime yapılan herhangi bir yazma kaybolur.

**S.** Ben bir my yerel olarak sabitlenmiş birimlerin geri yükleme işlemi başlatıldı ve anlık görüntü oluşturma gerçekleşti yoksa benim biriktirme listesi kataloğunda artık görüyorum. Ne için kullanılır?

**C.** Geri yükleme işlemi önce oluşturulan ve geri yükleme işlemi iptal edildi veya başarısız durumunda geri almak için kullanılan geçici bir anlık görüntü budur. Bu anlık görüntüyü silmeyin; geri yükleme işlemi tamamlandıktan sonra otomatik olarak silinir. Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez.

**S.** Yerel olarak sabitlenmiş bir birim kopyalayabilir miyim?

**C.** Evet, uygulayabilirsiniz. Ancak, yerel olarak sabitlenmiş birimin katmanlı birim varsayılan olarak kopyalanır. Hakkında daha fazla bilgi [yerel olarak sabitlenmiş bir birimi kopyalama](storsimple-8000-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim yük devrediliyor hakkında sorular
**S.** Cihazımı başka bir fiziksel cihaza yük devretme gerekir. My yerel olarak sabitlenmiş birimler üzerinde yerel olarak sabitlenmiş veya katmanlı başarısız?

**C.** Yerel olarak sabitlenmiş birimler hedef cihaz StorSimple 8000 serisi güncelleştirme 3 veya üzeri çalıştırıyorsa, yerel olarak sabitlenmiş yük devredildi.

Daha fazla bilgi [yük devretme ve DR, yerel olarak sabitlenmiş birimler sürümler arasında](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**S.** Olağanüstü Durum Kurtarma (DR) sırasında yerel olarak sabitlenmiş birimler anında geri yüklenir?

**C.** Evet, yerel olarak sabitlenmiş birimler anında yük devretme sırasında geri yüklenir. Yük devretme işleminin bir parçası olarak birim için meta veri bilgilerini buluttan çekilen hemen sonra birim hedef cihazda çevrimiçi duruma getirildikten ve konak tarafından erişilebilir. Bu arada, birimdeki verileri, arka planda karşıdan yüklemeye devam eder ve yük devretme boyunca bu birimlerde performans azaltılmış yaşayabilirsiniz.

**S.** Yük devretme işi tamamlandı, nasıl miyim hedef cihaza geri yükleniyorsa, yerel olarak sabitlenmiş birim ilerlemesini izleyebilirsiniz görüyorum?

**C.** Bir yük devretme işlemi sırasında yük devretme işi tamamlandığında tüm birimleri yük devretme kümesindeki alınan anında geri ve hedef cihazda çevrimiçi olarak işaretlenir. Bu, yük Devredilmiş herhangi bir yerel olarak sabitlenmiş birimler içerir; tüm veri birimi için indirilen ancak yerel GARANTİLERİN verilerin yalnızca kullanılabilir. Yük devretme işleminin bir parçası oluşturulan ilgili geri yükleme işleri izleme tarafından devralınırsa başarısız olan her yerel olarak sabitlenmiş bir birim için bu ilerlemeyi izleyebilirsiniz. Bu tek tek geri yükleme işleri, yalnızca yerel olarak sabitlenmiş birimler için oluşturulur.

**S.** Yük devretme sırasında bir birim türünü değiştirebilir miyim?

**C.** Hayır, bir yük devretme sırasında birim türünü değiştiremezsiniz. Başarısız olan üzerinden StorSimple 8000 çalışmakta olan başka bir fiziksel cihaz serisi güncelleştirme 3, birimler anlık görüntüde depolanan birim türünü temel yük devredildi.

**S.** Bir birim kapsayıcısı ile bulut Gereci için yerel olarak sabitlenmiş birimler üzerinde başarısız olabilir?

**C.** Evet, uygulayabilirsiniz. Yerel olarak sabitlenmiş birimler üzerinde katmanlı birimler olarak getirilir. Daha fazla bilgi [yük devretme ve DR, yerel olarak sabitlenmiş birimler sürümler arasında](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover)

