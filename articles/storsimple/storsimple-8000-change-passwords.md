---
title: StorSimple parolalarını değiştirme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti, StorSimple Snapshot Manager ve cihaz Yöneticisi parolalarını değiştirmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 7762f8499c67672f0a2ffed99e98baea4c940fa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874811"
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a>StorSimple parolalarını değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
Azure portalı **aygıt ayarları** seçeneği StorSimple cihaz Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazda yapılandırabilirsiniz tüm aygıt parametreleri içerir. Bu öğretici nasıl kullanabileceğinizi açıklar **güvenlik** altında seçeneği **aygıt ayarları** aygıt yöneticiniz veya StorSimple Snapshot Manager parolasını değiştirmek için.

## <a name="change-the-device-administrator-password"></a>Cihaz yöneticisi parolasını değiştirme
StorSimple cihazı erişmek için Windows PowerShell arabirimini kullandığınızda, bir cihaz Yöneticisi parolası girmeniz gerekir. İlk StorSimple cihazı ile bir hizmeti kaydedildikten sonra bu arabirim için varsayılan paroladır *Parola1*. Verilerinizin güvenliği için kayıt işleminin sonunda bu parolayı değiştirmeniz gereklidir. Kayıt işleminden çıkmak için bu parolayı değiştirmeden olamaz. Daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell üzerinden cihazı](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Windows PowerShell arabirimi üzerinden kayıt sırasında ilk ayarlandı parolayı daha sonra Azure portalı üzerinden değiştirilebilir. Cihaz Yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password"></a>Cihaz Yöneticisi parolasını değiştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Aygıtları Tablo listesi, seçin ve parola, değiştirmek istediğiniz aygıt'ı tıklatın.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. İçinde **ayarları** dikey penceresinde, Git **Aygıt Ayarları > Güvenlik**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **parola** cihaz Yöneticisi parolasını değiştirmek için.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. İçinde **parola** dikey penceresinde, 8'den 15 karakter içeren bir yönetici parolasını belirtin. Parola, 3 veya daha çok büyük harf, küçük harfler, sayısal ve özel karakter bir bileşimi olmalıdır.

6. Parolayı onaylayın.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Tıklatın **kaydetmek** tıklatıp onaylamanız istendiğinde, **Evet**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Cihaz Yöneticisi parolası şimdi güncelleştirilmesi gerekir. Windows PowerShell arabirimi erişmek için bu değiştirilmiş parolayı kullanabilirsiniz.

## <a name="set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlayın
StorSimple Snapshot Manager yazılımı Windows ana bilgisayarınıza bulunur ve yöneticilerin yerel ve bulut anlık görüntüleri biçiminde StorSimple cihazınızın yedeklerini yönetmelerine olanak tanır.

Bir cihaz StorSimple anlık görüntü Yöneticisi'nde yapılandırırken, cihazı IP adresi ve parola depolama cihazınızın kimlik doğrulaması sağlamak için istenir.

Azure portalı üzerinden StorSimple Snapshot Manager parolasını değiştirmek ya da ayarlayın. Ayarlamak veya StorSimple Snapshot Manager parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlamak için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Aygıtları Tablo listesi, seçin ve StorSimple Snapshot Manager parolasını ayarlamak veya değiştirmek istiyorsanız cihaz seçeneğine tıklayın.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. İçinde **ayarları** dikey penceresinde, Git **Aygıt Ayarları > Güvenlik**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **parola** ayarlamak veya StorSimple Snapshot Manager parolasını değiştirmek için.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. İçinde **parola** dikey penceresinde, 14 veya 15 karakter olan bir parola girin. Parola 3 veya daha çok büyük harf, küçük harfler, sayısal ve özel karakter bir bileşimi içerdiğinden emin olun.

6. Parolayı onaylayın.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Tıklatın **kaydetmek** tıklatıp onaylamanız istendiğinde, **Evet**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

StorSimple Snapshot Manager parolası şimdi güncelleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinmek [cihaz yapılandırmanızı değiştirme](storsimple-8000-modify-device-config.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

