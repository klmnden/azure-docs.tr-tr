---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 467af776af95cf035121250fdcadd2fee65d9805
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188416"
---
#### <a name="to-create-a-volume-container"></a>Birim kapsayıcısı oluşturmak için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Tablosal cihaz listesinden bir cihazı seçin ve tıklayın. 

    ![Birim kapsayıcısı dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. Cihaz panosunda **+ Birim kapsayıcısı ekle**’ye tıklayın

    ![Birim kapsayıcısı dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. **Birim kapsayıcısı ekle** dikey penceresinde:
   
   1. Cihaz otomatik olarak seçilir.
   2. Birim kapsayıcınıza bir **Ad** verin. Adı 3 ile 32 karakter arası uzunlukta olmalıdır. Birim kapsayıcısını bir kez oluşturduktan sonra yeniden adlandıramazsınız.
   3. Cihazdan buluta gönderilen verilerin şifrelenmesini etkinleştirmek için **Bulut Depolama Şifrelemesini Etkinleştir**’i seçin.
   4. 8 ile 32 karakter uzunlukta olan bir **Bulut Depolama Şifrelemesi Anahtarı** sağlayın ve onaylayın. Bu anahtar cihaz tarafından şifrelenmiş verilere erişmek için kullanılır.
   5. Bu birim kapsayıcısı ile ilişkilendirilecek bir **Depolama Hesabı** seçin. Mevcut bir depolama hesabını veya hizmet oluşturulduğu sırasında oluşturulan varsayılan hesabı seçebilirsiniz. Hizmet aboneliğine bağlanmayan depolama hesabını belirtmek için de **Yeni ekle** seçeneğini kullanabilirsiniz.
   6. Kullanılabilir tüm bant genişliğini tüketmek istiyorsanız **Bant genişliğini belirt** açılan listesinde **Sınırsız**’ı seçin. Bant genişliği denetimlerini görevlendirmek ve 1 Mb/sn ile 1000 Mb/sn arasında bir değer belirtmek için bu seçeneği **Özel** olarak da ayarlayabilirsiniz.
      Bant genişliği kullanım bilgileriniz varsa, **Bant genişliği şablonu seçin**’i belirterek zamanlama temelinde bant genişliği ayırabilirsiniz. Adım adım bir yordam için [Bant genişliği şablonu ekleyin](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template)’e gidin.

      ![Birim kapsayıcısı dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. **Oluştur**’a tıklayın.

        ![Birim kapsayıcısı dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       Birim kapsayıcısı başarıyla oluşturulduğunda size bildirilir.

       ![Birim kapsayıcısı oluşturma bildirimi](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   Yeni oluşturulan birim kapsayıcısı, cihazınıza yönelik birim kapsayıcıları listesine eklenir.

   ![Birim kapsayıcısı ekle dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


