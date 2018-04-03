---
title: Azure yığın ortak anahtar altyapısı sertifikaları Azure tümleşik yığını sistemleri dağıtım için hazırlanma | Microsoft Docs
description: Azure yığın PKI sertifikaları Azure tümleşik yığını sistemler için hazırlamayı açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dadb443f8b7739e3a18c0d3beb558d8c42e9d19c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure yığın PKI sertifikaları dağıtımı için hazırlama
Sertifika dosyalarını [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) Azure yığın sertifika gereksinimleri eşleşen özelliklere sahip dışarı ve içeri aktarılan gerekir.


## <a name="prepare-certificates-for-deployment"></a>Sertifikaları dağıtımı için hazırlama
Hazırlama ve Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

1.  Orijinal sertifika sürümlerini kopyalamak [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) dağıtım ana bilgisayarda bir dizine. 
  > [!WARNING]
  > Zaten içe, dışa aktarılan, değiştirilmiş veya silinmiş dosyaları doğrudan CA tarafından sağlanan dosyalarından herhangi bir şekilde kopyalamayın.

2.  Yerel Makine sertifika deposuna sertifikaların içeri aktarın:

    a.  Sağ tıklatın ve sertifika **PFX'i**.

    b.  İçinde **Sertifika Alma Sihirbazı'nı**seçin **yerel makine** içeri aktarma konumu olarak. **İleri**’yi seçin.

    ![Yerel makine içeri aktarma konumu](.\media\prepare-pki-certs\1.png)

    c.  Seçin **sonraki** üzerinde **içeri aktarmak için dosya** sayfası.

    d.  Üzerinde **özel anahtar korumasını** sayfasında, sertifika dosyaları için parolayı girin ve ardından etkinleştirmek **bu anahtarı verilebilir olarak işaretle. Bu sayede yedeklemek veya daha sonraki bir zamanda anahtarlarınızı aktarım** seçeneği. **İleri**’yi seçin.

    ![Anahtarı verilebilir olarak işaretle](.\media\prepare-pki-certs\2.png)

    e.  Seçin **tüm sertifika aşağıdaki depolama alanına yerleştir** ve ardından **Kurumsal güven** konumu olarak. Tıklatın **Tamam** sertifika deposu seçimi iletişim kutusunu kapatın ve ardından **sonraki**.

    ![Sertifika deposu yapılandırma](.\media\prepare-pki-certs\3.png)

  f.    Tıklatın **son** Sertifika Alma Sihirbazı'nı tamamlayın.

  g.    Dağıtımınız için sağlama tüm sertifikalar için işlemi yineleyin.

3. Sertifikayı PFX dosyası biçimi Azure yığın gereksinimleriyle verin:

  a.    Sertifika Yöneticisi'ni MMC konsolu açın ve yerel makine sertifika deposuna bağlanın.

  b.    Git **Kurumsal güven** dizin.

  c.    2. adımda aldığınız sertifikalardan birini seçin.

  d.    Görev çubuğu Sertifika Yöneticisi konsolundan seçin **Eylemler** > **tüm görevler** > **verme**.

  e.    **İleri**’yi seçin.

  f.    Seçin **Evet, özel anahtarı dışarı aktar**ve ardından **sonraki**.

  g.    Dışarı aktarma dosyası biçimi bölümünde **tüm genişletilmiş özellikleri dışarı** ve ardından **sonraki**.

  h.    Seçin **parola** ve sertifikalar için bir parola belirtin. Dağıtım parametre olarak kullanılan bu parolayı unutmayın. **İleri**’yi seçin.

  i.    Bir dosya adı ve dışarı aktarmak pfx dosyası konumunu seçin. **İleri**’yi seçin.

  j.    **Son**’u seçin.

  k.    2. adımda, dağıtımınız için alınan tüm sertifikalar için bu işlemi yineleyin.

## <a name="next-steps"></a>Sonraki adımlar
[PKI sertifikaları doğrula](validate-pki-certs.md)