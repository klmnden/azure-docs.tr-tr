---
title: "StorSimple donanım bileşenleri ve durum | Microsoft Docs"
description: "StorSimple Cihazınızı StorSimple Yöneticisi hizmeti aracılığıyla donanım bileşenlerini izlemek öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0d56a2ba-daf0-45ad-9610-8b8712dd5750
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 93df79c9d349c294b692148a19c9d881c4de4f7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-hardware-components-and-status"></a>İzleyici donanım bileşenleri ve durumunu StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Bu makalede, şirket içi StorSimple Cihazınızı çeşitli fiziksel ve mantıksal bileşenlerinde açıklanmaktadır. Ayrıca kullanarak aygıt bileşen durumunu izlemek nasıl açıklar **Bakım** StorSimple Yöneticisi hizmet sayfası. 

**Bakım** sayfa tüm StorSimple cihaz bileşenlerini donanım durumunu gösterir. 

8100 bileşenlerin listesini altında açıklamak üç bölüm bulunur:

* **Paylaşılan bileşenleri** – disk sürücüleri, kasası, PCM bileşenleri ve PCM sıcaklık satır voltaj ve geçerli algılayıcılar satır gibi bu denetleyicileri yer almaz.
* **Denetleyici 0 bileşenleri** – denetleyici 0, denetleyici, SAS genişletici bağlayıcı, denetleyici sıcaklık algılayıcıları ve gibi çeşitli ağ arabirimleri üzerinde bulunan bileşenleri.
* **Denetleyici 1 bileşenleri** – Denetleyici 1, 0 denetleyicisi için ayrıntılı benzer oluşturan bileşenleri.

8600 model Cihazınızı genişletilmiş demet, diskleri (EBOD) kasası karşılık gelen ek bileşenleri içerir. Bileşen listesi altında beş bölümleri vardır. Bu 8100 için açıklanan olanlarla aynıdır ve birincil muhafazada bileşenlerini içeren üç bölüm bulunur. Açıklayan ek için iki bölüme EBOD muhafazası vardır:

* **EBOD muhafazası paylaşılan bileşenleri** – EBOD muhafazası bileşenleri EBOD denetleyicisi parçası olmayan PCM sunmak.
* **EBOD denetleyici 0 bileşenleri** – EBOD denetleyicisi gibi EBOD muhafazası 0, üzerinde SAS genişletici ve bağlayıcı ve denetleyici sıcaklık algılayıcıları bulunan bileşenleri.
* **EBOD denetleyicisi 1 bileşenleri** – EBOD muhafazası 0 için 1, bu ayrıntılı benzer EBOD muhafazası oluşturan bileşenleri.

> [!NOTE]
> **Donanım durum bölümü bir StorSimple sanal cihazı (1100) bakım sayfasında mevcut değil.**
> 
> 

## <a name="monitor-the-hardware-status"></a>Donanım durumunu izleme
Bir aygıt bileşeni donanım durumunu görüntülemek için aşağıdaki adımları gerçekleştirin:

