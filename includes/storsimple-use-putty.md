---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 50a5c8d515e27db7c2c65b484cdecad8ff00baf8
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164183"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a>Seri konsol üzerinden bağlanmak için
1. Seri kabloyu cihaza bağlayın (doğrudan veya bir USB seri bağdaştırıcısı aracılığıyla).
2. **Denetim Masası**’nı açın; sonra da **Cihaz Yöneticisi**'ni açın.
3. COM bağlantı noktasını aşağıdaki çizimde gösterildiği gibi belirleyin.
   
     ![Seri konsol üzerinden bağlanma](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. PuTTY’yi başlatın. 
5. Sağ bölmede, **Bağlantı türü**’nü **Seri** olarak değiştirin.
6. Sağ bölmede, uygun COM bağlantı noktasını yazın. Seri yapılandırma parametrelerinin buradaki gibi ayarlandığından emin olun:
   
   * Hız: 115.200
   * Veri bitleri: 8
   * Dur bitleri: 1
   * Eşlik: Yok
   * Akış denetimi: Yok
     
     Bu ayarlar aşağıdaki çizimde gösterilmiştir.
     
     ![PuTTY ayarları](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Var sayılan akış denetimi ayarı işlemezse akış denetimini XON/XOFF olarak ayarlamayı deneyin.
     > 
     > 
7. Seri oturum başlatmak için **Aç**’a tıklayın.

