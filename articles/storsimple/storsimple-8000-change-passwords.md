---
title: Kullanarak StorSimple parolalarını değiştirme | Microsoft Docs
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
ms.openlocfilehash: ab874bbdcd47a4bfa9abfba721afa46d0f23a338
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60638040"
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a>StorSimple cihaz Yöneticisi hizmetini kullanarak StorSimple parolalarını değiştirmek için kullanın

## <a name="overview"></a>Genel Bakış
Azure portalında **cihaz ayarları** seçeneği, StorSimple cihaz Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazında yeniden tüm cihaz parametreleri içerir. Bu öğreticide nasıl kullanabileceğinizi açıklar **güvenlik** altındaki **cihaz ayarları** , cihaz yöneticisini veya StorSimple Snapshot Manager parolasını değiştirmek için.

## <a name="change-the-device-administrator-password"></a>Cihaz yöneticisi parolasını değiştirme
StorSimple cihazına erişmek için Windows PowerShell arabirimini kullandığınızda, bir cihaz Yöneticisi parolası girmeniz istenir. Bir hizmetle ilk StorSimple cihazı kaydettiğinizde, bu arabirim için varsayılan paroladır *Password1*. Verilerinizin güvenliği için kayıt işleminin sonunda bu parolayı değiştirmeniz gereklidir. Kayıt işleminden çıkmak için bu parolayı değiştirmeden olamaz. Daha fazla bilgi için [3. adım: Yapılandırma ve StorSimple için Windows PowerShell üzerinden cihazı kaydetme](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

İlk kayıt sırasında Windows PowerShell arabirimi aracılığıyla ayarlanan parolayı daha sonra Azure portalı üzerinden değiştirilebilir. Cihaz Yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password"></a>Cihaz Yöneticisi parolasını değiştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Tablosal cihaz seçin ve parolasını değiştirmek istediğiniz cihaza tıklayın.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. İçinde **ayarları** dikey penceresinde, Git **cihaz Ayarları > Güvenlik**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **parola** cihaz Yöneticisi parolasını değiştirmek için.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. İçinde **parola** dikey penceresinde, 8 ila 15 karakter içeren bir yönetici parolasını belirtin. Parola, 3 veya daha fazla büyük harf, küçük harfler, sayısal ve özel karakter birleşimi olmalıdır.

6. Parolayı onaylayın.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Tıklayın **Kaydet** onaylamanız istendiğinde tıklayın **Evet**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Artık cihaz Yönetici parolasının güncelleştirilmesi gerekir. Windows PowerShell arabirimine değiştirilmiş bu parolayı kullanabilirsiniz.

## <a name="set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlayın
StorSimple Snapshot Manager yazılımı Windows ana bilgisayarınıza bulunur ve yöneticilerin yerel ve bulut anlık görüntüleri biçiminde StorSimple cihazınızın yedeklerini yönetmelerine olanak tanır.

StorSimple Snapshot Manager'da bir cihazı yapılandırdığınız sırada, cihaz IP adresi ve depolama cihazınızın kimlik doğrulaması için parola sağlamanız istenir.

Azure portalı üzerinden için StorSimple Snapshot Manager parolasını değiştirmek ya da ayarlayın. StorSimple Snapshot Manager parolasını ayarlama veya değiştirme için aşağıdaki adımları gerçekleştirin.

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlamak için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Tablosal cihaz seçin ve StorSimple Snapshot Manager parolasını ayarlamak veya değiştirmek istediğiniz cihaza tıklayın.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. İçinde **ayarları** dikey penceresinde, Git **cihaz Ayarları > Güvenlik**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **parola** StorSimple Snapshot Manager parolasını ayarlama veya değiştirme için.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. İçinde **parola** dikey penceresinde, 14 veya 15 karakter bir parola girin. Parola birlikte 3 veya daha fazla büyük harf, küçük harfler, sayısal ve özel karakter içerdiğinden emin olun.

6. Parolayı onaylayın.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Tıklayın **Kaydet** onaylamanız istendiğinde tıklayın **Evet**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

StorSimple Snapshot Manager parolası artık güncelleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinin [cihaz yapılandırmanızı değiştirme](storsimple-8000-modify-device-config.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

