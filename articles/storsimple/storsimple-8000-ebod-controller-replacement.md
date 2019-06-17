---
title: StorSimple 8600 EBOD denetleyicisini değiştirme | Microsoft Docs
description: Kaldırın ve StorSimple 8600 cihazında birini veya ikisini EBOD denetleyicisi değiştirin açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: b05d1f36d1e74b3d915e216676859654fbcbacf3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60578700"
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>StorSimple Cihazınızda bir EBOD denetleyicisini değiştirme

## <a name="overview"></a>Genel Bakış
Bu öğretici bir hatalı EBOD Denetleyici Modülü Microsoft Azure StorSimple Cihazınızda nasıl değiştirileceğini açıklar. Bir EBOD Denetleyici Modülü değiştirilecek şekilde gerekir:

* Hatalı EBOD denetleyicisini kaldırma
* Yeni bir EBOD denetleyicisi yükleme

Başlamadan önce aşağıdaki bilgileri göz önünde bulundurun:

* Boş EBOD modüller tüm kullanılmayan yuvalarını eklenmelidir. Bir yuva açık olarak bırakılırsa muhafaza düzgün seyrek.
* EBOD denetleyicisi takılıp çıkarılabilen ve kaldırıldı veya değiştirilmesi. Değiştirme elde edene kadar başarısız bir modül kaldırmayın. Değiştirme işlemini başlattığınızda, 10 dakika içinde tamamlanmalıdır.

> [!IMPORTANT]
> Herhangi bir StorSimple bileşeni değiştirmek veya kaldırmak denemeden önce gözden geçirmenizi emin olun [güvenliği simgesi kuralları](storsimple-safety.md#safety-icon-conventions) ve diğer [güvenlik önlemlerini](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>EBOD denetleyicisini kaldırma
StorSimple Cihazınızı başarısız EBOD denetleyicisi modülünde değiştirmeden önce bir EBOD denetleyici modülü etkin ve çalışıyor olduğundan emin olun. Aşağıdaki yordamı ve tabloyu EBOD denetleyicisi modülün nasıl kaldırılacağı açıklanmaktadır.

#### <a name="to-remove-an-ebod-module"></a>EBOD modülünü kaldırmak için
1. Azure portalı açın.
2. Cihazınıza gidin ve gidin **ayarları** > **donanım sistem durumu**, ışığı etkin EBOD denetleyicisi modülü için durum yeşil olduğunu doğrulayın ve başarısız olan EBOD denetleyici için bir ışığı kırmızı modülüdür.
3. Cihazın arkasına başarısız EBOD Denetleyici Modülü bulun.
4. EBOD denetleyicisi modülü sistem dışı EBOD modülü almadan önce denetleyiciyi bağlama kablolar kaldırın.
5. Denetleyiciye bağlı EBOD Denetleyici Modülü tam SAS bağlantı noktasını not edin. EBOD modülü değiştirdikten sonra sistem için bu yapılandırmayı geri yüklemek için gerekli olacaktır.
   
   > [!NOTE]
   > Genellikle, bu olarak etiketli bir, bağlantı noktası olacak **içindeki konak** Aşağıdaki diyagramda.
   
    ![Devre kartına, EBOD denetleyicisi](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Şekil 1** arka of EBOD Modülü
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Hata Işığı |
   | 2 |Güç ışığı |
   | 3 |SAS Bağlayıcısı |
   | 4 |SAS LED'leri |
   | 5 |Seri bağlantı noktaları yalnızca factory kullanımı |
   | 6 |(Konak), bağlantı noktası |
   | 7 |Bağlantı noktası B (ana bilgisayarı devre dışı) |
   | 8 |Bağlantı noktası C (yalnızca Fabrika kullanın) |

## <a name="install-a-new-ebod-controller"></a>Yeni bir EBOD denetleyicisi yükleme
Aşağıdaki yordamı ve tabloyu kullanarak StorSimple Cihazınızı bir EBOD denetleyicisi modülünü yükleme işlemleri açıklanmaktadır.

#### <a name="to-install-an-ebod-controller"></a>EBOD denetleyicisi yüklemek için
1. EBOD cihaz arabirimi bağlayıcıya özellikle hasar olup olmadığını denetleyin. Tüm PIN'ler Eğilmiş yeni EBOD denetleyicisi yüklemeyin.
2. Mandal ilgisini kadar açık konuma tutma ile kutu modülü kaydırın.
   
    ![EBOD denetleyicisi yükleniyor](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Şekil 2** EBOD denetleyicisi modülünü yükleme
3. Mandal kapatın. Mandal ilgilenir gibi bir tıklama sesi.
   
    ![EBOD mandalı bırakılıyor](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Şekil 3** EBOD modülü mandalı kapatılıyor
4. Kabloları bağlanın. Bu değişiklik önce mevcut tam yapılandırma kullanın. Aşağıdaki diyagramda ve tablo kabloları bağlanma hakkında ayrıntılı bilgi için bkz.
   
    ![Kabloyla 4U cihazınızın güç bağlantısını yapın](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Şekil 4**. Yeniden bağlanan kabloları
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Birincil |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Denetleyici 0 |
   | 5 |Denetleyici 1 |
   | 6 |EBOD Denetleyicisi 0 |
   | 7 |EBOD denetleyicisi 1 |
   | 8 |EBOD muhafazası |
   | 9 |Güç Dağıtım birimleri |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

