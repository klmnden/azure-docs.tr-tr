---
title: StorSimple 8000 serisi donanım bileşenleri ve durum | Microsoft Docs
description: StorSimple Cihazınızı StorSimple cihaz Yöneticisi hizmeti aracılığıyla donanım bileşenlerinin izlemeyi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alkohli
ms.openlocfilehash: a987239669e7437a179f5f24034f4dbe45535663
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60632883"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-hardware-components-and-status"></a>StorSimple cihaz Yöneticisi hizmetini İzleyicisi donanım bileşenleri ve durumu

## <a name="overview"></a>Genel Bakış
Bu makalede, şirket içi StorSimple 8000 serisi Cihazınızı çeşitli fiziksel ve mantıksal bileşenlerin açıklanır. Ayrıca bir kullanarak cihaz bileşen durumunu izlemek nasıl açıklar **durum ve donanım sistem durumu** dikey penceresinde, StorSimple cihaz Yöneticisi hizmeti.

**Durum ve donanım sistem durumu** dikey penceresinde tüm StorSimple cihaz bileşenleri donanım durumunu gösterir.

8100 için bileşenler listesinde açıklayan üç bölümü vardır:

* **Paylaşılan bileşenleri** – disk sürücüleri, kutu, güç ve soğutma Modülü (PCM) bileşenleri ve PCM sıcaklık satır voltaj ve geçerli sensörlerden satır gibi bunlar denetleyicileri parçası değildir.
* **Denetleyici 0 bileşenleri** – denetleyici 0, denetleyici, SAS Genişleticisi ve bağlayıcı, denetleyici sıcaklık algılayıcıları ve çeşitli ağ arabirimleri gibi bulunan bileşenler.
* **Denetleyici 1 bileşenleri** – oluşturan denetleyici 0 için ayrıntılı benzer Denetleyici 1 bileşenleri.

8600 model Cihazınızı genişletilmiş bırakılmazsa, diskleri (EBOD) kasası için karşılık gelen ek bileşenleri içerir. Bileşen listesi altında beş bölüm vardır. Bu birincil muhafaza bileşenleri içeren ve 8100 için açıklanan olanlarla aynıdır üç bölüm bulunur. EBOD muhafazası için açıklayan iki ek bölümlere vardır:

* **EBOD Denetleyicisi 0 bileşenleri** – EBOD denetleyicisi gibi EBOD muhafazası 0, SAS Genişleticisi ve bağlayıcı ve denetleyici sıcaklık algılayıcıları bulunan bileşenler.
* **EBOD denetleyicisi 1 bileşenleri** – EBOD muhafazası 0 için EBOD muhafazası 1 için ayrıntılı benzer oluşturan bileşenleri.
* **EBOD muhafazası paylaşılan bileşenleri** – EBOD Muhafazası ve EBOD denetleyicisi parçası olmayan PCM bileşenleri sunar.

> [!NOTE]
> **Donanım durumunu, StorSimple bulut Gereci (8010/8020) için kullanılamıyor.**


## <a name="monitor-the-hardware-status"></a>Donanım durumunu izleme
Bir cihaz bileşeni donanım durumunu görüntülemek için aşağıdaki adımları gerçekleştirin:

