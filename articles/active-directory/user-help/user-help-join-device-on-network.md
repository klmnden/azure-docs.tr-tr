---
title: İş Cihazınızı kuruluşunuzun ağına - Azure Active Directory'ye ekleme | Microsoft Docs
description: İş cihazınızın, kuruluşunuzun ağına katılın öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2ba6b2d33c3fb5d9fda6821718ac61513a958b7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369148"
---
# <a name="join-your-work-device-to-your-organizations-network"></a>İş cihazınızın, kuruluşunuzun ağa katılmasını sağlayın.
Potansiyel olarak kısıtlanmış kaynaklara erişebilmesi için iş ait Windows 10 Cihazınızı kuruluşunuzun ağa katılmasını sağlayın.

## <a name="what-happens-when-you-join-your-device"></a>Cihazınızı ekleme ne olur
Windows 10 cihazınızın, kuruluşunuzun ağına birleştirdiğimiz olsa da, aşağıdaki eylemler gerçekleşir:

- Windows cihazınız, kuruluşunuzun ağına, kişisel hesabınızı kullanarak kaynaklarınıza erişmenize izin vererek kaydeder. Cihazınız kaydedildikten sonra oturum açmak ve kısıtlanmış kaynaklara erişmek için kuruluşunuzun kullanıcı adı ve parola kullanabilmeniz için Windows Cihazınızı ardından ağa katılır.

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, iki aşamalı doğrulama ya da aracılığıyla ayarlamak için istenebilir [multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgisi](user-help-security-info-overview.md).

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, otomatik olarak Microsoft Intune gibi mobil cihaz Yönetimi'nde kayıtlı olması. Intune kaydetme hakkında daha fazla bilgi için bkz. [Cihazınızı ıntune'a kaydetme](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Otomatik oturum açma kuruluş hesabınızı kullanarak oturum açma işlemi verilecektir.

## <a name="to-join-a-brand-new-windows-10-device"></a>Yeni bir Windows 10 cihazını ekleme için
Cihazınızı yeni ve henüz ayarlanmadı, Cihazınızı ağa katılmak için Windows ilk çalıştırma deneyimi (OOBE) işlemiyle gidebilirsiniz.

1. Yeni cihaz kurulumu ve OOBE işlemi başlatabilirsiniz.

2. Üzerinde **Microsoft'ta oturum açma** ekranında, iş yazın ya da Okul e-posta adresi.

    ![Ekranında e-posta adresiyle oturum açın](./media/user-help-join-device-on-network/join-device-oobe-signin.png)

3. Üzerinde **parolanızı girmeniz** ekranında, parolanızı yazın.

    ![Parola ekranınızın girin](./media/user-help-join-device-on-network/join-device-oobe-password.png)

4. Hesabınıza erişebilmesi için mobil Cihazınızda Cihazınızı onaylayın. 

    ![Mobil Bildirim ekranı](./media/user-help-join-device-on-network/join-device-oobe-mobile.png)

5. Gizlilik ayarlarınızı ayarlama ve Windows Hello'yu (gerekirse) ayarlama dahil OOBE işlemini tamamlayın.

    Cihazınız artık kuruluşunuzun ağına katıldı.

## <a name="to-make-sure-youre-joined"></a>Emin olmak için birleştirilmiş
Ayarlarınızı bakarak katılmış emin olabilirsiniz.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Seçin **işe veya okula erişim**ve, gibi ifade metin gördüğünüzden emin **bağlı *< kuruluşunuz >* Azure AD'ye**.

    ![Bağlı contoso hesabı ile iş veya Okul ekranına erişim](./media/user-help-join-device-on-network/join-device-oobe-verify.png)


## <a name="to-join-an-already-configured-windows-10-device"></a>Önceden yapılandırılmış bir Windows 10 cihazını ekleme için
Bulunuyorsa cihazınız ve bir süre için zaten ayarlanmış, ağa Cihazınızı eklemek için aşağıdaki adımları izleyebilirsiniz.

1. Açık **ayarları**ve ardından **hesapları**.

2. Seçin **işe veya okula erişim**ve ardından **Connect**.

    ![Access iş veya Okul ve Connect bağlantıları](./media/user-help-join-device-on-network/join-device-access-work-school-connect.png)

3. Üzerinde **bir iş veya Okul hesabı ayarlama** ekranındayken **bu cihazı Azure Active Directory'ye katılmasını**.

    ![Ayarlanmış bir iş veya Okul hesabı ekran](./media/user-help-join-device-on-network/join-device-setup-join-aad.png)

4. Üzerinde **geçelim, açan** ekranında, e-posta adresinizi yazın (örneğin, alain@contoso.com) ve ardından **sonraki**.

    ![Şimdi, ekranda imzalı Al](./media/user-help-join-device-on-network/join-device-setup-get-signed-in.png)

5. Üzerinde **parola gir** ekranında, parolanızı yazın ve ardından **oturum**.

    ![Parola girin](./media/user-help-join-device-on-network/join-device-setup-password.png)

6. Hesabınıza erişebilmesi için mobil Cihazınızda Cihazınızı onaylayın. 

    ![Mobil Bildirim ekranı](./media/user-help-join-device-on-network/join-device-setup-mobile.png)

7. Üzerinde **kuruluşunuz bu olduğundan emin olun** ekranında, doğru olduğundan emin olmak için bilgileri gözden geçirin ve ardından **katılın**.

    ![Bu, kuruluş doğrulama ekranı olduğundan emin olun](./media/user-help-join-device-on-network/join-device-setup-confirm.png)

8. Üzerinde **hazırsınız** ekranında **Bitti**.

    ![Tüm küme ekran işiniz](./media/user-help-join-device-on-network/join-device-setup-finish.png)

## <a name="to-make-sure-youre-joined"></a>Emin olmak için birleştirilmiş
Ayarlarınızı bakarak katılmış emin olabilirsiniz.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Seçin **işe veya okula erişim**ve, gibi ifade metin gördüğünüzden emin **bağlı *< kuruluşunuz >* Azure AD'ye**.

    ![Bağlı contoso hesabı ile iş veya Okul ekranına erişim](./media/user-help-join-device-on-network/join-device-setup-verify.png)

## <a name="next-steps"></a>Sonraki adımlar
Cihazınızın, kuruluşunuzun ağına katılın sonra tüm kaynaklarınızı kullanarak, iş veya Okul hesap bilgileri başlatabilmeniz gerekir.

- Kuruluşunuzun, telefonunuz gibi kişisel Cihazınızı kaydetmenizi istediği olup [kuruluşunuzun ağ üzerindeki kişisel cihazını kaydetmek](user-help-register-device-on-network.md).

