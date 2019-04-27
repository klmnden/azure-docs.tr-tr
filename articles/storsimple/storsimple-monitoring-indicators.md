---
title: StorSimple göstergeleri izleme | Microsoft Docs
description: StorSimple cihaz durumunu izlemek için kullanılan sesli uyarılar ve ışık – emitting diyotlar (LED'lerini) açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: ef8acf1c3c9211168ebacc8d62647f6789c745a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630657"
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>İzleme gösterge StorSimple Cihazınızı yönetmek için kullanın


## <a name="overview"></a>Genel Bakış
StorSimple Cihazınızı ışık – emitting diyotlar (LED'lerini) içerir ve alarmlar modülleri ve StorSimple cihazı genel durumunu izlemek için kullanabilirsiniz. İzleme gösterge aygıtın birincil muhafaza ve EBOD muhafazası donanım bileşenleri üzerinde bulunabilir. İzleme gösterge LED'lerini veya sesli uyarılar olabilir.

Bir modül durumunu göstermek için kullanılan üç LED durum vardır: yeşil, yeşil kırmızı amber ya da red amber yanıp.  

* Yeşil LED'lerini bir sağlıklı çalışma durumunu temsil eder.  
* Kırmızı amber LED'lerini göstermek için yeşil, kullanıcı müdahalesi gerektirebilir kritik olmayan koşullar varlığını yanıp.  
* Kırmızı amber LED'lerini modülü içinde mevcut bir kritik hata olduğunu gösterir.  

Bu makalenin geri kalanında, çeşitli izleme gösterge LED'lerini, StorSimple cihazında LED durumları ve ilişkili tüm sesli uyarılar göre cihaz durumu konumlarını açıklar.

## <a name="front-panel-indicator-leds"></a>Ön panelini gösterge LED'lerini
Olarak da bilinen ön panel *işlemleri paneli* veya *ops panelindeki*, sistemdeki tüm modülleri toplama durumunu görüntüler. Ön panelini StorSimple birincil ve EBOD muhafazası özdeştir ve aşağıda gösterilmiştir.  

   ![Cihaz Panosu][1]

Ön panelini aşağıdaki göstergelerden içerir:  

1. Sesi kapa düğmesi
2. Güç göstergesi LED (yeşil/kırmızı-amber)
3. Modülü hata göstergesi (kırmızı-amber/OFF üzerinde) YÖNLENDİRDİ
4. Mantıksal bir hata göstergesi (kırmızı-amber/OFF üzerinde YÖNLENDİRDİ
5. Birim kimliği görüntüleme  

Cihazın ön panelini LED'lerini hem de EBOD muhafazası arasındaki başlıca fark **sistem birimi kimlik numarası** LED ekranda gösterilen. Cihazda görüntülenen kimliği varsayılan birim **00**EBOD muhafazası üzerinde görüntülenen varsayılan birim kimlik bilgileriyse **01**. Bu cihaz açıldığında cihaz EBOD muhafazası arasında hızlı bir şekilde ayırt etmenize olanak sağlar. Cihazınızı devre dışı bırakılırsa, sağlanan bilgileri kullanmak [yeni bir cihazı açın](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) cihaz EBOD muhafazası ayırt etmek için.  

## <a name="front-panel-led-status"></a>Ön panelini LED durumu
Cihaz veya EBOD muhafazası için ön panelini üzerinde LED'lerini tarafından belirtilen durum tanımlamak için aşağıdaki tabloyu kullanın.  

| Sistem güç | Modül hatası | Mantıksal hata | Alarm | Durum |
| --- | --- | --- | --- | --- |
| Kırmızı-amber |KAPALI |KAPALI |Yok |Kayıp AC gücü yedek güç veya AC gücü üzerinde çalışan ve denetleyici modülleri kaldırıldı. |
| Yeşil |AÇIK |AÇIK |Yok |OPS paneli gücüyle 5 (sn) test durumu |
| Yeşil |KAPALI |KAPALI |Yok |Tüm İşlevler iyi Aç |
| Yeşil |AÇIK |Yok |PCM hata LED'lerini, fan hata LED'leri |PCM hataları üzerinde veya altında sıcaklık fanı hata |
| Yeşil |AÇIK |Yok |G/ç modülü LED'leri |Herhangi bir Denetleyici Modülü hatası |
| Yeşil |AÇIK |Yok |Yok |Kasa mantık hatası |
| Yeşil |Flash |Yok |Denetleyici Modülü, Modül durumu GEREKTİRİYORDU. PCM hata LED'lerini, fan hata LED'leri |Bilinmeyen Denetleyici Modülü türü yüklü, I2C bus hatası, Denetleyici Modülü önemli ürün verilerini (VPD) yapılandırma hatası |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Modül (PCM) gösterge LED'lerini soğutma güç
Güç soğutma Modülü (PCM) gösterge LED'lerini PCM modüllerin birincil ve EBOD muhafazası arkasında bulunabilir. Bu konu aşağıdaki LED'lerini StorSimple Cihazınızı durumunu izlemek için nasıl kullanılacağını açıklar.  

* Birincil kasası için PCM LED'leri
* EBOD muhafazası için PCM LED'leri

## <a name="pcm-leds-for-the-primary-enclosure"></a>Birincil kasası için PCM LED'leri
StorSimple cihazı 764W PCM modülüyle bir ek pil sahiptir. Cihaz için LED paneli aşağıda gösterilmiştir.  

   ![Birincil kasada PCM LED'leri][2]

LED Gösterge:

1. AC güç kesintisi
2. Fanı hatalı
3. Pil hata
4. PCM TAMAM
5. DC hatası
6. Pil iyi  

PCM durumunu LED panelinde gösterilir. Cihaz PCM LED paneli altı LED'lerini sahiptir. Dört bu LED'lerini güç kaynağı ve giriş durumunu görüntüler. Kalan iki LED'lerini yedek pil PCM modül durumunu gösterir. Aşağıdaki tablolarda PCM durumunu belirlemek için kullanabilirsiniz.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM gösterge LED'lerini güç kaynağı ve fanı
| Durum | PCM hazır (yeşil) | AC başarısız (amber) | Fan başarısız (amber) | DC başarısız (amber) |
| --- | --- | --- | --- | --- |
| AC güç (için kasa) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC güç (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC sunmak PCM ON - Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fanı başarısız) |KAPALI |KAPALI |AÇIK |Yok |
| PCM hata (üzerinden voltaj, geçerli üzerinden üzerinden amp) |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fanı tolerans dışı) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modu |Yanıp |KAPALI |KAPALI |KAPALI |
| PCM üretici yazılımı indirme |KAPALI |Yanıp |Yanıp |Yanıp |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Yedek pil PCM gösterge LED'lerini
| Durum | Pil iyi (yeşil) | Pil hata (amber) |
| --- | --- | --- |
| Pil yok |KAPALI |KAPALI |
| Pil var ve ücretlendirilir |AÇIK |KAPALI |
| Pil ücretlendirmeye veya bakım taburcu |Yanıp |KAPALI |
| Pil "soft" hata (kurtarılabilir) |KAPALI |Yanıp |
| Pil "sabit" hata (kurtarılamaz) |KAPALI |AÇIK |
| Pil disarmed |Yanıp |KAPALI |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>EBOD muhafazası için PCM LED'leri
EBOD muhafazası 580W PCM ve hiçbir ek pil sahiptir. EBOD muhafazası PCM paneli gösterge LED'lerini güç kaynakları ve giriş için vardır. Aşağıdaki çizim, bu LED'lerini gösterir.

   ![EBOD kasada PCM LED'leri][3] 

PCM durumunu belirlemek için aşağıdaki tabloyu kullanabilirsiniz.  

| Durum | PCM hazır (yeşil) | AC başarısız (amber) | Fan başarısız (amber) | DC başarısız (amber) |
| --- | --- | --- | --- | --- |
| AC güç (için kasa) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC güç (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC sunmak PCM ON – Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fanı başarısız) |KAPALI |KAPALI |AÇIK |X |
| (Üzerinde amp, voltaj, geçerli üzerinden üzerinden PCM hata |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fanı tolerans dışı) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modeli |Yanıp |KAPALI |KAPALI |KAPALI |
| PCM üretici yazılımı indirme |KAPALI |Yanıp |Yanıp |Yanıp |

## <a name="controller-module-indicator-leds"></a>Denetleyici Modülü gösterge LED'lerini
StorSimple cihazı LED'lerini birincil ve EBOD denetleyicisi modüller içerir.   

### <a name="monitoring-leds-for-the-primary-controller"></a>İzleme LED'lerini birincil denetleyici için
Aşağıdaki çizim birincil denetleyicisinde LED'lerini belirlemenize yardımcı olur. (Tüm bileşenlerin yönde yardımcı olmak için listelenir.)  

   ![İzleme LED'lerini - birincil denetleyici][4]

Denetleyici Modülü doğru şekilde çalışıp çalışmadığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="controller-indicator-leds"></a>Denetleyici gösterge LED'lerini
| LED | Açıklama |
| --- | --- |
| Kimliği LED (mavi) |Modül tanımlanmakta olduğunu gösterir. Mavi LED çalışan denetleyicisinde yanıp sönen, etkin denetleyiciyi denetleyicisidir ve diğeri yedek denetleyicisidir. Daha fazla bilgi için [Cihazınızda etkin denetleyiciyi belirleme](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Hata LED (amber) |Denetleyici bir hata gösterir. |
| Tamam LED (yeşil) |Sürekli yeşil denetleyici Tamam olduğunu gösterir. Yeşil yanıp bir denetleyici VPD yapılandırma hatası gösterir. |
| SAS etkinlik LED'lerini (yeşil) |Sürekli yeşil hiçbir geçerli etkinliği ile bir bağlantı gösterir. Yeşil yanıp, sürmekte olan etkinliği bağlantı olduğunu gösterir. |
| Ethernet durumu LED'leri |Sağ tarafta bağlantı/ağ etkinliğini gösterir: (sürekli yeşil) bağlantı (yeşil yanıp) etkin ağ etkinliği. Sol tarafta, ağ hızını gösterir: (sarı) 1000 Mb/sn (yeşil) 100 Mb/sn ve (Kapalı) 10 Mb/sn. Bileşen modeline bağlı olarak, ağ arabiriminin etkin olsa bile bu ışık yanıp. |
| POST LED'leri |Denetleyici açıldığında önyükleme ilerleme durumunu gösterir. StorSimple cihaz önyükleme yapamıyorsa, bu LED Microsoft önyükleme işleminde hata oluştuğu noktanın Support yardımcı olur. |

> [!IMPORTANT]
> ' % S'hata LED aydınlatma, denetleyici yeniden başlatılarak çözülemeyecek Denetleyici Modülü ile ilgili bir sorun yoktur. Denetleyiciyi yeniden başlatmak bu sorunu çözmezse Lütfen Microsoft Support başvurun.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>İzleme LED'lerini EBOD (EBOD muhafazası) için
Aşağıdaki çizimde gösterildiği gibi durumunu gösteren LED'lerini 6 Gb/sn SAS EBOD denetleyicisi vardır.  

  ![İzleme LED'lerini - EBOD muhafazası][5]

EBOD denetleyicisi modül normalde çalışıp çalışmadığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD denetleyicisi modülü gösterge LED'lerini
| Durum | G/ç modülü hazır (yeşil) | G/ç modülü hata (amber) | Ana bilgisayar bağlantı noktası etkinlik (yeşil) |
| --- | --- | --- | --- |
| Denetleyici Modülü Tamam |AÇIK |KAPALI |- |
| Denetleyici Modülü hatası |KAPALI |AÇIK |- |
| Dış konak bağlantı |- |- |KAPALI |
| Dış konak bağlantı – etkinliği yok |- |- |AÇIK |
| Dış konak bağlantı - etkinlik |- |- |Yanıp |
| Denetleyici Modülü meta veri hatası |Yanıp |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Birincil ve EBOD muhafazası disk sürücüsü gösterge LED'lerini
StorSimple cihazı muhafaza birincil ve EBOD muhafazası bulunan sürücü yok. Bu bölümde açıklandığı gibi her disk sürücüsünü izleme gösterge LED'lerini içerir. 

Disk sürücüleri için sürücü durumu yeşil gösterilir LED ve kırmızı amber ışığı her sürücü taşıyıcı modülü ön bağlanır. Aşağıdaki çizim, bu LED'lerini gösterir.

  ![Disk sürücüsü LED'leri][6]

Genel ön panelini LED durumu sırayla etkileyen her disk sürücüsü, durumu belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>EBOD muhafazası için disk sürücüsü gösterge LED'lerini
| Durum | Etkinlik Tamam LED (yeşil) | Hata LED (kırmızı amber) | OPS panelindeki LED ilişkili |
| --- | --- | --- | --- |
| Yüklü sürücü yok |KAPALI |KAPALI |None |
| Sürücü yüklü ve çalışır durumda |Etkinlik ile yanıp Aç/Kapat |X |None |
| SCSI kasa Hizmetleri'nden (SES) cihaz kimliği ayarlama |AÇIK |1 saniye üzerinde/1 ikinci kapalı yanıp |None |
| SES cihazı hata bit kümesi |AÇIK |AÇIK |Mantıksal hata (kırmızı) |
| Güç denetimi bağlantı hattı hatası |KAPALI |AÇIK |Modül hatası (kırmızı) |

## <a name="audible-alarms"></a>Sesli uyarılar
StorSimple cihaz muhafaza birincil ve EBOD muhafazası ilişkili sesli uyarılar içerir. Sesli bir uyarı, her iki ek ön Panoda (ops panelindeki olarak da bilinir) bulunur. Sesli uyarı, bir hata koşulu olduğunda bulunmadığını gösterir. Aşağıdaki koşullar uyarı etkinleştirir:  

* Fan arıza ya da hata
* Voltaj je mimo rozsah
* Üzerinden veya sıcaklık koşul altında
* Sıcaklık taşması
* Sistem hatası
* Mantıksal hata
* Güç kaynağı hatası
* Bir güç, soğutma Modülü (PCM) kaldırma  

Aşağıdaki tabloda, çeşitli uyarı durumları açıklar.  

### <a name="alarm-states"></a>Uyarı durumları
| Uyarı durumu | Eylem | Sesi kapa düğmeye basıldığını eylemi |
| --- | --- | --- |
| S0 |Normal mod: sessiz |İki kez bip sesi |
| S1 |Hata modu: 1/1 saniye ikinci kapalı |S2 veya S3 geçiş (bkz. Notlar) |
| S2 |Modu anımsatmak: aralıklı bip sesi |None |
| S3 |Yumuşak modu: sessiz |None |
| S4 |Kritik hata modu: sürekli Uyarısı |Kullanılabilir değil: sessiz etkin değil |

> [!NOTE]
> * 2 dakika içinde sessiz basmayın durumunda uyarı durumunda S1 durumu otomatik olarak S2 veya S3 geçer.  
> * Hata koşulu temizlendikten sonra uyarı durumlarını S1 S4 için S0 döndürür.  
> * Kritik hata durumu S4 başka bir durumdan girilebilir.  


Sesli uyarı ops panelinde sesini kapat düğmesine basarak sesi kapatmak. Sesi kapa anahtar olmayan el ile çalıştırılır otomatik ses kapatma iki dakika sonra meydana gelir. Uyarı kapatıldığında bu ile ilgili bir sorun hala olduğunu belirtmek için kısa aralıklı bip sesi devam edecektir. Tüm sorunlar temizlenmiştir, uyarının sessiz olur.

Aşağıdaki tabloda, çeşitli uyarı koşullar açıklanmaktadır.

### <a name="alarm-conditions"></a>Uyarı koşulları
| Durum | Severity | Alarm | OPS LED paneli |
| --- | --- | --- | --- |
| PCM uyarı – tek PCM DC Güç kaybı |Hata – yedeklilik kaybı olmadan |S1 |Modül hatası |
| PCM uyarı – tek PCM DC Güç kaybı |Hata – yedeklilik kaybı |S1 |Modül hatası |
| PCM fanı başarısız |Hata – yedeklilik kaybı |S1 |Modül hatası |
| SBB modülü PCM hata algıladı. |Hata |S1 |Modül hatası |
| PCM kaldırıldı |yapılandırma hatası |None |Modül hatası |
| Kasa yapılandırma hatası |Hata: kritik |S1 |Modül hatası |
| Düşük sıcaklık uyarı |Uyarı |S1 |Modül hatası |
| Yüksek sıcaklık uyarı |Uyarı |S1 |Modül hatası |
| Üzerinde sıcaklık Uyarısı |Hata: kritik |S1 |Modül hatası |
| I2C bus hatası |Hata – yedeklilik kaybı |S1 |Modül hatası |
| İletişim hatası (I2C) OPS paneli |Hata: kritik |S1 |Modül hatası |
| Denetleyici hatası |Hata: kritik |S1 |Modül hatası |
| SBB arabirimi modül hatası |Hata: kritik |S1 |Modül hatası |
| SBB arabirimi modülü hatası – Kalan çalışan hiçbir modül |Hata: kritik |S4 |Modül hatası |
| SBB arabirimi modülü kaldırıldı |Uyarı |None |Modül hatası |
| Güç denetimi hata sürücü |Uyarı: sürücü güç kaybı olmadan |S1 |Modül hatası |
| Güç denetimi hata sürücü |Hata – kritik; Sürücü güç kaybı |S1 |Modül hatası |
| Sürücü kaldırıldı |Uyarı |None |Modül hatası |
| Yeterli güç yok |Uyarı |yok |Modül hatası |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşenleri ve durum](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


