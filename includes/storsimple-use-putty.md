---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a5c62c67cd401c043352b06e6e6070a7fc0f1296
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118278"
---
#### <a name="to-connect-through-the-serial-console"></a>Seri konsol üzerinden bağlanmak için
1. Seri kabloyu cihaza bağlayın (doğrudan veya bir USB seri bağdaştırıcısı aracılığıyla).
2. **Denetim Masası**’nı açın; sonra da **Cihaz Yöneticisi**'ni açın.
3. COM bağlantı noktasını aşağıdaki çizimde gösterildiği gibi belirleyin.
   
     ![Seri konsol üzerinden bağlanma](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. PuTTY’yi başlatın. 
5. Sağ bölmede, **Bağlantı türü**’nü **Seri** olarak değiştirin.
6. Sağ bölmede, uygun COM bağlantı noktasını yazın. Seri yapılandırma parametrelerinin buradaki gibi ayarlandığından emin olun:
   
   * Hızı: 115,200
   * Veri bitleri: 8
   * Dur bitleri: 1
   * Eşlik: None
   * Akış denetimi: None
     
     Bu ayarlar aşağıdaki çizimde gösterilmiştir.
     
     ![PuTTY ayarları](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Var sayılan akış denetimi ayarı işlemezse akış denetimini XON/XOFF olarak ayarlamayı deneyin.
     > 
     > 
7. Seri oturum başlatmak için **Aç**’a tıklayın.

