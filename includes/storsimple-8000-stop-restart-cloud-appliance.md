---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: c10482029e6cfce7063d205161fed54030919c48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159675"
---
#### <a name="to-stop-and-start-a-cloud-appliance"></a>Bulut gerecini başlatmak ve durdurmak için

1. Bulut gerecini durdurmak için gerecinizin sanal makinesine gidin.
    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Komut çubuğundan **Durdur**'a tıklayın.

    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. Bir VM durdurulduğunda serbest bırakılır. Bulut gereci durdurulduğu sırada gerecin durumu **Serbest bırakılıyor** olur. Bulut gereci durdurulduktan sonra gerecin durumu **Durduruldu (serbest bırakıldı)** olur.

    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. VM durdurulduktan sonra VM’yi başlatmak için **Başlat**’a (düğme kullanılabilir hale gelir) tıklayın. Bulut gereci başlatıldıktan sonra gerecin durumu **Başlatıldı** olur.

    ![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

Bulut gerecini durdurmak ve başlatmak için aşağıdaki cmdlet'leri kullanın.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a>Bulut gerecini yeniden başlatmak için

Bulut gerecini yeniden başlatmak için gerecinizin sanal makinesine gidin. Komut çubuğundan **Yeniden Başlat**'a tıklayın. Sorulduğunda yeniden başlatma işlemini onaylayın. Bulut gereci kullanıma hazır olduğunda gerecin durumu **Çalışıyor** olur.

![StorSimple Cloud Appliance Sanal Makinesi](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

Bulut gerecini yeniden başlatmak için aşağıdaki cmdlet'i kullanın.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

