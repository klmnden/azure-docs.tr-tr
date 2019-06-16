---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 1cf5bbdad555c50c418851904f36a578522843b2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159710"
---
#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a>Bulut gerecinde ortak uç noktalar oluşturmak için

1. Azure Portal’da oturum açın.
2. **Sanal Makineler**’e gidin ve bulut gereciniz olarak kullanılan sanal makineyi seçip tıklayın.
    
3. Sanal makinenize gelen ve giden trafik akışını denetlemek için bir ağ güvenlik grubu (NSG) kuralı oluşturmanız gerekir. NSG kuralı oluşturmak için aşağıdaki adımları gerçekleştirin.
    1. **Ağ güvenlik grubu**’nu seçin.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Gösterilen varsayılan ağ güvenlik grubunu seçin.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. **Gelen güvenlik kuralları**’nı seçin.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Gelen güvenlik kuralı oluşturmak için **+ Ekle**’ye tıklayın.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        Gelen güvenlik kuralı ekle dikey penceresinde:

        1. İçin **adı**, aşağıdaki uç nokta adını yazın: WinRMHttps.
        
        2. **Öncelik** için 1000’den küçük bir sayı seçin (varsayılan kuralın önceliği). Değer ne kadar yüksek olursa öncelik o kadar düşük olur.

        3. **Kaynak** seçeneğini **Herhangi biri** olarak ayarlayın.

        4. **Hizmet** için **WinRM**’yi seçin. **Protokol** seçeneği otomatik olarak **TCP**; **Bağlantı noktası aralığı** ise **5986** olarak ayarlanır.

        5. Kuralı oluşturmak için **Tamam**'a tıklayın.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Gerçekleştirmeniz gereken son adım, ağ güvenlik grubunuzu bir alt ağ ile veya belirli bir ağ arabirimiyle ilişkilendirmektir. Ağ güvenlik grubunuzu bir alt ağla ilişkilendirmek için aşağıdaki adımları gerçekleştirin.
    1. **Alt ağlar**’a gidin.
    2. **+ İlişkilendir**’e tıklayın.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Sanal ağınızı ve ardından uygun alt ağı seçin.
    4. Kuralı oluşturmak için **Tamam**'a tıklayın.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Kural oluşturulduktan sonra, Genel Sanal IP (VIP) adresini belirlemek için kuralın ayrıntılarını görüntüleyebilirsiniz. Bu adresini kaydedin.


