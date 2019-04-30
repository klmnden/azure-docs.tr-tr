---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 48a3326dbe0e9eed4a5490e720248555586d189c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61488953"
---
### <a name="to-take-a-backup"></a>Yedek almak için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Tablosal cihaz listesinden cihazınızı seçmek için tıklayın ve **Tüm ayarlar**’a tıklayın. **Ayarlar** dikey penceresinde **Ayarlar > Yönet > Yedekleme ilkesi**’ne gidin.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu1.png)

2. **Yedekleme ilkesi** dikey penceresinde **+ İlke ekle**’ye tıklayın.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu2.png)

3. **Yedekleme ilkesi oluştur** dikey penceresinde, yedekleme ilkenize 3 - 150 arası karakterden oluşan bir ad verin.

4. Yedeği alınacak birimleri seçin. Birden fazla birim seçerseniz, kilitlenmeyle tutarlı yedek oluşturmak için bu birimler birlikte gruplandırılır.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu4.png)

5. **İlk zamanlamayı ekle** dikey penceresinde:

    1. Yedekleme türünü seçin. Daha hızlı geri yükleme gerçekleştirebilmek için **Yerel** anlık görüntüyü seçin. Veri dayanıklılığı için **Bulut** anlık görüntüsünü seçin.
    2. Yedekleme sıklığını dakika, saat, gün veya hafta cinsinden belirtin.
    3. Elde tutma süresini seçin. Elde tutma seçimleri yedekleme sıklığına bağlıdır. Örneğin, günlük ilkesi için elde tutma İlkesi hafta cinsinden belirtilebilirken, aylık ilkesi için elde tutma ay cinsindendir.
    4. Yedekleme ilkesinin başlangıç saatini ve tarihini seçin.
    5. Yedekleme ilkesini oluşturmak için **Tamam**’a tıklayın.

        ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Yedekleme ilkesi oluşturma işlemini başlatmak için **Oluştur**’a tıklayın. Yedekleme ilkesi başarıyla oluşturulduğunda size bildirilir. Yedekleme ilkeleri listesi de güncelleştirilir.
      
      ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Artık, birim verilerinizin zamanlanmış yedeklerini oluşturan bir yedekleme ilkesine sahipsiniz.




