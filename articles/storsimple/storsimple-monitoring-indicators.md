---
title: "StorSimple göstergeleri izleme | Microsoft Docs"
description: "Sesli uyarılar StorSimple cihazı durumunu izlemek için kullanılan ve açık – emitting diyotlar (LED'leri) açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 9ae0caec211dc1199f0abd2ce9bc0c7ad11c02ec
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Göstergeleri izleme StorSimple Cihazınızı yönetmek için kullanın


## <a name="overview"></a>Genel Bakış
StorSimple Cihazınızı ışık – emitting diyotlar (LED'leri) içerir ve uyarılar modüller ve StorSimple cihazı genel durumunu izlemek için kullanabilirsiniz. İzleme göstergeleri aygıtın birincil muhafaza ve EBOD muhafazası donanım bileşenleri üzerinde bulunabilir. İzleme göstergeleri LED'leri veya sesli uyarılar olabilir.

Bir modül durumunu göstermek için kullanılan üç LED durum vardır: yeşil, kırmızı amber ya da kırmızı amber yeşil yanıp.  

* Yeşil LED'leri işletim durumu sağlıklı temsil eder.  
* Kırmızı amber LED'leri göstermek için yeşil kullanıcı müdahalesi gerektirebilir kritik olmayan koşullar varlığını yanıp.  
* Kırmızı amber LED'leri modülü içinde mevcut kritik bir hata olduğunu gösterir.  

Bu makalenin sonraki bölümlerinde çeşitli izleme gösterge LED'lerinin, StorSimple cihazında LED durumları ve tüm ilişkili sesli uyarılar göre Aygıt durumu konumlarına açıklar.

## <a name="front-panel-indicator-leds"></a>Ön panel göstergesi LED'leri
Ön panel, olarak da bilinen *operations paneli* veya *ops paneli*, sistemdeki tüm modülleri toplama durumunu görüntüler. Ön panel StorSimple birincil ve EBOD muhafazası aynıdır ve aşağıda gösterilmiştir.  

   ![Cihaz Panosu][1]

Ön panel aşağıdaki göstergelerini içerir:  

1. Sessiz düğmesi
2. Güç göstergesi LED (kırmızı/yeşil-amber)
3. Modül hata göstergesi (kırmızı-amber/OFF üzerinde) neden
4. Mantıksal hata göstergesi (kırmızı-amber/OFF üzerinde neden
5. Birim kimliği görüntüleme  

Ana aygıt için ön panel LED'leri ve EBOD muhafazası için olanlar arasındaki farktır **sistem birimi kimlik numarası** LED ekranda gösterilen. Cihazda görüntülenen kimliği varsayılan birim **00**, üzerinde EBOD muhafazası görüntülenen varsayılan birim kimliği iken **01**. Bu, cihaz açıldığında cihaz EBOD muhafazası arasında hızlı bir şekilde ayırt olanak sağlar. Cihazınızı kapalı, sağlanan bilgileri kullanın. [üzerinde yeni bir aygıt kapatma](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) cihaz EBOD muhafazası ayırt etmek için.  

## <a name="front-panel-led-status"></a>Ön panel LED durumu
Aygıt veya EBOD muhafazası için ön panelindeki LED'leri tarafından gösterilen durumu tanımlamak için aşağıdaki tabloyu kullanın.  

| Sistem gücü | Modül hatası | Mantıksal hatası | Uyarı | Durum |
| --- | --- | --- | --- | --- |
| Kırmızı amber |KAPALI |KAPALI |Yok |Yedek güç veya AC güç açık ve modülleri kaldırıldı denetleyicisi işletim AC güç kesildi. |
| Yeşil |AÇIK |AÇIK |Yok |OPS paneli (5s'dir) gücüyle test durumu |
| Yeşil |KAPALI |KAPALI |Yok |Açma, iyi tüm işlevleri |
| Yeşil |AÇIK |Yok |PCM hataya LED'leri, fan hataya LED'leri |Fan arıza, üzerinde veya altında sıcaklık herhangi PCM hatası |
| Yeşil |AÇIK |Yok |G/ç modülü LED'leri |Bir Denetleyici Modülü hatası |
| Yeşil |AÇIK |Yok |Yok |Muhafaza mantık hatası |
| Yeşil |Flash |Yok |Modül durumu Denetleyici Modülü GEREKTİRİYORDU. PCM hataya LED'leri, fan hataya LED'leri |Bilinmeyen Denetleyici Modülü türü yüklü, I2C veri yolu hatası, Denetleyici Modülü önemli ürün verilerini (VPD) yapılandırma hatası |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Soğutma Modülü (PCM) göstergesi LED'leri güç
Güç soğutma Modülü (PCM) göstergesi LED'leri arkasında birincil muhafaza veya EBOD muhafazası PCM modüllerin bulunabilir. Bu konuda aşağıdaki LED'leri StorSimple Cihazınızı durumunu izlemek için nasıl kullanılacağı açıklanmaktadır.  

* Birincil muhafaza PCM LED'leri
* EBOD muhafazası PCM LED'leri

## <a name="pcm-leds-for-the-primary-enclosure"></a>Birincil muhafaza PCM LED'leri
StorSimple cihazı ek pil 764W PCM modülüyle sahiptir. Aşağıdaki çizimde aygıtın LED paneli gösterir.  

   ![Birincil kasada PCM LED'leri][2]

LED Gösterge:

1. AC güç kesintisi
2. Fan hatası
3. Pil hatası
4. PCM TAMAM
5. DC hatası
6. İyi pil  

PCM durumunu LED panelde gösterilir. Cihaz PCM LED paneli altı LED'leri sahiptir. Bu LED'leri dördünü güç kaynağı ve fan durumunu görüntüleyin. Kalan iki LED'leri PCM yedek pil modülünde durumunu gösterir. PCM durumunu belirlemek için aşağıdaki tabloları kullanabilir.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Güç kaynağı ve fan PCM göstergesi LED'leri
| Durum | PCM hazır (yeşil) | AC başarısız (amber) | Fan başarısız (amber) | DC başarısız (amber) |
| --- | --- | --- | --- | --- |
| Hiçbir AC üssüne (kasada) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC güç (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC sunmak PCM ON - Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fan başarısız) |KAPALI |KAPALI |AÇIK |Yok |
| PCM hataya (üzerinden geçerli üzerinden voltaj üzerinden amp) |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fan tolerans dışı) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modu |Yanıp |KAPALI |KAPALI |KAPALI |
| PCM üretici yazılımı yükleme |KAPALI |Yanıp |Yanıp |Yanıp |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Yedek pil için PCM göstergesi LED'leri
| Durum | Pil iyi (yeşil) | Pil hataya (amber) |
| --- | --- | --- |
| Pil mevcut değil |KAPALI |KAPALI |
| Pil mevcut ve dolu |AÇIK |KAPALI |
| Pil şarj veya bakım deşarj |Yanıp |KAPALI |
| Pil "yazılım" hatası (kurtarılabilir) |KAPALI |Yanıp |
| Pil "sabit" hata (kurtarılamaz) |KAPALI |AÇIK |
| Pil disarmed |Yanıp |KAPALI |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>EBOD muhafazası PCM LED'leri
EBOD muhafazası 580W PCM ve ek pil vardır. EBOD muhafazası PCM paneli yalnızca güç kaynakları ve fan için gösterge LED'lerinin sahiptir. Aşağıdaki çizimde, bu LED'leri gösterir.

   ![EBOD muhafazası üzerinde PCM LED'leri][3] 

PCM durumunu belirlemek için aşağıdaki tabloyu kullanın.  

| Durum | PCM hazır (yeşil) | AC başarısız (amber) | Fan başarısız (amber) | DC başarısız (amber) |
| --- | --- | --- | --- | --- |
| Hiçbir AC üssüne (kasada) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC güç (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC sunmak PCM ON – Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fan başarısız) |KAPALI |KAPALI |AÇIK |X |
| (Üzerinden geçerli üzerinden voltaj üzerinden amp PCM hatası |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fan tolerans dışı) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modeli |Yanıp |KAPALI |KAPALI |KAPALI |
| PCM üretici yazılımı yükleme |KAPALI |Yanıp |Yanıp |Yanıp |

## <a name="controller-module-indicator-leds"></a>Denetleyici Modülü göstergesi LED'leri
StorSimple cihazı LED'leri birincil denetleyici ve EBOD denetleyicisi modüllerini içerir.   

### <a name="monitoring-leds-for-the-primary-controller"></a>İçin birincil denetleyici LED'leri izleme
Aşağıdaki çizimde, birincil denetleyicisinde LED'leri tanımlamanıza yardımcı olur. (Tüm bileşenlerini yönde yardımcı olmak için listelenir.)  

   ![İzleme LED'leri - birincil denetleyici][4]

Denetleyici Modülü doğru şekilde çalıştığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="controller-indicator-leds"></a>Denetleyici göstergesi LED'leri
| LED | Açıklama |
| --- | --- |
| Kimliği LED (mavi) |Modül tanımlanan olduğunu gösterir. Bir çalışan denetleyicisinde mavi ışığı yanıp sönen, etkin denetleyicisi denetleyicisidir ve diğerinde bekleme denetleyicisidir. Daha fazla bilgi için bkz: [Cihazınızı etkin denetleyicisinde tanımlamak](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Hataya LED (amber) |Denetleyicideki bir hata gösterir. |
| Tamam LED (yeşil) |Sürekli yeşil denetleyicisi Tamam olduğunu gösterir. Yeşil yanıp denetleyicisi VPD yapılandırma hatası gösterir. |
| SAS etkinlik LED'leri (yeşil) |Sürekli yeşil hiçbir geçerli etkinliği ile bir bağlantı gösterir. Yeşil yanıp devam eden etkinlik bağlantı olduğunu gösterir. |
| Ethernet durum LED'leri |Sağdaki bağlantıyı/ağ etkinliği gösterir: (sürekli yeşil) bağlantı (yeşil yanıp) etkin ağ etkinliği. Sol tarafta ağ hızını gösterir: (sarı) 1000 Mb/sn, (yeşil) 100 Mb/sn ve (Kapalı) 10 Mb/sn. Ağ arabirimi etkinleştirilmemiş olsa bile bileşen modeline bağlı olarak bu ışık yanıp. |
| POST LED'leri |Denetleyici açıldığında önyükleme ilerleme durumunu gösterir. StorSimple cihazı önyükleme yapamıyorsa, bu LED Microsoft Hata gerçekleştiği önyükleme işlemi noktasında tanımlamak Support yardımcı olur. |

> [!IMPORTANT]
> Hataya LED aydınlatma, denetleyici yeniden başlatarak çözülmüş Denetleyici Modülü ile ilgili bir sorun yoktur. Denetleyici yeniden başlatılması bu sorunu çözmezse Microsoft Support temasa geçin.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>EBOD (EBOD muhafazası) için izleme LED'leri
6 Gb/sn SAS EBOD denetleyicilerinden her birinin durumunu aşağıdaki çizimde gösterildiği gibi belirtmek LED'leri sahiptir.  

  ![LED'leri - EBOD muhafazası izleme][5]

EBOD Denetleyici Modülü normal olarak çalışıyor olup olmadığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD denetleyicisi modülü göstergesi LED'leri
| Durum | G/ç modülü hazır (yeşil) | G/ç modülü hatası (amber) | Ana bilgisayar bağlantı noktası etkinliğini (yeşil) |
| --- | --- | --- | --- |
| Denetleyici Modülü Tamam |AÇIK |KAPALI |- |
| Denetleyici Modülü hatası |KAPALI |AÇIK |- |
| Hiçbir dış konak bağlantı |- |- |KAPALI |
| Dış konak bağlantı – etkinliği yok |- |- |AÇIK |
| Dış konak bağlantı - etkinliği |- |- |Yanıp |
| Denetleyici Modülü meta veri hatası |Yanıp |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>EBOD Muhafazası ve birincil kasası için disk sürücüsü göstergesi LED'leri
StorSimple cihazı birincil muhafaza ve EBOD muhafazası bulunan sürücü yok. Bu bölümde açıklandığı gibi her disk sürücüsünü izleme gösterge LED'lerinin içerir. 

Disk sürücüleri için sürücü durumu yeşil gösterilir LED ve kırmızı amber LED her sürücü taşıyıcı modülü ön bağlanır. Aşağıdaki çizimde, bu LED'leri gösterir.

  ![Disk sürücüsü LED'leri][6]

Sırayla genel ön panelini LED durumunu etkileyen her disk sürücüsünü, durumu belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>EBOD muhafazası için disk sürücüsü göstergesi LED'leri
| Durum | Etkinlik Tamam LED (yeşil) | Hataya LED (kırmızı-amber) | OPS paneli LED ilişkili |
| --- | --- | --- | --- |
| Yüklü hiçbir sürücü |KAPALI |KAPALI |None |
| Sürücü yüklü ve çalışır durumda |Etkinliği ile yanıp açık/kapalı |X |None |
| SCSI muhafaza hizmetlerini (SES) aygıt kimlik kümesi |AÇIK |1 saniye üzerinde/ikinci kapalı 1 yanıp |None |
| SES aygıt hataya bit kümesi |AÇIK |AÇIK |Mantıksal hataya (kırmızı) |
| Güç denetimi devresi hatası |KAPALI |AÇIK |Modül hatası (kırmızı) |

## <a name="audible-alarms"></a>Sesli uyarılar
StorSimple cihazı birincil muhafaza ve EBOD muhafazası ilişkili sesli uyarılar içerir. İşitsel bir alarm hem kasaları ön Panoda (ops paneli olarak da bilinir) bulunur. Bir hata koşulu mevcut olduğunda sesli alarm gösterir. Aşağıdaki koşullar uyarı etkinleştirir:  

* Fan hatası veya hatası
* Voltaj aralık dışında
* Üzerinden veya sıcaklık koşul altında
* Isı taşması
* Sistem hatası
* Mantıksal hatası
* Güç kaynağı hatası
* Modül (PCM) soğutma güç kaldırılması  

Aşağıdaki tabloda, çeşitli uyarı durumlarını açıklar.  

### <a name="alarm-states"></a>Uyarı durumları
| Uyarı durumu | Eylem | Sessiz düğmesine basıldı eylemiyle |
| --- | --- | --- |
| S0 |Normal modda: sessiz |İki kez bip |
| S1 |Hataya modu: 1 saniye üzerinde/ikinci kapalı 1 |S2 ya da S3 geçiş (notlara bakın) |
| S2 |Mod hatırlat: aralıklı bip |None |
| S3 |Yumuşak modu: sessiz |None |
| S4 |Kritik hata modu: sürekli Uyarısı |Kullanılabilir değil: sessiz etkin değil |

> [!NOTE]
> * 2 dakika içinde sessiz basmayın varsa uyarı durumda S1 durumu otomatik olarak S2 veya S3 geçer.  
> * Hata koşulu temizlendikten sonra uyarı durumlara S1 S4 S0 için döndür.  
> * Kritik hata durumu S4 başka bir durumdan girilebilir.  


Ops panelde sessiz düğmesine basarak sesli alarm sessiz. Sessiz anahtar olmayan el ile çalıştırılır halinde otomatik ses kapatma iki dakika sonra ortaya çıkar. Uyarı kapatıldığında ile ilgili bir sorun hala var olduğunu belirtmek için kısa aralıklı bip sesi için devam eder. Tüm sorunları kaldırıldığında uyarı sessiz olacaktır.

Aşağıdaki tabloda, çeşitli uyarı koşullar açıklanmaktadır.

### <a name="alarm-conditions"></a>Uyarı koşulları
| Durum | Önem Derecesi | Uyarı | OPS LED paneli |
| --- | --- | --- | --- |
| PCM uyarı – tek bir PCM DC Güç kaybı |Hata – artıklık hiçbir kaybı |S1 |Modül hatası |
| PCM uyarı – tek bir PCM DC Güç kaybı |Hata – artıklık kaybı |S1 |Modül hatası |
| PCM fan başarısız |Hata – artıklık kaybı |S1 |Modül hatası |
| SBB modülü PCM hatası algılandı |Hata |S1 |Modül hatası |
| PCM kaldırıldı |Yapılandırma hatası |None |Modül hatası |
| Muhafaza yapılandırma hatası |Hata-kritik |S1 |Modül hatası |
| Düşük sıcaklık uyarı |Uyarı |S1 |Modül hatası |
| Yüksek sıcaklık uyarı |Uyarı |S1 |Modül hatası |
| Sıcaklık Uyarısı |Hata-kritik |S1 |Modül hatası |
| I2C veri yolu hatası |Hata – artıklık kaybı |S1 |Modül hatası |
| İletişim hatası (I2C) OPS paneli |Hata-kritik |S1 |Modül hatası |
| Denetleyici hatası |Hata-kritik |S1 |Modül hatası |
| SBB arabirimi modül hatası |Hata-kritik |S1 |Modül hatası |
| SBB arabirimi modülü hatası – kalan düzgün modülünüz |Hata-kritik |S4 |Modül hatası |
| Kaldırılan SBB arabirimi Modülü |Uyarı |None |Modül hatası |
| Güç denetimi hata sürücü |Uyarı – sürücü güç kaybı olmaksızın |S1 |Modül hatası |
| Güç denetimi hata sürücü |Hata – kritik; Sürücü güç kaybı |S1 |Modül hatası |
| Sürücü kaldırıldı |Uyarı |None |Modül hatası |
| Kullanılabilir yeterli güç yok |Uyarı |Yok |Modül hatası |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşenleri ve durum](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


