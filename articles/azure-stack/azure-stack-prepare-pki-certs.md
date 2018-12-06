---
title: Azure Stack ortak anahtar altyapısı sertifikaları Azure Stack tümleşik sistemleri dağıtımı için hazırlama | Microsoft Docs
description: Azure Stack PKI sertifikaları için Azure Stack tümleşik sistemleri hazırlanma işlemi açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: c26dcbc2fab7cff3134846234cafaa481fd5ba1a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972047"
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure Stack PKI sertifikaları dağıtımı için hazırlama
Sertifika dosyalarını [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) içeri ve dışarı Azure yığını'nın sertifika gereksinimleri eşleşen özelliklere sahip.


## <a name="prepare-certificates-for-deployment"></a>Sertifikaları dağıtımı için hazırlama
Hazırlama ve Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

### <a name="import-the-certificate"></a>Sertifika içeri aktarma

1.  Orijinal sertifika sürümleri Kopyala [seçim, CA'dan alınan](azure-stack-get-pki-certs.md) dağıtım ana bilgisayardaki bir dizine. 
  > [!WARNING]
  > Doğrudan CA tarafından sağlanan dosyalarından herhangi bir şekilde, zaten içeri aktarıldı, dışarı, değiştirilmiş veya silinmiş dosyalar kopyalamayın.

1.  Sağ tıklatın ve sertifika **sertifikayı yükle** veya **PFX'i** bağlı olarak sertifika Yetkilinizden nasıl teslim edildi.

1. İçinde **Sertifika Alma Sihirbazı'nı**seçin **yerel makine** içeri aktarma konumu olarak. **İleri**’yi seçin. Aşağıdaki ekranda, sonraki yeniden tıklayın.

    ![Yerel makine içeri aktarma konumu](./media/prepare-pki-certs/1.png)

1.  Seçin **tüm sertifika aşağıdaki depolama alanına yerleştir** seçip **Kurumsal güven** konumu olarak. Tıklayın **Tamam** sertifika deposu seçimi iletişim kutusunu kapatın ve ardından **sonraki**.

    ![Sertifika deposu yapılandırma](./media/prepare-pki-certs/3.png)

    a. Bir PFX alıyorsanız, ek bir iletişim kutusu ile sunulur. Üzerinde **özel anahtar korumasını** sayfasında, sertifika dosyaları için parolayı girin ve sonra etkinleştirin **bu anahtarı verilebilir olarak işaretle. Bu sayede veya anahtarlarınızı sonradan** seçeneği. **İleri**’yi seçin.

    ![Anahtarı verilebilir olarak işaretle](./media/prepare-pki-certs/2.png)

1. İçeri aktarma işlemini tamamlamak için Son'u tıklatın.

### <a name="export-the-certificate"></a>Sertifika dışarı aktarma

Sertifika Yöneticisi MMC konsolu açın ve yerel makine sertifika deposuna bağlanın.

1. Microsoft Yönetim Konsolu'nu açın, Windows 10 Başlat menüsünden sağ tıklayın ve sonra Çalıştır'a tıklayın. Tür **mmc** Tamam'ı tıklatın.

1. Dosya,'ı Ekle/Kaldır ek bileşenini sonra sertifikaları seçin Ekle'ye tıklayın.

    ![Sertifikalar ek bileşenini Ekle](./media/prepare-pki-certs/mmc-2.png)
 
1. Bilgisayar hesabını seçin, İleri'ye tıklayın, sonra yerel bilgisayarı seçin sonra son. Ekle/Kaldır ek bileşenini sayfasını kapatmak için Tamam'a tıklayın.

    ![Sertifikalar ek bileşenini Ekle](./media/prepare-pki-certs/mmc-3.png)

1. Sertifikalar için Gözat > Kurumsal güven > sertifika konumu. Sağ tarafta sertifikanızı gördüğünüzü doğrulayın.

1. Görev çubuğu Sertifika Yöneticisi konsolundan seçin **eylemleri** > **tüm görevler** > **dışarı**. **İleri**’yi seçin.

  > [!NOTE]
  > Kaç tane Azure Stack bağlı olarak birden çok kez bu işlemi tamamlamak, sahip olduğunuz sertifikaları gerekebilir.

1. Seçin **Evet, özel anahtarı dışarı aktar**ve ardından **sonraki**.

1. Dışarı aktarma dosyası biçimi bölümünde **tüm genişletilmiş özellikleri dışarı** ve ardından **sonraki**.

1. Seçin **parola** ve sertifikalar için bir parola belirtin. Dağıtım parametresi olarak kullanıldığından, bu parolayı unutmayın. **İleri**’yi seçin.

1. Bir dosya adı ve dışarı aktarmak pfx dosyası için konum seçin. **İleri**’yi seçin.

1. **Son**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar
[PKI sertifikalarını doğrulama](azure-stack-validate-pki-certs.md)