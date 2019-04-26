---
title: Bir kuruluşun ağındaki - Azure Active Directory kişisel cihazlarını kaydetme | Microsoft Docs
description: Kuruluşunuzun korumalı kaynaklara erişebilmesi için kuruluşunuzun ağ üzerindeki kişisel cihazını kaydetmek öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: lizross
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 041c8bb6b4de2bbe2cbeb4c1a89e452239ae57bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473695"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Kuruluşunuzun ağındaki kullanarak kişisel Cihazınızı kaydetme
Kişisel Cihazınızı (genellikle bir telefon veya tablet) kuruluşunuzun ağındaki kaydedin. Cihazınız kaydedildikten sonra kuruluşunuzun kısıtlanmış kaynaklara erişebilir olacaktır.

>[!Note]
>Bu makalede Windows cihaz gösterim amaçlı kullanılmıştır, ancak iOS, Android ve macOS çalıştıran cihazlar da kaydedebilirsiniz.

## <a name="what-happens-when-you-register-your-device"></a>Cihazınızı kaydettiğinizde ne olur
Cihazınızın, kuruluşunuzun ağ kaydettirmekte, ancak aşağıdaki eylemler gerçekleşir:

- Windows cihazınızın, kuruluşunuzun ağ kaydeder.

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, iki aşamalı doğrulama ya da aracılığıyla ayarlamak için istenebilir [multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgisi](user-help-security-info-overview.md).

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, otomatik olarak Microsoft Intune gibi mobil cihaz Yönetimi'nde kayıtlı olması. Intune kaydetme hakkında daha fazla bilgi için bkz. [Cihazınızı ıntune'a kaydetme](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Kullanıcı adı ve parola için iş veya Okul hesabınızı kullanarak oturum açma işlemi verilecektir.

## <a name="to-register-your-windows-device"></a>Windows Cihazınızı kaydetmek için

Ağınızdaki kişisel cihazını kaydetmek için aşağıdaki adımları izleyin.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seçin **işe veya okula erişim**ve ardından **Connect** gelen **işe veya okula erişim** ekran.

    ![Bağlan seçeneği vurgulanmış iş veya Okul ekranına erişim](./media/user-help-register-device-on-network/register-device-access-work-school-connect.png)

3. Üzerinde **bir iş veya Okul hesabı Ekle** ekranında, iş veya Okul hesabınızın e-posta adresinizi yazın ve ardından **sonraki**. Örneğin, alain@contoso.com.

4. İş veya Okul hesabınızda oturum açın ve ardından **oturum**.

5. (İki aşamalı doğrulama kullanıyorsanız), kimlik doğrulama isteğini onaylama ve Windows Hello'yu (gerekirse) ayarlama dahil olmak üzere kayıt işlemini tamamlayın.

## <a name="to-verify-that-youre-registered"></a>Kayıtlı doğrulamak için
Ayarlarınızı bakarak kayıtlı emin olabilirsiniz.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seçin **işe veya okula erişim**ve iş veya Okul hesabınızı gördüğünüzden emin olun.

    ![Bağlı contoso hesabı ile iş veya Okul ekranına erişim](./media/user-help-register-device-on-network/register-device-setup-verify.png)

## <a name="next-steps"></a>Sonraki adımlar
Kuruluşunuzun ağına kullanarak kişisel Cihazınızı kaydettikten sonra kaynakların çoğunu erişebilir olmalıdır.

- Kuruluşunuzun iş Cihazınızı ekleme isterse, bkz. [iş cihazınızın, kuruluşunuzun ağına katılın](user-help-join-device-on-network.md).



