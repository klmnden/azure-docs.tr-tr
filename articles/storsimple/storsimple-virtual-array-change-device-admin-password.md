---
title: StorSimple sanal dizisi cihaz Yöneticisi parolasını değiştirme | Microsoft Docs
description: Cihaz Yöneticisi parolasını değiştirmek için Azure portalı veya StorSimple sanal dizisi web kullanıcı Arabirimi kullanmayı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5308badf439254062a8aefca1840eb21bc234ace
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60580409"
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>StorSimple sanal dizisi cihaz Yöneticisi parolasını StorSimple cihaz Yöneticisi aracılığıyla değiştirme

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizisi erişmek için Windows PowerShell arabirimini kullandığınızda, bir cihaz Yöneticisi parolası girmeniz istenir. StorSimple cihazı ilk sağlanan çalışmaya ve varsayılan paroladır *Password1*. Verilerinizin güvenliği için ilk kez oturum açın ve bu parolayı değiştirmeniz gereklidir varsayılan parolasının süresi dolar.

Cihaz, üretim ortamına dağıtıldıktan sonra herhangi bir zamanda cihaz Yöneticisi parolasını değiştirmek için yerel web kullanıcı Arabirimi veya Azure portalını kullanabilirsiniz. Bu yordamların her biri, bu makalede açıklanmıştır.

 ![Cihazlar dikey penceresi](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>Parolayı değiştirmek için Azure portalını kullanma

Azure Portalı aracılığıyla da cihaz Yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Azure portal aracılığıyla cihaz Yöneticisi parolasını değiştirmek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **Yönetim** bölümünde **cihazları**. Bu açılır **cihazları** tüm StorSimple Virtual Array cihazlarına listeleyen bir dikey pencere.

2. İçinde **cihazları** dikey penceresinde, parola değişikliği gerektiren cihaza çift tıklayın.

3. İçinde **ayarları** , cihazınız için dikey **güvenlik**.

4. İçinde **güvenlik ayarları** dikey penceresinde aşağıdakileri yapın:
   
   1. Ekranı aşağı kaydırarak **cihaz Yöneticisi parolasını** bölümü. 8 ila 15 karakter içeren bir yönetici parolasını belirtin.
   2. Parolayı onaylayın.
   3. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

Cihaz Yöneticisi parolası artık güncelleştirilir. Değiştirilen bu parola, yerel olarak kullanıcının cihaza erişmek için kullanabilirsiniz.

![Güvenlik ayarları dikey penceresi](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>Parolayı değiştirmek için yerel web kullanıcı arabirimini kullanın

Yerel web UI aracılığıyla cihaz Yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Yerel web UI aracılığıyla cihaz Yöneticisi parolasını değiştirmek için

1. Yerel web kullanıcı Arabirimi, tıklayın **Bakım** > **parola değişikliği** cihazınız için.
   
    ![password1 değiştirme](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Girin **geçerli parola**.
3. Sağlayan bir **yeni parola**. Parola en az 8 karakter uzunluğunda olmalıdır. 3 / 4'ünü içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.
   
    Parolanızı son 24 parolaları aynı olamayacağını unutmayın.
4. Onaylamak için parolayı yeniden girin.
   
    ![Parola2 değiştirme](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Sayfanın en altında tıklayın **Uygula**. Yeni parolayı şimdi uygulanır. Parola değiştirme başarılı olmazsa, aşağıdaki hatayı görürsünüz:
   
    ![parola hatası](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Parola başarıyla güncelleştirildikten sonra size bildirilir. Ardından, yerel olarak kullanıcının cihaza erişmek için bu değiştirilen parolayı kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [StorSimple Virtual Array'iniz yönetmek](storsimple-ova-web-ui-admin.md).

