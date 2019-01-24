---
title: Güvenlik bilgilerini - Azure Active Directory kullanarak uygulama parolaları ayarlamanız | Microsoft Docs
description: Her bir tarayıcı olmayan uygulamayla kullanmak için parolaları (uygulama parolaları)'i otomatik olarak oluşturulan ayarlama güvenlik bilgilerini kullanarak normal bir paroladan ayırın.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 211e282dc4334753b90d050dc82c8bf35ad145cd
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826216"
---
# <a name="manage-app-passwords-using-security-info-preview"></a>Güvenlik bilgilerini (Önizleme) kullanarak uygulama parolaları yönetme

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Outlook 2010 gibi belirli tarayıcı olmayan uygulamalar, iki aşamalı doğrulamayı desteklemez. Bu destek eksikliği, iki aşamalı doğrulama kullanıyorsanız, uygulama çalışmıyor anlamına gelir. Bu sorunla karşılaşmamak için normal parolasından ayrı her tarayıcı içi uygulaması ile birlikte kullanmak için otomatik olarak oluşturulan bir parola oluşturabilirsiniz.

Uygulama parolaları kullanırken unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulan ve tek uygulama girilen.

- Kullanıcı başına 40 parolalık bir sınır yoktur. Bir sonraki bu sınırı oluşturmayı denerseniz, yeni bir tane oluşturmak için izin verilen önce var olan bir parola silmek için istenir.

- Cihaz başına, uygulama başına değil bir uygulama parolasını kullanın. Örneğin, dizüstü bilgisayarınızda tüm uygulamalar için tek bir parola ve masaüstünüzde tüm uygulamalar için tek başka bir parola oluşturun.

    >[!Note]
    >(Outlook gibi) office 2013 istemcilerindeki yeni kimlik doğrulama protokolleri destekleyen ve iki aşamalı doğrulamayla birlikte kullanılabilir. Bu destek, iki aşamalı doğrulama açıldıktan sonra artık uygulama parolaları için Office 2013 istemcilerindeki gerektiğini anlamına gelir. Daha fazla bilgi için bkz. [Office 2013 ve Office 2016 istemci uygulamaları için nasıl modern kimlik doğrulaması çalıştığı](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) makalesi.

## <a name="create-and-delete-app-passwords-using-security-info"></a>Oluşturma ve güvenlik bilgilerini kullanarak uygulama parolalarını Sil

İki aşamalı doğrulama ile iş veya Okul hesabı ve yöneticinize güvenlik bilgisi deneyimi açık kullanırsanız, oluşturun ve uygulamalarım portalını kullanarak, uygulama parolalarını Sil.

Yöneticiniz güvenlik bilgisi deneyimi etkinleştirdiyseniz, bu henüz bilgileri ve yönergeleri izlemeniz gereken [iki aşamalı doğrulama için uygulama parolaları yönetme](multi-factor-authentication-end-user-app-passwords.md) bölümü.

### <a name="to-create-app-passwords-using-the-my-apps-portal"></a>Uygulamalarım portalı kullanarak uygulama parolaları oluşturmak için

1. İş veya Okul hesabınızda oturum açın.

2. Myapps.microsoft.com için sayfanın sağ üst köşesinden adınızı seçin ve ardından Git **profili**.

3. İçinde **hesabını yönetme** alanında **güvenlik bilgilerini Düzenle**.

    ![Profil düzenleme güvenlik bilgisi bağlantısının vurgulandığı ekran](media/security-info/security-info-profile.png)

4. İçinde **hesabınızı güvenli tutmak** ekranındayken **güvenlik bilgisi ekleyin**.

    ![Güvenlik bilgileri mevcut, düzenlenebilir bilgisi ekranla](media/security-info/security-info-edit-add-info.png)

5. İçinde **güvenlik bilgisi ekleyin** ekranındayken **uygulama parolası**.

6. İçinde **, uygulama parolası oluşturmanız** ekranında, uygulama parolası için bir ad yazın ve ardından **sonraki**.

    ![Uygulama parolanız isimlendirdiğiniz ekranı](media/security-info/security-info-name-app-password.png)

7. Seçin **kopyalama** parolayı panoya kopyalayın ve ardından **sonraki**.

    ![Kopyalama için uygulama parolasıyla ekran](media/security-info/security-info-create-app-password.png)
    
8. Uygulama parolası emin görünür **hesabınızı güvenli tutmak** ekran.

    ![Uygulama parolası ile güvenli ekran tutun](media/security-info/security-info-keep-secure-app-password.png)

### <a name="to-delete-app-passwords-using-the-my-apps-portal"></a>Uygulama parolaları uygulamalarım portalı kullanarak silmek için

1. Üzerinde **hesabınızı güvenli tutmak** ekranındayken **X** yanındaki silmek için uygulama parolası.

    ![Güvenli ekran tutmak, uygulama parolasını silme](media/security-info/security-info-keep-secure-delete-app-password.png)

2. İçinde **uygulama parolasını silme** ekranındayken **Sil**.

    ![Uygulama parolası Ekranı Sil](media/security-info/security-info-keep-secure-delete-app-password2.png)

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik bilgilerinizi güncelleştirmeniz gerekiyorsa, yönergeleri izleyin [güvenlik bilgilerinizi yönetmek](security-info-manage-settings.md) makalesi.

- Güvenlik bilgileri ve neler yapabileceğiniz hakkında daha fazla genel bilgi için bkz. [güvenlik bilgisi genel bakış](user-help-security-info-overview.md) 