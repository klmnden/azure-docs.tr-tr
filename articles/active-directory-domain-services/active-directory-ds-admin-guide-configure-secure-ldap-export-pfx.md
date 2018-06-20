---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde yapılandırma | Microsoft Docs
description: Güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yapılandırın
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: d2c7bd8b335ce49bed8e39812cccbe7ab474bf8f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36211535"
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Güvenli LDAP (LDAPS) Azure AD etki alanı Hizmetleri yönetilen etki alanı için yapılandırma

## <a name="before-you-begin"></a>Başlamadan önce
Tamamlandı olun [görev 1 - güvenli LDAP için bir sertifika edinin](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Görev 2 - güvenli LDAP sertifika verme bir. PFX dosyası
Bu görev başlamadan önce bir ortak sertifika yetkilisinden güvenli LDAP sertifikayı aldıktan veya otomatik olarak imzalanan bir sertifika oluşturduysanız emin olun.

LDAPS sertifikasını dışarı aktarmak için aşağıdaki adımları gerçekleştirin bir. PFX dosyası.

1. Tuşuna **Başlat** düğmesi ve türü **R**. İçinde **çalıştırmak** iletişim, türü **mmc** tıklatıp **Tamam**.

    ![MMC konsolunu başlatın](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Üzerinde **kullanıcı hesabı denetimi** İste'ye tıklayın **Evet** MMC (Microsoft Yönetim Konsolu) yönetici olarak başlatın.
3. Gelen **dosya** menüsünde tıklatın **Ekle/Kaldır ek bileşenini...** .

    ![Ek bileşeni MMC konsoluna ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. İçinde **Ekle veya Kaldır ek bileşenler** iletişim kutusunda **sertifikaları** ek bileşenini ve tıklatın **Ekle >** düğmesi.

    ![Sertifikalar ek bileşenini MMC konsoluna ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. İçinde **Sertifikalar ek bileşenini** seçin **bilgisayar hesabı** tıklatıp **sonraki**.

    ![Sertifikalar ek bileşenini bilgisayar hesabı ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Üzerinde **Bilgisayar Seç** sayfasında **yerel bilgisayar: (bu konsolun üzerinde çalıştığı bilgisayar)** tıklatıp **son**.

    ![Sertifikalar ek bileşenini - select bilgisayar ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. İçinde **Ekle veya Kaldır ek bileşenler** iletişim kutusunda, tıklatın **Tamam** MMC'ye Sertifikalar ek bileşenini eklemek için.

    ![Sertifikalar ek bileşenini bitti MMC'ye - Ekle](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. MMC penceresinde genişletmek için tıklatın **konsol kökü**. Yüklenen Sertifikalar ek bileşenini görmeniz gerekir. Tıklatın **sertifikalar (yerel bilgisayar)** genişletin. Genişletmek için tıklatın **kişisel** düğümünü, ardından **sertifikaları** düğümü.

    ![Açık kişisel sertifikalar deposuna](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Oluşturduğumuz otomatik olarak imzalanan sertifika görmeniz gerekir. Sertifikanın oluşturulması sırasında PowerShell Windows'ta bildirilen parmak iziyle eşleştiğinden emin olmak için sertifikanın özelliklerini inceleyebilirsiniz.
10. Kendinden imzalı bir sertifika seçin ve **sağ tıklatın**. Sağ tıklatma menüsünden seçin **tüm görevler** seçip **dışarı aktar...** .

    ![Sertifikayı dışarı aktarma](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. İçinde **Sertifika Verme Sihirbazı**, tıklatın **sonraki**.

    ![Dışarı aktarma Sertifika Sihirbazı](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Üzerinde **özel anahtarı dışarı** sayfasında, **Evet, özel anahtarı dışarı**, tıklatıp **sonraki**.

    ![Sertifika özel anahtarı dışarı aktar](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > Sertifika ile birlikte özel anahtarı dışarı aktarmanız gerekir. Sertifikanın özel anahtarı içermiyor bir PFX sağlarsanız, yönetilen etki alanınız için güvenli LDAP etkinleştirme başarısız olur.
    >
    >
13. Üzerinde **dışarı aktarma dosyası biçimi** sayfasında, **kişisel bilgi alış verişi - PKCS #12 (. PFX)** için dışarı aktarılan sertifikanın dosya biçimi olarak.

    ![Sertifika dosya biçimini Dışarı Aktar](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Yalnızca. PFX dosyasının biçimi desteklenmiyor. Sertifika verme. CER dosya biçimi.
    >
    >
14. Üzerinde **güvenlik** sayfasında, **parola** korumak için seçeneği ve bir parola yazın. PFX dosyası. İşlemin sonraki görev gerekecek beri bu parolayı unutmayın. Tıklatın **sonraki** devam etmek için.

    ![Sertifika dışarı aktarma için parola ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Bu parolayı not edin. Bu yönetilen etki alanı için güvenli LDAP etkinleştirirken gereken [görev 3 - güvenli LDAP yönetilen etki alanı için etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >
15. Üzerinde **dışarı aktarılan dosya** sayfasında, olduğu gibi sertifika dışarı aktarma konumu ve dosya adı belirtin.

    ![Sertifika dışa aktarma yolu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Aşağıdaki sayfasında, tıklatın **son** sertifikayı bir PFX dosyasına aktarmak için. Sertifika verildiğinde onay iletişim kutusu görmeniz gerekir.

    ![Bitti sertifikasını dışarı aktarma](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Sonraki adım
[Görev 3 - güvenli LDAP yönetilen etki alanı için etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
