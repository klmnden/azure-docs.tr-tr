---
title: "Aracılığıyla StorSimple cihaz Yöneticisi parolalarını değiştirme | Microsoft Docs"
description: "StorSimple Yöneticisi hizmeti, StorSimple Snapshot Manager ve cihaz Yöneticisi parolalarını değiştirmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d890b59595628ca3eeff1df258847c2bb54d29ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>StorSimple parolalarını değiştirmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Klasik Azure portalı **yapılandırma** sayfası bir StorSimple Yöneticisi hizmeti tarafından yönetilen bir StorSimple cihazda yapılandırabilirsiniz tüm aygıt parametreleri içerir. Bu öğretici nasıl kullanabileceğinizi açıklar **yapılandırma** aygıt yöneticiniz veya StorSimple Snapshot Manager parolasını değiştirmek için sayfa.

## <a name="change-the-device-administrator-password"></a>Cihaz yöneticisi parolasını değiştirme
StorSimple cihazı erişmek için Windows PowerShell arabirimini kullandığınızda, bir cihaz Yöneticisi parolası girmeniz gerekir. İlk StorSimple cihazı ile bir hizmeti kaydedildikten sonra bu arabirim için varsayılan paroladır *Parola1*. Verilerinizin güvenliği için kayıt işleminin sonunda bu parolayı değiştirmeniz gereklidir. Kayıt işleminden çıkmak için bu parolayı değiştirmeden olamaz. Daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell üzerinden cihazı](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Windows PowerShell arabirimi üzerinden kayıt sırasında ilk ayarlandı parola sonra Klasik Azure portalı değiştirilebilir. Cihaz Yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password"></a>Cihaz Yöneticisi parolasını değiştirmek için
1. Klasik portalda tıklatın **aygıtları** > **yapılandırma** cihazınız için.
2. Ekranı aşağı kaydırarak **cihaz Yöneticisi parolasını** bölümü. 8'den 15 karakter içeren bir yönetici parolasını belirtin. Parola, 3 veya daha çok büyük harf, küçük harfler, sayısal ve özel karakter bir bileşimi olmalıdır.
3. Parolayı onaylayın.
4. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

Cihaz Yöneticisi parolası şimdi güncelleştirilmesi gerekir. Windows PowerShell arabirimi erişmek için bu değiştirilmiş parolayı kullanabilirsiniz.

## <a name="change-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını değiştirme
StorSimple Snapshot Manager yazılımı Windows ana bilgisayarınıza bulunur ve yöneticilerin yerel ve bulut anlık görüntüleri biçiminde StorSimple cihazınızın yedeklerini yönetmelerine olanak tanır.

Bir cihaz StorSimple anlık görüntü Yöneticisi'nde yapılandırırken, cihazı IP adresi ve parola depolama cihazınızın kimlik doğrulaması sağlamak için istenir. Bu parolayı ilk Windows PowerShell arabirimi üzerinden yapılandırılır. Daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell üzerinden cihazı](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Windows PowerShell arabirimi üzerinden kayıt sırasında ilk ayarlandı parola sonra Klasik portalı yoluyla değiştirilebilir. StorSimple Snapshot Manager parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını değiştirmek için
1. Klasik portalda tıklatın **aygıtları** > **yapılandırma** cihazınız için.
2. Ekranı aşağı kaydırarak **StorSimple Snapshot Manager** bölümü. 14 veya 15 karakter olan bir parola girin. Parola 3 veya daha çok büyük harf, küçük harfler, sayısal ve özel karakter bir bileşimi içerdiğinden emin olun.
3. Parolayı onaylayın.
4. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

StorSimple Snapshot Manager parolası şimdi güncelleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-security.md).
* Daha fazla bilgi edinmek [cihaz yapılandırmanızı değiştirme](storsimple-modify-device-config.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

