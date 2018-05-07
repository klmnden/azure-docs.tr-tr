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
ms.openlocfilehash: 934585082e2832c41885874c82ab43d64a1fa361
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure yığın PKI sertifikaları dağıtımı için hazırlama
Sertifika dosyalarını [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) Azure yığın sertifika gereksinimleri eşleşen özelliklere sahip dışarı ve içeri aktarılan gerekir.


## <a name="prepare-certificates-for-deployment"></a>Sertifikaları dağıtımı için hazırlama
Hazırlama ve Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

### <a name="import-the-certificate"></a>Sertifika alma

1.  Orijinal sertifika sürümlerini kopyalamak [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) dağıtım ana bilgisayarda bir dizine. 
  > [!WARNING]
  > Zaten içe, dışa aktarılan, değiştirilmiş veya silinmiş dosyaları doğrudan CA tarafından sağlanan dosyalarından herhangi bir şekilde kopyalamayın.

2.  Sağ tıklatın ve sertifika **sertifikayı yükle** veya **PFX'i** nasıl sertifika, CA'teslim bağlı olarak.

3. İçinde **Sertifika Alma Sihirbazı'nı**seçin **yerel makine** içeri aktarma konumu olarak. **İleri**’yi seçin. Aşağıdaki ekranda sonraki yeniden tıklayın.

    ![Yerel makine içeri aktarma konumu](.\media\prepare-pki-certs\1.png)

4.  Seçin **tüm sertifika aşağıdaki depolama alanına yerleştir** ve ardından **Kurumsal güven** konumu olarak. Tıklatın **Tamam** sertifika deposu seçimi iletişim kutusunu kapatın ve ardından **sonraki**.

    ![Sertifika deposu yapılandırma](.\media\prepare-pki-certs\3.png)

    a. Bir PFX alıyorsanız, ek bir iletişim kutusu ile sunulur. Üzerinde **özel anahtar korumasını** sayfasında, sertifika dosyaları için parolayı girin ve ardından etkinleştirmek **bu anahtarı verilebilir olarak işaretle. Bu sayede yedeklemek veya daha sonraki bir zamanda anahtarlarınızı aktarım** seçeneği. **İleri**’yi seçin.

    ![Anahtarı verilebilir olarak işaretle](.\media\prepare-pki-certs\2.png)

5. Alma işlemini tamamlamak için Son'u tıklatın.

### <a name="export-the-certificate"></a>Sertifika verme

Sertifika Yöneticisi'ni MMC konsolu açın ve yerel makine sertifika deposuna bağlanın.

1. Microsoft Yönetim Konsolu'nu açın, Windows 10'da Başlat menüsünde sağ tıklayın ve sonra Çalıştır'ı tıklatın. Tür **mmc** Tamam'ı tıklatın.

2. Dosya,'ı Ekle/Kaldır ek bileşenini sonra select sertifikaları Ekle'yi tıklatın.

    ![Sertifikalar ek bileşenini ekleme](.\media\prepare-pki-certs\mmc-2.png)
 
3. Bilgisayar hesabını seçin, İleri'yi yerel bilgisayar Seç sonra son. Ekle/Kaldır ek bileşenini sayfasını kapatmak için Tamam'ı tıklatın.

    ![Sertifikalar ek bileşenini ekleme](.\media\prepare-pki-certs\mmc-3.png)

4. Sertifikalar için Gözat > Kurumsal güven > sertifika konumu. Sağ tarafta sertifikanızı gördüğünüzü doğrulayın.

5. Görev çubuğu Sertifika Yöneticisi konsolundan seçin **Eylemler** > **tüm görevler** > **verme**. **İleri**’yi seçin.

  > [!NOTE]
  > Kaç Azure yığın bağlı olarak birden çok kez bu işlemi tamamlamak, sahip sertifikaları gerekebilir.

4. Seçin **Evet, özel anahtarı dışarı aktar**ve ardından **sonraki**.

5. Dışarı aktarma dosyası biçimi bölümünde **tüm genişletilmiş özellikleri dışarı** ve ardından **sonraki**.

6. Seçin **parola** ve sertifikalar için bir parola belirtin. Dağıtım parametre olarak kullanılan bu parolayı unutmayın. **İleri**’yi seçin.

7. Bir dosya adı ve dışarı aktarmak pfx dosyası konumunu seçin. **İleri**’yi seçin.

8. **Son**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar
[PKI sertifikaları doğrula](azure-stack-validate-pki-certs.md)