1. Gidin **aygıtları**, belirli bir StorSimple cihazı seçin. Cihaz düzeyinde menüsüne gidin ve ardından tıklatıp **Bakım**. 
2. Bulun **donanım durum** bölümünde ve kullanılabilir bileşenlerini (yukarıda açıklandığı gibi) seçin. Listeyi genişletin ve çeşitli aygıt bileşenlerinin durumunu görüntülemek için bileşen etiket önceki bir ok tıklamanız yeterlidir. Bkz: [birincil kasa için ayrıntılı bileşen listesi](#component-list-for-primary-enclosure-of-storsimple-device) ve [EBOD muhafazası için ayrıntılı bileşen listesi](#component-list-for-ebod-enclosure-of-storsimple-device).
3. Bileşen Durumu yorumlamak için aşağıdaki renk kodlama düzenini kullanın:
   
   * **Yeşil onay** – Denotes bir **sağlıklı** veya **Tamam** bileşeni.
   * **Sarı** – bir bileşenin gösterir **uyarı** durumu.
   * **Kırmızı bir ünlem işareti** – Denotes sahip bir bileşen bir **hatası** veya **dikkat** durumu.
   * **Siyah metinle beyaz** – mevcut olmayan bir bileşeni gösterir.
4. Kullanımda olmayan bir bileşeni karşılaşırsanız bir **sağlıklı** durum, Microsoft Support başvurun. Uyarıları aygıtınızda etkinleştirilirse, bir e-posta uyarısı alırsınız. Başarısız donanım bileşeni değiştirmeniz gerekiyorsa, bkz: [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>StorSimple cihazı birincil kasası için bileşen listesi
Aşağıdaki tabloda şirket içi StorSimple Cihazınızı birincil muhafazada yer alan fiziksel ve mantıksal bileşenleri özetlenmektedir.

| Bileşen | Modül | Tür | Konum | Alan değiştirilebilen biriminin (FRU)? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| [0-11] yuvasında sürücü |Disk sürücüleri |Fiziksel |Paylaşılan |Evet |Bir satır için her bir SSD veya birincil muhafazada HDD sürücülerini sunulur. |
| Ortam sıcaklık algılayıcısı |Kasası |Fiziksel |Paylaşılan |Hayır |Kasa içinde sıcaklık ölçer. |
| Orta düzlemi sıcaklık algılayıcısı |Kasası |Fiziksel |Paylaşılan |Hayır |Orta düzlemi sıcaklık ölçer. |
| Sesli Uyarısı |Kasası |Fiziksel |Paylaşılan |Hayır |Kasa içinde sesli alarm alt işlevsel olup olmadığını gösterir. |
| Kasası |Kasası |Fiziksel |Paylaşılan |Evet |Bir kasa varlığını gösterir. |
| Kasa ayarları |Kasası |Fiziksel |Paylaşılan |Hayır |Kasa ön panel başvuruyor. |
| Satır voltaj algılayıcı |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı voltaj algılayıcıları ölçülen voltaj tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Satır geçerli algılayıcılar |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı geçerli algılayıcılar ölçülen geçerli tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Sıcaklık algılayıcı PCM içinde |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda giriş gibi sıcaklık algılayıcıları ve etkin nokta algılayıcılar ölçülen sıcaklık tolerans dahilinde olup olmadığını belirten görüntülenen durumlarına sahiptir. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır, her iki PCMs aygıtın arkasında bulunan güç kaynakları sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır, her iki PCMs içinde bulunan dört soğutma fanları sunulur. |
| Pil [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Tek bir çizgi her PCM yerleştirildiğinden yedek pil modüllerin sunulur. |
| Metis |Yok |Mantıksal |Paylaşılan |Yok |Piller durumunu görüntüler: olup şarj ihtiyaç duydukları ve yaşam sonu yaklaşan. |
| Küme |Yok |Mantıksal |Paylaşılan |Yok |Oluşturulduğunda kümenin iki Tümleşik Denetleyici modülleri arasında durumunu görüntüler. |
| Küme düğümü |Yok |Mantıksal |Paylaşılan |Yok |Kümenin bir parçası olarak denetleyicisinin durumunu gösterir. |
| Küme Çekirdeği |Yok |Mantıksal | |Yok |HDD depolama havuzu çoğu disk üyelik varlığı olduğunu gösterir. |
| HDD veri alanı |Yok |Mantıksal |Paylaşılan |Yok |Sabit disk sürücüsü (HDD) depolama Havuzu'ndaki verileri için kullanılan depolama alanı. |
| HDD yönetim alanı |Yok |Mantıksal |Paylaşılan |Yok |Yönetim görevleri için HDD depolama havuzunda ayrılan alanı. |
| HDD çekirdek alanı |Yok |Mantıksal |Paylaşılan |Yok |Küme çekirdeğini HDD depolama havuzunda ayrılan alanı. |
| HDD değiştirme alanı |Yok |Mantıksal |Paylaşılan |Yok |Denetleyici değiştirme HDD depolama havuzunda ayrılan alanı. |
| SSD veri alanı |Yok |Mantıksal |Paylaşılan |Yok |Katı hal sürücüsü (SSD) depolama Havuzu'ndaki verileri için kullanılan depolama alanı. |
| SSD NVRAM alanı |Yok |Mantıksal |Paylaşılan |Yok |NVRAM mantığı için ayrılmış SSD depolama havuzundaki depolama alanı. |
| HDD depolama havuzu |Yok |Mantıksal |Paylaşılan |Yok |HDD aygıttan oluşturulan mantıksal depolama havuzu durumunu görüntüler. |
| SSD depolama havuzu |Yok |Mantıksal |Paylaşılan |Yok |SSD aygıttan oluşturulan mantıksal depolama havuzu durumunu görüntüler. |
| Denetleyici [0-1] [state] |G/Ç |Fiziksel |Denetleyici |Evet |Denetleyici durumunu görüntüler ve kasa içinde herhangi bir etkin ya da bekleme modunda olup. |
| Sıcaklık algılayıcı denetleyicideki |G/Ç |Fiziksel |Denetleyici |Hayır |G/ç modülü, CPU sıcaklık, DIMM ve PCIe algılayıcılar gibi çok sayıda sıcaklık algılayıcıları karşılaştı sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| SAS genişletici |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama denetleyicisine bağlanmak için kullanılan seri ekli SCSI (SAS) genişletici, durumunu gösterir. |
| SAS Bağlayıcısı [0-1] |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama SAS genişletici bağlanmak için kullanılan her bir SAS Bağlayıcısı durumunu gösterir. |
| SBB Orta düzlemi bağlantı |G/Ç |Fiziksel |Denetleyici |Hayır |Orta düzlemi her denetleyici bağlanmak için kullanılan Orta düzlemi bağlayıcı durumunu gösterir. |
| İşlemci çekirdeği |G/Ç |Fiziksel |Denetleyici |Hayır |Her denetleyici içinde işlemci çekirdeği durumunu gösterir. |
| Muhafaza elektronik gücü |G/Ç |Fiziksel |Denetleyici |Hayır |Kasası tarafından kullanılan güç sistem durumunu gösterir. |
| Muhafaza elektronik tanılama |G/Ç |Fiziksel |Denetleyici |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemleri durumunu gösterir. |
| Temel kart yönetim denetleyicisi (BMC) |G/Ç |Fiziksel |Denetleyici |Hayır |Algılayıcılar donanım aygıtı izler ve Sistem Yöneticisi bağımsız bir bağlantı üzerinden iletişim kuran bir özel hizmet işlemci olan temel kart yönetim denetleyicisi (BMC) durumunu gösterir. |
| Ethernet |G/Ç |Fiziksel |Denetleyici |Hayır |Her ağ arabirimleri, diğer bir deyişle, yönetim ve veri bağlantı noktaları denetleyicisinde sağlanan durumunu gösterir. |
| NVRAM |G/Ç |Fiziksel |Denetleyici |Hayır |NVRAM, güç kesintisi olması durumunda uygulama kritik bilgileri korumak için hizmet pil tarafından yedeklenen bir geçici olmayan rasgele erişim belleği durumunu gösterir. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>StorSimple cihaz EBOD muhafazası için bileşen listesi
Aşağıdaki tabloda şirket içi StorSimple Cihazınızı EBOD muhafazası içinde yer alan fiziksel ve mantıksal bileşenleri özetlenmektedir.

| Bileşen | Modül | Tür | Konum | FRU? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| [0-11] yuvasında sürücü |Disk sürücüleri |Fiziksel |Paylaşılan |Evet |Tek bir çizgi her EBOD muhafazası önündeki HDD sürücülerini sunulur. |
| Ortam sıcaklık algılayıcısı |Kasası |Fiziksel |Paylaşılan |Hayır |Kasa içinde sıcaklık ölçer. |
| Orta düzlemi sıcaklık algılayıcısı |Kasası |Fiziksel |Paylaşılan |Hayır |Orta düzlemi sıcaklık ölçer. |
| Sesli Uyarısı |Kasası |Fiziksel |Paylaşılan |Hayır |Kasa içinde sesli alarm alt işlevsel olup olmadığını gösterir. |
| Kasası |Kasası |Fiziksel |Paylaşılan |Evet |Bir kasa varlığını gösterir. |
| Kasa ayarları |Kasası |Fiziksel |Paylaşılan |Hayır |OPS veya kasa ön panelini anlamındadır. |
| Satır voltaj algılayıcı |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı voltaj algılayıcıları ölçülen voltaj tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Satır geçerli algılayıcılar |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı geçerli algılayıcılar ölçülen geçerli tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Sıcaklık algılayıcı PCM içinde |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda giriş gibi sıcaklık algılayıcıları ve etkin nokta algılayıcılar ölçülen sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahiptir. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır, her iki PCMs aygıtın arkasında bulunan güç kaynakları sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır, her iki PCMs içinde bulunan dört soğutma fanları sunulur. |
| Yerel depolama [HDD] |Yok |Mantıksal |Paylaşılan |Yok |HDD aygıttan oluşturulan mantıksal depolama havuzu durumunu görüntüler. |
| Denetleyici [0-1] [state] |G/Ç |Fiziksel |Denetleyici |Evet |EBOD modülünde denetleyicilerinin durumunu görüntüler. |
| EBOD sıcaklık algılayıcıları |G/Ç |Fiziksel |Denetleyici |Hayır |Çok sayıda sıcaklık algılayıcıları her denetleyicisinden karşılaştı sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| SAS genişletici |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama denetleyicisine bağlanmak için kullanılan SAS genişletici durumunu gösterir. |
| SAS Bağlayıcısı [0-2] |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama SAS genişletici bağlanmak için kullanılan her bir SAS Bağlayıcısı durumunu gösterir. |
| SBB Orta düzlemi bağlantı |G/Ç |Fiziksel |Denetleyici |Hayır |Orta düzlemi her denetleyici bağlanmak için kullanılan Orta düzlemi bağlayıcı durumunu gösterir. |
| Muhafaza elektronik gücü |G/Ç |Fiziksel |Denetleyici |Hayır |Kasası tarafından kullanılan güç sistem durumunu gösterir. |
| Muhafaza elektronik tanılama |G/Ç |Fiziksel |Denetleyici |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemleri durumunu gösterir. |
| Aygıt denetleyicisi bağlantı |G/Ç |Fiziksel |Denetleyici |Hayır |EBOD g/ç modülü ve cihaz denetleyicisi arasındaki bağlantının durumunu gösterir. |

## <a name="next-steps"></a>Sonraki adımlar
* Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanmak için şu adrese gidin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).
* Düzeyi düşürülmüş veya başarısız durumuna sahip bir aygıt bileşeni gidermeniz gerekiyorsa oluştuysa, [göstergeleri izleme StorSimple](storsimple-monitoring-indicators.md). 
* Başarısız donanım bileşeni değiştirmek için bkz: [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).
* Aygıt sorunları yaşamaya devam ederseniz [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).

