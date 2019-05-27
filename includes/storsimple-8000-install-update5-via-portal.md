---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d2df7388018c463ba58b57be46945915210b84b6
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66171134"
---
#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure portalından bir güncelleştirmeyi yüklemek için

1. StorSimple hizmet sayfasında cihazınızı seçin.

    ![Cihaz seç](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. Gidin **cihaz ayarları** > **cihaz güncelleştirmeleri**.

    ![Cihaz güncelleştirmeleri tıklayın](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. Yeni güncelleştirmeler varsa, bir bildirim görüntülenir. Alternatif olarak **cihaz güncelleştirmeleri** dikey penceresinde tıklayın **Güncelleştirmeleri tara**. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. İş başarıyla tamamlandığında size bildirilir.

    ![Cihaz güncelleştirmeleri tıklayın](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. Bir güncelleştirmeyi cihazınıza uygulamadan önce sürüm notlarını gözden geçirmeniz önerilir. Güncelleştirmeleri uygulamak için tıklayın **Güncelleştirmeleri Yükle**. İçinde **düzenli güncelleştirmeleri onaylama** dikey penceresinde, güncelleştirmeleri uygulamadan önce tamamlamak için önkoşulları gözden geçirin. Cihazı güncelleştirin ve ardından hazır olduğunu göstermek için onay kutusunu işaretleyin **yükleme**.

    ![Cihaz güncelleştirmeleri tıklayın](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. Bir dizi önkoşul denetimi başlatılır. Bu denetimler şunlardır:
   
   * Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamaya yönelik **denetleyici durumu denetimleri**.
   * StorSimple cihazınızdaki tüm donanım bileşenlerinin sağlıklı olduğunu doğrulamaya yönelik **donanım bileşeni durum denetimleri**.
   * DATA 0’ın cihazınızda etkin olduğunu doğrulamaya yönelik **DATA 0 denetimleri**. Bu arabirim etkin değilse etkinleştirmeniz ve sonra yeniden denemeniz gerekir.

     Güncelleştirme indirildi ve yalnızca tüm denetimler başarıyla tamamlanırsa yüklü. Denetimler devam ederken size bildirilir. Ardından ön denetimler başarısız olursa hatanın nedenlerini ile sağlanır. Sorunları gidermek ve ardından işlemi yeniden deneyin. Bu sorunları kendi başınıza çözemiyorsanız Microsoft Desteği’ne başvurmanız gerekebilir.

7. Ön denetimler başarıyla tamamlandıktan sonra bir güncelleştirme işi oluşturulur. Güncelleştirme işi başarıyla oluşturulduğunda size bildirilir.
   
    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    Bundan sonra güncelleştirme, cihazınıza uygulanır.

9. Güncelleştirmenin tamamlanması birkaç saat sürer. Güncelleştirme işini seçin ve **Ayrıntılar**’a tıklayarak dilediğiniz zaman işin ayrıntılarını görüntüleyin.

    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update8.png)

     Ayrıca, güncelleştirme işinin ilerleme durumunu izleyebilirsiniz **cihaz Ayarları > işler**. Üzerinde **işleri** dikey penceresinde güncelleştirmenin ilerleme durumunu görebilirsiniz.

     ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. İş tamamlandıktan sonra gidin **cihaz Ayarları > cihaz güncelleştirmeleri**. Yazılım sürümü artık güncelleştirilmelidir.

