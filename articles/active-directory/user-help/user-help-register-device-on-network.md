---
title: Kuruluşunuzun ağındaki - Azure Active Directory kullanarak kişisel Cihazınızı kaydetme | Microsoft Docs
description: Kuruluşunuzun korumalı kaynaklara erişebilmesi için kuruluşunuzun ağ üzerindeki kişisel cihazını kaydetmek öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 7126a47bd90168c7d86fe9fcc05fab0a60955063
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180925"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Kuruluşunuzun ağındaki kullanarak kişisel Cihazınızı kaydetme
Kişisel Cihazınızı, genellikle bir telefonda veya tablette, kuruluşunuzun ağ kaydedin. Cihazınız kaydedildikten sonra kuruluşunuzun kısıtlanmış kaynaklara erişebilir olacaktır.

>[!Note]
>Bu makalede Windows cihaz gösterim amaçlı kullanılmıştır, ancak iOS, Android ve macOS çalıştıran cihazlar da kaydedebilirsiniz.

## <a name="what-happens-when-you-register-your-device"></a>Cihazınızı kaydettiğinizde ne olur
Cihazınızın, kuruluşunuzun ağ kaydettirmekte, ancak aşağıdaki eylemler gerçekleşir:

- Windows cihazınızın, kuruluşunuzun ağ kaydeder.

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, iki aşamalı doğrulama ya da aracılığıyla ayarlamak için istenebilir [multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgisi](user-help-security-info-overview.md).

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, otomatik olarak Microsoft Intune gibi mobil cihaz Yönetimi'nde kayıtlı olması. Intune kaydetme hakkında daha fazla bilgi için bkz. [Cihazınızı ıntune'a kaydetme](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Kişisel Microsoft hesabınız için kullanıcı adı ve parola kullanarak oturum açma işlemi verilecektir.

## <a name="to-register-your-windows-device"></a>Windows Cihazınızı kaydetmek için

Ağınızdaki kişisel cihazını kaydetmek için aşağıdaki adımları izleyin.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seçin **e-posta & hesaplarına**ve ardından **bir Microsoft hesabına Katıl**.

    ![E-posta & hesaplarına ve bir Microsoft hesabı bağlantıları Ekle](./media/user-help-register-device-on-network/register-device-email-and-accounts.png)

3. Üzerinde **Microsoft hesabınızı ekleyin** ekran, kişisel Microsoft hesabınızın e-posta adresinizi yazın.

    ![E-posta ile Microsoft hesabı ekran ekleme](./media/user-help-register-device-on-network/register-device-add-accounts.png)

4. Üzerinde **parola gir** ekranında, kişisel Microsoft hesabınızın parolasını yazın ve ardından **oturum**.

    ![Parola ekranı girin](./media/user-help-register-device-on-network/register-device-enter-password.png)

5. (İki aşamalı doğrulama kullanıyorsanız), kimlik doğrulama isteğini onaylama ve Windows Hello'yu (gerekirse) ayarlama dahil olmak üzere kayıt işlemini tamamlayın.

## <a name="to-make-sure-youre-registered"></a>Emin olmak için kayıtlı
Ayarlarınızı bakarak kayıtlı emin olabilirsiniz.

1. Açık **ayarları**ve ardından **hesapları**.

    ![Ayarları ekranında hesapları](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seçin **e-posta & hesaplarına**ve kişisel Microsoft hesabınızı gördüğünüzden emin olun.

    ![Bağlı contoso hesabı ile iş veya Okul ekranına erişim](./media/user-help-register-device-on-network/register-device-verify-account.png)

## <a name="next-steps"></a>Sonraki adımlar
Kuruluşunuzun ağına kullanarak kişisel Cihazınızı kaydettikten sonra kaynakların çoğunu erişebilir olmalıdır.

- Kuruluşunuzun iş Cihazınızı ekleme isterse, bkz. [iş cihazınızın, kuruluşunuzun ağına katılın](user-help-join-device-on-network.md).



