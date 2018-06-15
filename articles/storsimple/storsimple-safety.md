---
title: StorSimple cihazınız için güvenliği | Microsoft Docs
description: Güvenlik kuralları, kılavuzları ve konuları açıklar ve güvenli bir şekilde yüklemek ve StorSimple Cihazınızı çalıştırmak açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: dae6d535-1ca2-4d2b-b221-6819043aa068
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: alkohli
ms.openlocfilehash: a178e8880bcbcada9d66eaacf5ccbdb7c55957cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875868"
---
# <a name="safely-install-and-operate-your-storsimple-device"></a>Güvenli bir şekilde yüklemek ve StorSimple Cihazınızı çalıştırmak
![Uyarı simgesi](./media/storsimple-safety/IC740879.png)
![güvenliği bildirim simgesi okuma](./media/storsimple-safety/IC740885.png) **okuma güvenlik ve sistem durumu bilgileri**

Microsoft Azure StorSimple Cihazınızı geçerlidir bu makaledeki tüm güvenlik ve sistem durumu bilgileri okuyun. StorSimple Cihazınızı gelecekte başvurmak için ile birlikte gelen tüm yazdırılan kılavuzları tutun. Yönergeleri izleyin ve düzgün bir şekilde ayarlamak, kullanın ve bu ürünün önemli yaralanma veya ölüm riskini artırabilir veya cihaz veya cihazlara zarar için önemli hatası. A [bu kılavuzun indirilebilir sürümünü](http://www.microsoft.com/download/details.aspx?id=44233) da kullanılabilir.

## <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
İşte, ne zaman ayarlama ve Microsoft Azure StorSimple Cihazınızı çalıştırırken gözlenecek güvenlik önlemlerini gözden bulacaksınız simgeleri.

| Simgesi | Açıklama |
|:--- |:--- |
| ![Tehlike simgesi](./media/storsimple-safety/IC740879.png) **tehlike!** |Kaçınılması değil, ölüm veya ciddi yaralanma neden olur, zararlı bir durumu gösterir. Bu sinyal Word'ün en aşırı durumlarla sınırlı olacak. |
| ![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **uyarı!** |Kaçınılması değil, ölüm veya ciddi yaralanma neden olabilir, zararlı bir durumu gösterir. |
| ![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **dikkat!** |Kaçınılması değil, küçük veya Orta yaralanma neden olabilir, zararlı bir durumu gösterir. |
| ![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:** |Önemli ancak hasar ilgili olmayan kabul bilgi gösterir. |
| ![Elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **elektrik Şoku hasar** |Yüksek voltaj |
| ![Büyük ağırlık simgesi](./media/storsimple-safety/IC740883.png) **büyük ağırlık** | |
| ![Hiçbir kullanıcı tarafından bakımı yapılabilen parça simgesi](./media/storsimple-safety/IC740879.png) **hiçbir kullanıcı tarafından bakımı yapılabilen parça** |Düzgün şekilde eğitim almış sürece erişmez. |
| ![Güvenlik Uyarısı simgesi okuma](./media/storsimple-safety/IC740885.png)**önce tüm yönergelerini okuyun** | |
| ![İpucu tehlike simgesi](./media/storsimple-safety/IC740886.png) **ipucu tehlike** | |

## <a name="handling-precautions"></a>Önlemleri işleme
![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![büyük ağırlık simgesi](./media/storsimple-safety/IC740883.png) **uyarı!** 

Yaralanma riskini azaltmak için:

* Tam olarak yapılandırılmış bir muhafaza 32 kg (70 lb); tartmanız kendi kendinize yükseltmek çalışmayın.
* Muhafaza geçmeden önce her zaman iki kişinin ağırlık işlemek kullanılabilir olduğundan emin olun. Bu ağırlık Yükselt çalışılırken, bir kişinin injuries karşılayabilir unutmayın.
* Kasa, güç ve soğutma modülleri (PCMs) birim arkada bulunan işleyicilerinin tarafından kaldırın değil. Bunlar ağırlık olabilmesi için tasarlanmamıştır.

## <a name="connection-precautions"></a>Bağlantı uyarıları
![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Yaralanma, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Birden çok AC kaynaklar tarafından gücü kullanıldığında tam yalıtımı için tüm tedarik güç bağlantısını kesin.
* Kalıcı olarak birim taşımadan önce ya da herhangi bir yolla zarar görmüş olması düşünüyorsanız çıkarın.
* Güç kaynağı kablosu güvenli elektrik earth bağlantı sağlar. Güç uygulamadan önce muhafaza grounding Ulusal ve yerel gereksinimlerini karşıladığını doğrulayın.
* Muhafaza PCM'den kaldırılmasını önce güç bağlantısı her zaman kesildiğinden emin olun.
* Güç kaynağı kablosu üzerinde Tak olması koşuluyla, ana aygıt kesin, yuva çıkışlar ekipman bulunduğundan emin olun ve kolayca erişilebilir.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Elektrik bağlantılarından yangın veya aşırı olasılığını azaltmak için:

* Uygun güç kaynağı teknik belirtiminde ayrıntılı gereksinimlerini karşılamak için elektrik aşırı koruma sağlar.
* Bölündü güç kablosu ("Y" Müşteri adayları) kullanmayın.
* Geçerli güvenlik, çıkması ve sıcaklık gereksinimlerine uymak için hiçbir kapsar kaldırılmalı ve tüm yuvaları eklenti modülleri ile doldurulması gerekir veya boşlukları sürücü.
* Ekipman üreticisi tarafından belirtilen şekilde kullanıldığından emin olun. Bu donanım üreticisi tarafından belirtilmemiş bir şekilde kullanılırsa, donanım tarafından sağlanan koruma kesintiye uğrayabilir.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Düzgün çalışması donanımınızı ve ürün zarar önlemek için:

* Cihaz arkasındaki RJ45 bağlantı yalnızca bir Ethernet bağlantı noktalarıdır. Bunlar telekomünikasyon ağa bağlı gerekir.
* Cihaz ön arka soğutma tasarım uyum sağlayabilecek bir rafa yüklediğinizden emin olun.
* Tüm eklenti modülleri ve boş kalıplarını Sistem Kasası parçasıdır. Yenisini hemen eklenebilir, bunlar yalnızca kaldırılmalıdır. Sistem, tüm modülleri veya yerinde boşlukları olmadan çalıştırılmamalıdır.

## <a name="rack-system-precautions"></a>Raf sistem önlemler
Dolap raf cihazı bağladığınızda aşağıdaki güvenliği gereksinimleri dikkate alınmalıdır.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![ipucu tehlike simgesi](./media/storsimple-safety/IC740886.png) **uyarı!**

Üzerinden bir ipucu gelen yaralanma olasılığını azaltmak için:

* Raf tasarım yüklü kasaları toplam ağırlığını desteklemelidir ve raf tipping üzerinden normal kullanım veya yükleme sırasında itildiği veya engellemek uygun Dengeleme özellikleri eklemeniz gerekir.
* Bir raf yüklenirken, raf Alttan dolabilir ve üstten aşağı boş.
* Raf dışında birden fazla muhafaza toppling raf tehlike önlemek için bir zaman slayt değil.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Yaralanma, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Raf güvenli elektrik dağıtım sistemi olması gerekir. Kasa için atlayarak geçerli koruma sağlamanız gerekir ve yüklü kasaları toplam sayısına göre aşırı değil gerekir. Ad plakası üzerinde gösterilen elektrik tüketim derecelendirme uyulması.
* Elektrik dağıtım sistemi dolapta her kasa için güvenilir bir yerdeki sağlamanız gerekir.
* Elektrik dağıtım sistemi tasarımını dikkate toplam başından başlayarak sızıntısını tüm mahfazalarında tüm güç kaynakları gelen geçerli almanız gerekir. Her güç kaynağı her muhafazada başından başlayarak sızıntısını geçerli 1.0 mA maksimum 60 Hz 264 Volt olduğuna dikkat edin. Raf "yüksek SIZINTISINI geçerli. etiketleme gerektirebilir Plan (dünya) bağlantı bir tedarik bağlanmadan önce gereklidir."
* Kasa ile yapılandırıldığında raf UL 60950-1 IEC 60950-1/tr 60950-1 ve güvenlik gereksinimleri karşılaması gerekir.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Uygun raf sisteminizi soğutma için:

* Raf tasarım 35 derece derece (95 Fahrenhayt derece) ortam sıcaklığı maksimum muhafaza dikkate alır emin olun.
* Sistem low-pressure, arka Egzoz yükleme (geri baskısı raf kapıları ve engellerini geçmemesi 5 Pascal [0,5 mm su ölçer] tarafından oluşturulan) ile çalıştırılır.

## <a name="power-cooling-module-pcm-precautions"></a>Güç soğutma Modülü (PCM) önlemler
Aygıt ile iki PCMs çalışmak üzere tasarlanmıştır. Her PCMs güç kaynağı ve çift eksen fan vardır. Kritik bir koşul sırasında sistem hatası nedeniyle normal işlemleri devam ederken bir güç kaynağı sağlar. İki PCMs (ve bu nedenle güç kaynakları) her zaman yüklü olması gerekir. Tek bir PCM yedek güç sağlamaz. Bu nedenle, tek PCM başarısızlığını kesinti süresi veya olası veri kaybına neden olabilir.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Yaralanma, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Kapak PCM'den kaldırmayın. Elektrik Şoku içinde bir tehlike yoktur. PCM dönmek ve bir yedek almak için [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Düzgün çalışması donanımınızı ve ürün zarar önlemek için:

* 24 saat içinde başarısız PCM değiştirmeniz gerekir. Bir PCM yerini kaldırıldıktan sonra değiştirme kaldırıldıktan sonra 10 dakika içinde tamamlanması gerekir.
* Yenisini hemen yüklenebilir sürece bir PCM kaldırmayın. Muhafaza yerinde tüm modüllerin olmadan işletilen gereken değil.

## <a name="electrostatic-discharge-esd-precautions"></a>Elektrostatik deşarj (ESD) önlemler
![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

ESD ile ilgili aşağıdaki uyarıları inceleyin.

* Yüklü ve uygun bir antistatic bileğe kadar veya ankle strap denetlenebilir emin olun.
* Tüm geleneksel ESD önlemleri modülleri ve bileşenleri işlerken gözlemleyin.
* Devre kartına bileşenleri ve modül bağlayıcıları kişiyle kaçının.
* ESD zarar tarafından garanti kapsamında değildir.

## <a name="battery-disposal-precautions"></a>Pil elden önlemler
Güç kaynağı özel pil geçici, kısa vadeli güç kesintileri sırasında bellek içeriğini korumak için kullanır. Pil PCM yerleştirildiğinden. Pil hakkında aşağıdaki bilgileri unutmayın.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **uyarı!**

Kısayoluyla, yangın, açılımı, yaralanma veya ölüm riskini azaltmak için:

* Ulusal/bölge yönetmeliklere uygun olarak kullanılan piller elden.
* Değil ayır, crush, 60 derece derece (140 Fahrenhayt derece) ısı veya incinerate. PCM pil yalnızca sağlanan pil ile değiştirin. Başka bir pil kullanımını yangın veya açılımı riskini sayılabilir.
* Bu güç kaynağı ' kaldırdıysanız koruyucu uç başlıkları pille kullanın.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Ne zaman sevkiyat veya piller uçakla, aksi takdirde taşıma izleyin adresinde IATA Lityum pil Kılavuzu belge [http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx)

Bu güvenlik uyarıları gözden geçirdikten sonra sonraki adımlar kutusundan çıkarma, rafa ve Cihazınızı kablo üzeresiniz.

## <a name="next-steps"></a>Sonraki adımlar
* 8100 cihazınız için Git [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md).
* Bir 8600 aygıt için Git [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).

