---
title: Oluşturma bir. Azure AD Domain Services etki alanı için güvenli LDAP (LDAPS) Sertifika PFX dosyası
description: Bir Azure AD Domain Services'ı yönetmek için etki alanı bir güvenli LDAP sertifikası oluşturma
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: iainfou
ms.openlocfilehash: d89034610eb22108efc832d32ed9d8f593e9f12a
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67474386"
---
# <a name="create-a-pfx-file-with-the-secure-ldap-ldaps-certificate-for-a-managed-domain"></a>Oluşturma bir. Yönetilen bir etki alanı için güvenli LDAP (LDAPS) Sertifika PFX dosyası

## <a name="before-you-begin"></a>Başlamadan önce

Tam [1. Görev: Güvenli LDAP için sertifika edinme](configure-ldaps.md).

## <a name="task-2-export-the-secure-ldap-certificate-to-a-pfx-file"></a>2\. Görev: İçin güvenli LDAP sertifikasını dışarı aktarma bir. PFX dosyası

Bu görev başlamadan önce bir ortak sertifika yetkilisinden güvenli LDAP sertifikasını almak veya otomatik olarak imzalanan bir sertifika oluşturun.

LDAPS sertifikayı dışarı aktarmak için bir. PFX dosyası:

1. Tuşuna **Başlat** düğmesi ve türü **R**. İçinde **çalıştırma** iletişim kutusunda, türü **mmc** tıklatıp **Tamam**.

    ![MMC konsolunu başlatın](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Üzerinde **kullanıcı hesabı denetimi** İste'ye tıklayın **Evet** MMC (Microsoft Yönetim Konsolu) yönetici olarak başlatın.
3. Gelen **dosya** menüsünü tıklatın **Ekle/Kaldır ek bileşenini...** .

    ![Ek bileşeni MMC konsoluna ekleyin.](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. İçinde **ek bileşenler Ekle / Kaldır** iletişim kutusunda **sertifikaları** eklentisini seçeneğine tıklayıp **Ekle >** düğmesi.

    ![Sertifikalar ek bileşeni MMC konsoluna ekleyin.](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. İçinde **Sertifikalar ek bileşenini** seçin **bilgisayar hesabı** tıklatıp **sonraki**.

    ![Sertifikalar ek bileşenini bilgisayar hesabı ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Üzerinde **Bilgisayar Seç** sayfasında **yerel bilgisayar: (bu konsolun üzerinde çalıştığı bilgisayar)** tıklatıp **son**.

    ![Sertifikalar ek bileşenini - select bilgisayar ekleme](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. İçinde **ek bileşenler Ekle / Kaldır** iletişim kutusunda, tıklayın **Tamam** MMC'ye Sertifikalar ek bileşenini eklemek için.

    ![Sertifikalar ek bileşenini bitti MMC'ye - ekleyin.](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. MMC penceresinde genişletmek için tıklatın **konsol kökü**. Yüklenen Sertifikalar ek bileşenini görmeniz gerekir. Tıklayın **sertifikalar (yerel bilgisayar)** genişletin. Genişletmek için tıklatın **kişisel** düğümünü, ardından **sertifikaları** düğümü.

    ![Açık Kişisel Sertifikalar deposunda](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Oluşturduğumuz otomatik olarak imzalanan sertifika görmeniz gerekir. Sertifikayı oluştururken PowerShell'i windows üzerinde bildirilen parmak iziyle eşleştiğinden doğrulamak için bir sertifikanın özelliklerini inceleyebilirsiniz.
10. Kendinden imzalı bir sertifika seçin ve **sağ tıklayın**. Sağ tıklama menüsünden seçin **tüm görevler** seçip **dışarı aktar...** .

    ![Sertifikayı dışarı aktarma](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. İçinde **Sertifika Verme Sihirbazı**, tıklayın **sonraki**.

    ![Sertifika Dışarı Aktarma Sihirbazı'nı](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Üzerinde **özel anahtarı dışarı** sayfasında **Evet, özel anahtarı dışarı**, tıklatıp **sonraki**.

    ![Sertifika özel anahtarı dışarı aktar](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > Sertifika yanı sıra özel anahtarı dışarı aktarmanız gerekir. Sertifika için özel anahtar içermiyor bir PFX sağlarsanız, yönetilen etki alanınız için güvenli LDAP'yi etkinleştirme başarısız olur.
    >
    >

13. Üzerinde **dışarı aktarma dosyası biçimi** sayfasında **Personal Information Exchange – PKCS #12 (. PFX)** olarak dışa aktarılan sertifika için dosya biçimi.

    ![Sertifika dosya biçimini Dışarı Aktar](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Yalnızca. PFX dosya biçimi desteklenmiyor. Sertifikayı dışarı aktarma. CER dosyası biçimi.
    >
    >

14. Üzerinde **güvenlik** sayfasında **parola** korumak için seçeneği ve parola yazın. PFX dosyası. İşlemin sonraki görev gerekecek olduğundan, bu parolayı unutmayın. **İleri**’ye tıklayın.

    ![Sertifika dışarı aktarma için parola](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Bu parolayı not edin. Bu yönetilen etki alanı için güvenli LDAP etkinleştirilirken ihtiyaç [görev 3 - yönetilen etki alanı için güvenli LDAP'yi etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >

15. Üzerinde **dışarı aktarılan dosya** sayfasında, dosya adını ve konumunu istediğiniz sertifikayı dışarı aktarmak için belirtin.

    ![Sertifika dışarı aktarma yolu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Şu sayfada tıklayın **son** sertifikayı bir PFX dosyasına aktarmak için. Onay iletişim kutusunda, sertifikayı dışarı aktardığınızda görmeniz gerekir.

    ![Bitti sertifikasını dışarı aktarma](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)

## <a name="next-step"></a>Sonraki adım

[3. Görev: yönetilen etki alanı için güvenli LDAP'yi etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
