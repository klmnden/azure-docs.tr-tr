---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 592131bf6cca4c4c3c827de23742e8d52bcb4d1c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118295"
---
#### <a name="to-mount-initialize-and-format-a-volume"></a>Birimi bağlamak, başlatmak ve biçimlendirmek için
1. Microsoft iSCSI başlatıcısını başlatın.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Keşif** sekmesinde, **Portal Bul**’a tıklayın.
3. **Hedef Portal Bul** iletişim kutusuna iSCSI etkin ağ arabiriminin IP adresini girin ve ardından **Tamam**’a tıklayın. 
4. **iSCSI Başlatıcısı Özellikleri** penceresinin **Hedefler** sekmesinde **Bulunan hedefler**’i bulun. Cihaz durumu **Etkin Olmayan** olarak görünmelidir.
5. Hedef cihazı seçin ve ardından **Bağlan**’a tıklayın. Cihaz bağlandıktan sonra durum **Bağlandı** olarak değişmelidir. (Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için bkz. [Microsoft iSCSI Başlatıcısını Yükleme ve Yapılandırma][1]).
6. Windows konağında Windows Logo + X tuşlarına basıp **Çalıştır**’a tıklayın. 
7. **Çalıştır** iletişim kutusuna **Diskmgmt.msc** yazın. **Tamam**’a tıklayın, **Disk Yönetimi** iletişim kutusu görüntülenir. Sağ bölmede, konaktaki birimler gösterilir.
8. **Disk Yönetimi** penceresinde bağlanan birimler aşağıdaki çizimde gösterildiği gibi görünür. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.
   
     ![Birim biçimlendirmeyi başlatma](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Birime bir kez daha sağ tıklayın (disk adına tıklayın) ve ardından **Başlat**’a tıklayın.
10. Basit bir birimi biçimlendirmek için aşağıdaki adımları gerçekleştirin:
    
    1. Birimi seçip sağ tıklayın (sağ alanı tıklayın) ve **Yeni Basit Birim**’e tıklayın.
    2. Yeni Basit Birim sihirbazında birim boyutunu ve sürücü harfini belirtip birimi NTFS dosya sistemi olarak yapılandırın.
    3. 64 KB ayırma birimi boyutu belirtin. Bu ayırma birimi boyutu, StorSimple çözümde kullanılan yinelenenleri kaldırma algoritmalarıyla iyi çalışır.
    4. Hızlı biçimlendirme gerçekleştirin.

![Video var](./media/storsimple-mount-initialize-format-volume/Video_icon.png) **Video var**

Bir StorSimple biriminin nasıl bağlandığını, başlatıldığını ve biçimlendirildiğini gösteren bir video izlemek için [buraya](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/) tıklayın.

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
