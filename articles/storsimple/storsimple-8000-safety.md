---
title: StorSimple cihazınız için güvenliği | Microsoft Docs
description: Güvenlik kuralları, Kılavuzlar ve önemli noktalar ve güvenli bir şekilde yüklemek ve StorSimple Cihazınızı nasıl açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 66b881ab13e27ee457af4fa1bafb82ad14e9674d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60631692"
---
# <a name="safely-install-and-operate-your-storsimple-device"></a>Güvenli bir şekilde yüklemek ve StorSimple Cihazınızı çalışır
![Uyarı simgesi](./media/storsimple-safety/IC740879.png)
![güvenliği bildirim simgesi okuma](./media/storsimple-safety/IC740885.png) **güvenliği ve sistem durumu bilgileri OKUYABİLİRSİNİZ**

Microsoft Azure StorSimple cihazınıza uygulanır, bu makaledeki tüm güvenlik ve sistem durumu bilgilerini okuyun. Gelecek başvurular için StorSimple Cihazınızı ile birlikte gelen yazdırılan kılavuzları tutun. Yönergeleri izleyin ve düzgün bir şekilde ayarlamak ve bu ürün ciddi veya ölüm riskini artırabilir veya cihaz veya cihazlara zarar care hatası. A [bu kılavuzun indirilebilir sürümü](https://www.microsoft.com/download/details.aspx?id=44233) de kullanılabilir.

## <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Ne zaman ayarlama ve Microsoft Azure StorSimple Cihazınızı çalıştırırken gözlenecek güvenlik önlemlerini gözden bulabilirsiniz simgeler şunlardır.

| Simge | Açıklama |
|:--- |:--- |
| ![Tehlike simgesi](./media/storsimple-safety/IC740879.png) **tehlike!** |Önlenmiş değil, ayrıca ölüm ya da ciddi neden olur, zararlı bir durumu gösterir. Bu sinyal Word'ün en olağanüstü durumlar için sınırlı olacak. |
| ![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **uyarı!** |Önlenmiş değil, ayrıca ölüm ya da ciddi neden olabilir, zararlı bir durumu gösterir. |
| ![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **dikkatli olun!** |Önlenmiş değil, küçük veya Orta kasıt neden olabilir, zararlı bir durumu gösterir. |
| ![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:** |Önemli ancak tehlike ilgili değil olarak kabul bilgileri gösterir. |
| ![Elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **elektrik Şoku tehlike** |Yüksek voltajı |
| ![Büyük ağırlık simgesi](./media/storsimple-safety/IC740883.png) **büyük ağırlık** | |
| ![Hiçbir kullanıcı tarafından bakımı yapılabilen parça simgesi](./media/storsimple-safety/IC740879.png) **hiçbir kullanıcı tarafından bakımı yapılabilen parça** |Düzgün şekilde eğitim almış sürece erişim sağlanır. |
| ![Güvenlik Uyarısı simgesi okuma](./media/storsimple-safety/IC740885.png)**önce tüm yönergelerini okuyun** | |
| ![İpucu tehlike simgesi](./media/storsimple-safety/IC740886.png) **ipucu tehlike** | |

## <a name="handling-precautions"></a>Önlemler işleme
![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![büyük ağırlık simgesi](./media/storsimple-safety/IC740883.png) **uyarı!** 

Kasıt riskini azaltmak için:

* En fazla 32 kg (70'ten lb); tam olarak yapılandırılmış bir kutu Tart kendiniz lift denemeyin.
* Kasa geçmeden önce her zaman iki kişinin ağırlığı işlemek kullanılabilir olduğundan emin olun. Bu ağırlık lift çalışılıyor, bir kişinin injuries karşılayabilir dikkat edin.
* Kutu, birim arkada bulunan tutamaçları güç ve soğutma modülleri (PCMs) tarafından lift değil. Bunlar ağırlığı yararlanmak için tasarlanmamıştır.

## <a name="connection-precautions"></a>Bağlantı uyarıları
![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Kasıt, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Birden çok AC kaynak tarafından desteklenen, tam yalıtım tüm tedarik güç bağlantısını kesin.
* Kalıcı olarak birim taşımadan önce ya da herhangi bir yolla zarar görmüş olması düşünüyorsanız çıkarın.
* Güç kaynağı kablosu güvenli elektrik earth bağlantı sağlar. Güç uygulamadan önce grounding muhafaza, Ulusal ve yerel gereksinimlerini karşıladığını doğrulayın.
* Bir PCM kutudan kaldırma işleminden önceki güç bağlantısı her zaman kesildiğinden emin olun.
* Güç kaynağı kablo üzerinde Tak olması koşuluyla, ana cihazın bağlantısını kesmeden, yuva çıkışlar ekipman bulunduğundan emin olun ve kolayca erişilebilir durumda olur.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Elektrik bağlantılarından yangın veya elektriği olasılığını azaltmak için:

* Uygun bir güç kaynağına teknik belirtiminde detayları verilen gereksinimleri karşılamak için elektrik aşırı koruma sağlar.
* Bölündü güç kablosu ("Y" Müşteri adayları) kullanmayın.
* Geçerli güvenlik, arabellek ve sıcaklık gereksinimleri ile uyum sağlamak için hiçbir kapsar kaldırılmalıdır ve tüm yuvaları eklenti modülleri ile doldurulmalıdır veya boşlukları sürücü.
* Donanım üreticisi tarafından belirtilen şekilde kullanıldığından emin olun. Bu donanım üreticisi tarafından belirtilmemiş bir şekilde kullanılıyorsa, donanım tarafından sağlanan korumanın kesintiye uğrayabilir.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Toplulukların donanımınızı ve ürün hasarı önlemek için:

* Cihazın arkasına RJ45 bağlantı, yalnızca bir Ethernet bağlantı noktalarıdır. Bu bir telekomünikasyon ağa bağlı olmalıdır.
* Cihazın önden arkaya soğutma tasarım barındırabilecek bir rafa yüklediğinizden emin olun.
* Sistem kutusu, tüm eklenti modülleri ve boş kalıplarını parçasıdır. Hemen bir yedek eklenebilir, bunlar yalnızca kaldırılmalıdır. Sistem tüm modüller veya yerinde boşluk olmadan çalıştırılmalıdır değil.

## <a name="rack-system-precautions"></a>Raf sistem uyarıları
Dolap raf cihazı bağladığınızda, aşağıdaki güvenlik gereksinimleri dikkate alınmalıdır.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![ipucu tehlike simgesi](./media/storsimple-safety/IC740886.png) **uyarı!**

Üzerinden bir ipucu gelen kasıt olasılığını azaltmak için:

* Raf tasarım yüklü eklerin toplam ağırlığı desteklemesi ve sabitleniyor tipping veya yükleme veya normal kullanım sırasında üzerinden sonuna gönderilen rafa engellemek uygun özellikleri eklemeniz gerekir.
* Bir raf üstü yüklenirken, alt raf doldurun ve yukarıdan aşağı boş.
* Rafa dışında birden fazla muhafaza toppling raf tehlike önlemek için bir zaman slayt değil.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Kasıt, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Raf üstü bir elektrik güvenli dağıtım sistemi olması gerekir. Bu kasa için aşırı geçerli koruma sağlamanız gerekir ve yüklü eklerin toplam sayısına göre aşırı gerekir. Ad plakası üzerinde gösterilen elektrik tüketim derecelendirme gözlenmelidir.
* Elektrik dağıtım sistemi raftaki her kasa için güvenilir bir zemin sağlamanız gerekir.
* Elektrik dağıtım sistemin tasarımını toplam çığır sızıntı geçerli tüm ek olarak tüm güç kaynakları'ndan bulundurmalıdır. Her kutu içinde her güç kaynağı 60 Hz bir zemin sızıntı geçerli 1.0 mA maksimum 264 Volt olduğuna dikkat edin. "Yüksek SIZINTI geçerli. etiketleme rafa gerektirebilir Toprak (dünya) bağlantısını bir tedarik bağlanmadan önce gereklidir."
* Raf, kasa ile yapılandırıldığında UL 60950-1 ve IEC 60950-1/tr 60950 1 güvenlik gereksinimlerini karşılaması gerekir.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Uygun raf sisteminizi soğutma için:

* Raf tasarım 35 Santigrat (95 Fahrenhayt) ortam sıcaklığı maksimum muhafaza dikkate alır emin olun.
* Sistem low-pressure, arka Egzozu yükleme (raf kapılar ve 5 Pascal [0,5 mm su ölçer] aşmayacak şekilde engellerini tarafından oluşturulan ters baskı denen) ile çalıştırılır.

## <a name="power-cooling-module-pcm-precautions"></a>Güç soğutma Modülü (PCM) önlemler
Cihaz, iki PCMs ile çalışmak üzere tasarlanmıştır. Güç kaynağı ve bir çift eksen fanı PCMs vardır. Kritik bir koşul sırasında sistem hatası normal işlemler devam ederken bir güç kaynağı sağlar. İki PCMs (ve bu nedenle güç kaynakları) her zaman yüklü olması gerekir. Tek bir PCM yedek güç sağlamaz. Bu nedenle, tek PCM hatasını, kapalı kalma süresi veya olası veri kaybına neden olabilir.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) ![elektrik Şoku simgesi](./media/storsimple-safety/IC740882.png) **uyarı!**

Kasıt, elektrik Şoku veya ölüm olasılığını azaltmak için:

* Bu işlem arka planda PCM kaldırmayın. Elektrik Şoku içinde bir olma tehlikesi yoktur. PCM döndürür ve bir yedek almak için [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Toplulukların donanımınızı ve ürün hasarı önlemek için:

* 24 saat içinde PCM'yi değiştirmeniz gerekir. Bir PCM'yi değiştirme kaldırıldıktan sonra değiştirme, kaldırma sonraki 10 dakika içinde tamamlanmalıdır.
* Hemen bir yedek yüklenebilir sürece bir PCM kaldırmayın. Kasa yerde tüm modüller olmadan çalıştırılması gerekir değil.

## <a name="electrostatic-discharge-esd-precautions"></a>Elektrostatik taburcu (ESD) önlemler
![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

ESD ile ilgili aşağıdaki uyarıları inceleyin.

* Yüklenmiş ve kullanıma uygun bir antistatic bilek veya bileklikleri strap emin olun.
* Tüm geleneksel ESD önlemler modüller ve bileşenleri işlerken gözlemleyin.
* Devre kartına bileşenleri ve modül bağlayıcılar ile ilgili kişi kaçının.
* ESD zarar garanti tarafından kapsanmıyor.

## <a name="battery-disposal-precautions"></a>Pil elden önlemler
Güç kaynağı, geçici, kısa vadeli güç kesintileri sırasında bellek içeriğini korumak için özel bir pil kullanır. Pil PCM yerleştirildiğinden. Pil hakkında aşağıdaki bilgileri unutmayın.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png) **uyarı!**

Şortu, Ateş, açılımı, yaralanma veya ölüm riskini azaltmak için:

* Kullanılan pil Ulusal/bölge düzenlemelerine uygun şekilde elden.
* Değil ayır, paramparça, 60 Santigrat derece (140 Fahrenhayt) ısı veya incinerate. Pil PCM yalnızca sağlanan pil ile değiştirin. Başka bir pil kullanımını yangın veya açılımı riski sunabilir.
* Bu güç kaldırılırsa koruyucu uç başlıkları pille kullanın.

![Fark simgesi](./media/storsimple-safety/IC740881.png) **dikkat edin:**

Sevkiyat ya da aksi takdirde pil uçakla taşıma adresinde IATA Lithium pil rehberi belgesi izleyin [https://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](https://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx)

Bu güvenlik uyarıları gözden geçirdikten sonra sonraki adımlara Cihazınızı kutusundan çıkarma, rafa ve cihazınızın kablolarını bağlama üzeresiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Bir 8100 cihazınız için Git [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md).
* Bir 8600 model Cihazınızı için Git [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).