1. Gidin **cihazları**, belirli bir StorSimple cihazı seçin. Git **İzleyici > donanım sistem durumu**.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Bulun **donanım bileşenleri** bölümünde ve kullanılabilir bileşenlerini seçin. Bileşen etiketi için listeyi genişletin ve çeşitli cihaz bileşenlerinin durumunu görüntülemek için tıklamanız yeterlidir. Bkz: [birincil kasa için ayrıntılı bileşen listesi](#component-list-for-primary-enclosure-of-storsimple-device) ve [EBOD muhafazası için ayrıntılı bileşen listesi](#component-list-for-ebod-enclosure-of-storsimple-device).

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. Bileşen Durumu yorumlamak için aşağıdaki renk kodlama düzenini kullanın:
   
   * **Yeşil onay** – sağlıklı bileşeniyle gösterir **Tamam** durumu.
   * **Sarı** – düzeyi düşürülmüş bir bileşeni gösterir **uyarı** durumu.
   * **Kırmızı ünlem** – Denotes olan başarısız bir bileşen bir **hatası** durumu.
   * **Siyah metinle beyaz** – var olmayan bir bileşen gösterir.
   
   Bileşenleri olan, bir cihaz aşağıdaki ekran görüntüsünde gösterilmektedir **Tamam**, **uyarı**, ve **hatası** durumu.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   Genişletme **paylaşılan bileşenleri listesini**, NVRAM ve küme düzeyi düşürülmüş görebiliriz.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   Genişletme **Denetleyici 1 bileşenleri** listesinde, biz görebilirsiniz küme düğümü başarısız oldu.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. Kullanımda olmayan bir bileşen karşılaşırsanız bir **sağlıklı** durum, Microsoft Support başvurun. Uyarılar, Cihazınızda etkinse, e-posta uyarısı alırsınız. Başarısız donanım bileşeni değiştirmeniz gerekiyorsa, bkz. [StorSimple donanım bileşeni değişimi](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>StorSimple cihazının birincil kasası için bileşen listesi
Aşağıdaki tabloda birincil muhafazada, şirket içi StorSimple Cihazınızı (mevcut 8100 ve 8600) bulunan fiziksel ve mantıksal bileşenler açıklanmaktadır.

| Bileşen | Modül | Tür | Location | Alan bulunduğu yerde değiştirilebilen biriminin (FRU)? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Sürücü yuva [0-11] |Disk sürücüleri |Fiziksel |Paylaşılan |Evet |SSD veya HDD sürücülerini birincil muhafazada her biri için bir satır görüntülenir. |
| Ortam sıcaklığı algılayıcısı |Kutu |Fiziksel |Paylaşılan |Hayır |Kasanın içinde sıcaklık ölçer. |
| Orta düzlem sıcaklık algılayıcısı |Kutu |Fiziksel |Paylaşılan |Hayır |Orta düzlem sıcaklık ölçer. |
| Sesli uyarı |Kutu |Fiziksel |Paylaşılan |Hayır |Kasanın içinde sesli uyarı alt işlevsel olup olmadığını belirtir. |
| Kutu |Kutu |Fiziksel |Paylaşılan |Evet |Bir kasayı varlığını gösterir. |
| Kutu ayarları |Kutu |Fiziksel |Paylaşılan |Hayır |Kasa ön panelini ifade eder. |
| Satır voltaj algılayıcı |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı voltaj algılayıcıları ölçülen voltaj tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Geçerli satırı algılayıcılar |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı geçerli sensörlerden ölçülen geçerli tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| PCM sıcaklık algılayıcıları |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda sıcaklık algılayıcıları gibi giriş ve algılayıcılar etkin nokta ölçülen sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır her güç kaynakları aygıtın arkasında bulunan iki PCMs içinde sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır için her iki PCMs içinde bulunan dört soğutma fanları sunulur. |
| Pil [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır her yedek pil PCM yerleştirildiğinden modülleri için sunulur. |
| Metis |Yok |Mantıksal |Paylaşılan |Yok |Pil durumunu görüntüler: olup şarj duydukları ve ömrü sonu yaklaşan. |
| Küme |Yok |Mantıksal |Paylaşılan |Yok |Oluşturulan kümenin iki tümleşik denetleyicisi modüller arasında durumunu görüntüler. |
| Küme düğümü |Yok |Mantıksal |Paylaşılan |Yok |Kümenin bir parçası olarak denetleyici durumunu belirtir. |
| Küme Çekirdeği |Yok |Mantıksal | |Yok |HDD depolama havuzu çoğu disk üyelik varlığını gösterir. |
| HDD veri alanı |Yok |Mantıksal |Paylaşılan |Yok |Verileri sabit disk sürücüsü (HDD) depolama havuzunda kullanılan depolama alanı. |
| HDD yönetim alanı |Yok |Mantıksal |Paylaşılan |Yok |Yönetim görevleri için HDD depolama havuzunda ayrılan alanı. |
| HDD çekirdek alanı |Yok |Mantıksal |Paylaşılan |Yok |Küme çekirdeği için HDD depolama havuzunda ayrılan alanı. |
| HDD değiştirme alanı |Yok |Mantıksal |Paylaşılan |Yok |Denetleyici değiştirme HDD depolama havuzunda ayrılan alanı. |
| SSD veri alanı |Yok |Mantıksal |Paylaşılan |Yok |Verileri katı hal sürücüsü (SSD) depolama havuzu için kullanılan depolama alanı. |
| SSD NVRAM alanı |Yok |Mantıksal |Paylaşılan |Yok |SSD depolama havuzundaki NVRAM mantığı için ayrılmış depolama alanı. |
| HDD depolama havuzu |Yok |Mantıksal |Paylaşılan |Yok |HDD CİHAZDAN oluşturulur mantıksal depolama havuzunun durumunu görüntüler. |
| SSD depolama havuzu |Yok |Mantıksal |Paylaşılan |Yok |SSD CİHAZDAN oluşturulur mantıksal depolama havuzunun durumunu görüntüler. |
| Denetleyici [0-1] [durumu] |G/Ç |Fiziksel |Denetleyici |Evet |Denetleyici durumu görüntüler ve kasa içinde etkin veya bekleme modunda olup. |
| Denetleyicideki sıcaklık algılayıcıları |G/Ç |Fiziksel |Denetleyici |Hayır |G/ç modülü, CPU sıcaklık algılayıcıları DIMM ve PCIe gibi çok sayıda sıcaklık sensörünün karşılaştı sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| SAS Genişleticisi |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama denetleyicisine bağlanmak için kullanılan seri ekli SCSI (SAS) genişletici, durumunu gösterir. |
| SAS Bağlayıcısı [0-1] |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama için SAS Genişleticisi bağlanmak için kullanılan her SAS Bağlayıcısı durumunu gösterir. |
| SBB Orta düzlem bağlantı |G/Ç |Fiziksel |Denetleyici |Hayır |Her denetleyici için Orta düzlem bağlanmak için kullanılan Orta düzlem bağlayıcı durumunu gösterir. |
| İşlemci çekirdeği |G/Ç |Fiziksel |Denetleyici |Hayır |Her denetleyici içinde işlemci çekirdeği durumunu gösterir. |
| Kutu elektronik donanım gücü |G/Ç |Fiziksel |Denetleyici |Hayır |Kasa tarafından kullanılan güç sistem durumunu gösterir. |
| Kutu elektronik donanım tanılaması |G/Ç |Fiziksel |Denetleyici |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemlerin durumunu gösterir. |
| Temel Kart Yönetim denetleyicisine (BMC) |G/Ç |Fiziksel |Denetleyici |Hayır |Donanım cihazı sensörlerden izler ve bağımsız bir bağlantı aracılığıyla Sistem Yöneticisi ile iletişim kurar bir özel hizmet işlemci olan temel kart yönetim denetleyicisine (BMC) durumunu gösterir. |
| Ethernet |G/Ç |Fiziksel |Denetleyici |Hayır |Her ağ arabirimleri, diğer bir deyişle, yönetim denetleyicisinde sağlanan veri bağlantı noktalarını ve durumunu gösterir. |
| NVRAM |G/Ç |Fiziksel |Denetleyici |Hayır |NVRAM, güç kesintisi olması durumunda uygulama öneme bilgileri korumak için hizmet pil tarafından yedeklenen bir rastgele erişim geçici olmayan belleği durumunu gösterir. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>StorSimple cihaz EBOD muhafazası için bileşen listesi
EBOD muhafazada, şirket içi StorSimple Cihazınızı (yalnızca 8600 modelde varsa) bulunan fiziksel ve mantıksal bileşenler aşağıdaki tabloda açıklanmaktadır.

| Bileşen | Modül | Tür | Location | FRU? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Sürücü yuva [0-11] |Disk sürücüleri |Fiziksel |Paylaşılan |Evet |Bir satır, her bir kuyruğun EBOD muhafazası HDD sürücülerini sunulur. |
| Ortam sıcaklığı algılayıcısı |Kutu |Fiziksel |Paylaşılan |Hayır |Kasanın içinde sıcaklık ölçer. |
| Orta düzlem sıcaklık algılayıcısı |Kutu |Fiziksel |Paylaşılan |Hayır |Orta düzlem sıcaklık ölçer. |
| Sesli uyarı |Kutu |Fiziksel |Paylaşılan |Hayır |Kasanın içinde sesli uyarı alt işlevsel olup olmadığını belirtir. |
| Kutu |Kutu |Fiziksel |Paylaşılan |Evet |Bir kasayı varlığını gösterir. |
| Kutu ayarları |Kutu |Fiziksel |Paylaşılan |Hayır |OPS veya kasanın ön panelini belirtir. |
| Satır voltaj algılayıcı |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı voltaj algılayıcıları ölçülen voltaj tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Geçerli satırı algılayıcılar |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda satırı geçerli sensörlerden ölçülen geçerli tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| PCM sıcaklık algılayıcıları |PCM |Fiziksel |Paylaşılan |Hayır |Çok sayıda sıcaklık algılayıcıları gibi giriş ve algılayıcılar etkin nokta ölçülen sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır her güç kaynakları aygıtın arkasında bulunan iki PCMs içinde sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşılan |Evet |Bir satır için her iki PCMs içinde bulunan dört soğutma fanları sunulur. |
| Yerel depolama [HDD] |Yok |Mantıksal |Paylaşılan |Yok |HDD CİHAZDAN oluşturulur mantıksal depolama havuzunun durumunu görüntüler. |
| Denetleyici [0-1] [durumu] |G/Ç |Fiziksel |Denetleyici |Evet |EBOD modülünde denetleyicilerin durumunu görüntüler. |
| EBOD sıcaklık sensörlerden |G/Ç |Fiziksel |Denetleyici |Hayır |Çok sayıda sıcaklık sensörünün her denetleyicisinden karşılaştı sıcaklık tolerans dahilinde olup olmadığını gösteren görüntülenen durumlarına sahip. |
| SAS Genişleticisi |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama denetleyicisine bağlanmak için kullanılan SAS genişleticiye sahip durumunu gösterir. |
| SAS Bağlayıcısı 0-2 |G/Ç |Fiziksel |Denetleyici |Hayır |Tümleşik depolama için SAS Genişleticisi bağlanmak için kullanılan her SAS Bağlayıcısı durumunu gösterir. |
| SBB Orta düzlem bağlantı |G/Ç |Fiziksel |Denetleyici |Hayır |Her denetleyici için Orta düzlem bağlanmak için kullanılan Orta düzlem bağlayıcı durumunu gösterir. |
| Kutu elektronik donanım gücü |G/Ç |Fiziksel |Denetleyici |Hayır |Kasa tarafından kullanılan güç sistem durumunu gösterir. |
| Kutu elektronik donanım tanılaması |G/Ç |Fiziksel |Denetleyici |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemlerin durumunu gösterir. |
| Cihaz denetleyicisini bağlantısı |G/Ç |Fiziksel |Denetleyici |Hayır |EBOD g/ç modülü ve cihaz denetleyicisi arasındaki bağlantının durumunu gösterir. |

## <a name="next-steps"></a>Sonraki adımlar
* Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmak için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).
* Düşürülmüş veya başarısız durumuna sahip bir cihaz bileşeni sorun gidermeniz gerekiyorsa, başvurmak [göstergeleri izleme StorSimple](storsimple-monitoring-indicators.md).
* Başarısız donanım bileşeni değiştirmek için bkz: [StorSimple donanım bileşeni değişimi](storsimple-hardware-component-replacement.md).
* Cihaz sorunları yaşamaya devam ederseniz [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

