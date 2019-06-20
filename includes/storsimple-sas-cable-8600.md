---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 51e1fd18b52d7e215ba43be540156199fb41778e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188405"
---
#### <a name="to-attach-the-sas-cables"></a>SAS kablolu jbod'lere eklemek için
1. Birincil ve ebod belirleyin. İki kasaları ilgili kendi arka düzlem bakarak tanımlanabilir. Yönergeler için aşağıdaki resme bakın. 
   
    ![Düzlem birincil ve ebod](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Birincil ve ebod görünümü geri**
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Birincil |
   | 2 |EBOD muhafazası |
2. Birincil ve ebod seri numaraları bulun. Seri numarası etikette her kutu geri ümünü Temizle için yapıştırılır. Seri numaralarını iki kasaları üzerinde aynı olmalıdır. [Microsoft Support başvurun](../articles/storsimple/storsimple-contact-microsoft-support.md) hemen seri numaralarını eşleşmiyorsa. Seri numaralarını bulmak için aşağıdaki çizime bakın.
   
    ![Seri numarası etiketinin arka görünümü](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Seri numarası konumu**
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |Kutu, ümünü Temizle |
3. EBOD muhafazası birincil kasası için şu şekilde bağlanmak için sağlanan SAS kablolu jbod'lere kullanın:
   
   1. Muhafaza birincil ve EBOD muhafazası üzerindeki dört SAS bağlantı noktaları belirleyin. SAS bağlantı noktasını birincil muhafaza EBOD etiketlenir ve EBOD muhafazası A bağlantı noktası için aşağıdaki çizimde, kablolama SAS gösterildiği karşılık gelir.
   2. Sağlanan SAS kablolu jbod'lere EBOD bağlantı noktası için bağlantı noktası a bağlanın.
   3. EBOD Denetleyicisi 0 bağlantı noktasını EBOD Denetleyicisi 0 bağlantı noktası A bağlanması gerekir. EBOD denetleyicisi 1 üzerinde bağlantı noktası bir denetleyici 1 EBOD noktasından bağlanması gerekir. Yönergeler için aşağıdaki çizime bakın. 
      
      ![Cihazınız için SAS kabloları](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS kabloları**
      
      | Etiket | Açıklama |
      |:--- |:--- |
      | A |Birincil |
      | B |EBOD muhafazası |
      | 1 |Denetleyici 0 |
      | 2 |Denetleyici 1 |
      | 3 |EBOD Denetleyicisi 0 |
      | 4 |EBOD denetleyicisi 1 |
      | 5, 6 |SAS bağlantı noktalarını birincil kutu (EBOD etiketli) |
      | 7, 8 |SAS bağlantı noktalarını EBOD muhafazası (A bağlantı noktası) |

