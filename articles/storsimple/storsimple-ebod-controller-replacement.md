---
title: "Bir StorSimple EBOD denetleyicisi Değiştir | Microsoft Docs"
description: "Bir StorSimple 8600 model Cihazınızı biri veya her ikisi EBOD denetleyicilerinde kaldırdığınızda ve açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8cbfa507-1a56-4e24-99dd-7db9abd3b850
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: b6834e00fd225261296f92b463f9847deecffcdb
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>EBOD denetleyicisi, StorSimple Cihazınızda değiştirin
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [EBOD denetleyicisi, StorSimple Cihazınızda Değiştir](storsimple-8000-ebod-controller-replacement.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bu öğretici, hatalı bir EBOD Denetleyici Modülü, Microsoft Azure StorSimple Cihazınızda Değiştir açıklanmaktadır. EBOD Denetleyici Modülü değiştirmek için aktarmanız gerekir:

* Hatalı EBOD denetleyicisi Kaldır
* Yeni bir EBOD denetleyicisi yükleme

Başlamadan önce aşağıdaki bilgileri göz önünde bulundurun:

* Boş EBOD modülleri tüm kullanılmayan yuvaları eklenmesi gerekir. Bir yuva açık olarak bırakılırsa muhafaza düzgün seyrek erişimli değil.
* EBOD denetleyicisi hot Swap ve yerini veya kaldırılabilir. Yenisini elde edene kadar başarısız bir modül kaldırmayın. Değiştirme işlemini başlattığınızda, 10 dakika içinde bitmesi gerekir.

> [!IMPORTANT]
> Kaldırın veya herhangi bir StorSimple bileşeni değiştirmek denemeden önce gözden geçirmenizi emin olun [güvenliği simgesi kuralları](storsimple-safety.md#safety-icon-conventions) ve diğer [güvenlik önlemlerini](storsimple-safety.md).
> 
> 

## <a name="remove-an-ebod-controller"></a>EBOD denetleyicisi Kaldır
StorSimple Cihazınızı başarısız EBOD denetleyicisi modülünde değiştirmeden önce bir EBOD denetleyici modülü etkin ve çalışıyor olduğundan emin olun. Aşağıdaki yordamı ve tabloyu EBOD denetleyicisi modülün nasıl kaldırılacağı açıklanmaktadır.

#### <a name="to-remove-an-ebod-module"></a>EBOD modülünü kaldırmak için
1. Azure Klasik Portalı'nı açın.
2. Gidin **aygıtları** > **Bakım** > **donanım durum**, doğrulayın active EBOD denetleyicisi LED durumu Modül yeşil ve başarısız EBOD Denetleyici Modülü LED kırmızı.
3. Cihaz arkasındaki başarısız EBOD Denetleyici Modülü bulun.
4. Sistem dışı EBOD modülü çıkarmadan önce EBOD Denetleyici Modülü denetleyicisine bağlanmak kabloları kaldırın.
5. Denetleyiciye bağlı EBOD Denetleyici Modülü tam SAS bağlantı noktasını not edin. EBOD modülü değiştirdikten sonra sistem bu yapılandırmayı geri yüklemek için gerekli. 
   
   > [!NOTE]
   > Genellikle, bu olarak etiketli bağlantı noktası A olacaktır **içinde ana bilgisayar** Aşağıdaki diyagramda.
   > 
   > 
   
    ![Devre kartı olarak EBOD denetleyicisi](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Şekil 1** geri of EBOD Modülü
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Hata Işığı |
   | 2 |Güç ışığı |
   | 3 |SAS Bağlayıcısı |
   | 4 |SAS LED'leri |
   | 5 |Yalnızca Fabrika kullanımı için seri bağlantı noktaları |
   | 6 |(Ana), bağlantı noktası |
   | 7 |Bağlantı noktası B (ana bilgisayarı devre dışı) |
   | 8 |Bağlantı noktası C (yalnızca Fabrika kullanın) |

## <a name="install-a-new-ebod-controller"></a>Yeni bir EBOD denetleyicisi yükleme
Aşağıdaki yordamı ve tabloyu StorSimple Cihazınızı EBOD Denetleyici Modülü yükleneceği açıklanmaktadır.

#### <a name="to-install-an-ebod-controller"></a>EBOD denetleyicisi yüklemek için
1. EBOD aygıt arabirimi bağlayıcıya özellikle hasara karşı denetleyin. Tüm PIN'ler Bükülü yeni EBOD denetleyicisi yüklemeyin.
2. Mandal devreye kadar açık konumda tutma ile modülü muhafaza kaydırın.
   
    ![EBOD denetleyicisi yükleniyor](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Şekil 2** EBOD denetleyici modülü yükleniyor
3. Mandal kapatın. Mandal prosese gibi bir tıklama sesi.
   
    ![EBOD mandalı bırakılıyor](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Şekil 3** EBOD modülü mandalı kapatılıyor
4. Kabloları yeniden bağlanın. Değiştirme önce mevcut tam yapılandırmasını kullanın. Aşağıdaki diyagramda ve tablo kabloları bağlanma hakkında ayrıntılı bilgi için bkz.
   
    ![Kabloyla 4U cihazınızın güç bağlantısını yapma](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Şekil 4**. Yeniden bağlanan kabloları
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Birincil kasası |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Denetleyici 0 |
   | 5 |Denetleyici 1 |
   | 6 |EBOD denetleyici 0 |
   | 7 |EBOD Denetleyici 1 |
   | 8 |EBOD muhafazası |
   | 9 |Güç Dağıtım birimleri |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-hardware-component-replacement.md).

