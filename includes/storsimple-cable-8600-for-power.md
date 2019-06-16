---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9b9922602218280d58331a755ed0dfed7df96f40
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155904"
---
#### <a name="to-cable-your-device-for-power"></a>İçin cihazınızın güç bağlantısını yapın
> [!NOTE]
> StorSimple Cihazınızda iki kasaları yedekli PCMs içerir. Her kasa için PCMs yüklenmeli ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlandınız.
> 
> 

1. Tüm PCMs güç anahtarlarda OFF konumda olduğundan emin olun.
2. Birincil muhafaza güç kablosu her iki PCMs için bağlanın. Güç kablosu aşağıda kırmızı power kablolama diyagramdaki tanımlanır.
3. Birincil kasada iki PCMs ayrı güç kaynakları kullandığınızdan emin olun.
4. Güç kablosu raf dağıtım birimleri gücüyle diyagram kablo gücünü gösterildiği gibi ekleyin.
5. EBOD muhafazası için 2-4 arası adımları yineleyin.
6. EBOD muhafazası üzerinde her PCM açık konuma üzerinde güç anahtarı döndürerek açın.
7. EBOD muhafazası yeşil LED'lerini arkasındaki EBOD denetleyicinin üzerinde etkinleştirilir denetleyerek açık olduğunu doğrulayın.
8. Birincil muhafaza açık konuma her PCM anahtar döndürerek açın.
9. Sistem üzerinde LED'lerini açık olan cihaz denetleyicisi sağlayarak olduğunu doğrulayın.
10. Dört LED'lerini EBOD denetleyicisi SAS bağlantı noktasını yanındaki yeşil olduğunu doğrulayarak EBOD denetleyicisi ve cihaz denetleyicisi arasındaki bağlantıyı etkin olduğundan emin olun.
    
    > [!IMPORTANT]
    > Sisteminiz için yüksek kullanılabilirlik sağlamak için kesinlikle Aşağıdaki diyagramda da görüldüğü düzeni kablo gücüne bağlı kalmanızı öneririz.
    > 
    > 
    
    ![Kabloyla 4U cihazınızın güç bağlantısını yapın](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Güç kabloları**
    
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
    | 9 |Pdu'lar |

